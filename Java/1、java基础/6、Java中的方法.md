## Java中的方法

### 一、方法的介绍

方法，把某些需要重复使用的代码段放在一块，可供重复调用。

方法的好处：代码得到了重复使用

方法本质：
	方法就是一段代码片段，并且这段代码片段：
	可以完成某个特定的功能，可以被重复的使用

方法，对应英语单词：Method
方法在C语言中叫做函数/Function

方法定义在类体之中，在一个类当中可以定义多个方法，
方法编写的位置没有先后顺序，可以随意

方法体当中不能再定义方法！！！！

方法体由java语句构成，方法体中的代码遵循自上而下执行

```java

public class MethodTest02
{
	public static void main(String[] args){
		
		//调用函数，计算两个int类型数据的和
		MethodTest02.sumInt(10,20);	
		MethodTest02.sumInt(666,888);	
		MethodTest02.sumInt(1000,2022);	

	}
	
	// 单独定义一个方法
	// 该方法完成计算两个int类型数据的和，并且将结果输出
	public static void sumInt(int a,int b){
		int c = a+b;
		System.out.println(a+"+"+b+"="+c);
	}

}
```

### 二、方法的描述

```java
/*
	关于java语言中的方法：
		
		1、方法怎么定义，语法结构：
			
			[修饰符列表] 返回值类型 方法名(形式参数列表){
				方法体;
			}
		2、对以上的语法结构进行解释
			2.1、关于修饰符列表
				* 可选项，不是必须的
				* 目前统一写成：public static
				* 方法的修饰符列表中有static关键字的话，调用时：
					类名.方法名(实际参数列表);
			2.2、返回值类型
				* 什么是返回值？
					一个方法是可以完成某个特定功能的，这个功能结束之后大多数都是需要返回
					最终执行结果的，执行结果可能是一个具体存在的数据。而这个具体存在的数据就是返回值

				* 返回值类型？
					返回值是一个具体存在的数据，数据都是有类型的，此时需要指定的是返回值的具体类型

				* 返回值类型都可以指定哪些类型呢？
					java任意一种类型都可以，包括基本数据类型和引用数据类型

				*也可能这个方法执行结束之后不返回任何数据，java中规定，当一个方法执行结束之后不返回任何数据的话，返回值类型位置必须编写：void关键字

				* 返回值类型：
					byte、short、int、long、float、double、char、boolean

				* 返回值类型若不是void，表示这个方法执行结束之后必须返回一个具体的数据
					当方法执行结束没有返回任何数据的话，编译器报错。

					return "值";并且要求“值”的数据类型必须和方法的返回值类型一致。
				
				* 返回值类型是void的时候，在方法体内不能编写“return 值;”这样的语句
					可以编写“return;”这样的语句
				
				* 只要带有return关键字的语句执行，return语句所在的方法结束，强行终止当前方法

			2.3、方法名：
				* 只要是合法的标识符就行
				* 方法名最好见名知意
				* 方法名最好是动词
				* 方法名首字母小写，后面每个单词首字母大写
			
			2.4、形式参数列表：简称形参
				* 形参是局部变量：int a;double b;String s;
				* 形参的个数可以是0~N个
				* 多个形参之间用"逗号"隔开
				* 形参中起决定作用的是形参的数据类型，形参的名字就是局部变量的名字
				* 方法调用时，实际给方法传递的真是数据，被称为：实际参数，简称实参
				* 实参列表和形参列表必须满足数量相同，类型对应想同
				例如：
					方法定义
					public static int sum(int a,int b){
					
					}
					方法调用
					sum("abc","a");编译器报错


			2.5、方法体必须由大括号括起来，自上而下执行，
				由java语句构成，每一个java语句以";"结尾。
		3、方法怎么调用？
			方法只有在调用的时候才会执行
			语法规则：<方法的修饰符列表当中有static>
				类名.方法名(实参列表);
*/

/*
public 表示公开的
class表示定义类
MethodTest03是一个类名
*/


public class MethodTest03
{
	// 类体
	// 类体中不能直接编写java语句，除声明变量之外
	// 方法出现在类体当中

	// 方法
	// public表示公开的
	// static表示静态的
	// void表示方法执行不返回任何数据
	// main是方法名：主方法
	// (String[] args)：形式参数列表，其中String[]是一种固定的写法
	// 所以以下只有args这个局部变量名是随意的
	// 主方法固定写法
	public static void main(String[] args){

		//调用sum方法
		MethodTest03.sum(10,20);

	}

	// 自定义方法，不是程序的入口
	// 方法作用：计算两个int类型数据的和，不要求返回结果
	// 修饰符列表：public static
	// 返回值类型：void
	// 方法名：sum
	// 形参列表：(int a,intb)
	// 方法体
	public static void sum(int a,int b){
		int c = a+b;
		System.out.println(c);
	}
    
    // 无参数无返回值的方法(如果方法没有返回值，不能不写，必须写void，表示没有返回值)
    public void f1() {
        System.out.println("无参数无返回值的方法");
    }
    
    /**
    * 有参数无返回值的方法
    * 参数列表由零组到多组“参数类型+形参名”组合而成，多组参数之间以英文逗号（,）隔开，形参类型和形参名之间以英文空格隔开
    */
	public void f2(int a, String b, int c) {
    System.out.println(a + "-->" + b + "-->" + c);
	}
    
    // 有返回值无参数的方法（返回值可以是任意的类型,在函数里面必须有return关键字返回对应的类型）
    public int f3() {
        System.out.println("有返回值无参数的方法");
        return 2;
	}
    
    // 有返回值有参数的方法
    public int f4(int a, int b) {
        return a * b;
    }
    
    // return在无返回值方法的特殊使用
    public void f5(int a) {
        if (a>10) {
        return;//表示结束所在方法 （f5方法）的执行,下方的输出语句不会执行
    }
        System.out.println(a);
    }
}
```

