# Java异常

## 什么是异常

在程序运行过程中，意外发生的情况，背离我们程序本身意图的表现，都可以理解为异常

## 异常分类

### Throwable:异常根类

#### Error

Error 是程序无法处理的错误，表示运行应用程序中较严重问题

##### 常见错误

1：虚拟机错误VirtualMachineError

2：内存溢出OutOfMemoryError

3：线程死锁ThreadDeath

对于设计合理的应用程序来说，既即使确实发生了错误，本质上也不应该试图去处理它所引起的异常状况

#### Exception

Exception是程序本身可以处理的异常。异常处理通常指针对这种类型异常的处理

##### 分类

1：检查异常Checked Exception -> IO异常、SQL异常

2：非检查异常Unchecked Exception：编译器不要求强制处置的异常 -> 空指针异常、数组下标越界异常、算术异常、类型转换异常

## try-catch-finally

```
public void method(){
    try{
        //代码段1
        //产生异常的代码块2
    }catch(异常类型ex){
        //对异常进行处理的代码段3
    }finally{
        //代码段4
    }
}
```

语法要求：try块后可接零个或多个catch块，如果没有catch块，则必须跟一个finally块

输入代码：

```Scanner input=new Scanner(System.in);
Scanner input=new Scanner(System.in);
int one=input.nextInt();
```

### printStackTrace()

提示出错信息

```
catch(Exception e){
    System.out.println("程序出错啦);
    e.printStackTrace();
}
```

### 算术异常

```
catch(ArithmeticException e){
            e.printStackTrace();
            System.out.println("除数不为0");
        }
```

### 注意

写多重catch时，`catch(Exception e){}`写最后，防止失效

### 终止finally执行语句

在catch块中写上`System.exit(1);`

可以终止程序运行

**finally中不要写return**

### 实际应用中的经验与总结

1、处理运行时异常时，采用逻辑去合理规避同时辅助try-catch处理

2、在多重catch块后面，可以加一个catch(Exception)来处理可能会被遗漏的异常

3、对于不确定的代码，也可以加上try-catch，处理潜在的异常

4、尽量去处理异常，切忌只是简单地调用`printStackTrace()`去打印输出

5、具体如何处理异常，要根据不同的业务需求和异常类型去决定

6、尽量添加finally语句块去释放占用的资源

## throw & throws

可以通过throws声明将要抛出何种类型的异常，通过throw将产生的异常抛出

### throws

如果一个方法可能会出现异常，但没有能力处理这种异常，可以在方法声明处用throws子句来声明抛出异常

```
public void method() throws Exception1,Exception2,..,ExceptionN{
    //可能产生异常的代码
}
```

当方法抛出异常列表中的异常时，方法将不对这些类型及其子类类型的异常作处理，而抛向调用该方法的方法，由他去处理

**当子类重写父类抛出异常的方法时，声明的异常必须是父类方法所声明异常的同类或子类**

### throw

throw用来抛出一个异常

方案1：自己处理

```
public void method(){
    try{
        //代码段1
        throw new 异常类型();
    } catch (异常类型 ex)
    //对异常进行处理的代码段2
}
```

方案2：谁调用谁处理

```
public void method() throws 异常类型{
    //代码段1
    throw new 异常类型();
}
```

调用者自己处理，也可以继续上抛

throws抛出的异常类型可以是父类，但不可以是子类

## 自定义异常类

定义一个类，去继承Throwable类或者它的子类

```
public class HotelAgeException extends Exception{
    public HotelAgeException(){
        super("18岁以下，80岁以上必须由亲友陪同");
    }
}
```

`throw new HotelAgeException();`

## 异常链

有时候我们会捕获一个异常后再抛出另一个异常

顾名思义就是：将异常发生的原因一个传一个串起来，即把底层的异常信息传给上层，这样逐层抛出。

实现方法：

1：构造方法代参

```
catch(HotelAgeException e){
    throw new Exception("我是新产生的异常1",e);
}
```

2：initCause()方法

