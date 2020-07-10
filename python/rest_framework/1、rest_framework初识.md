## rest_framework初识

在开发过程中，通常会进行前后端分离设计，这样不仅有助于加快前后端的开发速度，降低前后端代码的耦合度，还有利于提高后端代码的适用性，比如一个API接口可以同时供web端和app端进行使用。首先了解python中API开发，python中的API主要有两种视图的处理：

- **FBV**：Function base view（基于函数的视图）

- **CBV**：Function base view（基于类的视图）

    CBV：基于反射实现，根据请求方式的不同，执行不同的方法：get,post,put,delete等

### 一、restful 规范

随着代码编写，逐渐形成了一种大家公认的，比较合理的接口开发和命名规范，这种规范就被称为restful风格。是建议在开发的过程中遵守restful接口规范，但并不是强制要求。restful接口规范主要有以下10种。

1. ##### 普通接口，用用户管理示例，需要四个接口：

    ```python
    def get_user(request):
    	pass
    def add_user(request):
    	pass
    def put_user(request):
    	pass
    def delete_user(request):
    	pass
    ```

    ##### 使用restful风格，一个接口处理一条数据：

    ```python
    def users(request):
        uses_list = ["柚子", "西瓜"]
        if request.method == "GET":
            pass
        elif request.method == "POST":
            pass
        elif request.method == "PUT":
            pass
        elif request.method == "DELETE":
            pass
        return HttpResponse(json.dumps(uses_list))
    ```

3. ##### url中带上api进行区分

    `www.youzi.com/api/`

4. ##### 版本

    `www.youzi.com/api/v1`

5. ##### 路径

    url尽量使用名词：`www.youzi.com/api/v1/user`

6. ##### method

    - **GET**:从服务器获取资源
    - **POST**:在服务器新建一个资源
    - **PUT**：更新全部资源
    - **PATCH**:更新部分资源
    - **DELETE**：删除资源

7. ##### 过滤

    - `www.youzi.com/api/v1/user/limit=10`
    - `www.youzi.com/api/v1/user/offsey=10`
    - `www.youzi.com/api/v1/user/type=1`

8. ##### 状态码

    | 状态码 | 说明                                                         |
    | ------ | ------------------------------------------------------------ |
    | 200    | 成功                                                         |
    | 201    | 用户新建或修改数据成功(POST/PUT/PATCH)                       |
    | 202    | Accepted，表示异步任务已经进入后台排队                       |
    | 204    | 用户删除数据成功(DELETE)                                     |
    | 301    | 一次重定向                                                   |
    | 302    | 永久重定向                                                   |
    | 401    | 没有权限                                                     |
    | 403    | 有权限，但是访问禁止了，可能是没通过其他的校验，比如django的csrf |
    | 404    | 数据不存在                                                   |
    | 406    | 数据请求格式不正确，比如请求json，但是只有xml                |
    | 410    | 用户请求的数据被永久删除，且不会再得到了                     |
    | 422    | 当创建对象时，发生校验码错误                                 |
    | 500    | 服务器错误                                                   |

9. ##### 错误处理

    ```
    {
    	error:"password error"
    }
    ```

10. ##### 返回结果

    对不同的请求方式返回不同的结果，例如查看列表或者查看详情。

11. ##### 在列表页返回详情页的链接

    ```
    [
    	{
    		"id":"1",
    		"name":"youzi",
    		"url":www.youzi.com/api/v1/fruit/1
    	},
    	{
    		"id":"2",
    		"name":"xigua",
    		"url":www.youzi.com/api/v1/fruit/2
    	},
    ]
    ```

### 二、Django rest_framework框架

RestFramework是一个能快速为我们提供API接口，方便我们编程的框架。依赖于django框架。自带很多实用的功能，并且提供了丰富的扩展性，我们可以通过重写一些方法进行二次开发。

