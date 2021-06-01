## Django的From类

Django表单系统的核心组件是Form类，Form类标书一张表单并决定它如何工作以及呈现。

类似模型类中的字段映射到数据库字段的方式，表单类的字段会映射到HTML表单的input元素。ModelForm通过Form映射模型类的字段到HTML表单的input元素，Django的admin就基于此。

表单字段本身也是累，他们管理表单数据并在提交表单时执行验证。DateField和FileField处理的数据类型差别较大，所以必须用来处理不同的字段。

在浏览器中，表单字段以HTML“控件”（用户界面的一个片段）的形式展现给我们，每个字段类型都有与之相匹配的控件类，但在必要时可以覆盖。

### 一、在Django 中构建一张表单

Form类：在forms.py中去定义：

```python
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
```

当设置了最大长度时，在前端会自动校验输入的字符的长度，在前端中的表现形式为：

```python
<label for="your_name">Your name: </label>
<input id="your_name" type="text" name="your_name" maxlength="100" required>
```

**视图**

发回Django网站的表单数据由视图来处理，一般和发布这个表单用的是同一个视图。这允许我们重用一些相同的逻辑。

为了处理标表单，需要将它实例化到一我们希望发布的URL的对应的视图中：

**views.py:**

```python
from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import NameForm

def get_name(request):
    # 当POST请求时
    if request.method == 'POST':
        # 获取提交的数据
        form = NameForm(request.POST)
        # 检验是否符合
        if form.is_valid():
            #处理数据
            your_name = form.cleaned_data['your_name']
            return HttpResponseRedirect('/thanks/')

    # 当发起的是一个GET请求时，把表单返回
    else:
        form = NameForm()

    return render(request, 'name.html', {'form': form})
```

如果访问视图时用的GET请求，它会创建一个空的表单实例并将其放置在模板上下文中进行渲染。

如果表单提交用的是POST请求，那么该视图将再次创建一个表单实例并使用请求中的数据填充它：form = NameForm(request.POST)，叫做“将数据绑定到表单”。

调用表单的is_valid()方法；如果不为True时，讲表单返回到模板。这时表单将不再为空，所以HTML表单将用之前提交的数据进行填充，放到可以根据需要进行编辑和修正的位置。

如果is_valid()为True，我们就能在cleaned_data属性中找到所有通过验证的表单数据。

**模板**

在模板文件中不需要做太多的操作：

```python
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit">
</form>
```

所有表单字段及其属性将通过Django模板语言从{{ form }}中被解包为HTML标记。