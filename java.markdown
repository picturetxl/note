# Java基础

**笔记注释说明**


##Eclipse使用
command ／ 注释
alt ／ 自动提示
ctrl shift f 规范代码
ctrl shift h 查找类

> 编译运行过程
[![primitive](./photos/complier.png)]()

---

##设计原则
+ 封装变化
+ 多用组合 少用继承
+ 针对接口编程 不针对实现编程
+ 为交互对象之间的松耦合而努力
+ 类应该对扩展开放，对修改关闭---》要扩展类而不要修改类


##细节
> static final int   MIN_VALUE = 0x80000000;//-2147483648
> static final int   MAX_VALUE = 0x7fffffff;//
> 
> 
> a=a+b  和a+=b 的区别

```java
	public static void main(String[] args) {
		byte a = 127;
		byte b = 127;
		a=(byte) (a+b);//必须强制类型转换 否则报错
//		a+=b;//不报错 += java会自动类型转换
		System.out.println(a);//-2
	}

```

> java基本数据类型的字节数
byte     1字节               
short    2字节               
int      4字节               
long     8字节               
char     2字节（C语言中是1字节）可以存储一个汉字
float    4字节               
double   8字节               
boolean  false/true(理论上占用1bit,1/8字节，实际处理按1byte处理)   

> 内部类

```java

public class Circle {
	private double radius;
	public static int count = 0;
	public Circle(double radius) {
		this.radius = radius;
	}
	class Draw{ //成员内部类
		public void drawshape() {
			double radius = 0 ;//只可以是final
			System.out.println("drawshape");
			System.out.println(count); //可以访问静态变量
			System.out.println(radius); //可以访问成员变量
		}
	}
	
	static class Paint{ //静态内部类
		private int color;
		public Paint(int color) {
			this.color = color;
		}
		public void painter(int color) {
			System.out.println(color);
		}
	}
	
	public static void main(String[] args) {
		Circle circle = new Circle(10);
		System.out.println(circle.radius);
		//外部类类名.内部类类名 xxx = 外部类对象名.new 内部类类名()
		Circle.Draw draw = circle.new Draw();//new 成员内部类
		draw.drawshape();
		//外部类类名.内部类类名 xxx = new 外部类类名.内部类类名()
		Circle.Paint paint = new Circle.Paint(10);//new 静态内部类
	}
}

```

> 父类名

```java 

import java.util.Date;

public class Test2 extends Date{
	public void test() {
		//super 是Date类 但是getClass是运行时绑定且是final的
		System.out.println(super.getClass().getName());
	}
	public static void main(String[] args) {
		new Test2().test();//cn.txl.headfirst.ch11.Test2
		//java.util.Date
		System.out.println(new Test2().getClass().getSuperclass().getName());
	}
}


````

> StringBuffer 和 StringBuilder 

```java
	//A thread-safe, mutable sequence of characters.
	StringBuffer buffer = new StringBuffer();
	/**
	 * 线程不安全的可变字符序列 适合单线程
	 */
	StringBuilder builder = new StringBuilder();

```


> splite 和 StringTokenizer

```java
package cn.txl.headfirst.ch11;
import java.util.StringTokenizer;
public class TestForStringTokenizer {
	public static void main(String[] args) {
		String name = "Harry James  Potter   ";
		String[] names = name.split("\\s");//\s表示空格 推荐使用spile()
		/**
		 * Harry:5
		 * James:5
		 * :0
		 * Potter:6
		 */
		for(String s:names){
			System.out.println(s + ":" + s.length());//最后的空格不会加到数组里面的string中去
		}
		
		StringTokenizer strToken=new StringTokenizer(name);
		//当有拆分的子字符串时，输出这个字符串
		/**
		 * Harry
		 * James
		 * Potter
		 */
		while(strToken.hasMoreTokens()){
			System.out.println(strToken.nextToken());
		}
	}
}

```

> return 和 finally
> finally 在return之后执行

```java
package cn.txl.headfirst.ch11;

