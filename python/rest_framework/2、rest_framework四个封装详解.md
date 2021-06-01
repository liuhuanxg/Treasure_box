## REST framework的四个处理

上篇文章中我们说到rest framework的APIView中对request进行了[再封装以及校验](https://blog.csdn.net/qq_42486675/article/details/106610104)，本篇文章主要从源码的角度分析这些方法，通过重写实现自己的功能模块快速开发。

我们接着看向下的流程。首先先看`APIView`中的关键源码，本篇主要讲解self.initial(request, *args, **kwargs)之后进行的操作：

```python
class APIView(View):
	def dispatch(self, request, *args, **kwargs):
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
	def initial(self, request, *args, **kwargs):
        self.format_kwarg = self.get_format_suffix(**kwargs)
        neg = self.perform_content_negotiation(request)
        request.accepted_renderer, request.accepted_media_type = neg
        # 1、版本处理
        version, scheme = self.determine_version(request, *args, **kwargs)
        request.version, request.versioning_scheme = version, scheme
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

        return Request(
            request,
            parsers=self.get_parsers(),
            authenticators=self.get_authenticators(),
            negotiator=self.get_content_negotiator(),
            parser_context=parser_context
        )
```

### 一、用户登录认证

在网站访问过程中，有些页面比如网站首页一般不需要用户进行登录，有些页面比如个人中心，用户必须登录之后才能访问，如果不使用统一的登录校验，就需要在每个视图中对用户是否登录加以校验，这样会出现很多重复的代码，所以我们可以实现一个用户登录校验装置进行身份校验，在需要的时候调用即可。

在常规方法中我们可以使用`装饰器`等实现，本篇主要介绍关于`rest framework`中自定义的用户登录验证，就是上边源码的`2`部分，通过重写这些登录认证，达到快速开发登录认证功能。

首先新建一个django项目，在model.py文件中建立数据库：

```python
class UserInfo(models.Model):
    user_type_choices = (
        (1, "普通用户"),
        (2, "VIP"),
        (3, "SVIP"),
    )
    username = models.CharField(null=True,max_length=32, verbose_name="用户名",unique=True)
    password = models.CharField(null=True,max_length=32, verbose_name="密码")
    user_type = models.IntegerField(choices=user_type_choices,verbose_name="用户类别")
    def __str__(self): return self.username


class UserToken(models.Model):
    user = models.OneToOneField("UserInfo",on_delete=models.CASCADE)
    token = models.CharField(max_length=32,verbose_name="token")
```

该模型是我们用户登录和权限校验的基础，目前不进行密码加密。建立完数据库之后，首先在数据库中插入几条用户。

1. #### rest framework中登录类介绍

    rest ramework中自定义了用户登录认证类，我们可以通过继承和方法重写这些认证类，实现用户的登录校验。

    - 在上边代码中我们已经知道会执行`perform_authentication()`判断用户是否登录，接着向下看

        ```python
        def perform_authentication(self, request):
             request.user
        ```

    - 在`perform_authentication`中返回了request.user，我们已经知道这个request是封装之后的Request，接着看Request中的源代码，我们可以找到这部分代码：

        ```python
        @property
        def user(self):
           if not hasattr(self, '_user'):
                with wrap_attributeerrors():
                    self._authenticate()
           return self._user
        ```

    - 可以发现，通过执行`self._authenticate()`进行接下来的校验

        ```python
        def _authenticate(self):
            # 循环每一个对象
            for authenticator in self.authenticators:
                try:
                    # 执行认证类的authenticate方法
                    # 1、如果出现异常，执行self._not_authenticated()
                    # 2、如果没报错，接着向下执行
                    # 3、返回None代表没有进行赋值
                    user_auth_tuple = authenticator.authenticate(self)
                    except exceptions.APIException:
                        self._not_authenticated()
                        raise
        
                        if user_auth_tuple is not None:
                            self._authenticator = authenticator
                            # 把元组的数据赋值给request
                            # 第一个元组赋值给user
                            # 第二个赋值给auth
                            self.user, self.auth = user_auth_tuple
                            return
        
                        self._not_authenticated()
        ```

    - 可以看到，通过循环`self.authenticators`，执行每一个的`authenticate`方法，接着我们去找`self.authenticators`的来源，来源于APIView对request进行第一次封装时候：

        ```python
        def initialize_request(self, request, *args, **kwargs):
            """
            Returns the initial request object.
            """
            parser_context = self.get_parser_context(request)
        
            return Request(
                request,
                parsers=self.get_parsers(),
                authenticators=self.get_authenticators(),
                negotiator=self.get_content_negotiator(),
                parser_context=parser_context
            )
        ```

    - 在上边代码中可以看到有`authenticators`，接着找`self.get_authenticators()`

        ```python
        def get_authenticators(self):
            """
                Instantiates and returns the list of authenticators that this view can use.
                """
            return [auth() for auth in self.authentication_classes]
        ```

    - 可以看到最终的来源于一个列表推导式，我们接着看`self.authentication_classes`，在APIView的类变量中可以看到：

        ```python
        authentication_classes = api_settings.DEFAULT_AUTHENTICATION_CLASSES
        ```

        最终的来源是默认的配置文件中，最终源码中所有需要的组件已经全部拿到，这时我们就可以重写这些方法实现自己的登录校验。

2. #### 重写登录认证类

    知道了源代码的组件之后，我们就可以开始复写了。

    1. ##### 先开发一个登录接口，下发token，作为是否登录的判定条件

        ```PYTHON
        from rest_framework.views import APIView
        from  .models import UserInfo,UserToken
        from django.http import JsonResponse
        
        def md5(user):
            import hashlib
            import time
            ctime = str(time.time())
            m = hashlib.md5(bytes(user,encoding="utf-8"))
            m.update(bytes(ctime,encoding="utf-8"))
            return m.hexdigest()
        
        class AuthView(APIView):
        	#此时虽然没有使用登录认证，其实已经在默认使用了rest framework的的登录认证
            def __init__(self,**kwargs):
                super().__init__(**kwargs)
                self.ret = {
                    "code":10000,
                    "msg":None
                }
        
            def post(self, request, *args, **kwargs):
                """用户登录"""
                user = request._request.POST.get("username")
                pwd = request._request.POST.get("password")
                obj = UserInfo.objects.filter(username=user,password=pwd)
                if not obj:
                    self.ret["code"] = 10001
                    self.ret["msg"] = "用户名或者密码有误"
        
                else:
                    # 为登录的用户创建token
                    token = md5(user)
                    print(token)
                    UserToken.objects.update_or_create(
                        user = obj[0],
                        defaults={"token":token}
                    )
                    self.ret["code"] = 10000
                    self.ret["msg"] = "login success"
                    self.ret["token"] = token
                return JsonResponse(self.ret)
        ```

    2. ##### 在登录校验类中进行判断

        重写校验时，必须有这两个方法，详情参见上面源码流程。

        ```python
        class Authtication(object):
            """重写用户登录校验必须有这两个方法"""
            def authenticate(self,request):
                """authenticate"""
                token = request._request.GET.get("token")
                token_obj = UserToken.objects.filter(token=token).first()
                if not token_obj:
                    raise exceptions.AuthenticationFailed("用户认证失败")
                # 在rest_framework内部会将元组内容赋值给request以供后续使用
                # 赋值在其他的类中就可以使用request.user,request.auth进行调用
                return (token_obj.user, token_obj)
            	
            # 认证失败时给浏览器返回的响应头
            def authenticate_header(self,request):
                # print(request._request)
                # return 'Basic realm="%s"'% "api"
                pass
        ```

3. #### 局部使用认证类

    认证类开发完成之后，就可以使用了，此时我们写一个新的接口，查看用户的个人中心，用户必须登录之后才能查看，这种校验方式是局部校验，只在某个类中添加`authentication_classes`属性，该属性必须为列表格式。

    ```python
    class Userinfo(APIView):
        """用户详情"""
        # 加上用户登录校验
     	authentication_classes = [Authtication]
        def get(self,request,*args,**kwargs):
            ret = {"code": 1000, "msg": None, "date": None}
            return JsonResponse(ret)
    
        def post(self,request,*args,**kwargs):
            return HttpResponse("post")
    
        def put(self,request,*args,**kwargs):
            return HttpResponse("put")
    
        def delete(self,request,*args,**kwargs):
            return HttpResponse("delete")
    ```

4. #### 在全局配置认证类

    全局校验时，需要添加在`settings.py`配置中，默认作用于全局，影响范围比较广。

    ```python
    # 配置全局用户认证
    REST_FRAMEWORK = {
        "DEFAULT_AUTHENTICATION_CLASSES":["rest_source.utils.auth.Authtication"],  # 全局应用登录校验
        "UNAUTHENTICATED_USER":None,       # request.user = None
        "UNAUTHENTICATED_TOKEN":None,        # request.auth = None
    }
    ```

5. #### rest framework中自带的认证

    `BaseAuthentication`是所有认证的基类，所以我们以后可以直接继承`BaseAuthentication`。

    ```python
    from rest_framework.authentication import BaseAuthentication
    ```

    对`Authtication`继承`BaseAuthentication`

    ```python
    class Authtication(BaseAuthentication):	
    	pass
    ```

### 二、权限

权限是系统开发不可缺少的部分，有些视图需要权限才能访问，有些不要权限。权限部分流程与登录认证类似，所以我们直接从重写权限认证开始。

1. #### 重写权限类

    ```python
    from rest_framework.permissions import BasePermission
    class MyPermission(BasePermission):
        message = "您无权限查看！"
        def has_permission(self,request,view):
            print("view",view)
            if request.user.user_type != 3:
                return False
            return True
    
    ```

2. #### 局部使用权限

    ```python
    ORDER_DICT = {
        1:{
            "name":"柚子",
            "price":18,
            "number":10,
            "address":"北京",
        },
        2: {
            "name": "苹果",
            "price": 18,
            "number": 10,
            "address": "北京",
        }
    }
    class OrderView(APIView):
        """订单"""
        """只有svip才能看"""
        permission_classes= [MyPermission]
        def get(self,request,*args,**kwargs):
            ret = {"code":1000,"msg":None,"data":None}
            ret["data"] = ORDER_DICT
            return JsonResponse(ret)
    
        def post(self,request,*args,**kwargs):
            return HttpResponse("post")
    
        def put(self,request,*args,**kwargs):
            return HttpResponse("put")
    
        def delete(self,request,*args,**kwargs):
            return HttpResponse("delete")
    
    ```

3. #### 全局配置权限

    全局配置也是一样，需要在settings中添加配置：

    ```python
    # 配置全局用户认证
    REST_FRAMEWORK = {
        "DEFAULT_AUTHENTICATION_CLASSES":["rest_source.utils.auth.Authtication"],  # 全局应用登录校验
        "UNAUTHENTICATED_USER":None,       # request.user = None
        "UNAUTHENTICATED_TOKEN":None,        # request.auth = None
        "DEFAULT_PERMISSION_CLASSES":["rest_source.utils.permission.MyPermission"],# 全局应用权限校验
    }
    ```


### 三、访问频率限制(节流)

访问频率限制一般用于网站反爬中，通过判断用户ip在某个时间段的访问频率，当超过某个频率时就先让用户等待一段时间，防止网站被恶意爬虫爬崩。

1. #### 重写访问限制类

    ```python
    from rest_framework.throttling import BaseThrottle
    import time
    
    # 定义全局变量存放访问频率
    VISIT_CODE = {}
    
    class VisitThrottle(BaseThrottle):
        """60s只能访问3次"""
    
        def __init__(self):
            self.history = None
    
        def allow_request(self, request, view):
            ip = request._request.META.get("REMOTE_ADDR")
            ctime = time.time()
            # 第一次访问，记录数据
            if ip not in VISIT_CODE:
                VISIT_CODE[ip] = [ctime]
                return True
            # 判断第一次访问和最后一次访问的关系
            history = VISIT_CODE[ip]
            self.history = history
            while history and ctime - 60 > history[-1]:
                history.pop()
            if len(history) < 3:
                history.insert(0, ctime)
                return True
            return False
    
        def wait(self):
            """访问拒绝时返回需要等待的时间"""
            ctime = time.time()
            return int(60-(ctime-self.history[-1]))
    
    ```

2. #### 局部使用

    ```python
    class AuthView(APIView):
    
        # 设置局部不需要认证登录
        authentication_classes = []
        # 设置局部不需要鉴权
        permission_classes= []
        # 设置局部访问频率控制
        throttle_classes = [VisitThrottle]
    
        def __init__(self,**kwargs):
            super().__init__(**kwargs)
            self.ret = {
                "code":10000,
                "msg":None
            }
    
        def post(self, request, *args, **kwargs):
            """用户登录"""
            user = request._request.POST.get("username")
            pwd = request._request.POST.get("password")
            obj = UserInfo.objects.filter(username=user,password=pwd)
    
            if not obj:
                self.ret["code"] = 10001
                self.ret["msg"] = "用户名或者密码有误"
    
            else:
                # 为登录的用户创建token
                token = md5(user)
                print(token)
                UserToken.objects.update_or_create(
                    user = obj[0],
                    defaults={"token":token}
                )
                self.ret["code"] = 10000
                self.ret["msg"] = "login success"
                self.ret["token"] = token
            return JsonResponse(self.ret)
    ```

3. #### 全局使用

    全局使用时跟以上两个相同，加入到`settings.py`中：

    ```python
    # 配置全局用户认证
    REST_FRAMEWORK = {
        "DEFAULT_AUTHENTICATION_CLASSES":["rest_source.utils.auth.Authtication"],   # 全局应用登录校验
        "UNAUTHENTICATED_USER":None,       # request.user = None
        "UNAUTHENTICATED_TOKEN":None,        # request.auth = None
        "DEFAULT_PERMISSION_CLASSES":["rest_source.utils.permission.MyPermission"],  # 全局应用权限校验
        "DEFAULT_THROTTLE_CLASSES":["rest_source.utils.visitthrottle.VisitThrottle"] # 全局应用ip限制
    }
    
    ```

4. #### 系统自带的访问限制

    系统源码位置：

    ```python
    from rest_framework.throttling import BaseThrottle,SimpleRateThrottle
    ```

    系统源码：

    ```python
    class SimpleRateThrottle(BaseThrottle):
        """
        A simple cache implementation, that only requires `.get_cache_key()`
        to be overridden.
    
        The rate (requests / seconds) is set by a `rate` attribute on the View
        class.  The attribute is a string of the form 'number_of_requests/period'.
    
        Period should be one of: ('s', 'sec', 'm', 'min', 'h', 'hour', 'd', 'day')
    
        Previous request information used for throttling is stored in the cache.
        """
        cache = default_cache
        timer = time.time
        cache_format = 'throttle_%(scope)s_%(ident)s'
        scope = None
        THROTTLE_RATES = api_settings.DEFAULT_THROTTLE_RATES
    
        def __init__(self):
            if not getattr(self, 'rate', None):
                self.rate = self.get_rate()
            self.num_requests, self.duration = self.parse_rate(self.rate)
    
        def get_cache_key(self, request, view):
            """
            Should return a unique cache-key which can be used for throttling.
            Must be overridden.
    
            May return `None` if the request should not be throttled.
            """
            raise NotImplementedError('.get_cache_key() must be overridden')
    
        def get_rate(self):
            """
            Determine the string representation of the allowed request rate.
            """
            if not getattr(self, 'scope', None):
                msg = ("You must set either `.scope` or `.rate` for '%s' throttle" %
                       self.__class__.__name__)
                raise ImproperlyConfigured(msg)
    
            try:
                return self.THROTTLE_RATES[self.scope]
            except KeyError:
                msg = "No default throttle rate set for '%s' scope" % self.scope
                raise ImproperlyConfigured(msg)
    
        def parse_rate(self, rate):
            """
            Given the request rate string, return a two tuple of:
            <allowed number of requests>, <period of time in seconds>
            """
            if rate is None:
                return (None, None)
            num, period = rate.split('/')
            num_requests = int(num)
            duration = {'s': 1, 'm': 60, 'h': 3600, 'd': 86400}[period[0]]
            return (num_requests, duration)
    
        def allow_request(self, request, view):
            """
            Implement the check to see if the request should be throttled.
    
            On success calls `throttle_success`.
            On failure calls `throttle_failure`.
            """
            if self.rate is None:
                return True
    
            self.key = self.get_cache_key(request, view)
            if self.key is None:
                return True
    
            self.history = self.cache.get(self.key, [])
            self.now = self.timer()
    
            # Drop any requests from the history which have now passed the
            # throttle duration
            while self.history and self.history[-1] <= self.now - self.duration:
                self.history.pop()
            if len(self.history) >= self.num_requests:
                return self.throttle_failure()
            return self.throttle_success()
    
        def throttle_success(self):
            """
            Inserts the current request's timestamp along with the key
            into the cache.
            """
            self.history.insert(0, self.now)
            self.cache.set(self.key, self.history, self.duration)
            return True
    
        def throttle_failure(self):
            """
            Called when a request to the API has failed due to throttling.
            """
            return False
    
        def wait(self):
            """
            Returns the recommended next request time in seconds.
            """
            if self.history:
                remaining_duration = self.duration - (self.now - self.history[-1])
            else:
                remaining_duration = self.duration
    
            available_requests = self.num_requests - len(self.history) + 1
            if available_requests <= 0:
                return None
    
            return remaining_duration / float(available_requests)
    ```

    通过查看源码可以知道关键的属性有`scope`，和关键方法`get_cache_key`，这是rest framework预留给开发人员的参数和方法，我们可以通过重写这两个方法快速实现节流功能。

5. #### 重写系统方法

    ```python
    from rest_framework.throttling import SimpleRateThrottle
    class VisitThrottle1(SimpleRateThrottle):
        """仿写系统自带的节流：未登录用户的访问限制"""
        scope = "Youzi"
    
        def get_cache_key(self, request, view):
            return self.get_ident(request)
    ```

    全局应用配置：

    ```python
    # 配置全局用户认证
    REST_FRAMEWORK = {
        "DEFAULT_AUTHENTICATION_CLASSES":["rest_source.utils.auth.Authtication"],   # 全局应用登录校验
        "UNAUTHENTICATED_USER":None,       # request.user = None
        "UNAUTHENTICATED_TOKEN":None,        # request.auth = None
        "DEFAULT_PERMISSION_CLASSES":["rest_source.utils.permission.MyPermission"],  # 全局应用权限校验
        # "DEFAULT_THROTTLE_CLASSES":["rest_source.utils.visitthrottle.VisitThrottle"] # 全局应用ip限制
        "DEFAULT_THROTTLE_CLASSES":["rest_source.utils.visitthrottle.VisitThrottle1"], # 全局应用ip限制
        "DEFAULT_THROTTLE_RATES":{"youzi":"3/m"}, # 参数固定格式
    }
    ```

6. #### 重写已经登录的用户限制

    ```python
    class UserThrottle1(SimpleRateThrottle):
        """仿写系统自带的节流：已经登录的用户访问限制"""
        scope = "YouziUser"
    
        def get_cache_key(self, request, view):
            return self.get_ident(request)
    ```

    全局应用多个，已经登录的每分钟可以访问10次，没有登陆的使用局部，每分钟可以访问三次：

    ```python
    # 配置全局用户认证
    REST_FRAMEWORK = {
        "DEFAULT_AUTHENTICATION_CLASSES":["rest_source.utils.auth.Authtication"],   # 全局应用登录校验
        "UNAUTHENTICATED_USER":None,       # request.user = None
        "UNAUTHENTICATED_TOKEN":None,        # request.auth = None
        "DEFAULT_PERMISSION_CLASSES":["rest_source.utils.permission.MyPermission"],  # 全局应用权限校验
        # "DEFAULT_THROTTLE_CLASSES":["rest_source.utils.visitthrottle.VisitThrottle"] # 全局应用ip限制
        "DEFAULT_THROTTLE_CLASSES":["rest_source.utils.visitthrottle.UserThrottle1"], # 全局应用ip限制
        "DEFAULT_THROTTLE_RATES":{
            "Youzi":"3/m",
            "YouziUser":"10/m"
        },
    }
    ```

    局部应用每分钟可以访问3次：

    ```python
    class AuthView(APIView):
        # 设置局部不需要认证登录
        authentication_classes = []
        # 设置局部不需要鉴权
        permission_classes = []
    
        throttle_classes = [VisitThrottle1]
    
        def __init__(self, **kwargs):
            super().__init__(**kwargs)
            self.ret = {
                "code": 10000,
                "msg": None
            }
    
        def post(self, request, *args, **kwargs):
            """用户登录"""
           pass
    ```

### 四、版本控制

在代码开发的过程中，往往会有版本的迭代功能，







在代码开发过程中，阅读源码是不可缺少的部分，希望通过本篇文章可以帮助您提高阅读源码的兴趣和能力。最后把一句话送给读者：Either outstanding or out. （要么出众，要么出局）。