## this、static、final和修饰符

### 一、this的使用

#### （1）this使用概述

```java
/**
 * 关于java语言中的this关键字：
 * 	1、this 是一个关键字，翻译为：这个
 * 	2、是一个引用，保存在堆内存中对象的内部，指的是对象自身
 * 	3、创建100个java对象，每一个对象都有一个this，也就是有100个不同的this
 * 	4、this可以出现在实例方法中，this指向当前正在执行这个动作的对象
 * 	5、this多数情况下可以不写
 * 	6、this不能使用在带有static的方法中
 * **/
public class Customer {
	
	String name;   // 实例变量
	public Customer() {
		
	}
	
	public static void main(String[] args) {
		
	}
	
	// 不带有static关键字的方法
	// 比如顾客的购物行为
	// 每一个顾客购物的结果不同
	// 所以购物行为属于对象级别的行为
	// 由于每一个对象在执行购物这个动作的最终结果不同，所以购物这个动作必须有对象参与
	
	// 重点：没有static关键字的方法被称为实例方法
	// 重点：没有static关键字的变量称为实例变量
	// 注意：当一个行为/动作执行的过程当中是需要对象参与的，name这个方法一定要定义为实例方法，不要带static
	
	// 以下方法定义为实例方法，因为每个顾客在真正购物时候，最终的结果都是不同的
	public void shopping() {
		
		// 由于name是一个实例变量，所以这个name访问的时候一定访问的是当前对象的实例变量
		// System.out.println(name+"在购物");
		System.out.println(this.name+"在购物");
	}
	
	
	// static方法不需要对象，直接使用类名调用，所以执行过程中没有对象
	public static void doSome() {
		// 执行过程中没有当前对象，因为带有static的方法是通过类名.方法名访问的
		// 或者说这个上下文中没有当前对象，自然也不存在this
		// doSome方法调用不是对象去调用，是一个类名去调用
		// name是一个实例变量，以下代码没有实例，所以会编译报错
		// System.out.println(name);
	}
	
	public static void doOther() {
		// 想访问时，需要先创建对象
		Customer c = new Customer();
		// 这里访问的name不再是当前调用的实例的name，而是c实例的name
		System.out.println(c.name);
	}
	
}

```

**this内存图分配：**

<img src="image/this%E5%86%85%E5%AD%98%E5%9B%BE.png">

#### （2）this在哪里使用

```java
/*
 * this可以使用在哪里？
 * 	1、可以使用在实例方法当中，代表当前对象
 * 	2、可以使用在构造方法中，通过当前的构造方法调用其他的构造方法
 *重点【记忆】：this()只能放在有效代码的第一行
 * */
public class Date {
	private int year;
	private int month;
	private int day;
	
	// 无参构造器
	/* 需求：当调用时，默认日期为1970,1,1
	 * */
	public Date() {
		
		/*
		this.year = 1970;
		this.month = 1;
		this.day = 1;
		*/
		// 函数可以通过调用下边的构造方法
		// 前提是不能创建新的对象,以下代码代表创建新的对象
		// new Date(1960,1,1);
		
		// 需要采用这种方法进行调用，调用构造方法的另一种方式

		this(1970,1,1);
		
		
	}
	// 有参构造器
	public Date(int year, int month, int day) {
		
		this.year = year;
		this.month = month;
		this.day = day;
	}
	
	public int getYear() {
		return year;
	}
	
	public void setYear(int year) {
		this.year = year;
	}
	public int getMonth() {
		return month;
	}
	public void setMonth(int month) {
		this.month = month;
	}
	public int getDay() {
		return day;
	}
	public void setDay(int day) {
		this.day = day;
	}
	
	public void print() {
		System.out.println(this.year+"年"+this.month+"月"+this.day+"日");
	}
	
}

```

#### （3）案例练习

