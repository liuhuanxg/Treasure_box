## 创建Vue实例

每个Vue应用程序都通过使用以下功能创建一个新的Vue实例：

```javascript
<script>	
var vm = new Vue({
// options
})
</script>
```

j经常会使用vm（ViewModel）来引用Vue实例

### 数据与方法

1、创建Vue实例后，它将在其data对象中找到所有属性添加到Vue的反应系统中。当这些属性的值更改时，视图中的数据也会发生改变。

```javascript
<script>    
	var data = { a : 1 };    
	var vm  = new Vue({
        data:data    
    });    
	console.log(vm.a == data.a);    
	vm.a = 2;    
	console.log(vm.a,data.a);     // 修改属性时原来的值会相应修改    
</script>
```

使用Object.freeze()时，可以防止更改现有的属性

html文件：

```html
<div id="example">    
	{{ a }}    
	<button v-on:click="a=2">change it</button>
</div>

```

```javascript
<script>
var obj = {    foo:"bar"};
Object.freeze(obj);   //加上这个属性之后，vue绑定的数据不能做反向修改
new  Vue({    
    el:'#app',    
    data:obj,
});
</script>
```

视图数据更新只针对已经存在的数据，如果添加新的属性时，不会触发视图更新如：

```
vm.b = 'hi'
```

2、Vue实例还有许多其他有用的实例属性和方法。这些以$为前缀，以区别用户定义的属性。

```javascript
var data = {a :1};
var vm  = new  Vue({    
    el:"#example",    			       		
    data:data
});
console.log(vm.$data === data); //true
console.log(vm.$el === document.getElementById('example'));   //true
vm.$watch('a',function (newValue,oldValue) {    
    //当vm.a发生改变时会触发该方法    
    console.log(vm.a,111111111);    
    //打印新、旧数值    
    console.log(newValue,oldValue)
});
```

### 实例生命周期挂钩

每个Vue实例在创建时都会经历一系列初始化步骤-例如，它需要设置数据观察，编译模板，将实例安装到DOM以及在数据更改时更新DOM。在此过程中，它还运行称为**生命周期挂钩的**函数，使用户有机会在特定阶段添加自己的代码。

例如，created钩子可用于在创建实例后运行代码：

```javascript
new Vue({
data: {
	a: 1
},
created: function () {
// this代表vm本身
console.log('a is: ' + this.a)
}
})
// => "a is: 1"
```

还有其他钩子，将在实例的生命周期的不同阶段被调用，如[`mounted`](https://vuejs.org/v2/api/#mounted)，[`updated`](https://vuejs.org/v2/api/#updated)和[`destroyed`](https://vuejs.org/v2/api/#destroyed)。调用的所有生命周期挂钩，其`this`上下文都指向调用它的Vue实例

### 生命周期图

<img src="image/lifecycle.png">