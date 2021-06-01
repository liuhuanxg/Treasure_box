### Meta选项

1. #### abstract

    如果设置为True时，该模型为抽象基类，在创建表时不创建。

    abstract = True

2. #### app_label

    如果模型是在INSTALLED_APPS中定义之外的app中，则必须声明其所属的应用用程序

    app_label = 'myapp'

3. #### verbose_name

    对象的可读名称，在admin后台上单数形式的名称。

    verbose_name = '新闻'

4. #### verbose_name_plural

    对象的可读名称，在admin后台上复数形式的名称。

    verbose_name_plural = '新闻'

5. #### db_table

    用于模型的数据库表的名称

    db_table='music_album'

6. #### ordering

    对象的默认排序，用于在获取列表时使用

    ordering = ['-id']

7. #### permission

    创建额外的权限，将自动为模型创建除了add,change,delete,view之外新的权限。

    ```
    permissions = (("can_deliver_pizzas", "Can deliver pizzas"),)
    ```

8. #### default_permissions

    模型的默认权限，可以重新自定义此列表。例如：如果应用不需要任何默认权限，可以将其设置为空列表。必须在创建模型之前在模型上指定它，以防止创建任何遗漏的权限。`('add', 'change', 'delete', 'view')`

9. #### indexes

    在模型中定义索引。

    ```python
    from django.db import models
    
    class Customer(models.Model):
        first_name = models.CharField(max_length=100)
        last_name = models.CharField(max_length=100)
    
        class Meta:
            indexes = [
                models.Index(fields=['last_name', 'first_name']),
                models.Index(fields=['first_name'], name='first_name_idx'),
            ]
    ```