```
catch(Exception e){
    Exception e1=new Exception("我是新产生的异常2");
    e1.initCause(e);
    throw e1;
}
```

# Java包装类

每一种基本类型都有对应的包装类

## 装箱和拆箱

我们可以通过装箱和拆箱的操作实现包装类和基本数据类型之间的转换

也可以实现基本数据类型和字符串类型之间的转换

# Java字符串

## 创建String对象的方法

### 方式一

`String s1="imooc";       //创建一个字符串对象imooc，名为s1`   

### 方式二

`String s2=new String();           //创建一个空字符串对象，名为s2` 

### 方式三

`String s3=new String("imooc");            //创建一个字符串对象imooc，名为s3`

### String的常用方法

| `int length()`                                  | 返回当前字符串的长度                             |
| :---------------------------------------------- | ------------------------------------------------ |
| `int indexOf(int ch)`                           | 查找ch字符在该字符串中第一次出现的位置           |
| `int indexOf(String str)`                       | 查找str子字符串在该字符串中第一次出现的位置      |
| `int lastIndexOf(int ch)`                       | 查找ch字符在该字符串中最后一次出现的位置         |
| `int lastIndexOf(String st)`                    | 查找str子字符串在该字符串中最后一次出现的位置    |
| `String substring(int beginIndex)`              | 获取从biginIndex位置开始到结束的子字符串         |
| `String substring(int beginIndex,int endIndex)` | 获取从beginIndex位置开始到endIndex位置的子字符串 |
| `String trim()`                                 | 返回去除了前后空格的字符串                       |
| `boolean equals(Object obj)`                    | 将该字符串与指定对象比较，返回true或false        |
| `String toLowerCase()`                          | 将字符串转换为小写`                              |
| `String toUpperCase()`                          | 将字符串转换为大写                               |
| `char charAt(int index)`                        | 获取字符串中指定位置的字符                       |
| `String[] split(String regex,int limit)`        | 将字符串分割为子字符串，返回字符串数组           |
| `byte[] getBytes()`                             | 将该字符串转换为byte数组                         |

### String中==和equals()方法的区别

==是比较两个对象是否相等

equals()是比较两个字符串内容是否相等

### 字符串和二进制之间的转换

```
//定义一个字符串
        String str="JAVA 编程 基础";
//将字符串转换为byte数组，并打印输出
        byte[] character=str.getBytes();
        for (byte str1: character
             ) {
            System.out.println(str1);
        }
//将byte数组转换为字符串
        String str2=new String(character);
        System.out.println(str2);
```

## 字符串StringBuilder

String和StringBuilder的区别：

​              String具有不可变性，而StringBuilder不具备

建议：

​           当频繁操作字符串时，使用StringBuilder

**StringBuilder和StringBuffer**：

​           二者基本相似

​           StringBuffer是线程安全的，StringBuilder则没有，所以性能略高



# Java集合

### 集合概述

Java中的集合是工具类，可以存储任意数量的具有共同属性的对象

**应用场景**：

1：无法预测存储数据的数量

2：同时存储具有一对一关系的数据

3：需要进行数据的增删

4：数据重复问题

### 集合框架的体系结构

#### 1：collection        类的对象

List(序列)                     Queue(队列)                          Set(集)

有序允许重复          有序，允许重复                 无序，不允许重复

ArrayList                    LinkedList                               HashSet            //实现类

#### 2：Map                  键值对

HashMap

### list概述

1：List是元素有序并且可以重复的集合，称为序列

2：List可以精确地控制每个元素的插入位置，或删除某个位置的元素

3：List的两个主要实现类是ArrayList和LinkedList

#### ArrayList

1：ArrayList底层是由数组实现的

2：动态增长，以满足应用程序的需求

3：在列表尾部插入或删除数据非常有效

4：更适合查找和更新元素

5：ArrayList中的元素可以为null

**定义**

`ArrayList <Notice> noticeList=new ArrayList<Notice>();`

**添加**

`noticeList.add(notice1);`

**显示**

```
for(int i=0;i<noticeList.size();i++){
    System.out.println(i+1+":"+noticeList.get(i).getTitle());
}
```

**在某位置上添加**

```
Notice notice4=new Notice(4,"不想工作","某锋",new Date());
noticeList.add(1,notice4);
```

**删除**

```
noticeList.remove(1);
```

**修改**

```
notice4.setTitle("超喜欢工作的");
noticeList.set(1,notice4);
```

### Set

Set是元素无序并且不可以重复的集合，被称为集

HashSet是Set的一个重要实现类，称为哈希集

HashSet中的元素无序并且不可以重复

HashSet中只允许一个null元素

具有良好的存取和查找性能

**导入包**

```
import java.util.HashSet;
import java.util.Set;
import java.util.Iterator;
```

#### Iterator(迭代器)

Iterator接口可以以统一的方式对各种集合元素进行遍历

hasNext()方法检测集合中是否有下一个元素

next()方法返回集合中的下一个元素

#### 添加

`set.add("blue");`

#### 显示

```
Iterator it=set.iterator();
while(it.hasNext()){
            System.out.print(it.next()+" ");
        }
