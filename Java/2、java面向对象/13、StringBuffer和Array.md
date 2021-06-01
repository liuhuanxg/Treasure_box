## StringBuffer、数组排序，Integer、Character类

### 一、StringBuffer/StringBuilder(掌握)

1. #### StringBuffer/StringBuilder区别

    StringBuffer是线程安全的可变字符串

    StringBuilder是县城不安全的可变字符串。

    和StringBuffer的功能一样。就是效率高一些，但是不安全。

2. #### 构造方法

    ```java
    /**
     * 	StringBuffer：线程安全的可变字符串	
     * 	可以改变内容和长度
     * 	
     * 	StringBuffer和String的区别？
     * 		A：StringBuffer的长度可变
     * 		B：String的长度不可变
     * 
     * 	构造方法：
     * 		StringBuffer():构造一个其中不带字符的字符串缓冲区，其初始容量为16个字符
     * 		StringBuffer(int capacity):构造一个不带字符，但具有指定初始容量的字符串缓冲区
     * 		StringBuffer(String str):构造一个字符串缓冲区，并将其内容初始化为自定的字符串长度
     * 
     * 	成员方法：
     * 		public int length():返回长度(字符数)实际值
     * 		public int capacity():返回当前容量理论值
     * 
     * */
    public class StringBufferTest001 {
    	
    	public static void main(String[] args) {
    		
    		// StringBuffer():构造一个不带字符的字符串缓冲区，初始容量为16个字符
    		StringBuffer strb = new StringBuffer();
    		System.out.println("strb:"+strb);
    		System.out.println("strb.length:"+strb.length());  	// 0
    		System.out.println("strb.capacity:"+strb.capacity());// 16
    		
    		// StringBuffer(int capacity):构造一个不带字符，但带有指定初始容量的字符串缓冲区
    		StringBuffer sb2 = new StringBuffer(20);
    		System.out.println("sb2:"+sb2);
    		System.out.println("sb2.length:"+sb2.length());  	// 0
    		System.out.println("sb2.capacity:"+sb2.capacity()); // 20
    		
    		// StringBuffer(String str):构造一个字符串缓冲区，并将其内容初始化为指定的字符串内容。
    		StringBuffer sb3 = new StringBuffer("helloworld");
    		System.out.println("sb3:" + sb3);
    		System.out.println("sb3.length():" + sb3.length());
    		System.out.println("sb3.capacity():" + sb3.capacity());
    	}
    	
    }
    
    ```

    

