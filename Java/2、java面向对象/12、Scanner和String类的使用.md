## Scanner和String的使用

### 一、Scanner的使用

```java
/**
 * Scanner的使用
 * 	1、JDK5以后，帮助实现键盘录入数据
 * 
 * 	2、构造方法：
 * 		public Scanner(InputStream is)
 * 		Scanner sc = new Scanner(System.in);
 * 
 * 	3、成员方法：
 * 		A：hasNextXxx()	判断是否是xxx类型的元素
 * 		B：nextXxx() 	获取xxx类型的元素
 * 
 * 	4、常用的三个方法
 * 		nextInt():	获取一个int类型的数据
 * 		next()/nextLine():	获取一个String类型的数据
 * 
 *  5、注意的小问题：
 *  	int  	--->	int
 *  	String	--->	String
 *  	String	--->	int
 *  	int		--->	String
 *  	
 *  	如何解决：
 * 			A：所有数据都用String接收，将来要什么，就转换为什么
 * 			B：重新创建一个新的Scanner对象
 * */

import java.util.Scanner;
public class ScannerTest001 {
	
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		
		if(sc.hasNextInt()) {
			int number = sc.nextInt();
			System.out.println("number"+number);
		}else {
			String str1 = sc.nextLine();
			System.out.println("str1"+str1);
		}
		
		int x = sc.nextInt();
		
		sc = new Scanner(System.in);
		
		String y = sc.nextLine(); // 把回车换行给了这里
		System.out.println(x);
		System.out.println(y);	
	}
}

```

### 二、String类

1. #### String的构造方法

    ```java
    /**
     * 	1、字符串：由多个字符组成的一串数据
     * 
     * 	2、构造方法有：	
     * 		public String():创建String对象
     * 		public String(byte[] bytes):把字节数组转成字符串
     * 		public String(byte[] bytes,int index,int length):把字节数组的一部分转成字符串
     * 		public String(char[] value):把字符数组转成字符串
     * 		public String(char[] bytes,int index,int length):把字符数组的一部分转成字符串
     * 		public String(byte[] bytes):把字节数组转成字符串
     * 		public String(String original):把字符串转成字符串
     * 
     * 	3、问题：
     * 		1、输出语句输出任何对象名称的时候，默认调用的是该对象的toString()方法。
     * 			toString()方法默认输出的是包名...类名@哈希值的十六进制
     * 			如果输出一个对象名称的时候，发现不是这个格式，说明该类重写了toString()方法
     * 		2、返回字符串的长度
     * 			public int length()
     * 
     * 		面试题：数组有length()吗？String有length()吗？
     * 				没有				有
     * 		
     * 	4、字符串是常量，它的值创建之后不能更改
     * 
     * 	5、String s =  new String("hello");和String s = "hello";的区别
     * 		==：比较的引用类型时比较的是地址
     * 		equals()：默认比较的是地址值。String类重写了equals()方法，该方法的作用是比较字符串的内容是否相等
     * 
     * */
    public class StringTest001 {
    	int number=1;
    	
    	public static void main(String[] args) {
    		// public String()  创建String对象
    		String s1 = new String();
    		System.out.println("s1:"+s1);
    		System.out.println("s1.length():"+s1.length());
    		System.out.println("--------------------------");
    		
    		// public String(byte[] bytes):把字节数组转成字符串。
    		byte[] bytes = { 97, 98, 99, 100, 101 };
    		String s2 = new String(bytes); // 把数值转成对应的字符值
    		System.out.println("s2:" + s2);
    		System.out.println("s2.length():" + s2.length());
    		System.out.println("--------------------------");
    		
    		// public String(byte[] bytes,int index,int length):把字节数组中的一部分转成字符串
    		// String s3 = new String(bytes, 1, 2);
    		String s3 = new String(bytes, 0, bytes.length);
    		System.out.println("s3:" + s3);
    		System.out.println("s3.length():" + s3.length());
    		System.out.println("--------------------------");
    
    		// public String(char[] value):把字符数组转成字符串
    		char[] chs = { 'a', 'b', 'c', 'd', 'e', '林', '青', '霞' };
    		String s4 = new String(chs);
    		System.out.println("s4:" + s4);
    		System.out.println("s4.length():" + s4.length());
    		System.out.println("--------------------------");
    	
    	
    		// public String(char[] value,int index,int count):把字符数组的一部分转成字符串
    		// 需求：我要输出的字符串是:de林青
    		String s5 = new String(chs, 3, 4);
    		System.out.println("s5:" + s5);
    		System.out.println("s5.length():" + s5.length());
    		System.out.println("--------------------------");
    	
    		// public String(String original):把字符串转成字符串
    		String s6 = new String("helloworld");
    		System.out.println("s6:" + s6);
    		System.out.println("s6.length():" + s6.length());
    		System.out.println("--------------------------");
    	
    		// Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。
    		String s7 = "helloworld";
    		System.out.println("s7:" + s7);
    		System.out.println("s7.length():" + s7.length());
    		
    		// 字符串创建之后不能修改，但是可以修改变量的引用
    		String s8 = "hello";
    		s8 += "\tworld";
    		System.out.println("s8:"+s8);
    		}
    		
    
    		// 可以重写toString,输出实例化的对象时，会输出对应的额内容
    		public String toString() {
    			return "111";
    		}
    }
    
    ```