public class TryReturn {
	public static int test() {
		int x=1;
		try {
			return x;
		}finally { //在return之后执行，但是return已经返回了 所以 s等于1
			x=2;
			System.out.println("test():"+x);//test():2
		}
		
	}
	/**result:
	 * test():2
	 * 1
	 * @param args
	 */
	public static void main(String[] args) {
		int s = new TryReturn().test();
		System.out.println(s);//1
	}
}

```

**语义不同**

* if:如果...就...
* while:只要...就...

**面向对象**

* 某个事物或者东西 ***‘有什么’***，会有什么样的***'动作'***或者说，你会对该事物/东西 ***'做什么'***

**类的成员：实例变量和方法**

**内存分配**
> 栈和堆
[![memory0](./photos/memory0.png)]()

> 方法
[![momery1](./photos/memory1.png)]()

> 局部变量
[![memory2](./photos/memory2.png)]()

> 实例变量：保存在所属的对象中，位于堆上，如果是个对对象的引用，则引用与对象都是在堆上
[![memory](./photos/memory.png)]()





**继承的层次大多数都只有一层或者两层**


**类可以被设置成不被继承**

+  非公有的类只能被同一个包的类作出子类
+  使用final ：表示继承树的末端，不能被继承
+  让类只拥有private的构造程序
+  如果想要防止特定的方法被覆盖，可以将该方法标识上final这个修饰符。将整个类标识成final表示没有任何的方法可以被覆盖。

**覆盖的规则**

+ 参数必须要一样，且返回类型必须要兼容（一样的类型或者该类型的子类）
+ 不能降低方法的存取权限：存取权限必须相同或者更为开放
+ 自己写的类要去覆盖Object类的hashCode() equals() toString()方法

**重载**：参数不同就可以

+ 重载可以有同一方法的多个不同参数版本以方便调用
+ 返回类型可以不同，只要所有的参数不同就行
+ 可以任意设定overload的方法的存取权限

>五个设计步骤
>> 1. 找出具有共同属性和行为的对象：有什么共同点？有什么相关性？  狮子 老虎 狼 狗 猫
>>2. 设计代表共同状态和行为的类  动物 叫 吃
>>3. 决定子类是否需要让某项行为（方法的实现）有特定不同的运作方式  叫的方式和吃的种类不同
>>4. 通过寻找使用共同行为的子类来找出更多抽象化的机会 狼和狗可能有某些共同的行为 狮子 老虎 猫也是
>>5. 完成类的继承层次 犬科 猫科


**多态**

[![pylominal](./photos/pylominal.png)]()

> 三个多态技巧
>>
>>
>>

> 8种让程序更有适应性的方法
>>
>>
>>
>>


**原则**

+ 如果新的类不能对其他的类通过IS-A测试时，就设计不继承其他类的类
+ 只有在需要某类的特殊化版本时，以覆盖或增加新的方法来继承现有的类
+ 当你需要定义一群子类的模板，又不想让程序员初始化此模板时，设计出抽象的类给他们用
+ 如果想要定义出类可以扮演的角色，使用接口
+ DRY:dont't repeat yourself!


**构造函数**

+ 不会被继承
+ 在创建新对象时，所有继承下来的构造函数都会执行
+ 就算是抽象的类也有构造函数，虽然你不能对抽象的类执行new操作，但是抽象的类还是父类，因此它的构造函数会在具体的子类创建出实例时执行
+ 只要你有自己写的构造函数（老兄，我有自己的构造函数不用你管了），编译器都不会帮你写
+ 多个构造函数，参数一定要不一样：编译器看的是参数的类型和顺序，不是参数的名字
+ 构造函数chain
[![constructor_chain](./photos/constructor_chain.png)]()
+调用父类构造函数
[![super](./photos/super.png)]()
+ 有参数的父类构造函数
[![primitive_range](./photos/有参数的父类构造函数.png)]()
+ this():
> 使用this()来从某个构造函数调用同一个类的另一个构造函数
> this()只能在构造函数中，且必须是第一行语句
> this() & super() 不可兼得：每个构造函数可以选择调用super()或者this() 但不能同时调用


* 类所描述的是对象知道什么和执行什么
* 同一类型的每个对象能够有不同的方法行为

> 基础数据类型
[![primitive](./photos/primitive.png)]()

---

> 基础数据类型的范围
[![primitive_range](./photos/primitive_range.png)]()

---

> 引用类型
[![primitive_range](./photos/reference.png)]()

---

> 引用类型说明
[![primitive_range](./photos/reference2.png)]()

---
**变量**

1. 实例变量是声明在类内而不是方法中
2. 局部变量是声明在方法中
3. 局部变量在使用前必须初始化


**==和equals()**

+ ==比较基本数据类型：比较的是字节组合就是二进制
+ ==比较两个引用是否都指向同一对象


**值传递就是拷贝传递**

---

> 引用类型说明
[![development_process](./photos/development_process.png)]()


**布尔运算符**

+ && || :短路操作
+ & |:位操作符 但是java虚拟机一定要计算运算符两边的算式


**import**
> 运用import只是为了帮助程序员省下每个类前面的包名称，不用每次写又臭又长的包限制
> 
> 程序不会因为用了import而变大变慢
> 
> java.lang包是个预先被引用的包。不需要import
> 
> java.lang.String 和 java.lang.System是独一无二的class 
> 
> 
> 
> 
> 


**Java类**

+ **java.util**



> ArrayList -- 集合
> 
> java.util.ArrayList
> 
> 单纯的数组会比较快，但是谁会为了这一点点效能而放弃一堆写好又有威力的功能，保存基本数据类型，也是可以的，用个包装类把基本数据烈性包装起来就可以了。在Java 5以上，包装工作都会自动进行。而且基本数据类型很少用到。
> 
>+ 创建时不用确定大小
> 
>+ 自动调整大小


```java
//创建
ArrayList<Egg> myList = new ArrayList<Egg>();