3. #### 成员方法

    - ##### 添加

        ```java
        /**
         * 	添加功能：
         * 		public StringBuffer append(String str):追加数据，往已有的数据后面添加
         * 		public StringBuffer insert(int offset,String str):往指定位置添加数据
         * 		(超出下标报错)
         * */
        public class StringBufferTest002 {
        	public static void main(String[] args) {
        		// 创建字符串缓冲区对象
        		StringBuffer sb = new StringBuffer();
        		StringBuffer sb2 = sb.append("s");
        		System.out.println(sb2);	 	// s
        		System.out.println(sb2==sb); 	// true
        		System.out.println("sb2:"+sb2);	// s
        		System.out.println("sb:"+sb); 	// s
        		
        		sb.append("hello");
        		sb.append("world");
        		System.out.println(sb2==sb); 	// true
        		System.out.println(sb2);		// shelloworld
        		
        		// 链式编程
        		sb.append("hello").append("world").append("java");
        		System.out.println(sb2==sb); 	// true
        		
        		StringBuffer st2 = new StringBuffer();
        		st2.insert(0, "baLeiTe");  // 超出内容最大下标会报错
        		System.out.println(st2);
        	}
        }
        
        ```

    - ##### 删除

        ```java
        /**
         * 	删除功能：
         * 		public StringBuffer deleteCharAt(int index):删除指定位置的字符
         * 		public StringBuffer delete(int start,int end):删除start到end的数据，前包后不包		
         * 		超出下标报错
         * */
        public class StringBufferTest003 {
        
        	public static void main(String[] args) {
        		StringBuffer sb = new StringBuffer();
        		
        		sb.append("hello").append("world");
        		System.out.println(sb);
        		
        		StringBuffer s = sb.deleteCharAt(0);   		// 超出下标会报错
        		System.out.println(sb);						// elloworld
        		System.out.println(s);						// elloworld
        			
        		sb.delete(0, 1);							// 超出下标报错
        		System.out.println(sb);						// lloworld				
        	}
        }
        
        ```

    - ##### 替换

        ```java
        /**
         * 	字符串替换
         * 		public StringBuffer replace(int start,int end,String str)
         * 		用给定的字符串替换从指定位置开始到指定位置结束的数据
         *		超出下标会报错
         * */
        public class StringBufferTest004 {
        	public static void main(String[] args) {
        		StringBuffer buffer = new StringBuffer("HelloWorld");
        		buffer.replace(0, 1, "hello");
        		System.out.println(buffer);		// helloelloWorld
        	}
        }
        
        ```

    - ##### 反转

        ```java
        /**
         * 	字符串反转
         * 		public StringBuffer reverse()
         * */
        public class StringBufferTest005 {
        	public static void main(String[] args) {
        		StringBuffer buffer = new StringBuffer("helloworld");
        		buffer.reverse(); 
        		System.out.println(buffer); //	dlrowolleh
        	}
        }
        ```

    - ##### 截取

        ```java
        package com.bj.study.StringBuffer;
        
        /**
         * 	截取功能：返回值是String类型，本身没有发生改变
         * 	public String substring(int start)
         * 	从指定位置开始到末尾，超出字符串长度会报错
         * 
         * 	public String substring(int start,int end)
         * 	从指定位置开始到指定位置结束	
         * 
         * */
        
        public class StringBufferTest006 {
        	public static void main(String[] args) {
        		StringBuffer buffer =  new StringBuffer("helloworld");
        		String s = buffer.substring(0);
        		System.out.println(s);		// helloworld
        		
        		String s1 = buffer.substring(0,1);
        		System.out.println(s1);		// h
        	}
        }
        ```

4. #### 练习

    1. ##### StringBuffer和String相互转换

        ```java
        /**	
         * 	StringBuffer 和 String转换
         * */
        
        public class StringBufferTest007 {
        	public static void main(String[] args) {
        		StringBuffer buffer = new StringBuffer("hello");
        		String s = new String(buffer);
        		
        		String s1 = new String("hello");
        		StringBuffer sb = new StringBuffer(s1);
        	}
        }
        
        ```

    2. ##### 数组转换成字符串

        ```java
        /**
         * 	练习：把数组拼接成字符串
         * */
        public class StringBufferTest008 {
        	public static void main(String[] args) {
        		int[] arr = {11,22,33,44,55};
        		
        		String result = arrayToString(arr);
        		System.out.println(result);
        	}
        
        	private static String arrayToString(int[] arr) {
        		
        		StringBuffer sb = new StringBuffer();
        		sb.append("[");
        		for (int i=0;i<arr.length;i++) {
        			if(i==arr.length) {
        				sb.append(arr[i]);
        			}else {
        				sb.append(arr[i]+",");
        			}
        		}
        		sb.append("]");
        		return new String(sb);
        	}
        }
        
        ```

    3. ##### 判断字符串是不是回文字符串

        ```java
        /**
         * 	判断字符串是不是回文字符串：
         * 		如：aba，anmbmna
         * */
        public class StringBufferTest009 {
        	public static void main(String[] args) {
        		String s = "anmbmn";
        		StringBuffer buffer  = new StringBuffer(s);
        		buffer.reverse();
        		String s1 = new String(buffer);
        		if(s.equals(s1)) {
        			System.out.println(true);
        		}else {
        			System.out.println(false);
        		}	
        	}
        }
        
        ```

    4. ##### 看程序写结果

        ```java
        package com.bj.study.StringBuffer;
        
        /**
         *	1、String和StringBuffer和StringBuilder的区别
         *	    A:String是固定长度，StringBuffer和StringBuilder长度可变
         *		B:StringBuffer线程安全，效率低。StringBuilder线程不安全，效率高
         *	
         *	2、StringBuffer和数组的区别
         *		A:StringBuffer的长度可变，可以存储任意数据类型，最终结果都是一个字符串
         *		B:数组长度固定，存储同一种数据类型的元素
         *	
         *	3、看程序写结果
         *		String作为参数传递和StringBuffer作为参数传递的区别
         *		
         *		String是一种引用数据类型，在作为参数传递时，可以当做基本类型来看。因为传递的是常量值
         * 
         * */
        
        public class StringBufferTest010 {
        	public static void main(String[] args) {
        		String s1 = "hello";
        		String s2 = "world";
        		System.out.println(s1 + "---" + s2); // hello---world
        		change(s1, s2);
        		System.out.println(s1 + "---" + s2);// hello---world
        
        		StringBuffer sb1 = new StringBuffer("hello");
        		StringBuffer sb2 = new StringBuffer("world");
        		System.out.println(sb1 + "---" + sb2);// hello---world
        		change(sb1, sb2);
        		System.out.println(sb1 + "---" + sb2);// hellossss---worldoooo
        	}
        
        	private static void change(StringBuffer sb1, StringBuffer sb2) {
        		sb1.append("ssss");
        		sb2.append("oooo");	
        	}
        
        	private static void change(String s1, String s2) {
        		s1 += "ssss";
        		s2 += "oooo";
        	}
        }
        ```

