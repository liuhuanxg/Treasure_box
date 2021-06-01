## 包的使用和Object类介绍

### 一、包的概念

```java
/*
    关于java语言中的包机制
	1、包又称为package，java中引入package这种语法机制主要为了方便程序的管理
	不同功能的类被分门别类放到不同的软件包中，查找比较方便，管理比较方便，易维护
	
	2、怎么定义package？
	 —— 在java源程序的第一行上编写package语句
	 —— package只能编写一个语句
	 —— 语法结构：
	    package 包名;
	3、包名的命名规范
	    公司的域名倒序 + 项目名 + 模块名 + 功能名;
	    采用这种方式的重名几率比较低，因为公司域名具有全球唯一性
	    例如：
		com.bjpowernode.oa.user.service
		org.apach.tomcat.core
	4、包名要求全部小写，包名也是标识符，必须遵守标识符命名规则
	5、一个包将来对应一个目录
	6、使用步骤，以com.bjpowernode.javase.day11为例：
		第一种方式：
			先使用javac Test01.java编译生成javac字节码：指定编码加：-encoding utf-8
			手动创建com/bjpowernode/javase/day11文件夹
			运行：java com.bjpowernode.javase.day11.Test01
		第二种方式：
			javac -d 编译之后存放路径 java源文件的路径
*/
package com.bjpowernode.javase.day11; // 对应四个目录

public class Test01
{
	public static void main(String[] args){
		
	} 
}
```

```java
package com.bjpowernode.javase.day11; // 对应四个目录
class Test02 
{
	public static void main(String[] args) 
	{
		com.bjpowernode.javase.day11.Test01 t = new com.bjpowernode.javase.day11.Test01();
		System.out.println(t); //com.bjpowernode.javase.day11.Test01@15db9742

		// Test01和Test02在同一个包下，默认在当前包下找，可以使用这种方式
		Test01 tt = new Test01();
		System.out.println(tt); //com.bjpowernode.javase.day11.Test01@6d06d69c
	}
}

```

```java
package org.apache;

/*
	import语句用来导入其他类，同一个类下的类不需要导入
	不在同一个包下的类需要导入
	
	import 包名
	import 包名.*

结论：什么时候需要import导入
	不在同一个包中
	不在java.lang包下
*/
import com.bjpowernode.javase.day11.Test01;

class Test04 
{
	public static void main(String[] args) 
	{
		com.bjpowernode.javase.day11.Test01 t = new com.bjpowernode.javase.day11.Test01();
		System.out.println(t); //com.bjpowernode.javase.day11.Test01@15db9742

		Test01 tt = new Test01();
		System.out.println(tt); //com.bjpowernode.javase.day11.Test01@15db9742
		
		// java.lang.*不需要导入，系统自动引入
		// lang：language语言包，是java的核心类，不需要手动导入
		String str = "name";
	}
}

```

### 二、Java中自带的包

1. #### Object类常见方法总结：

    (**1)是类层次结构的根类，所有类都直接或者间接的继承自该类。**

    (**2)构造方法：**

    - 有一个无参构造方法。

    **(3)成员方法：**

    ```java
    public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。
    
    public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
    public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。
    
    protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。
    
    public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。
    
    public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
    
    public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
    
    public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。
    
    public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。
    
    public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
    
    ```




