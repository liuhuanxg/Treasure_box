## QuerySet API使用

QuerySet可以构造、过滤、切片、和大致的结果而不实际访问数据库。除非执行某些操作来评估查询集，否则实际上不会发生对数据库的查询活动。

### **(1)** **基本属性**

1. **迭代**

    QuerySet是可迭代的，并且在第一次对其迭代是会执行其数据库查询。如：

    ```python
    for e in Entry.objects.all():
    	 print(e.headline)
    ```

2. **切片**

    可以使用python的数组切片语法对QuerySet集进行切片操作。

3. **len**

    返回QuerySet列表的长度。

4. **list**

    使用list()进行强制转化。entry_list = list(Entry.objects.all())

5. **exists**

    判断结果集中是否存在至少一个结果。Entry.objects.all().exists()。

### **(2)** **查询**

1. **filter**

    ​	条件查找，是一个结果集，多个条件时使用逗号(,)隔开。Entry.objects.filter(id=3,name=’张三’)。

2. **exclude**

    筛选出与给定查找参数不匹配的结果，是一个结果集，多个条件时使用逗号(,)隔开。Entry.objects.exclude(id=3,name=’张三’)。括号里面的参数属于and关系。当想使用or关系时要写多个exclude()。

3. **annotate**

    表达式可以是简单值，也可以是对模型（或任何相关模型）上字段的引用，也可以是针对与对象中的对象相关的对象计算出的聚合表达式（平均值，总和等）

    ```python
    from django.db.models import Count
    q = Blog.objects.annotate(Count('entry'))
    ```

4. **order_by**

    ​	默认情况下可以在**Meta**给出的ordering排序元组进行排序，可以使用order_by()对ordering的方法进行覆盖，额外对查询结果进行排序。

    如：

    ```python
    Entry.objects.order_by('headline')
    ```

    在字段名称前加负号(-)代表降序。

    如果想要随机排序，可以使用"?"，如下：

    ```python
    Entry.objects.order_by('?')
    ```

    **注意：**order_by('?')查询比较缓慢。

    多次使用order_by时会使前边的排序规则失效。

5. **reverse**

    使用reverse()可以翻转查询集元素的返回顺序。如：检索查询集中”最后”五个项目，可以执行以下操作：

    ```python
    Entry.objects.order_by('headline').reverse()[:5]
    ```

6. **distinct**

    消除重复的行。

    ```python
    Entry.objects.order_by('pub_date').distinct('pub_date')
    Entry.objects.order_by('blog').distinct('blog')
    ```

7. **values**

    返回QuerySet用作迭代器时返回的字典，而不是模型实例，这些词典中的每一个都代表对象，键对应于模型对象的属性名称。

    ```python
    Blog.objects.filter(name__startswith='Beatles')
    #<QuerySet [<Blog: Beatles Blog>]>
    Blog.objects.filter(name__startswith='Beatles').values()
    #<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
    ```

    values()方法可以采用可选的位置参数，如果指定字段，则每个字典将仅包含指定的字段的键/值，如果不指定字段，则每个字段将为数据库表忠中的每个字段包含一个键和值。如：

    ```python
    Blog.objects.values()
    #<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
    
    Blog.objects.values('id', 'name')
    #<QuerySet [{'id': 1, 'name': 'Beatles Blog'}]>
    ```

    values()子句中的聚合在同一values()子句中的其他参数之前应用。如果需要按另一个值分组，请将其添加到更早的values()子句中。例如：

    ```python
    from django.db.models import Count
    Blog.objects.values('entry__authors',entries=Count('entry'))
    #<QuerySet [{'entry__authors': 1, 'entries': 20}, {'entry__authors': 1, 'entries': 13}]
    
    Blog.objects.values('entry__authors').annotate(entries=Count('entry'))
    #<QuerySet [{'entry__authors': 1, 'entries': 33}]>
    ```

8. **values_list**

    类似于values，但是返回元组形式的数据，每个元组包含来自传递到values_list()中调用的相应字段或表达式的值。

    ```python
     Entry.objects.values_list('id', 'headline')
    #<QuerySet [(1, 'First entry'), ...]>
    
    from django.db.models.functions import Lower	
    Entry.objects.values_list('id', Lower('headline'))
    #<QuerySet [(1, 'first entry'), ...]>
    ```

    如果仅传递单个字段，则也可以传递flat参数。如果flat为True，则表示返回的结果是单个值，而不是一个元组。

    ```python
    Entry.objects.values_list('id').order_by('id')
    #<QuerySet[(1,), (2,), (3,), ...]>
    
    Entry.objects.values_list('id', flat=**True**).order_by('id')
    #<QuerySet [1, 2, 3, ...]>
    ```

    可以通过named=True来获取结果named tuple()

    ```python
    Entry.objects.values_list('id', 'headline', named=**True**)
    QuerySet [Row(id=1, headline='First entry'), ...]>
    ```

    如果不向values_list()中传递任何值，则按照声明的顺序返回模型中的所有字段，通常需要获取某个模型实例的特定字段值，因此需要values_list()配合get()使用。

    ```python
    Entry.objects.values_list('headline', flat=**True**).get(pk=1)
    #'First entry'
    ```