### 二、数组排序

1. #### 冒泡排序

    ```java
    /**
     * 	冒泡排序
     * 		外层循环控制比较次数，每次比较相邻的两个元素，第一次排序完之后最大的值一定在最后
     * */
    public class ArrayBubbleSort {
    	public static void main(String[] args) {
    		int[] arr = {13, 69, 80, 81, 101 };
    		for(int i=1;i<arr.length;i++) {
    			// 设置标志位，如果某一次没有任何数据交换，说明数据有序
    			boolean flag = true;  
    			for(int j=0;j<arr.length-i;j++) {
    				if (arr[j]>arr[j+1]) {
    					int temp = arr[j];
    					arr[j] = arr[j+1];
    					arr[j+1] = temp;
    					flag = false;
    				}
    			}
    			// 测试：当数组本身有序时，一次排序就结束程序
    			System.out.println(111111);
    			if(flag) {
    				break;
    			}
    		}
    		
    		for(int i=0;i<arr.length;i++) {
    			System.out.println(arr[i]);
    		}
    		
    	}
    }
    
    ```

2. #### 选择排序

    ```java
    /**
     * 	选择排序：
     * 		每一次先定义一个初识的最小值下标
     * 		然后在剩余部分选出未排序数组中最小的值，如果最小值的下标与初识定义的不同，就交换位置
     * 		第一次排完序之后最小的一定在最前边
     * 
     * */
    public class ArraySelectSort {
    	
    	public static void main(String[] args) {
    		int[] arr = {24,13,30,69,80,3};
    		
    		for(int i=0;i<arr.length-1;i++) {
    			int index = i;
    			for (int j =i+1;j<arr.length;j++) {
    				if(arr[j]<arr[index]) {
    					index = j;
    				}
    			}
                // 最小值下标与最开始定义的不同
    			if(index!=i) {
    				int temp = arr[index];
    				arr[index] = arr[i];
    				arr[i] = temp;
    			}
    		}
    		for (int k=0;k<arr.length;k++) {
    			System.out.println(arr[k]);
    		}
    		
    	}
    }
    
    ```