2. #### ==和equals的用法

    **==**：作用主要是判断两个对象的地址是不是相等。即判断两个对象是不是同一个对象（基本数据类型**==**比较的是值，引用数据类型**==**比较的是内存地址）。

    **equals()：**作用也是判断两个对象是否相等。但它一般有两种使用情况。

    - 情况1：类没有覆盖equals()方法。则通过equals()比较该类的两个对象时，等价于使用"=="比较。
    - 情况2：类覆盖了equals()方法。一般，我们覆盖equals()方法来比较两个对象的内容是否相等；若他们的内容相等，则返回true(即认为两个对象相等)。

    ```java
    /**
     * 	==和equals的区别
     * */
    public class StringTest003 {
    	public static void main(String[] args) {
    		// ==和equal的区别
    		String s1 = new String("abc");
    		String s2 = new String("abc");
    		System.out.println(s1==s2);			// false	
    		System.out.println(s1.equals(s2));	// true
    		
    		
    		String s3 = "abc";
    		String s4 = "abc";
    		System.out.println(s3==s4);			// true	
    		System.out.println(s3.equals(s4));	// true
    		
    		String s5 = "abc";
    		String s6 = new String("abc");
    		System.out.println(s5==s6);			// false	
    		System.out.println(s5.equals(s6));	// true
    	}
    }
    
    ```

    **练习：看程序说结果**

    ```java
    /**
     * 	看程序写结果
     * 	字符串变量相加：先开空间，再加内容
     * 	字符串常量相加：先加，再找，没有开辟空间
     * */
    public class StringTest004 {
    
    	public static void main(String[] args) {
    		
    		String s1 = "Hello";	
    		String s2 = "World";
    		String s3 = "HelloWorld";
    		String s4 = s1 + s2;
    		String s5 = "Hello" + "World";
    		System.out.println(s4);
    		System.out.println(s5);
    		
    		System.out.println(s3==s4);				// false
    		System.out.println(s3=="Hello"+"World");// true	
    		System.out.println(s3==s5);				// true	
    		System.out.println(s3.equals(s4));		// true
    		System.out.println(s3.equals(s5));		// true
    	}
    }
    
    ```

