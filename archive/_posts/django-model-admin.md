---
title: Django ModelAdmin的使用
date: 2020-07-31 10:17:57
tags:
- Development
- Django
categories:
- Dev
---



>   若想在Admin中管理自己定义的model，需要在app下的admin.py中创建ModelAdmin。



# 创建

```python
from django.contrib import admin
from myproject.myapp.models import Author

class AuthorAdmin(admin.ModelAdmin):
    pass
admin.site.register(Author, AuthorAdmin)
```

```python
from django.contrib import admin
from .models import Author

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    pass
```

或者直接

```pyth
from django.contrib import admin
from myproject.myapp.models import Author

admin.site.register(Author)
```

注册后可在admin页面中管理'Author' Model。





# 属性



## 1. fields, exclude 

在编辑页面中包含或除去某些字段

```python
from django.contrib import admin

class AuthorAdmin(admin.ModelAdmin):
    fields = ('name', 'title') 

class AuthorAdmin(admin.ModelAdmin):
    exclude = ('birth_date',)
```



## 2. fieldsets 

如上fields的加强版

```python
from django.contrib import admin

class FlatPageAdmin(admin.ModelAdmin):
    fieldsets = (
        (None, {
            'fields': ('url', 'title', 'content', 'sites')
        }),
        ('Advanced options', {
            'classes': ('collapse',),
            'fields': ('registration_required', 'template_name'),
        }),
    )
```

