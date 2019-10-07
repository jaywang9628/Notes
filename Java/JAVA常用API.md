- **API使用方法**
	1. 看包路径
	2. 看构造方法
	3. 看方法摘要
- **引用类型的一般使用步骤**
	1. **导包**
	如果使用的目标类和当前类在同一个包下，则可以省略导包语句不写（java.lang包下的内容也不需要导入）
	2. **创建**
	类名 对象名 = new 类名称（）;
	3. **使用**
	对象名.成员方法名（）
# Scanner类(java.util.Scanner)
>键盘输入数据到程序中的类
```java
Scanner in = new Scanner(System.in);
int num = in.nextInt();
String num = in.next();
```
# 匿名对象（Anonymous）

> 没有名字的对象(只能使用唯一的一次就丢掉了)
```java
new Person();
new Person().name = "赵又廷"
new Person().showName();//我叫null
//产生了三个不同的对象
```
>使用建议：如果只需要使用唯一的一次，就可以用匿名对象。
```java
int num = new Scanner(System.in).nextInt()
methodParam(new Scanner(System.in))//使用匿名对象来传参
//匿名对象还可以作为返回值
```

# Random类(java.util.Random)
> 用来生成随机数字
```java
Random r = new Random();
int num = r.nextInt();//获取一个随机的int数字
int num = r.nextInt(3);//获取[0,3)的随机int数字,左闭右开
int num = r.nextInt(3)+1;//获取1~3中的随机数
```

# ArrayList集合（java.util.ArrayList）
变量名字里保存的是地址值
## 对象数组

```java
Person[] array = new Person[2];
Person one = new Person("迪丽热巴",18);
Person two = new Person("马尔扎哈",28);
array[0] = one;//将one中的地址值赋值到数组的0号元素上
array[1] = two;
System.out.println(array[0]);//输出地址值

Person person = array[0]；
System.out.println(person.getName());//迪丽热巴
```
> 对象数组缺点：一旦创建，程序运行期间长度不可改变

## ArrayList集合

>与数组的两个区别
>	- ArrayList长度可变
>	- ArrayList含有toString方法
```java
//<E>代表泛型，统一的类型，而且必须要引用类型
ArrayList<String> list = new ArrayList<>();
System.out.println(list);//输出[]
//直接打印得到的是内容（含有toString方法），内容是空则是空的中括号

list.add("赵丽颖");//add添加元素到尾部，返回值是添加动作是否成功（boolean）
get(index)//返回元素
remove(index)//删除元素，返回改元素
size()//返回集合元素大小,数组是length，不带括号

for(int i =0;i<list.size();i++){
	System.out.println(list.get(i));
}//遍历ArrayList
```
- **ArrayList存储基本数据类型**
使用包装类（都位于java.lang）
	- ArrayList< Byte>
	- ArrayList< Short>
	- ArrayList< Integer>
	- ArrayList< Long>
	- ArrayList< Float>
	- ArrayList< Double>
	- ArrayList< Character>
	- ArrayList< Boolean>
> jdk1.5后支持自动装箱和自动拆箱

# String类（java.lang.String）
>双引号里都是String类的对象，字符串时常量，一旦创建，里面的内容永不改变，所以字符串可以共享使用。
>效果上相当于char[]字符数组，但底层原理是byte[]字节数组。
## 构造方法

- `public String()`
- `public String(char[] array)`
- `public String(byte[] array) `把字节数组转换为字符串
- `String str = "abc"`直接创建

## 字符串常量池

```java
String str1 = "abc";
String str2 = "abc";//str1和str2都在字符串常量池里

char[] charArray = {"a","b","c"};
String str3 = new String(charArray);
str1 == str2;//true
str1 == str3;//False
//对于基本类型，==数值的比较
//对于引用类型，==是地址值的比较
```
双引号创建的在字符串常量池中，字符串常量池在堆中

## 常用方法