1. #### 普通的基于类的视图

    基于类的视图需要继承django.views中的View类，在**urls**中调用类视图的`as_view()`方法，请求时首先会执行`dispatch()`方法，在`dispatch()`方法中通过`getattr()`进行反向映射找到要请求的方法如：`get`，`post`，`put`，`patch`等。

    ##### 以下是使用类视图的示例：

    - **views代码：**

        ```python
        from django.views import View
        class TeachersView(View):
            """teacher view"""
          	def dispatch(self, request, *args, **kwargs):
                """重写dispatch方法"""
                print("before")
                ret = super().dispatch(request, *args, **kwargs)
                print("after")
                return ret
        
            def get(self,request,*args,**kwargs):
                return HttpResponse("get")
        
            def post(self,request,*args,**kwargs):
                return HttpResponse("post")
        
            def put(self,request,*args,**kwargs):
                return HttpResponse("put")
        
            def delete(self,request,*args,**kwargs):
                return HttpResponse("delete")
            
        ```

    - **urls代码**

        ```python
        from django.urls import path
        from django.views.decorators.csrf import csrf_exempt
        urlpatterns = [
            path('teachers/', csrf_exempt(views.TeachersView.as_view()), name="teachers")
        ]
        ```

        **django.view.View中的部分源码：**

        ```python
        class View:
            """
            Intentionally simple parent class for all views. Only implements
            dispatch-by-method and simple sanity checking.
            """
        
            http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']
        
            def __init__(self, **kwargs):
                """
                Constructor. Called in the URLconf; can contain helpful extra
                keyword arguments, and other things.
                """
                # Go through keyword arguments, and either save their values to our
                # instance, or raise an error.
                for key, value in kwargs.items():
                    setattr(self, key, value)
        
            @classonlymethod
            def as_view(cls, **initkwargs):
                """Main entry point for a request-response process."""
        		
                def view(request, *args, **kwargs):
                    self = cls(**initkwargs)
                    if hasattr(self, 'get') and not hasattr(self, 'head'):
                        self.head = self.get
                    self.request = request
                    self.args = args
                    self.kwargs = kwargs
                    """执行dispatch方法"""
                    return self.dispatch(request, *args, **kwargs)
             ''''''
                def dispatch(self, request, *args, **kwargs):
                # Try to dispatch to the right method; if a method doesn't exist,
                # defer to the error handler. Also defer to the error handler if the
                # request method isn't on the approved list.
                if request.method.lower() in self.http_method_names:
                    handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
                else:
                    handler = self.http_method_not_allowed
                return handler(request, *args, **kwargs)
        ```

        

2. #### rest_framework的使用

    安装：pip install reset_framework

    简单示例：

    ```python
    from rest_framework.views import APIView
    # APIView继承的是django.views中的View，只是对request进行了再封装。
    class TestPage(APIView):
        """试卷"""
        def get(self,request,*args,**kwargs):
            return HttpResponse("get")
    
        def post(self,request,*args,**kwargs):
            return HttpResponse("post")
    
        def put(self,request,*args,**kwargs):
            return HttpResponse("put")
    
        def delete(self,request,*args,**kwargs):
            return HttpResponse("delete")
    ```

    ##### APIView源码讲解：

    APIView继承的也是django.view中的View，只是重写了`dispatch()`方法，对request进行了再封装。主要有四部分的操作

    1. **版本控制**
    2. **验证用户是否登录**
    3. **校验权限**
    4. **访问频率限制**

    在以后逐渐讲解各部分的用法，感兴趣的可以先阅读源码。

    ##### 部分源码：

    ```python
    class APIView(View):
      
    	def dispatch(self, request, *args, **kwargs):
            """
            `.dispatch()` is pretty much the same as Django's regular dispatch,
            but with extra hooks for startup, finalize, and exception handling.
            """
            self.args = args
            self.kwargs = kwargs
            # 对原生的request进行再封装
            # Request(request)
            # 原生的request = request._request
            request = self.initialize_request(request, *args, **kwargs)
            self.request = request
            self.headers = self.default_response_headers  # deprecate?
    
            try:
                # 序列化request
                self.initial(request, *args, **kwargs)
    
                # Get the appropriate handler method
                if request.method.lower() in self.http_method_names:
                    handler = getattr(self, request.method.lower(),
                                      self.http_method_not_allowed)
                else:
                    handler = self.http_method_not_allowed
    
                response = handler(request, *args, **kwargs)
    
            except Exception as exc:
                response = self.handle_exception(exc)
    
            self.response = self.finalize_response(request, response, *args, **kwargs)
            return self.response
        
        ''''''
        # 序列化数据
    	def initial(self, request, *args, **kwargs):
            """
            Runs anything that needs to occur prior to calling the method handler.
            """
            self.format_kwarg = self.get_format_suffix(**kwargs)
    
            # Perform content negotiation and store the accepted info on the request
            neg = self.perform_content_negotiation(request)
            request.accepted_renderer, request.accepted_media_type = neg
    
            # Determine the API version, if versioning is in use.
            # 1、版本处理
            version, scheme = self.determine_version(request, *args, **kwargs)
            request.version, request.versioning_scheme = version, scheme
    
            # Ensure that the incoming request is permitted
    
            # 2、验证是否登录
            self.perform_authentication(request)
    
            # 3、校验权限
            self.check_permissions(request)
            # 4、限制访问频率
            self.check_throttles(request)
        def initialize_request(self, request, *args, **kwargs):
        """
            Returns the initial request object.
            """
            parser_context = self.get_parser_context(request)
    		
            # 对request进行再封装
            return Request(
                request,
                parsers=self.get_parsers(),
                authenticators=self.get_authenticators(),
                negotiator=self.get_content_negotiator(),
                parser_context=parser_context
            )
    ```
    
    

