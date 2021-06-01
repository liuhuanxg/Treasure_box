### Django自带的分页方法

1、引入

```python
from django.core.paginator import Paginator,PageNotAnInteger,EmptyPage
```

2、使用

```python
p = Paginator(new_list,1)		#news_list代表所有的数据,1代表每页的条数	
number = p.num_pages			#总的页码数
page_range = p.page_range  		#所有页码的迭代器
new_list.has_previous()			#是否有上一条数据
new_list.has_next()				#是否有下一条数据
new_list.previous_page_number()	#上一页的页码
new_list.next_page_number()		#下一页的页码

```

3、示例

```python
from .models import News
def news(request):
    news_list = News.objects.order_by('id')
    p = Paginator(news_list,1)
    number = p.num_pages  #页码的总数
    page_range = p.page_range
    try:
        page = int(request.GET.get('page', 1))
    except:
        page = 1
    try:
        data = p.page(page)
    except:
        raise Http404
    page = int(page)
    if page < 4:   #一次只返回四个页码
        page_list = page_range[:4]
    elif page + 4 > number:
        page_list = page_range[-4:]
    else:
        page_list = page_range[page - 2:page + 2]
    print(p.count)
    return render(request, 'changed/news.html',{"news":news,"new_list":data,"page_range":page_list})

```

在前端中使用：

```html
 {% if new_list.has_previous %}
     <li><a href="?page={{ new_list.previous_page_number }}" class="pag-item">&lt;&lt;</a></li>
 {% endif %}
{% for page in page_range %}
     {% if page == request.GET.page|add:0 %}
         <li><a href="?page={{ page }}" class="pag-item pag-active" >{{ page }}</a></li>
     {% else %}
          <li><a href="?page={{ page }}" class="pag-item" >{{ page }}</a></li>
      {% endif %}
{% endfor %}
       {% if new_list.has_next %}
          <li><a href="?page={{ new_list.next_page_number }}" class="pag-item">&gt;&gt;</a></li>
       {% endif %}
```

4、自定义分页器方法

```python
def set_page(data,num,page):
    """
    :param data: 所有的数据
    :param num:  每页的数据个数
    :param page: 当前的页码
    :return:
    """
    p = Paginator(data,num)
    number = p.num_pages
    page_range = p.page_range
    try:
        page = int(page)
        data = p.page(page)
    except:
        data = p.page(1)
    if page < 5:  # 一次只返回5个页码
        page_list = page_range[:5]
    elif page + 4 > number:
        page_list = page_range[-5:]
    else:
        page_list = page_range[page - 3:page + 2]
    return data,page_list
```