3. #### 插入排序

    ```java
    package com.bj.study.Array;
    
    /**
     * 	使用插入排序：
     *  	每次从未排序的数组中拿出第一个元素跟已排序的列表的最后一个比较
     *  	如果位置错误就交换位置。
     * */
    
    public class InsertSort {
    	public static void main(String[] args) {
    		int[] arr = {24,13,30,69,80,3};;
    		for(int i=1;i<arr.length;i++) {
    			for(int j=i;j>0;j--) {
    				if(arr[j]<arr[j-1]) {
    					int s = arr[j];
    					arr[j] = arr[j-1];
    					arr[j-1] = s;
    				}else {
    					break;
    				}
    			}
    		}
    		for(int i=0;i<arr.length;i++) {
    			System.out.printn(arr[i]);
    		}
    		
    	}
    }
    ```

4. #### 顺序查找

    ```java
    package com.bj.study.Array;
    
    /**
     * 	基本查找：数组无序，按循序进行查找
     * 	折半查找：数组有序，每次比较数组中间值
     * 
     * */
    
    public class ArrayFindTest001 {
    	
    	public static void main(String[] args) {
    		/**数组无序，顺序查找*/
    		int[] arr1 = {8,10,20,11,23,40};
    		int number1 = 8;
    		boolean flag = false;
    		for(int i=0;i<arr1.length;i++) {
    			if(number1==arr1[i]) {
    				flag = true;
    				System.out.println(flag);
    				break;
    			}
    		}
    		if(!flag) {
    			System.out.println(flag);
    		}
    	}
    }
    
    ```

5. #### 折半查找

    ```java
    package com.bj.study.Array;
    
    /** 
     * 	折半查找:数组有序
     * */
    public class ArrayFindTest002 {
    	public static void main(String[] args) {
    		int[] arr = {1,2,3,4,5,6,7,8,9,10,11,12,13,14};
    		int number = 4;
    		
    		int start = 0;
    		int end = arr.length-1;
    		while(start<=end) {
    			if(number == arr[(start+end)/2]) {
    				System.out.println((start+end)/2);
    				break;
    			}else if(number < arr[(start+end)/2]) {
    				end = (start+end)/2 - 1;
    			}else {
    				start = (start+end)/2 + 1;
    			}
    		}
    		
    	}
    }
    
    ```

6. #### 数组自带排序和查找

    ```java
    package com.bj.study.Array;
    
    /**
     * 	Arrays：针对数组进行操作的工具类。提供了排序，查找等功能
     * 	
     * 	成员方法：
     * 		public static String toString(int[] a):数组转换成字符串
     * 		public static void sort(int[] a):排序(快速排序)
     * 		public static void binarySearch(int[] a,int key):二分查找
     * 
     * 	注意：	
     * 		如果数组本身是无序的，不能直接使用二分查找
     * */
    import java.util.Arrays;
    public class ArrayTest {
    	public static void main(String[] args) {
    		int[] arr = {20, 30, 10, 29, 89};
    		
    		System.out.println(arr);
    		
    		String s = Arrays.toString(arr);
    		System.out.println(s);	
    		System.out.println(s.length()); // 20
    		Arrays.sort(arr);
    		System.out.println(Arrays.toString(arr));
    		
    		System.out.println(Arrays.binarySearch(arr,10));
    		
    	}
    }
    
    ```

### 三、Integer类

1. #### 进制转换

    ```java
    package com.bj.study.Integer;
    /**
     * 需求1：我给出了一个数据，我要判断这个数据是不是在int范围呢?肿么办呢?
     * 需求2：我给出一个数据100，我要得到它的二进制，八进制，十六进制? 三进制，五进制，七进制???
     * 那么，有没有比较简单的方式让我们来实现这样的需求呢?有。
     * 而基本类型是做不到的，因为基本类型没有功能可以使用。所以，这种的操作最好是能有功能实现。
     * 然后我们调用功能即可。
     * 为了简化我们针对基本类型数据的更复杂的操作，java就针对每种基本类型提供了一个包装类类型，基本类型包装类。
     * byte		Byte
     * short	Short
     * int		Integer
     * long		Long
     * float	Float
     * double	Double
     * char		Character
     * boolean	Boolean
     * */
    
    public class IntegerTest001 {
    	public static void main(String[] args) {
    		// public static final int MAX_VALUE
    		// public static final int MIN_VALUE
    		// if(数据>=Integer.MIN_VALUE && 数据<=Integer.MAX_VALUE){}
    
    		// public static String toBinaryString(int i)
    		System.out.println(Integer.toBinaryString(100));
    		// public static String toOctalString(int i)
    		System.out.println(Integer.toOctalString(100));
    		// public static String toHexString(int i)
    		System.out.println(Integer.toHexString(100));
    	}
    }
    
    ```