```

#### 避免重复数据

为了避免出现重复数据，需要在定义类中重写`hashCode()`和`equals(Object obj)`方法

```
@Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + month;
        result = prime * result + ((name == null) ? 0 : name.hashCode());
        result = prime * result + ((species == null) ? 0 : species.hashCode());
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        //判断对象是否相等，相等则返回true，不用继续比较属性了
        if (this==obj) {
            return true;
        }
        //判断obj是否是Cat类的对象
        if (obj.getClass()==Cat.class) {
            Cat cat=(Cat)obj;
            return cat.getName().equals(name)&&(cat.getMonth()==month)&&(cat.getSpecies().equals(species));
        }
        return false;
    }
```

#### 查询

```
//在集合中查找花花的信息并输出
        if (set.contains(s1)) {
            System.out.println("花花找到了！");
            System.out.println(s1);
        } else {
            System.out.println("花花没找到！");
        }
//在集合中使用名字查找花花的信息
        boolean flag=false;
        Cat c = null;
        it=set.iterator();
        while (it.hasNext()) {
            c=(Cat)it.next();
            if (c.getName().equals("花花")) {
                flag=true;
                break;
            }
        }
        if (flag) {
            System.out.println("花花找到了");
            System.out.println(c);
        } else {
            System.out.println("花花没找到");
        }
```

### Map

Map中的数据是以键值对（key-value)的形式存储的

key-value以Entry类型的对象实例存在

可以通过key值快速地查找value

一个映射不能包含重复的键

每个键最多只能映射到一个值

#### HashMap

基于哈希表的Map接口的实现

允许使用null值和null键

key值不允许重复

HashMap中的Entry对象是无序排列的

##### 导入包

```
import java.util.HashMap;
import java.util.Map;
```

##### 对象实例化

`Map <String,String> animal=new HashMap<String,String>();`

##### 添加数据

```
int i = 0;
        while (i < 3) {
            System.out.println("请输入key值(单词)：");
            String key=console.next();
            System.out.println("请输入value值(注释)：");
            String value=console.next();
            animal.put(key,value);
            i++;
        }
```

##### 打印输出

```
//打印输出value的值(直接使用迭代器)
        System.out.println("——————————");
        System.out.println("使用迭代器输出所有的value");
        Iterator <String> it=animal.values().iterator();
        while (it.hasNext()) {
            System.out.println(it.next()+" ");
        }
//打印输出key和value的值
//通过entrySet方法
        System.out.println("通过entrySet方法得到key - value:");
        Set<Map.Entry<String,String>> entrySet=animal.entrySet();
        for (Map.Entry<String,String> entry:entrySet
             ) {
            System.out.print(entry.getKey()+"-");
            System.out.println(entry.getValue());
        }
