---
title: 自制编程语言--Tohka
date: 2021-03-11 14:23:19
categories:
- Tohka
---



>   主要实现词法分析器parser

# lex和token

lex，即lexeme的缩写，意为词汇、单词，其本身意为着字符串本身。例如字符串"import"就是一个lex。lex是词法分析器的输入，即词法分析器需要直接识别的原始材料。

token，是符号、象征的意思。它不是最直观的字面量。token是词法分析器的输出，即分析器读取了lex后的产出物。词法分析器所产出的东西很多，因此这个产出物通常是一个复合的结构，里面包含了lex、lex的长度、lex的位置等信息。token是供语法分析器使用的，用以校验语法。



# parser.h

```c++
/*
 * @Author: Nestcc
 * @Date: 2021-03-12 17:18:38
 * @LastEditors: Nestcc
 * @LastEditTime: 2021-03-16 12:21:56
 * @Description:  < file content >
 */

#ifndef _PARSER_H_
#define _PARSER_H_

enum TokenType {
    TOKEN_UNKNOWN,

    // data type
    TOKEN_NUM,
    TOKEN_STRING,
    TOKEN_ID,
    TOKEN_INTERPOLATION,

    // key word
    TOKEN_VAR,		        //'var'
    TOKEN_FUN,		        //'fun'
    TOKEN_IF,		        //'if'
    TOKEN_ELSE,	     	    //'else'	
    TOKEN_TRUE,	     	    //'true'
    TOKEN_FALSE,	     	    //'false'
    TOKEN_WHILE,	     	    //'while'
    TOKEN_FOR,	     	    //'for'
    TOKEN_BREAK,	     	    //'break'
    TOKEN_CONTINUE,          //'continue'
    TOKEN_RETURN,     	    //'return'
    TOKEN_NULL,	     	    //'null'

    // class and module
    TOKEN_CLASS,	     	    //'class'
    TOKEN_THIS,	     	    //'this'
    TOKEN_STATIC,     	   //'static'
    TOKEN_IS,		   // 'is'
    TOKEN_SUPER,	     	   //'super'
    TOKEN_IMPORT,     	   //'import'

    //分隔符
    TOKEN_COMMA,		   //','
    TOKEN_COLON,		   //':'
    TOKEN_LEFT_PAREN,	   //'('
    TOKEN_RIGHT_PAREN,	   //')'
    TOKEN_LEFT_BRACKET,	   //'['
    TOKEN_RIGHT_BRACKET,	   //']'
    TOKEN_LEFT_BRACE,	   //'{'
    TOKEN_RIGHT_BRACE,	   //'}'
    TOKEN_DOT,		   //'.'
    TOKEN_DOT_DOT,	   //'..'

    //简单双目运算符
    TOKEN_ADD,		   //'+'
    TOKEN_SUB,		   //'-'
    TOKEN_MUL,		   //'*' 
    TOKEN_DIV,		   //'/' 
    TOKEN_MOD,		   //'%'

    //赋值运算符
    TOKEN_ASSIGN,	   //'='

    // 位运算符
    TOKEN_BIT_AND,	   //'&'
    TOKEN_BIT_OR,	   //'|'
    TOKEN_BIT_NOT,	   //'~'
    TOKEN_BIT_SHIFT_RIGHT,  //'>>'
    TOKEN_BIT_SHIFT_LEFT,   //'<<'

    // 逻辑运算符
    TOKEN_LOGIC_AND,	   //'&&'
    TOKEN_LOGIC_OR,	   //'||'
    TOKEN_LOGIC_NOT,	   //'!'

    //关系操作符
    TOKEN_EQUAL,		   //'=='
    TOKEN_NOT_EQUAL,	   //'!='
    TOKEN_GREATE,	   //'>'
    TOKEN_GREATE_EQUAL,	   //'>='
    TOKEN_LESS,		   //'<'
    TOKEN_LESS_EQUAL,	   //'<='

    TOKEN_QUESTION,	   //'?'

    //文件结束标记,仅词法分析时使用
    TOKEN_EOF		   //'EOF'
};

struct Token {
    TokenType type;
    const char *start;
    uint32_t length;
    uint32_t line_no;
};

class Parser {
public:
    Parser() = default;
    Parser(VM *vm, const char *file, const char *sourceCode);

    /**
     * @description: 获取前一个字符
     * @param {*}
     * @return {*}
     */
    char look_ahead();

    /** 
     * @description: 下一个字符是否是期望的，是则读入并返回true，否则返回false
     * @param {*}
     * @return {*}
     */
    void metch_token();

    /**
     * @description: 跳过连续的空白
     * @param {*}
     * @return {*}
     */
    void skip_blanks();

    /**
     * @description: 解析token
     * @param {TokenType} type
     * @return {*}
     */    
    void parse_id(TokenType type);

    /**
     * @description: 
     * @param {*}
     * @return {*}
     */
    void get_next_token();
    void consume_curr_token();
    void consume_next_token();

    bool metch_next_char(char expected_char);


private:
    const char *file;
    const char *source_code;
    const char *next_char_ptr;
    char curr_char;
    Token curr_token;
    Token prev_token;
    VM *vm;

    Parser *parent;

    // 处于内嵌表达式之中时,期望的右括号数量.
    // 用于跟踪小括号对儿的嵌套
    int interpolation_expect_rparen_num;

    
    void get_next_char();
};

#endif

```