```java
public class Test {
	public static void main(String[] args) {
			
		// 完整方法和省略方法分别调用method1和method2
		Test.method1();	
		method1();
		Test t = new Test();
		t.method2();
		t.method1();
	}
	
	public static void method1() {
		System.out.println("method1 调用");
		
		// 完整方法调用
		Test.doSome();
		// 省略方法调用
		doSome();
		// 完整方法调用
		Test t = new Test();
		t.doOther();
		// 完整访问i
		System.out.println("完整的i："+t.i);
	}
	
	public void method2() {
		System.out.println("method2 调用");
		Test.doSome();
		doSome();
		
		this.doOther();
		doOther();
		
		System.out.println("完整的i:"+this.i+"缩写的："+i);
	}
	
	int i;
	public static void doSome() {
		System.out.println("do some!");
	}
	
	public void doOther() {
		System.out.println("do Other!");
	}
}

```

### 二、static的使用

#### （1）static用法概述

带有static时候表示静态的，不管是方法还是变量都上升为类级别的，跟具体的对象无关。

```java
/*
 *  什么时候成员变量声明为实例变量呢？
 *  	所有对象都有这个属性，但是这个属性随着对象的变化而变化
 * 
 *  什么时候成员变量声明为静态变量呢？
 *  	所有对象都有这个属性，但是这个属性不随着对象的变化而变化
 *  
 *  静态变量在类加载的时候，存储在方法区内存中
 *  
 *  所有静态的数据，都可以用“类名.”或“引用.”的方式访问
 *  采用“引用.”访问时，及时引用为null，也不会出现空指针异常
 *  
 *  
 *  关于java的static关键字：
 *  1、static的英语为静态的
 *  2、static修饰的方法是静态方法，访问使用"类名."方法名的方式访问
 *  3、static修饰的变量是静态变量
 *  4、所有static修饰的都为静态的，都可以使用“类名.”的方式调用
 *  5、static修饰的元素都是类级别的特征，跟具体的对象无关。
 * */
public class Chinese {
	
	// 身份证号【每一个对象身份证号不同】
	int id;
	
	// 姓名【每一个对象姓名不同】
	String name;
	
	// 国籍【每一个对象由于都是chinese实例化的，所以每一个中国人的国籍都是中国】
	// 无论通过Chinese类实例化多少对象，这些对象国籍都是“中国”
	// 带static代表静态变量，内存在方法区内存中
	static String Country = "中国";
	

	public Chinese(int id, String name) {
		this.id = id;
		this.name = name;
	}
	
	public static void doSome() {
		System.out.println("Chinese doSome");
	}
}

```

```java
public class Chinese2 {
	public static void doSome() {
		System.out.println("Chinese2 doSome");
	}
}
```

```java
public class ChinestTest {

	public static void main(String[] args) {
		
		Chinese zhangsan = new Chinese(1,"zhangsan");
		System.out.println("身份证号："+zhangsan.id+"，姓名："+zhangsan.name+"，国籍："+Chinese.Country);
		
		Chinese lisi = new Chinese(2,"lisi");
		System.out.println("身份证号："+lisi.id+"，姓名："+lisi.name+"，国籍："+Chinese.Country);
		
		Chinese2 wangwu = new Chinese2();
		wangwu = null;
		wangwu.doSome();
	
	}
}

```

#### （2）方法什么时候定义为静态的？

```java
/*
 * 方法什么时候定义为静态的？
 * 	方法描述的是动作，当所有对象执行这个动作的时候，最终产生的影响是一样的，
 * 	那么这个动作已经不再属于一个对象动作了，可以将这个动作提升为类级别的动作，模板级别的动作。
 * 
 * 
 *   静态方法无法直接访问实例变量和实例方法
 * 
 * 大多数方法都定义为实例方法，一般一个行为或者一个动作在发生的时候，都需要对象的参与
 * 	但是也有例外，例如：大多数“工具类”中的方法都是静态的，因为工具类中的方法不需要实例，直接用“类名
 *.”的方式调用
 * 
 * */


public class StaticTest {
	
	// 实例变量
	int i =10;
	
	// 实例方法
	public void doSome() {
		
	}
	
	public static void main(String[] args) {
		System.out.println(MathUtil.sumInt(10, 20));
	}
	
}

```

**工具类示例：**

```java
public class MathUtil {

	public static int sumInt(int a, int b) {
		// TODO Auto-generated method stub
		return a+b;
	}
}

```

#### （3）静态代码快和实例代码块