```

##### 查找并输出

```
//通过单词找到注释并输出
        //使用keySet方法
        String strSearch=console.next();
        //1.取得keySet
        System.out.println("请输入要查找的单词");
        Set<String> keySet=animal.keySet();
        //2.遍历keySet
        for (String key : keySet) {
            if (strSearch.equals(key)) {
                System.out.println("找到了！"+"键值对为："+key+"-"+animal.get(key));
                break;
            }
        }
```

# Java线程

**进程的概念**：进程是指可执行程序并存放在计算机存储器的一个指令序列，它是一个动态执行的过程

**线程：**线程是比进程还要小的运行单位，一个进程包含多个线程

## Thread和Runnable接口介绍

Thread是一个线程类，位于java.lang包下

| 构造方法                            | 说明                                                       |
| ----------------------------------- | ---------------------------------------------------------- |
| Thread()                            | 创建一个线程对象                                           |
| Thread(String name)                 | 创建一个具有指定名称的线程对象                             |
| Thread(Runnable target)             | 创建一个基于Runnable接口实现类的线程对象                   |
| Thread(Runnable target,String name) | 创建一个基于Runnable接口实现类，并且具有指定名称的线程对象 |

**Thread类的常用方法**

| 方法                             | 说明                                     |
| -------------------------------- | ---------------------------------------- |
| public void run()                | 线程相关的代码写在该方法中，一般需要重写 |
| public void start()              | 启动线程的方法                           |
| public static void sleep(long m) | 线程休眠m毫秒的方法                      |
| public void join()               | 优先执行调用Join()方法的线程             |

**Runnable接口**

只有一个方法run();

Runnable是Java中用以实现线程的接口

任何实现线程功能的类都必须实现该接口

# Java输入流

**输出流**：

流就是指一连串流动的字符，以先进先出的方式发送信息的通道

文件输入——————读

文件输出——————写

## File类

文件：相关记录或放在一起的数据的集合

在Java中，使用**java.io.File**类对文件进行操作

### 创建File对象

**方式一：**

`File file1 = new File("C:\\Users\\hasee\\Desktop\\score.txt");`

**注意**：前提得先存在score文件；windows系统用两个`\`符号，linux系统用`/`符号

**方式二：**

有时候有用子目录的必要：

`File file1 = new File("C:\\Users\\hasee","Desktop\\score.txt");`

**方式三：**

```
File file = new File("C:\\Users");
File file1 = new File(file,"hasee\\Desktop\\score.txt");
```

### 判断是文件还是目录

```
 //判断是文件还是目录
        System.out.println("是否是目录："+file1.isDirectory());
        System.out.println("是否是文件："+file1.isFile());
```

### 创建目录

**创建一个目录：**

```
File file2 = new File("C:\\Users\\hasee\\Desktop","HashSet");
        if (!file2.exists()) {
            file2.mkdir();
        }
```

**创建多级目录：**

```
File file2 = new File("C:\\Users\\hasee\\Desktop\\HashSet");
        if (!file2.exists()) {
            file2.mkdirs();
        }