//加入元素
Egg s = new Egg();
myList.add(s);

//加入特定位置的元素
Egg b = new Egg();
myList.add(3,b);//在3位置上添加b对象引用

//查询大小
int theSize = myList.size();

//查询特定元素
boolean isIn = myList.contains(s);

//查询特定元素的位置
int idx = myList.indexOf(s);

//判断集合是否为空
boolean empty = myList.isEmpty();


//删除元素
myList.remove(s);


```

---


##排序
```java
package cn.txl.practise;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
/**
 * 集合排序自定义对象
 * 实现方式一：实现COmparable接口
 * 然后将对象变成List
 * 再使用Collections.sort()方法
 * @author taolun
 *
 */
public class Student implements Comparable<Student>{
	private String name;
	private int age;
	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Student [name=" + name + ", age=" + age + "]";
	}
	@Override
	public int compareTo(Student o) {
		if(this.age<o.age) {
			return -1;
		}else if(this.age == o.age) {
			return 0;
		}else {
			return 1;
		}
	}
	public static void main(String[] args) {
		Student s1 = new Student("Hon",10);
		Student s2 = new Student("Jhon",20);
		Student s3 = new Student("Smith",50);
		Student s4 = new Student("Chery",30);
		Student s5 = new Student("Hello",90);
		List<Student> studentList = new ArrayList<>(Arrays.asList(s1,s2,s3,s4,s5));
		//第一种方式
		Collections.sort(studentList);
		for(Student s : studentList) {
			System.out.println(s);
		}
	}
	
}

```


```java
package cn.txl.practise;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
/**
 * 集合排序自定义对象
 * 由于实现方式一不够灵活，因为假如要实现升序降序多种排序，则很繁琐
 * 实现方式二：直接用内部类
 * @author taolun
 *
 */
public class Student2 {
	private String name;
	private int age;
	public Student2(String name, int age) {
		this.name = name;
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Student [name=" + name + ", age=" + age + "]";
	}
	
	public static void main(String[] args) {
		Student2 s1 = new Student2("Hon",10);
		Student2 s2 = new Student2("Jhon",20);
		Student2 s3 = new Student2("Smith",50);
		Student2 s4 = new Student2("Chery",30);
		Student2 s5 = new Student2("Hello",90);
		List<Student2> studentList = new ArrayList<>(Arrays.asList(s1,s2,s3,s4,s5));
		Collections.sort(studentList, new Comparator<Student2>() {
			@Override
			public int compare(Student2 o1, Student2 o2) {
				if(o1.getAge()<o2.getAge()) {
					return -1;
				}else if(o1.getAge() == o2.getAge()) {
					return 0;
				}else {
					return 1;
				}
			}
		});
		for(Student2 s : studentList) {
			System.out.println(s);
		}
	}
	
}