```java
String str1 = "abcd";

char[] charArray = {"a","b","c","d"};
String str2 = new String(charArray);

"abcd".equals(str1);//比较内容是否相等，建议常量在前
"ABCD".equalsIgnoreCase(str2);//忽略大小写比较内容

str1.length();//长度，数组.length是没有括号的，要比较
str1.concat(str2);//拼接字符串,等于+
str1.charAt(1);//获取1号位置的char字符
str.indexOf("bc")；//返回字符第一次出现的位置索引，没有则返回-1

str1.substring(1,3);//返回"bc"的副本子字符串,右闭左开
str1.substring(3);//返回"abc"

str1.toCharArray();//返回字符数组
str1.getBytes();//返回字节数组
str1.replace("ab","**");//返回**cd

str1 = "aaa,bbb";
str1.split(",");//根据“,”来分割。aaa,bbb,ccc
//split(regex)方法是一个正则表达式(regular expression),如果按照.切，必须用\\.
```

# static 静态
- 使用static关键字修饰符，不需要创建对象，直接就能通过类名称访问
- 虽然可以通过对象名.来访问，但是推荐使用类名称来访问
	- 类名称.静态变量
	- 类名称.静态方法
- 本类中的静态方法访问可以省略类名称
- ==静态不能直接访问非静态（成员变量，成员方法），非静态可以访问静态==
因为内存中先有静态，后有非静态
- ==静态方法当中不能用this关键字==
因为this代表当前对象
- 内存图
==方法区里有1个静态区==
通过类名称访问静态成员变量的时候，全程和对象没有关系，只和类有关系
# Arrays工具类（java.util.Arrays）
与数组相关的工具类，里面提供了大量的静态方法，用来实现数组的常见操作。
```java
int[] arr1 = {1,2,3}

Arrays.toString(arr1);//转换为字符串
Arrays.sort(arr1);//数值升序排序，字符串按大小升序，自定义的类需要有Comparable或Comparator的接口。
 
```

# Math类(java.util.Math)
数学相关的工具类，提供大量静态方法完成与数学运算相关的操作
```java
public static double abs(double num);//获取绝对值
public static double ceil(double num);//向上取整
public static double floor(double num);//向下取整
public static double round(double num);//四舍五入
```

# Object类（java.lang.Object）
Object所有类的父类(超类)，类层次的根类
## Object类的方法

```java
p1.toString();//返回对象的字符串表示。如果没有重写(@override)，则显示地址值
p1.equals(p2);//指示其他某个对象与此对象是否相等（基本数据类型比较值，引用数据类型比较地址值）
```
## 重写equals方法

比较属性值

```java
//Object obj = p2.多态：弊端：无法使用子类特有的内容（属性和方法），使用向下转型把Object类型转为Person
public boolean equals(Object obj){
	if(obj==this)return true;//提高效率
	if (obj==null)return false;//提高效率
	if(obj instanceof Person){//判断是否同类
		Person p = (Person)obj;
		boolean b =assert this.name.equals(p.name)&&this.age==p.age;//比较两个对象的属性，一个是this(p1)，一个是p（obj=p2）
		return b;
	}
	return false;
}
```
>以上代码可以IDEA可以直接重写(java7的版本)。

## Objects类（区别于Object类）

```java
String s1 = null;
String s2 = "abc";
boolean b2 = Objects.equals(s1,s2);//允许空指针异常
```
# Date类
## Date类(java.util.Date)

表示时间和日期的类
表示特定的瞬间（精确到毫秒）
毫秒值的作用：可以对时间和日期进行计算

- 时间原点：1970-1-1，使用System.currentTimeMillis（）可以获取经历了多少毫秒。
注意：中国东八区，会把时间增加8小时
- 构造方法与成员方法

```java
//构造方法
Date date1 = new Date();//获取当前的时间Sun Aug 08 15:36:27 CST 2088   CHINA STANDAR TIME 
Data date2 = new Date(0L);//时间原点
Data date3 = new Date(123L);
```
```java
//成员方法
date.getTime();//日期转换为毫秒
```
## DateFormat类(java.text.DateFormat)

日期和时间格式化子类的==抽象类==（Format这个基类的子类），可完成String与Date的来回转化