```

### 创建文件

```
if (!file1.exists()) {
            try {
                file1.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
```

注意捕获异常

## 字节流

字节输入流InputStream

字节输出流OutputStream

![1537883671200](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1537883671200.png)

![1537883945121](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1537883945121.png)

### FileInputStream

| 方法名                                    | 描述                                                   |
| ----------------------------------------- | ------------------------------------------------------ |
| public int read()                         | 从输入流中读取一个数据字节                             |
| public int read(byte[] b)                 | 从输入流中将最多b.length个字节的数据读入一个byte数组中 |
| public int read(byte[] b,int off,int len) | 从输入流中将最多len个字节的数据读入byte数组中          |
| public void close()                       | 关闭此文件输入流并释放与此流有关的所有系统资源         |

`read()`方法的**实现**：

```
try {
            FileInputStream fis = new FileInputStream("C:\\Users\\hasee\\Desktop\\la.txt");
            int n = 0;
            while ((n = fis.read()) != -1) {
                System.out.print((char) n);
            }
            fis.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
```

```
try {
            FileInputStream fis = new FileInputStream("C:\\Users\\hasee\\Desktop\\la.txt");
            byte[] b = new byte[100];
            //从头开始读取五个数
            fis.read(b,0,5);
            System.out.println(new String(b));
            fis.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
```

### FileOutputStream

`write()`方法的**实现**：

```
FileOutputStream fos;
        FileInputStream fis;
        try {
            //true:在文件后面继续写内容
            fos = new FileOutputStream("C:\\Users\\hasee\\Desktop\\la.txt",true);
            fis = new FileInputStream("C:\\Users\\hasee\\Desktop\\la.txt");
            fos.write(50);
            fos.write('a');
            System.out.println(fis.read());
            System.out.println((char)fis.read());
            fos.close();
            fis.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
```

### 文件的拷贝

```
FileInputStream fis = new FileInputStream("C:\\Users\\hasee\\Desktop\\7.png");
            FileOutputStream fos = new FileOutputStream("C:\\Users\\hasee\\Desktop\\7copy.png");
            int n = 0;
            byte[] b = new byte[1024];
            while ((n = fis.read(b)) != -1) {
                fos.write(b,0,n);
            }
            fis.close();
            fos.close();
```

## 缓冲流概述

缓冲输入流 BufferedInputStream

缓冲输出流 BufferedOutputStream

缓冲区不满的情况下，会调取`flush()`方法，进行强制清空，缓冲区满了，自动执行写操作

`close()`方法必须写，也是强制清空缓冲区数据进行输出

## 字符流概述

字符输入流Reader

字符输出流Writer

### 字节字符转换流

InputStreamReader

OutputStreamWriter

```
try {
            FileInputStream fis = new FileInputStream("C:\\Users\\hasee\\Desktop\\la.txt");
            InputStreamReader isr = new InputStreamReader(fis,"GBK");
            FileOutputStream fos = new FileOutputStream("C:\\Users\\hasee\\Desktop\\la1.txt");
            OutputStreamWriter osw = new OutputStreamWriter(fos,"GBK");
            int n = 0;
            char[] cbuf = new char[10];
            while ((n = isr.read(cbuf)) != -1) {
                osw.write(cbuf,0,n);
                osw.flush();
            }
```

## 对象序列化

**序列化：**把Java对象转换为字节序列的过程

**反序列化：**把字节序列恢复为Java对象的过程

**步骤：**

-创建一个类，继承Serializable接口

-创建对象

-将对象写入文件

-从文件读取对象信息

对象输入流ObjectInputStream

对象输出流ObjectOutputStream

```
//定义Goods类的对象
        Goods goods1 = new Goods("gd001","电脑",3000);
        try {
            FileOutputStream fos = new FileOutputStream("C:\\Users\\hasee\\Desktop\\la.txt");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            FileInputStream fis = new FileInputStream("C:\\Users\\hasee\\Desktop\\la.txt");
            ObjectInputStream ois = new ObjectInputStream(fis);
            //将Goods对象信息写入文件
            oos.writeObject(goods1);
            oos.writeBoolean(true);
            oos.flush();
            //读对象信息
            try {
                Goods goods = (Goods)ois.readObject();
                System.out.println(goods);
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
            System.out.println(ois.readBoolean());
            fos.close();
            oos.close();
            fis.close();
            ois.close();
```

# Json

**json**:(JavaScript Object Natation) javascript的对象表现形式，但是目前已经发展成一种轻量级的数据交换格式

**特点**：完全独立于语言的文本格式，跨平台！有结构的，方便人和机器解析

**使用场景**：

1：不同语言之间的数据传递（Json就是String，但是它是有格式的）,后台的List--->JSON然后前台才可以解析

2：SSH---->EasyUI|EXTJS|AJAX     SSH---->Flex(Adobe FLASH)       HTML5 WebSocket---->JavaScript

**Json与XML、Properties的区别**：

1：json是轻量级别的，而xml是重量级别的(web.xml)，目前xml一般用于框架的配置

2：json是有结构的，但是properties仅仅是key value

## 语法结构

### Json对象语法结构

`{"key":"java","key2":"net"}`

![1538275016485](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1538275016485.png)

### Json数组表现结构

`[`{"key":"java","key2":"net"}`,"abc"]`

![1538275086995](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1538275086995.png)

### Json数值型

`-3   3.5    3.5e+7(3.5*10^7)  5e-7   2e2`

## 实例

### Java对象转成Json

```
Song song = new Song();
System.out.println(JSONSerializer.toJSON(song));
```

### Java表转成Json

```
ArrayList <Song> musicList1=new ArrayList<Song>();
......
System.out.println(JSONSerializer.toJSON(musicList1);
```

## 特殊情况处理

### static成员属性不能转换为Json

#### 解决方法一：

将get和set方法中删除static(不推荐)

#### 解决方法二：

如果返回的是static，或者返回的类型不确定，那么可以采用map或者自己构建json格式

**自己构建：**

```
JSONObject object = new JSONObject();
object.put("age",student.getAge());
object.put("date",student.getDate());
object.put("name",student.getName());
System.out.println(object.toString());
```

### 有自关联的问题，即转换时出现递归循环错误

`private Student student = this;`

#### 解决方案

```
//通过配置jsonConfig来过滤相应的参数
JsonConfig config = new JsonConfig();
//设置需要排除哪些字段
config.setExcludes(new String[]{"student","date"});
System.out.println(JSONObject.fromObject(student,config));
```

上述方法有一定缺陷，即如果student实例化的的不是this，也同样会过滤掉

#### 解决方案一：

```
student.setStudent(new Student());
//通过配置jsonConfig来过滤相应的参数
JsonConfig config = new JsonConfig();
//设置需要排除哪些字段
config.setExcludes(new String[]{"student","date"});
System.out.println(JSONObject.fromObject(student,config));
```

#### 解决方案二：

```
//通过配置jsonConfig来过滤相应的参数
JsonConfig config = new JsonConfig();
//设置需要排除哪些字段
config.setExcludes(new String[]{"student","date"});
//设置如果有些字段是自关联则过滤  
//STRICT：缺省值，是否自关联都要转化
//LENIENT：如果有自关联对象，则值设置为null（推荐）
//NOPROP：如果自关联则忽略属性（推荐需要排除密码字段时使用）
1：/*config.setCycleDetectionStrategy(CycleDetectionStrategy.STRICT);*/
2：/*config.setCycleDetectionStrategy(CycleDetectionStrategy.LENIENT);*/
3：config.setCycleDetectionStrategy(CycleDetectionStrategy.NOPROP)
System.out.println(JSONObject.fromObject(student,config));

```

### Date格式处理

新建一个类`DateJsonValueProcessor`

通过自定义日期的处理类，来格式化日期数据

```
JsonConfig config = new JsonConfig();
//指定某个Json类型的处理方式
DateJsonValueProcessor dataValue = new DateJsonValueProcessor();
config.registerJsonValueProcessor(Date.class,dataValue);
JSONObject.fromObject(student,config);
```

自定义类：

```
package test;

import net.sf.json.JsonConfig;
import net.sf.json.processors.JsonValueProcessor;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * DateJsonValueProcessor class   Date格式化处理
 *
 * @author lamar
 * @date 2018/9/30
 */
public class DateJsonValueProcessor implements JsonValueProcessor {
    private String format = "yyyy-MM-dd HH:mm:ss";
    private SimpleDateFormat sdf = new SimpleDateFormat(format);
    /**
     * 针对数组
     */
    public Object processArrayValue(Object arg0, JsonConfig arg1) {
        return null;
    }

    /**
     *针对对象
     */
    public Object processObjectValue(String arg0, Object arg1, JsonConfig arg2) {
        if (arg1 == null) {
            return "";
        } else if (arg1 instanceof Date) {
            return sdf.format((Date) arg1);
        } else {
            return arg1.toString();
        }
    }
}
```

