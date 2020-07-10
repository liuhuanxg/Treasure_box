### 一、函数免除csrf校验

```python
from django.views.decorators.csrf import csrf_exempt# 免除csrf校验@csrf_exempt
def users(request):    
	uses_list = ["柚子", "西瓜"]    
	return HttpResponse(json.dumps(uses_list))
```

### 二、对类免除csrf校验

1. #### 第一种方式

    ```python
    # dispatch是类视图的根方法，通过dispatch进行反射找到其他请求
    
    from django.views.decorators.csrf import csrf_exempt
    from django.utils.decorators import method_decorator
    class StudentsView(View):
        """student view"""
    	@method_decorator(csrf_exempt)
        def dispatch(self, request, *args, **kwargs):
            print("before")
            ret = super(StudentsView, self).dispatch(request, *args, **kwargs)
            print("after")
            return ret(request, *args, **kwargs)
        
        def get(self,*args,**kwargs):
            return HttpResponse("get")
    
        def post(self,*args,**kwargs):
            return HttpResponse("post")
    
        def put(self,*args,**kwargs):
            return HttpResponse("put")
    
        def delete(self,*args,**kwargs):
            return HttpResponse("delete")
    ```

2. #### 第二种方式

    ```python
    from django.views.decorators.csrf import csrf_exempt
    from django.utils.decorators import method_decorator
    
    @method_decorator(csrf_exempt,name="dispatch")
    class StudentsView(View):
        """student view"""
    
        def get(self,*args,**kwargs):
            return HttpResponse("get")
    ```

3. #### 第三种方式

    ```python
    from django.views.decorators.csrf import csrf_exempt
    class MyBaseView(object):
        @csrf_exempt
        def dispatch(self, request, *args, **kwargs):
            print("before")
            ret = super(MyBaseView, self).dispatch(request, *args, **kwargs)
            print("after")
            return ret
    ```

4. #### 第四种，在url中添加

    ```python
    from django.views.decorators.csrf import csrf_exempt
    urlpatterns = [
        path('teachers/', csrf_exempt(TeachersView.as_view()), name="teachers"),
    ]
    ```

5. asd