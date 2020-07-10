## 方法重载和递归方法

### 一、方法重载

方法重载：两个方法的功能虽然不同，但是实现的功能类似，让程序员调用的时候就像调同一个方法。

#### （1）方法重载示例：

```java
/*
	体验方法重载的优点：
		* 程序员调用方法的时候比较方便
	
	前提：功能相似，功能不同时需要编写不同的方法。

*/

public class OverloadTest02
{
	public static void main(String[] args){
		
			// 调用方法
		int result1 = sum(1,2);
		System.out.println(result1);

		// 调用方法
		double result2 = sum(1.0,2.0);
		System.out.println(result2);

		// 调用方法
		long result3 = sum((long)1,(long)2);
		System.out.println(result3);
	}

	// 定义一个方法，计算两个int类型数据的和
	public static int sum(int a, int b){
		return a+b;
	}
	
	// 定义一个方法，计算两个double类型数据的和
	public static double sum(double a, double b){
		return a+b;
	}
	
	// 定义一个方法，计算两个long类型数据的和
	public static long sum(long a, long b){
		return a+b;
	}
}
```

#### （2）深入解析方法重载

```java
/*
方法重载：
	
	1、方法重载又被称为overload

	2、什么时候考虑使用方法重载方法名相同
		* 功能相似的时候，尽可能让方法名相同
		* 功能不同时要让方法名不同

	3、什么条件满足之后构成了方法重载
		* 在同一个类中
		* 方法名相同
		* 参数列表不同：
			- 数量不同
			- 类型不同
			- 顺序不同
		
	4、方法重载和什么有关系，和什么没关系？
		* 方法重载和方法名+参数列表有关系
		* 和返回值的类型没关系
		* 和修饰符列表无关

*/


public class OverloadTest03
{
	public static void main(String[] args){
		
		m1(3);
		m1();
		m1(3,5);
		m1(1.0,3);

	}
	
	public static void m1(int a){} 
	public static void m1(){} 
	public static void m1(int a,int b){}
	public static void m1(double a,int b){} 
	
	// 编译报错
	/*
	public static void x(){}
	public static int x(){}
	*/
}
```

#### （3）方法重载示例

```java
/*
	方法重载的具体应用
*/


public class OverloadTest04
{
	public static void main(String[] args){
		U.p("10");
		U.p(10);
		U.p(20.0);
		U.p('1');
	}
}


class U
{
	public static void p(byte a){
		System.out.println(a);
	}
	public static void p(short a){
		System.out.println(a);
	}
	public static void p(int a){
		System.out.println(a);
	}
	public static void p(long a){
		System.out.println(a);
	}
	public static void p(double a){
		System.out.println(a);
	}
	public static void p(boolean a){
		System.out.println(a);
	}
	public static void p(char a){
		System.out.println(a);
	}
	public static void p(String a){
		System.out.println(a);
	}
}

```

### 二、递归方法

#### （1）递归方法详解

```java
/*
	关于方法的递归调用
		1、什么是递归？
			方法自身调用自身。
			a(){
				a();
			}
		2、递归是很耗费内存的，递归算法可以不用的时候尽量别用

		3、以下程序会造成"栈内存溢出"错误：
			Exception in thread "main" java.lang.StackOverflowError
			栈内存溢出错误
			错误无法挽回，JVM停止工作。
		
		4、递归必须要有结束条件，没有结束条件一定会发生内存溢出错误。

*/

public class RecursionTest01
{
	public static void main(String[] args){

		System.out.println("main begin");
		doSome();
		System.out.println("main over");
	}

	public static void doSome(int num){
		
		// System.out.println("doSome begin");
		doSome();  // 这行代码不结束，下一行程序是不执行的
		System.out.println("doSome over");
	}
}
```

#### （2）递归方法示例

```java
/*
	1、使用递归实现斐波那契数列
	2、使用递归实现1~N的和
	3、计算阶乘
*/

public class RecursionTest02
{
	public static void main(String[] args){
		int resultFeb = Feb(5);
		System.out.println(resultFeb);

		int resultSum = sum(3);
		System.out.println(resultSum);

		int resultCheng = cheng(5);
		System.out.println(resultCheng);
	}
	
	// 1、 Feb那契数列
	public static int Feb(int a){
		if(a ==1 || a==2){
			return 1;
		}
		return Feb(a-1)+Feb(a-2);
	}
	
	// 2、使用递归计算1~N的和
	public static int sum(int n){
		if(n==1){
			return 1;
		}
		return n + sum(n-1);
	}
	
	// 3、计算阶乘
	public static int cheng(int n){
		if (n==1)
		{
			return 1;
		}
		return n*cheng(n-1);
	}

}
```

