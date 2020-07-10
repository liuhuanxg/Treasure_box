### URL调度器

#### 一、请求处理流程

```python
from django.urls import path,include
```

当一个用户请求Django站点的页面时，Django系统按以下流程执行python代码：

1. Django首先确定要使用的根URLconf模块，通常，这是在settings.py中设置的[`ROOT_URLCONF`](https://docs.djangoproject.com/zh-hans/2.1/ref/settings/#std:setting-ROOT_URLCONF)的值。但是如果传入`HttpRequest`对象具有[`urlconf`](https://docs.djangoproject.com/zh-hans/2.1/ref/request-response/#django.http.HttpRequest.urlconf) 属性（由中间件设置），则将使用其值代替[`ROOT_URLCONF`](https://docs.djangoproject.com/zh-hans/2.1/ref/settings/#std:setting-ROOT_URLCONF)设置。
2. Django加载该Python模块并查找变量 `urlpatterns`。这是[`django.urls.path()`](https://docs.djangoproject.com/zh-hans/2.1/ref/urls/#django.urls.path) 和/或[`django.urls.re_path()`](https://docs.djangoproject.com/zh-hans/2.1/ref/urls/#django.urls.re_path)实例的Python列表。
3. Django依次匹配每个URL模式，在与请求的URL匹配的第一个模式停下来。
4. 一旦与其中一个URL模式匹配，Django就会导入并调用给定的视图，该视图是一个简单的Python函数（或基于类的视图）。该视图将传递以下参数：
    - 一个[`HttpRequest`](https://docs.djangoproject.com/zh-hans/2.1/ref/request-response/#django.http.HttpRequest)实例。
    - 如果匹配的URL模式未返回命名组，则来自正则表达式的匹配项将作为位置参数。
    - 关键字参数由与路径表达式匹配的任何命名部分组成，并由可选的`kwargs`参数中指定的任何参数覆盖 。[`django.urls.path()`](https://docs.djangoproject.com/zh-hans/2.1/ref/urls/#django.urls.path)[`django.urls.re_path()`](https://docs.djangoproject.com/zh-hans/2.1/ref/urls/#django.urls.re_path)
5. 如果没有URL模式匹配，或者在此过程中的任何时候引发异常，Django都会调用一个适当的错误处理视图。

#### 二、URL中携带的参数

1. 在URL中传递的参数，要使用尖括号去传递参数。

    ```python
    path('articles/<int:year>/', views.year_archive),
    ```

2. 参数的类型

    - str

        匹配任何非空字符串，但路径分隔符除外'/'。如果在表达式中不包含转换器，则为默认设置。

    - int

        匹配零或任何正整数。返回一个int。

    - slug

        匹配一个由ASCII字母或数字以及连字符和下划线字符组成的任何条形字符串。

    - uuid

        匹配格式化的UUID。为防止多个URL映射到同一页面，必须包含破折号并且字母必须小写。例如：**075194d3-6885-417e-a8a8-6c931e272f00**

    - path

        匹配任何非空字符串，包括路径分隔符'/'。可以匹配完整的URL路径进行匹配，而不仅仅是与URL路径的一部分进行匹配str。

#### 三、使用正则表达式

​		如果路径和转换器不足以定义URL模式时，则还可以使用正则表达式。此时可以使用re_path()代替path()。在python正则表达式中，命名真这个表达式组的语法为(?P<name>pattern)，name为组的名称，pattern是匹配的某种模式。

```python
from django.urls import path, re_path

from . import views

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<slug>[\w-]+)/$', views.article_detail),
]
```