# parser.cpp

```c++
/*
 * @Author: Nestcc
 * @Date: 2021-03-12 17:18:43
 * @LastEditors: Nestcc
 * @LastEditTime: 2021-03-16 12:27:10
 * @Description:  < file content > 
 */

#include <iostream>
#include <string>

#include "common.h"
#include "vm.h"
#include "parser.h"

struct KeywordToken {
    char *keyword;
    uint8_t     length;
    TokenType   token;
};  //关键字(保留字)结构

//关键字查找表
KeywordToken keywords_token[] = {
   {"var",      3,  TOKEN_VAR},
   {"fun",      3,  TOKEN_FUN},
   {"if",       2,  TOKEN_IF},
   {"else",     4,  TOKEN_ELSE},
   {"true",     4,  TOKEN_TRUE},
   {"false",    5,  TOKEN_FALSE},
   {"while",	5,  TOKEN_WHILE},
   {"for",	    3,  TOKEN_FOR},
   {"break",    5,  TOKEN_BREAK},
   {"continue", 8,  TOKEN_CONTINUE},
   {"return",   6,  TOKEN_RETURN},
   {"null",     4,  TOKEN_NULL},
   {"class",    5,  TOKEN_CLASS},
   {"is",       2,  TOKEN_IS},
   {"static",   6,  TOKEN_STATIC},
   {"this",     4,  TOKEN_THIS},
   {"super",    5,  TOKEN_SUPER},
   {"import",   6,  TOKEN_IMPORT},
   {NULL,       0,  TOKEN_UNKNOWN}
};

static TokenType id_or_keyword(const char *str, size_t length) {
    for (size_t i = 0; keywords_token[i].keyword; i += 1) {
        if (keywords_token[i].length == length && 
        memcpy(keywords_token[i].keyword, str, length) == 0) {
            return keywords_token[i].token;
        }
    }

    return TOKEN_ID;
}

char Parser::look_ahead() {
    return *next_char_ptr;
}

bool Parser::metch_next_char(char expected_char) {
    if (look_ahead() == expected_char) {
        get_next_char();
        return true;
    }
    return false;
}

void Parser::get_next_char() {
    curr_char = *next_char_ptr++;
}

void Parser::skip_blanks() {
    while (isspace(curr_char)) {
        if (curr_char == '\n') { curr_token.line_no += 1; }
        get_next_char();
    }
    return ;
}

void Parser::parse_id(TokenType type) {
    while (isalnum(curr_char) || curr_char == '_') {
        get_next_char();
    }

    size_t wlen = (size_t) (next_char_ptr - curr_token.start - 1);

    if (type != TOKEN_UNKNOWN) { curr_token.type = type; }
    else { curr_token.type = id_or_keyword(curr_token.start, wlen); }

    curr_token.length = wlen;
    return ;
}

Parser::Parser(VM* vm, const char* file, const char* source_code) {
    this -> vm = vm;
    this -> file = file;
    this -> source_code = source_code;

    curr_char = *this -> source_code;
    next_char_ptr = this -> source_code + 1;
    curr_token.line_no = 1;
    curr_token.type = TOKEN_UNKNOWN;
    curr_token.start = nullptr;
    curr_token.length = 0;
    
    prev_token = curr_token;
    
    interpolation_expect_rparen_num = 0;

};
```