1. **静态代码块**

    ```java
    /*
     * 可以使用static关键字定义"静态代码块"
     * 1、语法格式
     * 	static{
     * 		java语句	
     * 	}
     * 
     * 2、静态代码快在类加载的时候执行，并且只执行一次
     * 
     * 3、静态代码块在一个类中可以编写多个，并且遵循自上而下的顺序依次执行
     * 
     * 4、静态代码块的作用：
     *  	- 和具体的需求有关，例如项目中在类加载的时刻，或者时机执行代码完成日志的记录。
     *  	- 那么这段记录日志的代码可以放在静态代码块中，完成日志记录
     *  	— 静态代码块是java为程序员准备的一个特殊的时刻，这个特殊的时刻被称为：类加载时刻
     * */
    
    public class StaticTest01 {
    
    	static {
    		System.out.println("static1");
    	}
    	static {
    		System.out.println("static2");
    	}
    	public static void main(String[] args) {
    		
    	}
    }
    
    ```
    
2. **实例代码块**

    ```java
    /*
     * 	实例代码块
     * 		1、实例代码块也可以编写多个，也是遵循自上而下的顺序执行
     * 		2、实例代码块在构造方法执行之前执行，构造方法执行一次，实例代码执行一次
     * 		3、实例代码块也是java语言为程序员准备的一个特殊时刻，这个特殊时刻被称为：对象初始化时机。
     * 
     * */
    	
    public class Test {
    	public Test(){
    		System.out.println("Test类构造函数");
    	}
    	{
    		System.out.println("1");
    	}
    	{
    		System.out.println("2");
    	}
    	public static void main(String[] args) {
    		System.out.println("main begin");
    		Test t = new Test();
    		Test t1 = new Test();
    	}
    }
    
    ```

### 三、final关键字

```java
/**
 * 	关于java语言中的final关键字
 * 		1、final是一个关键字，表示最终的，不可变的
 * 		2、final修饰的类无法被继承
 * 		3、final修饰的方法无法被覆盖
 * 		4、final修饰的变量一旦赋值之后不可重新赋值
 * 		5、final修饰的实例变量：必须手动赋值，不能使用系统的默认值
 * 			final int age = 20;
 * 			
 * 		6、final修饰的引用：
 * 			final User user = new User(30);user=null;  // 报错
 * 			一旦指向某个对象之后，不能再指向其他对象，那么被指向的对象无法被垃圾回收机制回收
 * 			final User user = new User(30);user.id = 50;   // 正确
 * 			虽然指向某个对象之后不能指向其他对象，但是所指向的对象内部的内存是可以修改的
 * 		7、final修饰的实例变量一般都要和static连用，被称为常量：
 * 			常量定义的语法格式是：public static final 类型 常量名 = 值;
 * 			Java语法规范要求所有的常量的名字全部大写，每个单词之间使用下划线链接
 * 			常量示例：public static final String GUO_JI = "中国";	
 * 
 * */
public class FinalTest01 {
	public static void main(String[] args) {
		
		final int  i = 10;
//		  i = 20;  // 编译报错，无法为final修饰的变量赋值
		System.out.println(i);
		
		final int m;
		m = 20;   // 编译通过，不能二次赋值
		System.out.println(m);
	}
}	

```

### 四、访问控制修饰符

```java
package com.bjpower.javase.test005;

/**
 *  访问控制修饰符
 *  1、访问控制修饰符来控制元素的访问范围
 *  2、访问控制修饰符包括：
 *  	public			公开的，在任何位置都可以访问
 *  	protected		同包，子类中可以访问
 *  	缺省(无修饰符)	同包可以访问
 *  	private			只能在本类中访问
 *  3、访问控制修饰符可以修饰类、变量、方法。。。
 *  4、当某个数据只希望子类使用：protected进行修饰
 *  5、修饰符范围：
 *  	private < 缺省 < protected < public
 *  6、类只能定义public或者缺省
 * 
 * */
public class Test01 {
	
	public static void main(String[] args) {
		
	}
}

```

```java
package com.bjpower.javase.test005;

public class User {
	
	// 受保护的
	protected int i = 20;
	
	// 缺省的
	int j = 20;
}
```

