## ModelAdmin方法

1. ### save_model

    根据save_model方法是添加还是更改HttpRequest对象，覆盖此方法可进行保存或保存后的操作。调用super().save_model()以使用Model.save()方法来保存对象。

    ```python
    class GoodsInfoAdmin(admin.ModelAdmin):
    	def save_model(self, request, obj, form, change):   	
        	obj.seller=request.user   
    	super(GoodsInfoAdmin, self).save_model(request, obj, form, change)
    ```

2. **delete_model**

    重写此方法可以执行删除前或者删除后的操作。调用super().delete_model()以使用Model.delete()删除对象。

    重写此方法可为”删除所选对象“操作自定义删除过程。

3. **get_ordering**

    该方法采用request作为参数，并且返回与属性相似的list或tuple用于排序。如：

    ```python
    class PersonAdmin(admin.ModelAdmin):
    
        def get_ordering(self, request):
            if request.user.is_superuser:
                return ['name', 'rank']
            else:
                return ['name']
    ```

4. get_readonly_fields

    ```python
    class PersonAdmin(admin.ModelAdmin):
        def get_readonly_fields(self, request, obj=None):   
                if obj:      
                    if request.user.is_superuser:         
                        return ['publisher']      
                    return ['publisher']   
                return ['publisher']
    ```

    

5. get_list_display

    指定HttpRequest,返回一个list或者tuple类型的字段名称，这些名称将显示在更改列表是图中。类似list_display。

6. get_urls

    在`get_urls`上一个方法`ModelAdmin`中相同的方式，URL配置要用于该返回的ModelAdmin的URL。因此，可以对url进行扩充：

    ```python
    from django.contrib import admin
    from django.template.response import TemplateResponse
    from django.urls import path
    
    class MyModelAdmin(admin.ModelAdmin):
        def get_urls(self):
            urls = super().get_urls()
            my_urls = [
                path('my_view/', self.my_view),
            ]
            return my_urls + urls
    
        def my_view(self, request):
            # ...
            context = dict(
               # Include common variables for rendering the admin template.
               self.admin_site.each_context(request),
               # Anything else you want in the context...
               key=value,
            )
            return TemplateResponse(request, "sometemplate.html", context)
    ```

    如果要使用管理员布局，请从`admin/base_site.html`以下扩展：

    ```python
    {% extends "admin/base_site.html" %}
    {% block content %}
    ...
    {% endblock %}
    ```

    在此示例中，`my_view`将通过访问 `/admin/myapp/mymodel/my_view/`（假设管理URL位于）`/admin/`

7. formfield_for_foreignkey

    对外键关联的数据做筛选：

    ```python
    class RecordAdmin(admin.ModelAdmin):
        def formfield_for_foreignkey(self, db_field, request, **kwargs):
            if db_field.name == "version":
                kwargs['queryset'] = Version.objects.all().exclude(oss_links='')
            return super().formfield_for_foreignkey(db_field, request, **kwargs)
    ```

8. formfield_for_manytomany

    与`formfield_for_foreignkey`方法类似，`formfield_for_manytomany`可以重写该方法以将默认表单字段更改为多对多字段。例如，如果所有者可以拥有多辆汽车，并且汽车可以属于多个所有者（多对多关系），则可以过滤`Car`外键字段以仅显示拥有的汽车`User`：

    ```python
    class MyModelAdmin(admin.ModelAdmin):
        def formfield_for_manytomany(self, db_field, request, **kwargs):
            if db_field.name == "cars":
                kwargs["queryset"] = Car.objects.filter(owner=request.user)
            return super().formfield_for_manytomany(db_field, request, **kwargs)
    ```

9. formfield_for_choice_field

    与`formfield_for_foreignkey`和`formfield_for_manytomany` 方法一样，`formfield_for_choice_field`可以重写该方法以更改已声明选择的字段的默认formfield。例如，如果超级用户可用的选择与普通员工可用的选择不同，则可以按以下步骤进行：

    ```python
    class MyModelAdmin(admin.ModelAdmin):
        def formfield_for_choice_field(self, db_field, request, **kwargs):
            if db_field.name == "status":
                kwargs['choices'] = (
                    ('accepted', 'Accepted'),
                    ('denied', 'Denied'),
                )
                if request.user.is_superuser:
                    kwargs['choices'] += (('ready', 'Ready for deployment'),)
            return super().formfield_for_choice_field(db_field, request, **kwargs)
    ```

10. 待续。。。