2. #### 构造方法

    ```java
    package com.bj.study.Integer;
    
    /**
     * Integer的构造方法：
     * Integer(int value)： 把int类型的值包装成Integer类型
     * Integer(String s): 把数字类型的字符串转换成Integer类型
     */
    
    public class IntegerTest002 {
    	public static void main(String[] args) {
    		// 方式1
    		int number = 100;
    		Integer i = new Integer(number);
    		System.out.println("i:" + i); //  100
    
    		// 方式2
    		String s = "100";
    		// String s = "abc"; // NumberFormatException:因为你给定的数据不是数字形式的字符串数据
    		Integer i2 = new Integer(s);
    		System.out.println("i2:" + i2); // 100
    	}
    }
    
    ```

3. #### int和String类型转换

    ```java
    package com.bj.study.Integer;
    /**
     * int和String类型的相互转换。
     * 
     * int -- String
     * 		String.valueOf(number)
     * 		Integer.toString(number)
     * 
     * String -- int
     * 		Integer.parseInt(s);
     */
    public class IntegerTest003 {
    	public static void main(String[] args) {
    		// int -- String
    		int number = 100;
    		// 方法1
    		String s1 = number + "";
    		//方式2
    		String s2 = String.valueOf(number);
    		//方式3
    		//int -- Integer -- String
    		Integer i = new Integer(number);
    		String s3 = i.toString();
    		//方式4
    		String s4 = Integer.toString(number);
    		System.out.println("--------------");
    
    		String s = "100";
    		//方式1
    		//String -- Integer -- int
    		Integer ii = new Integer(s);
    		//public int intValue()
    		int num = ii.intValue();
    		//方式2
    		//public static int parseInt(String s)
    		int num2 = Integer.parseInt(s);
    	}
    }
    
    ```

4. #### 进制转换

    ```java
    package com.bj.study.Integer;
    
    /**
     * 	常用的基本进制转换
     *		public static String toBinaryString(int i)
     *		public static String toOctalString(int i)
     *		public static String toHexString(int i)
     * 	十进制到其他进制
     *		public static String toString(int i,int radix)
     *	其他进制到十进制
     *		public static int parseInt(String s,int radix)
     * */
    
    public class IntegerTest004 {
    	public static void main(String[] args) {
    		// public static String toString(int i,int radix):
    		// 进制的范围是2-36
    		System.out.println(Integer.toString(100, 2)); 	// 1100100
    		System.out.println(Integer.toString(100, 8));	// 144
    		System.out.println(Integer.toString(100, 16));	// 64
    		System.out.println(Integer.toString(100, 1));	// 100
    		System.out.println(Integer.toString(100, 100));	// 100
    		System.out.println(Integer.toString(100, 50));	// 100
    		System.out.println(Integer.toString(100, 25));	// 40
    		System.out.println(Integer.toString(100, 37));	// 100
    		System.out.println(Integer.toString(100, 32));	// 34
    		System.out.println(Integer.toString(100, 35));	// 2u
    		System.out.println(Integer.toString(100, 36));	// 2s
    		System.out.println(Integer.toString(100, 7));	// 202
    		System.out.println("----------------------");
    
    		// 其他进制到十进制
    		// public static int parseInt(String s,int radix)
    		System.out.println(Integer.parseInt("100", 2));	// 4
    		System.out.println(Integer.parseInt("100", 8));	// 64
    		System.out.println(Integer.parseInt("100", 16));// 256	
    		System.out.println(Integer.parseInt("300", 12));// 432
    	}
    }
    
    ```