```

##转化
```java

package cn.txl.practise;

import java.util.Arrays;
import java.util.List;

public class ChangeAll {
	
	public static void main(String[] args) {
		/**
		 * 如果转化的数组是普通类型的数组 int[]array，那么Arrays.asList(array)之后
		 * 要用List<int[]>去接，也就是说这个list中里面存的是int[]数组对象
		 * 且只能修改，不能再向list中添加或者删除
		 */
		int[] array = {1,2,3};
		List<int[]> list = Arrays.asList(array);//一旦这样，不可以添加删除元素,返回的是固定大小的数组
//		System.out.println(list.size());//1 int[]是一个类型-看作对象
		int[] ints1 = (int[]) list.get(0);
		for(int i : ints1){
            System.out.print(i);
        }
		System.out.println();
		Integer[] arrayInteger = {1,2,3};
		List<Integer> list2 = Arrays.asList(arrayInteger);
//		System.out.println(list2.size());//3
		for(Integer i : list2) {
			System.out.print(i);
		}
		System.out.println();
//		list.set(0,4);//error  
		int[] add= {22,22};
		list.set(0,add);
		int[] ints2 = (int[]) list.get(0);
		for(int i : ints2){
            System.out.print(i);
        }
		System.out.println();
		list2.set(0, 4);//只能更改
		for(Integer i : list2) {
			System.out.print(i);
		}
	}
	
	
}

```

###字符数组转为字符串
```java
char[] s = {'h','e','l','l','o'};
String ss = String.copyValueOf(s);
```

+ **javax** x 表示java的扩展类

>+ javax.swing
>
##常见代码
```java
	//ArrayList 转字符串数组
	return listString.toArray(new String[0]);
```

---
```java
 * 大写65-90
 * 小写97-122
 * 大写转小写
 * 大写-小写=-32
 * 小写=大写+32

