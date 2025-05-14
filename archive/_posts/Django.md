---
title: Django一些常用操作
date: 2020-07-27 15:10:00
categories:
- Dev
---



# 判断用户是否登录

```python
request.user.is_authenticated
```



# 需要用户登录

``` python
from django.contrib.auth.mixins import LoginRequiredMixin

class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
```

或者

``` python
from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    ...
```



# Model中datetime默认为当前时间

``` python
from django.utils.timezone import now

models.DateTimeField(default=now)
```





# 图片删除

```python
def delete(self, using=None, keep_parents=False):
    storage, path = self.image.storage, self.image.path
    storage.delete(path)
    super(NepArticleImage, self).delete()
```



# 3.0后xframe显示

```python
from django.views.decorators.clickjacking import xframe_options_exempt

@xframe_options_exempt
def __(request):
    ...
```





# 数据中有"<"类似符号

当传到template中的数据包含"<"之类的符号，例如传入html片段时，若直接使用{{ content }}会导致"<"被转码，进而使"<"变为"\&gt;"。如：

```html
<div>&lt;p&gt;ssdasdsa&lt;/p&gt;</div> // <p>ssdasdsa</p>
```

需要改为一下写法：

```
{{ content | safe }}
```

其中 safe可以防止转意。