5. #### 底层原理

    ```java
    package com.bj.study.Integer;
    
    /**
     * 	JDK5新特性：
     * 		自动装箱：int	-- integer
     * 			底层方法：public static Integer valueOf(int i)
     * 		自动拆箱：Integer -- int
     * 			底层方法：public int intValue()
     * 	
     * 	注意：对象不能为null
     * 	开发原则：
     * 		只要是对象做操作，肯定先判断对象是否为null，如果不为null，才能继续操作
     * 	
     * */
    public class IntegerTest005 {
    	
    	public static void main(String[] args) {
    		// Integer i = new Integer(100);
    		Integer i = 100; // 自动装箱
    		// Integer i = Integer.valueOf(100);
    
    		i += 200; // i = i + 200
    		// i = Integer.valueOf(i.intValue() + 200);
    
    		System.out.println(i);
    	}
    }
    
    ```

6. #### 练习:看程序说结果

    ```java
    package com.bj.study.Integer;
    /**
     * 	看程序写结果
     * */
    public class IntegerTest006 {
    	public static void main(String[] args) {
    		Integer i1 = new Integer(127);
    		Integer i2 = new Integer(127);
    		System.out.println(i1 == i2);// false
    		System.out.println(i1.equals(i2));// true
    
    		Integer i3 = new Integer(128);
    		Integer i4 = new Integer(128);
    		System.out.println(i3 == i4);// false
    		System.out.println(i3.equals(i4));// true
    
    		Integer i5 = 127;
    		Integer i6 = 127;
    		System.out.println(i5 == i6);// true
    		System.out.println(i5.equals(i6));// true
    
    		Integer i7 = 128;
    		Integer i8 = 128;
    		System.out.println(i7 == i8);// false
    		System.out.println(i7.equals(i8));// true
    
    		// 要想知道为什么，就必须看源码。
    		// public static Integer valueOf(int i)
    		//char ch = 127;
    		//Integer i = Integer.valueOf(ch);
    		// 通过查看源码我们知道如果数据在-128到127之间，是从一个缓存数组中返回的。
    		// 如果不在这个范围内，就是重新创建的new出来的对象。
    	}
    }
    
    ```

### 四、Character类

1. #### 构造方法和类型转换

    ```java
    package com.bj.study.Character;
    
    /**
     * 	Character 类在对象中包装一个基本类型char的值；
     * 	此外，该类还提供了几种方法：
     * 		确定字符的类别（小写字母，数字等等），并将字符从大写转换成小写
     * */
    
    public class CharacterDemo {
    	public static void main(String[] args) {
    		// 构造方法
    		Character ch = new Character('a');
    		System.out.println(ch);
    		
    		/** 字符转换*/
    		// public static boolean isUpperCase(char ch)
    		System.out.println(Character.isUpperCase('a'));	// false
    		System.out.println(Character.isUpperCase('A'));	// true
    		System.out.println(Character.isUpperCase('0'));	// false
            
    		// public static boolean isLowerCase(char ch)
    		System.out.println(Character.isLowerCase('a'));	// true
    		System.out.println(Character.isLowerCase('A'));	// false
    		System.out.println(Character.isLowerCase('0'));	// false
            
    		// public static boolean isDigit(char ch)
    		System.out.println(Character.isDigit('a'));		// false
    		System.out.println(Character.isDigit('A'));		// false
    		System.out.println(Character.isDigit('0'));		// true
            
    		// public static char toUpperCase(char ch)
    		System.out.println(Character.toUpperCase('a'));	// A
    		System.out.println(Character.toUpperCase('A'));	// A
            
    		// public static char toLowerCase(char ch)
    		System.out.println(Character.toLowerCase('a'));	// a
    		System.out.println(Character.toLowerCase('A'));	// a
    		
    	}
    }
    
    ```

    