9. **dates**

    **dates**（field，kind，order ='ASC'），返回结果是一个QuerySet，结果是一个datetime.date对象列表，该对象列表表示的内容中也定种类的所有可用日期QuerySet。

    Field应该是DateField类型，kind可以为year，month，week，day。

    -  “year”返回该字段的所有不同年份值的列表。
    -  “month”返回该字段的所有不同年份/月份值的列表。
    -  “week”返回该字段的所有不同年份/星期值的列表。所有日期均为星期一。
    - “day”返回该字段所有不同年/月/日值的列表。

10. **none**

    创建一个空的查询集，该查询集不返回任何对象，并且在访问结果时不执行任何操作。

    ```python
    Entry.objects.none()
    <QuerySet []>
    from django.db.models.query import EmptyQuerySet
    
    isinstance(Entry.objects.none(), EmptyQuerySet)
    True
    ```

11. **all**

    查询所有的数据。

12. **raw**

    执行该查询会进行原始SQL查询。

    ```python
    for p in Person.objects.raw('SELECT * FROM myapp_person'):     		
    	print(p)  #John SmithJane Jones
    ```

13. **Q**

    and（&）连接符：

    **from** **django.db.models** **import** Q

    以下为等价查询：

    ```python
    Model.objects.filter(x=1) & Model.objects.filter(y=2)
    Model.objects.filter(x=1, y=2) 
    Model.objects.filter(Q(x=1) & Q(y=2))
    等效于	SELECT ... WHERE x=1 AND y=2
    ```

14. **or(|)**

    ```python
    Model.objects.filter(x=1) | Model.objects.filter(y=2)
    from django.db.models import Q
    Model.objects.filter(Q(x=1) | Q(y=2))
    ```

    SQL等效项：

    SELECT ... WHERE x=1 OR y=2;

15. **contains**

    模糊查询，区分大小写。

    ```python
    Entry.objects.get(headline__contains='Lennon')
    ```

16. **icontains**

    模糊查询，不区分大小写。

    ```python
    Entry.objects.get(headline__icontains='Lennon')
    ```

17. **in**

    查询在列表、元组或其他查询集中的数据，也可接收字符串。

    ```python
    Entry.objects.filter(id__in=[1, 3, 4])
    
    Entry.objects.filter(headline__in='abc')
    ```

    ​	**等价于：**

    ```mysql
    SELECT ... WHERE id IN (1, 3, 4);
    
    SELECT ... WHERE headline IN ('a', 'b', 'c');
    ```

18. **gt**

    大于。

    ```python
    Entry.objects.filter(id__gt=4)
    ```

19. **gte** 	

    大于等于。

    ```python
    Entry.objects.filter(id__gte=4)
    ```

20. **lt**

    小于。

    ```python
    Entry.objects.filter(id__lt=4)
    ```

21. **lte**

    小于等于。

    ```python
    Entry.objects.filter(id__lte=4)
    ```

22. **startswith**

    区分大小写，以....开头

    ```python
    Entry.objects.filter(headline__startswith='Lennon')
    ```

23. **istartswidth**

    不区分大小写，以...开头

    ```python
    Entry.objects.filter(headline__istartswith='Lennon')
    ```

24. **endswith**

    区分大小写，以...结束

    ```python
    Entry.objects.filter(headline__endswith='Lennon')
    ```

25. **iendswith**

    区分大小写，以...结束

    ```python
    Entry.objects.filter(headline__iendswith='Lennon')
    ```

26. **range**

    范围查询。

    ```python
    import datetime
    
    start_date = datetime.date(2005, 1, 1)
    
    end_date = datetime.date(2005, 3, 31)
    
    Entry.objects.filter(pub_date__range=(start_date, end_date))
    ```