3. #### String类的判断功能

    ```java
    /**
     * 	String类的判断功能
     * 	boolean equals(Object obj):比较字符串的内容是否相同，严格区分大小写
     * 	boolean equalsIgnoreCase(String str):比较字符串的内容是否相同，不考虑大小写
     * 	boolean contains(String str):判断是否包含指定的小串，区分大小写
     * 	boolean startsWith(Stirng str):判断是否以指定的字符串开头
     * 	boolean endsWith(String str):判断是否以指定的字符串结尾
     * 	boolean isEmpty():判断字符串的内容是否为空		
     * 
     * */
    
    public class StringDemo {
    	public static void main(String[] args) {
    		String s = "helloworld";
    		System.out.println(s.equals("helloworld")); // true
    		System.out.println(s.equals("Helloworld")); // false
    		
    		System.out.println(s.equalsIgnoreCase("helloworld")); // true
    		System.out.println(s.equalsIgnoreCase("Helloworld")); // true
    		
    		System.out.println(s.contains("world")); 	// true
    		System.out.println(s.contains("Hello")); 	// false
    		
    		System.out.println(s.startsWith("hello"));	// true
    		System.out.println(s.endsWith("wrold"));	// false
    		
    		String s1 = "";
    		System.out.println(s1.isEmpty());	// true
    		System.out.println(s1 == "");		// true
    	}
    }
    
    ```

    案例练习：

    ```java
    /**
     * 	字符串案例练习：			
     * 		用户有三次机会输入用户名和密码，系统给提示剩余机会
     * */
    import java.util.Scanner;
    public class StringTest001 {
    
    	public static void main(String[] args) {
    		String username = "admin";
    		String password = "admin";
    		System.out.println("--------------------------");
    		System.out.println("欢迎来到蜃楼商城");
    		System.out.println("您有三次机会输入账号和密码");
    		System.out.println("机会用完之后需要等账号解锁");
    		System.out.println("祝您好运！");
    		System.out.println("--------------------------");
    		int i = 3;
    		while(i>0) {
    			Scanner sc = new Scanner(System.in);
    			String sc_username = sc.next();
    			System.out.println(sc_username);
    			if (sc_username.equals(username)) {
    				String sc_password = sc.next();
    				if (sc_password.equals(password)) {
    					System.out.println("登录成功");
    					break;
    				}
    			}
    			i-=1;
    			if (i==0) {
    				System.out.println("机会已经用完，请联系管理员");
    			}else {
    			System.out.println("您还有"+i+"次机会");
    			}
    		}
    	}
    	
    }	
    
    ```

4. #### String类的获取功能

    ```java
    /**
     * 	String类的获取功能：
     * 		int length():获取字符串的长度
     * 		char charAt(int index):返回字符串中指定位置的字符
     * 		int indexOf(int ch):返回字符在字符串中第一次出现的位置
     * 		int indexOf(Stirng str):返回指定字符串在字符串中第一次出现的位置
     * 		int indexOf(String str,int fromIndex):返回指定字符串从指定位置开始在字符串中第一次出现的位置
     * 		String substring(int start):返回从指定位置开始到末尾的子串
     * 		String substring(int start,int end):放回从指定位置开始到指定位置结束的子串，注意：前包后不包
     * 
     * */
    
    
    public class StringDemo {
    	
    	public static void main(String[] args) {
    		
    		String str1 = "hello";
    		System.out.println(str1.length());  		// 5
    		System.out.println(str1.charAt(1)); 		// e
    		// System.out.println(str1.charAt(10)); 	// index of range
    		System.out.println(str1.indexOf('h')); 		// 0
    		System.out.println(str1.indexOf("h")); 		// 0
    		System.out.println(str1.indexOf('l',3)); 	// 3
    		System.out.println(str1.substring(0)); 		// hello
    		System.out.println(str1.substring(0,3)); 	// hel
    	}
    }
    
    ```
    
    ##### 练习1：遍历每个小字符串

    ```java
/**
     * 	字符串中的遍历：
     * 		遍历每个子字符串
     * 
     * */
    public class StringTest001 {
    	
    	public static void main(String[] args) {
    		String s1 = "HelloWorld";
    		for(int index=0;index<s1.length();index++) {
    			System.out.println("下标为"+index+"的是"+s1.charAt(index));
    		}
    	}
    }
    
    ```
    
    ##### 练习2：统计字符串个数

    ```java
/**
     * 	用户从键盘输入一串字符串：分别统计：
     * 		大写字符个数
     * 		小写字母个数
     * 		数字个数
     * 
     * */
    
    import java.util.Scanner;
    public class StringTest002 {
    	
    	public static void main(String[] args) {
    		
    		Scanner sc = new Scanner(System.in);
    		String s = sc.next();
    		int bigCount= 0;
    		int smallCount= 0;
    		int intCount= 0;
    		
    		for(int i=0;i<s.length();i++) {
    			char ch = s.charAt(i);
    			if(ch >= 'A' && ch <= 'Z') {
    				bigCount+=1;
    			}else if(ch >= 'a' && ch <= 'z') {
    				smallCount+=1;
    			}else if(ch >= '0' && ch <= '9') {
    				intCount+=1;
    			}
    		}
    		System.out.println("大写字母有:"+bigCount);
    		System.out.println("小写字母有:"+smallCount);
    		System.out.println("数字有:"+intCount);
    	}
    }
    
    ```
    
    