### 三、方法的调用顺序

方法的调用不分顺序，只要在同一个类体中都可以调用，其中main方法是程序的进口，从main方法开始执行。

```java
/*
	方法的调用不一定在main方法中，只要是程序可以执行到的位置，
	都可以调用其他方法
*/


public class MethodTest04
{
	public static void sum(int a,int b){
		int c = a+b;
		System.out.println(c);

		//调用dosome方法
		MethodTest04.dosome();
	}

	public static void main(String[] args){
		
		//调用some方法
		MethodTest04.sum(10,20);
	}
	
	public static void dosome(){
		System.out.println("Hello World!");
	}

}
```

### 四、方法的传参

```java
public class MethodTest05
{
	public static void main(String[] args){
		
		// 传参错误
		// MethodTest05.sum();
		
		// 正确
		MethodTest05.sum(10,20);
		
		// 转换数据类型
		MethodTest05.sum((long)3.0,20);
	}

	public static void sum(long a,long b){
		System.out.println(a+b);
	}
}
```

为什么java中只有值传递？

首先了解程序语言设计中有关将参数传递给方法（或函数）的专业术语：

- **按值调用（call by value）**：表示方法接受的是调用者提供的值
- **引用调用（call by reference）**：表示方法接收的是调用者提供的变量地址。一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。

Java程序语言设计总是采用按值调用。也就是说：方法得到的是所有参数值的一个拷贝，也就是说，方法不能修改传递给它的任何参数变量的内容。

```java
public static void main(String[] args) {
    int num1 = 10;
    int num2 = 20;

    swap(num1, num2);

    System.out.println("num1 = " + num1);// num1=10
    System.out.println("num2 = " + num2);// num2=20
}

public static void swap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;

    System.out.println("a = " + a);  // a=20
    System.out.println("b = " + b);  // b=10
}
```

```java
	public static void main(String[] args) {
		int[] arr = { 1, 2, 3, 4, 5 };
		System.out.println(arr[0]);//1
		change(arr);
		System.out.println(arr[0]); //0
	}

	public static void change(int[] array) {
		// 将数组的第一个元素变为0
		array[0] = 0;
	}
```

```java
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Student s1 = new Student("小张");
		Student s2 = new Student("小李");
		Test.swap(s1, s2);
		System.out.println("s1:" + s1.getName()); // 小张
		System.out.println("s2:" + s2.getName()); // 小李
	}

	public static void swap(Student x, Student y) {
		Student temp = x;
		x = y;
		y = temp;
		System.out.println("x:" + x.getName());  // 小李
		System.out.println("y:" + y.getName());	 // 小张
	}
}
```

下面再总结一下 Java 中方法参数的使用情况：

- 一个方法不能修改一个基本数据类型的参数（即数值型或布尔型）。
- 一个方法可以改变一个对象参数的状态。
- 一个方法不能让对象参数引用一个新的对象。

### 五、方法的调用规则

```java
/*
	方法调用：
		1、方法的修饰符列表当中有static关键字，完整的调用方式是：类名.方法名(实参列表);
		2、但是，有的时候“类名.”可以省略
			* 在当前类中调用时，类名可以不写
			* 在其他类中调用时必须要写
		3、建议一个java源文件只定义一个class

*/

public class MethodTest06
{
	public static void main(String[] args){
		
		//调用
		m();
	}
	public static void m(){
		System.out.println("m method execute!");
		m2();
		A.m2();
	}

	public static void m2(){
		System.out.println("m2 method execute!");
	}
}


class A
 {
	 public static void main(String[] args){

		// 调用其他类中的方法
		MethodTest06.m();		
	 }

	 public static void m2(){
		System.out.println("A's method execute!");
	 }
 }
```

### 六、方法的内存分配

方法调用时候，分配的内存空间在栈内存中，进行压栈操作，当栈中的内存过多时，会报栈溢出错误。

```java
/*
	分析以下程序的输出结果
*/

public class MethodTest07
{	
	/*
		main begin
		m1 begin
		m2 begin
		m3 begin
		m3 over
		m2 over
		m1 over
		main over
		当前main方法结束之后整个程序才结束
	*/
	public static void main(String[] args){
		System.out.println("main begin");
		m1();
		System.out.println("main over");
	}

	public static void m1(){
		System.out.println("m1 begin");
		m2();
		System.out.println("m1 over");
	}

	public static void m2(){
		System.out.println("m2 begin");
		m3();
		System.out.println("m2 over");
	}

	public static void m3(){
		System.out.println("m3 begin");
		System.out.println("m3 over");
	}
}
```