27. **F**

    F允许Django在未实际链接数据的情况下，具有对数据库字段的值的引用。通常情况下会在更新数据时先从数据库里对原始数据取出后放在内存里，然后编辑某些属性进行提交。如：

    ```python
    order = Order.objects.get(orderid='123456789')
    order.amount += 1
    order.save()
    ```

    此时的SQL语句等效为：

    ```mysql
    UPDATE core_order SET ..., amount = 22 WHERE core_order.orderid = '123456789' 
    # ...表示Order中的其他值，在这里会重新赋一遍值; 22表示为计算后的结果
    ```

    当使用F()函数时：

    ```python
    from django.db.models import F
    from core.models import Order
     
    order = Order.objects.get(orderid='123456789')
    order.amount = F('amount') - 1
    order.save()
    ```

    此时SQL语句等价于：

    ```mysql
    UPDATE core_order SET ..., amount = amount + 1 WHERE core_order.orderid = '123456789'
    ```

    在使用此种方法跟新数据之后，需要重新加载数据来使数据库中的值与程序中的值对应：

    ```python
    order= Order.objects.get(pk=order.pk) 
     
    #  或者使用更加简单的方法：
    order.refresh_from_db()
    ```

### **(3)** **不适用于缓存的查询**

1. **count**

    返回一个整数，该整数表示数据库中与匹配的对象数。

2. **in_bulk**

    获取字段值（id_list）和filed_name这些值的列表，并返回将每个值映射到具体给定字段值的对象实例的字典。如果id_list未提供，则返回查询集中所有对象。Filed_name必须是唯一字段，并且默认为主键。

    ```python
     Blog.objects.in_bulk([1])
    
    #{1: <Blog: Beatles Blog>} 
    
    Blog.objects.in_bulk([1, 2])
    
    #{1:<Blog:BeatlesBlog>,2:<Blog:CheddarTalk>}
    
    Blog.objects.in_bulk([]){}Blog.objects.in_bulk()
    
    #{1: <Blog: Beatles Blog>, 2: <Blog: Cheddar Talk>, 3: <Blog: Django Weblog>}
    
    Blog.objects.in_bulk(['beatles_blog'], field_name='slug')
    
    #{'beatles_blog': <Blog: Beatles Blog>}
    ```

    **注意：**

    如果在in_bluk中传递一个空列表，则会得到一个空字典。

3. **latest**

    根据给定的字段返回列表中的最新对象。例：

    ```python
    Entry.objects.latest('pub_date')
    ```

4. **first**

    返回结果集中匹配到的第一个对象。

    ```python
    p = Article.objects.order_by('title', 'pub_date').first()
    ```

5. **last**

    类似于first，返回结果集中的最后一个。

6. **aggregate**

    返回计算出的合计值（平均值、总和等）的字典QuerySet。每个参数都指定一个值，该值将包含在返回的字典中。例：

    ```python
    from django.db.models import Count
    
    q = Blog.objects.aggregate(Count('entry'))
    {'entry__count': 16}
    ```

    

7. **get**

    根据条件查询单条数据，查不到时会报错。如果希望结果集中返回一行，则可以使用get()不加任何参数的行来返回改行的对象：

    ```python
    entry = Entry.objects.filter(...).exclude(...).get()
    ```

### **(4)** **保存**

1. **create**

    可以一步创建对象并将其全部保存：

    ```python
    p = Person.objects.create(first_name="Bruce", last_name="Springsteen")
    ```

2. **save**

    ```python
    p = Person(first_name="Bruce", last_name="Springsteen")
    
    p.save(force_insert=**True**)
    ```

    或者：

    ```python
    P=Person()
    p.first_name="Bruce"
    p.last_name="Springsteen"
    p.save()
    ```

3. **get_or_create**

    一种使用给定查找对象的便捷方法kwargs（如果模型的所有字段均具有默认值，则为空），并在必要时创建一个对象。

4. **update_or_create**

    一种使用给定对象更新对象的便捷方法，该update_or_create方法尝试根据给定的值从数据库中获取对象kwargs。如果找到匹配项，它将更新defaults字典中传递的字段 。

### **(5)** **修改**

1. **update**

    对指定的字段执行SQL更新查询，并返回匹配的行数（如果某些行已经具有新值，则该行数可能不等于更新的行数）

    ```python
    Entry.objects.filter(pub_date__year=2010).update(comments_on=False, headline='This is old')
    ```

2. **save**

    使用save方法进行修改。

    ```python
    e = Entry.objects.get(id=10)
    
    e.comments_on = False
    e.save()
    ```

### **(6)** **删除**

1.  **delete**

    对QuerySet执行删除操作。例：

    ```python
    Entry.objects.filter(blog=b).delete()
    
    #(4, {'weblog.Entry': 2, 'weblog.Entry_authors': 2})
    ```

    