```
 
##设计模式
> 单例模式



>工厂模式


>观察者模式

>策略模式


 
  
##Javadoc
+ @author taolun 类的作者
+ The {@code String} class represents character strings. 用代码字体展示它
+ Case mapping is based on the Unicode Standard version specified by the {@link java.lang.Character Character} class.  用指向特定的包，类或者一个引用类的成员名的文档的可见文本标签插入在线链接
+  * @see     java.lang.Object#toString() 

##Java8
+ Lambda(匿名函数)：为了应对变化的需求，如何做到最懒
+ 流:Java8中的Streams的概念使得Collections的许多方面得以推广，让代码更加易读，并允许并行处理流元素
+ 默认方法：可以在接口中使用默认方法，在实现类没有实现方法时提供方法内容---》主要是为了在不改变接口的情况下，扩充接口


行为参数化：把方法传递给方法

函数式编程：把方法传递给方法，同时也能够返回代码并将其包含在数据结构中


##Spring
###DI注入依赖
装配Bean的方式：

+ 自动装配
+ JavaConfig装配
+ xml装配
不用框架的销毁：
[![primitive_range](./photos/不用框架的销毁.png)]()

常见的连接池错误：
[![primitive_range](./photos/常见的错误.png)]()

配连接池的正确示例：
一般将连接的信息写在properties里面
[![primitive_range](./photos/配连接池.png)]()

注解说明

+ @ComponentScan //自动扫描CDPlayerConfig所在包下的组件Component
	+ @ComponentScan(basePackage="包名") 指定基础包
	+ @ComponentScan(basePackage={"包名1"，"包名1"}) 指定多个基础包
	+ @ComponentScan(baseClasses={CDPlayer.class,DVDPlayer.class}) 指定包中含有的类或者接口
+ @Component //声明是一个组件 spring会自动的创建一个bean
+ @Autowired //自动装配:根据类型找 如果找到多个 按照名称再找 默认是第一个字母小写的类名，
	+ @Autowired(required=false) 如果没有匹配的bean，那么在应用上下文创建的时候，Spring就会抛出一个异常，为了避免异常的出现，可以将属性required设置为false，但是需要谨慎对待，因为没有进行代码的null检查的话，就会让这个未装配的bean出现空指针异常
+ @Configuration 在类上声明，表明这个类是一个配置类，该类应该包含在应用上下文中如何创建bean的细节
+ @Bean(name="sgtpeppers") 为方法上声明 创建的bean的名字和方法同名 且是单例
+ @ContextConfiguration(classes = CDPlayerConfig.class) // 为CDPlayerTest加载配置
+ @Import(CDPlayerConfig.class) //引入带有@Configuration的java类。 配置在JavaConfig中的CDPlayer加载到spring容器中
+ @ImportResource(locations= {"cn/txl/springstudy/ch02/mix/config.xml"}) //配置在xml中的BlankDisc加载到spring容器中

##AOP
面向切面：在运行时，动态地将代码切入到类的指定方法，指定位置上的编程思想
##ERROR
+ xml路径
```java
2月 23, 2019 5:39:50 下午 org.springframework.context.support.ClassPathXmlApplicationContext prepareRefresh
信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@21213b92: startup date [Sat Feb 23 17:39:50 CST 2019]; root of context hierarchy
2月 23, 2019 5:39:50 下午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [resources/knight.xml]
Exception in thread "main" org.springframework.beans.factory.BeanDefinitionStoreException: IOException parsing XML document from class path resource [resources/knight.xml]; nested exception is java.io.FileNotFoundException: class path resource [resources/knight.xml] cannot be opened because it does not exist
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:344)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:304)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:181)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:217)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:188)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:252)
	at org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(AbstractXmlApplicationContext.java:127)
	at org.springframework.context.support.AbstractXmlApplicationContext.loadBeanDefinitions(AbstractXmlApplicationContext.java:93)
	at org.springframework.context.support.AbstractRefreshableApplicationContext.refreshBeanFactory(AbstractRefreshableApplicationContext.java:129)
	at org.springframework.context.support.AbstractApplicationContext.obtainFreshBeanFactory(AbstractApplicationContext.java:614)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:515)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:139)
	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:83)
	at cn.txl.spring.ch01.KnightMain.main(KnightMain.java:6)
Caused by: java.io.FileNotFoundException: class path resource [resources/knight.xml] cannot be opened because it does not exist
	at org.springframework.core.io.ClassPathResource.getInputStream(ClassPathResource.java:172)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:330)
	... 13 more
```
+ 解决:classpath 默认是在src下
[![constructor_chain](./photos/classpath.png)]()
```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("cn/txl/springstudy/knight.xml");
```




##Spring MVC

第一个例子
[![constructor_chain](./photos/目录结构.png)]()

+ 项目名：创建maven项目的时候，有个Group id 对应的就是项目名
+ spring的配置文件名：项目名-servlet.xml 且要放在WEB-INF目录下
+ src/main/java:java文件目录

>spring的配置文件

```java

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
         http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.3.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd">
    <!-- 自动扫描包，实现支持注解的IOC -->
    <context:component-scan base-package="com.zhangguo.springmvc01" />
    <!-- Spring MVC不处理静态资源 -->
    <mvc:default-servlet-handler />
    <!-- 支持mvc注解驱动 -->
    <mvc:annotation-driven />
    <!-- 配置url的访问的时候的地址栏输入的信息：去掉前缀和后缀 -->
  <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver"
        id="internalResourceViewResolver">
        <!-- 默认的前缀就是/WEB-INF/view/ -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 默认的后缀名是jsp -->
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```

> web.xml配置文件

```java

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
    id="WebApp_ID" version="3.0">

    <servlet>
        <servlet-name>SpringMVC01</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--  <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param> -->
        <load-on-startup>1</load-on-startup>
    </servlet>
	<!-- 两个servlet-mapping的servlet-name要一样 -->
    <servlet-mapping>
        <servlet-name>SpringMVC01</servlet-name>
        <!-- 拦截了请求 -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app> 
```