5. #### String类的转换功能

    ```java
    package com.bj.study.test006;
    
    /**
     * 	String的转换功能
     * 		byte[] getBytes():把字符串转换为字节数组
     * 		char[] toCharArray():把字符串转换为字符数组
     * 		static String valueOf(char[] chs):把字符数组转换成字符串
     * 		static String valueOf(int i):把int类型的数据转成字符串
     * 										把任意类型转换为字符串的方法。
     * 		String toLowerCase():把字符串转小写
     * 		String toUpperCase():把字符串转大写
     * 		String concat(String str):字符串的连接
     * 
     * */
    
    
    public class StringDemo {
    
    	public static void main(String[] args) {
    		
    		String s = "abcde";
    
    		// byte[] getBytes():把字符串转换为字节数组
    		byte[] bys = s.getBytes();
    		for (int x = 0; x < bys.length; x++) {
    			System.out.println(bys[x]);
    		}
    		
    		// char[] toCharArray():把字符串转换为字符数组
    		char[] chs = s.toCharArray();
    		System.out.println(chs);
    		for (int x = 0; x < chs.length; x++) {
    			System.out.println(chs[x]);
    		}
    		
    		// static String valueOf(char[] chs):把字符数组转成字符串
    		String s2 = String.valueOf(chs);
    		System.out.println("s2:" + s2);
    		System.out.println("----------------");
    
    		// static String valueOf(int i):把int类型的数据转成字符串
    		int number = 100;
    		String s3 = number + "";
    		String s4 = String.valueOf(number);
    		System.out.println("s3:" + s3);
    		System.out.println("s4:" + s4);
    		System.out.println("----------------");
    
    		// String toLowerCase():把字符串转小写
    		// String toUpperCase():把字符串转大写
    		System.out.println("toLowerCase():" + "HelloWorld11张三".toLowerCase());
    		System.out.println("toUpperCase():" + "HelloWorld11张三".toUpperCase());
    		System.out.println("----------------");
    
    		// String concat(String str):字符串的连接
    		String s5 = "hello";
    		String s6 = "world";
    		String s7 = s5.concat(s6);
    		String s8 = s5 + s6;
    		System.out.println("s7:" + s7);
    		System.out.println("s8:" + s8);
    		
    	}
    }
    
    ```

    ##### 练习：字符串转换

    ```java
    /**
     * 	练习：用户从键盘输入一串字符串：
     * 		将首字母转换成大写，其余转换成小写
     * 
     * */
    import java.util.Scanner;
    public class StringTest001 {
    	
    	public static void main(String[] args) {
    		Scanner sc = new Scanner(System.in);
    		String s = sc.next();
    		String first = s.substring(0,1).toUpperCase();
    		String end = s.substring(1).toLowerCase();
    		
    		String newWords = first+end;
    		System.out.println(newWords);
    	}
    }
    
    ```