- 格式化（日期→文本）
- 解析（文本→日期）
- 使用子类SimpleDataFormat类来构造模式

```java
//构造方法:SimpleDateFormat(String pattern)
//y年，M月，d日，H小时，m分，s秒
//构造模式
SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy年MM月dd日 HH点mm分ss秒");
//1.格式化
Date data = new Date();
String text = sdf1.format(date);

//2.解析
Date date = sdf1.parser("yyyy-MM-dd HH:mm:ss");//如果字符串和构造方法中的模式不一样，程序就会抛出异常。调用一个抛出异常的方法，就必须处理这个异常，要么throws继续声明抛出这一个异常，要么try...catch自己处理这个异常。
//会输出Sun Aug 08 15:36:27 CST 2088

```
# Calendar类（java.util.Calendar）
日历类，在Date类之后出现，替换了Date类许多方法。
==抽象类==，里面提供了很多操作日历字段的方法（YEAR,MONTH,DAY_OF_MONTH,HOUR）,静态方法getInstance()返回了Calendar类的子类对象
```java
Calendar c = Calender.getInstance();//多态，获取日历类对象

//常用成员方法
c.get(Calendar.YEAR);//获取年
c.get(Calendar.MONTH)+1;//外国是从0到11月

c.set(Calendar.YEAR=2088);//设定年份

c.add(Calendar.YEAR,2);//年份加2
c.getTime();//日历转换为日期
```
# System类（java.lang.System）
提供了大量的静态方法，获取与系统相关的信息和系统级操作
## currentTimeMillis

```java
	//获取一个操作花的时间
	long t1 = System.currentTimeMillis();
	System.out.print("go");
	long t2 = System.currentTimeMillis();
	t = t2 -t1;
```
## arraycopy()

```java
//参数：
//Object src 源数组
//int srcPos 源数组的起始位置
//Object dest 目标数组
//int destPos 目标数组起始位置
//int Length 复制的数组长度
int[] src ={1,2};
int[] dest = {3,4};
System.arraycopy(src,0,dest,0,3);
```
# StringBuilder类（java.lang）
## 原理（字符串缓冲区）

- 字符串缓冲区，可以提高字符串的操作效率（看做一个长度可以变化的字符串）
底层也是一个数组，但是没有被final修饰，可以改变长度
- 在内存中始终是一个数组，占用空间少，效率高
- 初始容量16个字节，如果超出会自动扩容

## 构造方法和append方法

- 构造方法：带参数String类或不带参数
- append方法
可以添加任意类型的字符串形式，并返回StringBuilder对象自身
返回的是this,所以不需接受返回值
	- *链式编程：方法返回的是一个对象，可以根据对象继续调用方法:bu1.append("ga").append(2).append(true)*

## toString()方法和reverse()方法

toString()方法把StringBuilder变为String

> toString()和构造方法能实现两者的互换
reverse()方法可以反转内容	 
# 基本类型包装类（java.lang）
## 概念

基本数据类型使用很方便，但没有对应的方法来操作
所以使用一个类把基本数据类型包装起来，这个类叫包装类
包装类内可以定义一些方法，用来操作基本类型的数据

## 装箱与拆箱

==为了使得java更加彻底的面向对象==
基本类型与对应包装类型之间的转换

- **装箱**：基本类型数据包装到包装类中
	- 构造方法(过时了，会自动)
	Interger(int v)/Interger(String s)
	- 静态方法
	ValueOf(int v)/static Interger ValueOf(String s)
- **拆箱**
	- 成员方法intValue()

>IDEA如果检查方法过时了，就会有一个删除线在词上面。
- **自动装箱与自动拆箱**
```java
Integer in = 1;//自动装箱
in = in +2;//自动拆箱再自动装箱（包装类无法直接参与运算，就会自动转换为基本类型的数据，再参与计算）
//ArrayList里也经常用
```
## 基本类型与字符串之间的转换

- 基本类型转化成字符串
	1. +""
	2. 使用包装类的静态方法toString()
	3. 使用String类中的静态方法valueOf（int i）

## 字符串转化为基本类型

使用parserInt等