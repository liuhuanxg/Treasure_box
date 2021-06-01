## 初识Vue

### 一、介绍

Vue是用于构建用户界面的渐进框架。是从头设计的，可以逐步采用，并且可以根据不同的用例轻松地在库和框架之间扩展。它包含一个仅着眼于视图层的可访问的核心库，以及一个支持库的生态系统，可帮助解决大型单页应用中的复杂性。

#### 浏览器兼容性：

vue.js支持所有符合ES5的浏览器（不支持IE8及以下版本）。

github链接：https://github.com/vuejs/vue

### 使用

在html文件中引入：

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
第一个vue示例：

```html
<!DOCTYPE html>
<html lang="en">
    <head>    
        <meta charset="UTF-8">    
        <title>Vue-Test</title>    
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    </head>
    <body>
        <div id="app">    
            {{ foo }}
        </div>
        <script>    
              var vm  = new Vue({ 
              	el:'#app',  //绑定id
                data:{
                    foo:'hello World'
                }
              });    
        </script>
    </body>
</html>
```

运行结果：

<img src="image/1574227453301.png">