6. #### String替换、去除空格、比较

    ```java
    /**
     * 	字符串替换
     * 		String replace(char old,char new)
     * 		String replace(String old,String new)
     * 
     * 	字符串去除空格：
     * 		String trim()：去除两边的空格
     * 	
     *	按字典顺序比较两个字符串：a-z
     * 	 	int compareTo(String str)
     * 		int compareToIgnoreCase(String str) 
     * 
     * */
    public class StringDemo {
    	
    	public static void main(String[] args) {
    		String s = "helloworld";
    
    		String s2 = s.replace('l', 'b');
    		System.out.println("s:" + s);
    		System.out.println("s2:" + s2);
    
    		String s3 = s.replace("owo", "ak47");
    		String s4 = s.replace("j", "h");
    		System.out.println("s3:" + s3);
    		System.out.println("s4:" + s4);
    		
    		String s5 = "    hello     ";
    		System.out.println("s5:" + s5);
    		System.out.println("s5.trim:" + s5.trim());
    		
    		String s6 = "hello";
    
    		System.out.println(s.compareTo("hello")); // 0
    		System.out.println(s.compareTo("Hello")); // 32
    		System.out.println(s.compareTo("mello")); // -5
    		System.out.println(s.compareTo("hgllo"));//第一个不同字母之差  -2
    	}
    }
    
    ```

    ##### 练习1：把数组中的数据按指定格式拼接成字符串

    ```java
    /**
     * 	练习：把数组中的数据按照指定格式拼接成一个字符串
     * 			举例：int[] arr = {1,2,3}
     * 			输出：[1,2,3]
     * 
     * */
    public class StringDemo2 {
    	public static void main(String[] args) {
    		
    		int[] arr = {1,2,3};
    		String s = "[";
    		for(int i=0;i<arr.length;i++) {
    			if(i==arr.length-1) {
    				s += arr[i];
    			}else {
    				s += arr[i]+",";
    			}
    		}
    		s += "]";
    		System.out.println(s);
    	}
    }
    
    ```

    ##### 练习2：反转字符串

    ```java
    /**
     * 	练习：对用户输入的字符串进行反转	
     * 		例：输入abc	
     * 			输出：cba
     * */
    
    import java.util.Scanner;
    public class StringDemo3 {
    	public static void main(String[] args) {
    		
    		Scanner sc = new Scanner(System.in);
    		String s = sc.nextLine();
    		
    		String result = "";
    		for (int i=s.length()-1;i>=0;i--) {
    			result += s.charAt(i);
    		}
    		System.out.println(result);
    	}
    }
    
    ```

    ##### 练习3：查找长字符串中小字符串的出现次数

    ```java
    /**
     * 	练习4：统计长的字符串中某个小字符串出现的次数
     * */
    public class StringDemo4 {
    	public static void main(String[] args) {
    		String maxString = "woaijavawozhenaijavawozhendeaijavawozhendehenaijavaxinbuxinwoaijavagun";
    		String minString = "java";
    
    		int count = getCount(maxString, minString);
    		System.out.println(count);
    		System.out.println("hello".indexOf("z",0));
    		
    	}
    
    	// 写功能实现：
    	// 形式参数:String maxString,String minString;
    	// 返回值类型:int
    	public static int getCount(String maxString, String minString) {
    		// 定义统计变量
    		int count = 0;
    
    		// 先查找一次
    		int index = maxString.indexOf(minString);
    		// 定义一个变量，用于记录每次最新的查找位置
    		int startIndex = 0;
    
    		// 判断位置是不是-1，如果是，就不继续了
    		while (index != -1) {
    			// 统计变量加1
    			count++;
    			// 计算最新的查找位置
    			startIndex = index + minString.length();
    			// 从最新的查找位置，再查一次小串在大串中出现的位置
    			index = maxString.indexOf(minString, startIndex);
    		}
    
    		return count;
    	}
    }
    
    ```

    ##### 练习5：比较两个字符串是否相等

    ```java
    package com.bj.study.test007;
    
    /**
     * 	案例5：自己写一个方法，判断两个字符串是否相等
     * 
     * 	分析：
     * 		A：给出两个字符串
     * 		B:比较长度是否相等，不同返回false
     * 		C:把字符串转换成字符数组
     * 		D：比较数组中每一个位置的值
     * */
    public class StringDemo5 {
    	
    	public static void main(String[] args) {
    		String s1 = "abcdef";
    		String s2 = "abcd";
    		boolean result = checkString(s1,s2);
    		System.out.println(result);
    	}
    
    	private static boolean checkString(String s1, String s2) {
    		if (s1.length() == s2.length()) {
    			for(int i=0;i<s1.length();i++) {
    				if(s1.charAt(i) == s2.charAt(i)) {
    					return true;
    				}
    			}
    			return false;
    		}
    		return false;
    	}
    	
    }
    
    ```

    