# Java学习

## P157-常用API学习-System

System是一个工具类，提供一些与系统相关的方法
|方法名|说明|
|:-:|:-:|
|pulic static void exit(int status)|终止当前Java虚拟机|
|public static long currentTimeMills|返回当前系统的毫秒值形式|
|public static void arraycopy(数据源数组,起始索引,目的地数组,起始索引,拷贝个数)|数组拷贝|

计算机时间原点:**1970年1月1日00:00:00**,我国在东八区,有八个小时时差,所以应该是**1970年1月1日00:08:00**

> public static void exit(int status),参数status，0为正常退出,非0是不正常推出

> public static long currentTimeMills()这个方法可以用它来计算函数的执行时间,下面是一个例子

```java
long start = System.currentTimeMillis();
        for (int i = 1; i < 1000; i++) {
            boolean result = isPrime1(i);
            if (result) {
                System.out.println(i);
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("程序执行时间" + (end - start));

        long start1 = System.currentTimeMillis();
        for (int i = 1; i < 100; i++) {
            boolean result = isPrime2(i);
            if (result) {
                System.out.println(i);
            }
        }
        long end1 = System.currentTimeMillis();
        System.out.println("程序执行时间" + (end1 - start1));

    }

    public static boolean isPrime1(int num) {
        for (int i = 2; i < num; i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static boolean isPrime2(int num) {
        for (int i = 2; i < Math.sqrt(num); i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }
```

这段代码是用来检测这两个判断是否为质数的执行速度的

> public static void arraycopy(数据源数组,起始索引,目的地数组,起始索引,拷贝个数)

```java
        // 如何使用System.arraycopy
        //基本数据类型
        int[] arr1 = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        int[] arr2 = new int[10];
        //参数一:要拷贝的源数组
        //参数二:拷贝的索引
        //参数三:目标数组
        //参数四:目标数组索引
        //参数五:需要复制的个数
        System.arraycopy(arr1, 0, arr2, 0, 10);

        for (int arr22 : arr2) {
            System.out.println(arr22);
        }
        
        //引用数据类型
        Student s1 = new Student("wx", 12);
        Student s2 = new Student("ww", 22);
        Student s3 = new Student("wa", 99);

        Student[] stuArr = { s1, s2, s3 };
        Person[] perArr = new Person[3];
        System.arraycopy(stuArr, 0, perArr, 0, 3);
        for (Person person : perArr) {
            System.out.println(person.getAge() + "," + person.getName());
```

> 如果数据源数组和目的地数组都是基本数据类型,那么就需要保持两者的类型相同,否则就会报错例如:int[]不能拷贝到double[]中
>如果数据源数组和目的地数组都是引用数据类型,那么子类对象可以赋值给父类类型,

## P158-常用API学习-Runtime

<font size=5>Runtime</font>
//这是一个修改，这是第二个
>Runtime表示当前虚拟机的运行环境

这个类对象不能自己new,需要使用方法获取

|方法名|说明|
|:-:|:-:|
|public static Runtime getRuntime()|获取系统的运行环境对象|
|public void exit(int status)|停止虚拟机,跟上面的System.exit(int staus)相同|
|public int availableProcessors()|获取CPU线程数|
|public long maxMemory()|JVM虚拟机从系统中获取的总内存大小(单位byte)|
|pulic long totalMemory()|JVM虚拟机已经获取的内存大小(单位byte)|
|public long freeMemory()|JVM剩余内存大小(单位byte)|
|public Process exec(String command)|运行cmd命令|

>温馨提示:如果想要将byte转换为KB或者MB，需要byte / 1024 = KB ,因为1kb=1024byte

```java
恶搞别人个关机程序
package JavaP158;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.IOException;

import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.WindowConstants;

public class JavaP58DemoJFame extends JFrame implements ActionListener {
    // 将按钮对象定义到成员位置,以便为了调用
    JButton yesBut = new JButton("帅炸了");
    JButton midBut = new JButton("我错了");
    JButton badBut = new JButton("好丑");
    // 获取系统runtime对象

    public JavaP58DemoJFame() {
        intitJFame();
        initView();
        // 设置显示
        setVisible(true);
    }

    public void intitJFame() {
        // 设置宽高
        setSize(500, 600);
        // 设置标题
        setTitle("恶搞基友");
        // 设置关闭模式
        setDefaultCloseOperation(WindowConstants.DO_NOTHING_ON_CLOSE);
        // 设置一直在最上面
        setAlwaysOnTop(true);
        // 设置居中
        setLocationRelativeTo(null);
        // 取消默认位置
        setLayout(null);
    }

    public void initView() {
        getContentPane().removeAll();
        JLabel test = new JLabel("你觉得我帅吗");

        // System.out.println(test.getText());
        test.setBounds(200, 150, 300, 50);
        // 设置按钮尺寸和位置
        yesBut.setBounds(100, 250, 300, 50);
        midBut.setBounds(200, 325, 100, 30);
        badBut.setBounds(160, 400, 180, 30);
        // 绑定事件监听
        yesBut.addActionListener(this);
        midBut.addActionListener(this);
        badBut.addActionListener(this);
        // 将按钮添加到界面中
        getContentPane().add(yesBut);
        getContentPane().add(midBut);
        getContentPane().add(badBut);
        // 添加提示语
        getContentPane().add(test);
        getContentPane().repaint();
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        // 获取是哪个对象
        Object object = e.getSource();
        if (object == yesBut) {
            showDiwlog("谢谢夸奖");
        } else if (object == midBut) {
            showDiwlog("我错了");
            try {
                // 执行系统cmd程序
                Runtime.getRuntime().exec(new String[] { "shutdown", "-a" });
            } catch (IOException e1) {
                // TODO Auto-generated catch block
                e1.printStackTrace();
            }
        } else if (object == badBut) {
            showDiwlog("你完了");
            try {
                // 执行系统cmd程序
                Runtime.getRuntime().exec(new String[] { "shutdown", "-s", "-t", "100" });
            } catch (IOException e1) {
                // TODO Auto-generated catch block
                e1.printStackTrace();
            }
        }
    }

    public void showDiwlog(String context) {// 跳出弹框
        JDialog jdialog = new javax.swing.JDialog();
        // 设置弹窗尺寸
        jdialog.setSize(200, 150);
        // 始终再最上面
        jdialog.setAlwaysOnTop(true);
        // 设置一直居中
        jdialog.setLocationRelativeTo(null);
        // 弹窗不关闭就无法操作下面的界面
        jdialog.setModal(true);
        // 设置描述,添加到弹窗里面
        JLabel warn = new JLabel(context);
        // 设置描述大小
        warn.setBounds(0, 0, 200, 150);
        // 将弹窗添加到弹窗容器内
        jdialog.getContentPane().add(warn);
        // 让弹窗显示
        jdialog.setVisible(true);
    }
    执行方法:
    package JavaP158;

    public class JavaP158Demo {
        public static void main(String[] args) {
            new JavaP58DemoJFame();
        }

    }

}

```

## P159-常用API-Object

- Object类是所有类的顶级父类,所有类间接或直接继承于Object类
- Object类可以被所有子类访问,所以要学习Object类中的方法

Object构造方法
|方法名|空参构造|
|:-:|:-:|
|public Object()|空参构造|
|public String toString()|返回对象的字符串表示形式|
|public Object clone(int a)|对象克隆|

- **toString方法**

```java
// 1.toString,将对象以字符串格式返回
        Object object = new Object();
        String str = object.toString();
        System.out.println(str);// java.lang.Object@5f5a92bb
        // 有一个细节:这里并没有写toString,但是直接输出还是跟上面的相同
        // System:类名
        // out:静态常量名,这里的final修饰的是引用数据类型,地址值不能变,内容可以变
        // println():方法名
        // 参数:标识要输出的内容
        // 核心逻辑,当我们打印一个对象的时候,底层会调用对象的toString方法,把对象变成字符串,然后再打印再控制台上,打印完毕换行处理
    
        System.out.println(object);
       //下面是图例
```

 ![alt text](image-4.png)
![alt text](image-6.png)
![alt text](image-7.png)

- **equals方法**

>默认比较的是地址值,图例如下

![alt text](image-8.png)

如果不想要比较地址值的话,可以重写equals方法

```java
       //这是对equals的小练习
        String s1 = "abc";
        StringBuilder s2 = new StringBuilder("abc");
        
        System.out.println(s1.equals(s2));
        // 因为equals是被s1调用的，而s1是字符串,调用的是字符串里面的equals方法
        // 字符串里面的equals方法,先判断参数是否是字符串
        // 但是s2是StringBuilder的类对象,所以就不相等
        // 所以就会返回false
        System.out.println(s2.equals(s1));
        // 这里equals是被s2调用的,而s2是StringBuilder类对象
        // 在StringBuilder中,没有重写equals方法
        // 所以调用的是object中的equals方法
        // 在object中,==号比较的是地址值
        // 所以这两个记录地址值不一样,就会返回false
    }
```

## P160-常用API-浅克隆，深克隆，对象工具类Objects

把A对象的属性完全拷贝给B对象,也叫对象拷贝,对象复制

> protected权限修饰符解释
   总之，当B extends A的时候，在子类B的作用范围内，只能调用本子类B定义的对象的protected方法(该方法从父类A中继承而来)。而不能调用其他A类（A 本身和从A继承）对象的protected方法

克隆方式

- 浅克隆:不管对象内部属性是基本数据类型还是引用数据类型,都会完全拷贝过来
- 深克隆:基本数据类型拷贝过来,字符串复用,引用数据类型会重新创建新的

现在使用深克隆可以使用一个第三方工具Gson，只要导入就可以了

[Gson下载链接](https://search.maven.org/remotecontent?filepath=com/google/code/gson/gson/2.10.1/gson-2.10.1.jar)

使用方式

```java
// 对象的浅克隆，深克隆和对象工具类Objects
        int[] data = { 1, 2, 3, 4, 5, 67 };
        // 1.先创建一个对象
        User u1 = new User(12, "wx", "123456", "美国大区", data);
        System.out.println(u1);
        // 2.进行对象的克隆
        // 书写细节:重写Object中的clone方法
        // 让JavaBean类实现Cloneable接口
        // 创建原对象并调用clone就可以了

        User u2 = (User) u1.clone();
        System.out.println(u2);
        // Object中的clone是浅克隆
        
        int[] add = u1.getData();
        add[0] = 100;
        System.out.println(u1);
        System.out.println(u2);

        //如果需要进行深克隆,需要重写Object类中的clone方法
        //或者使用gson外部库
------------------------------------------


        /
        // Gson是一个外部库,需要导入
        // 创建一个对象
        Gson gson = new Gson();
        // 将对象变成Json字符串
        String s = gson.toJson(u1);
        // 打印Json字符串
        System.out.println(s);
        // 将字符串变回对象,有两个参数，前面是Json字符串，后面是对象的类型
        User user = gson.fromJson(s, User.class);
        // 打印user对象
        System.out.println(user);
        // 可以代替深克隆使用
        User user2 = gson.fromJson(s, User.class);
        System.out.println(user2);
```

Objects是一个工具类,提供了一些方法去完成一些功能

|序号|方法名|说明|
|:-:|:-:|:-:|
|1|public static boolean equals(Object a,Object b)|先做非空判断,然后比较两个对象|
|2|public staic boolean isNull(Object a)|判断对象是否为Null，是则返回true|
|3|public staic boolean nonNull(Object a)|判断对象是否为Null，是则返回true|

- 1:细节

```java
// Objects工具类学习
        Student s1 = new Student("zhangsan", 12);
        Student soneHalf = null;
        Student s2 = new Student("zhangsan", 12);
        // System.out.println(s1.equals(s2));
        System.out.println(Objects.equals(s1, s2));
        System.out.println(Objects.equals(soneHalf, s2));
        // Objects.equals方法细节
        // 方法底层会判断s1是否为null,如果为Null，直接返回false
        // 如果s1不为null,那么就利用s1再次调用equals方法
        // 如果s1是Student类型,所以最终还是会调用Student中的equals方法
        // 如果没有重写,比较地址值，如果重写了,比较属性值

```

![alt text](image-9.png)

![alt text](image-10.png)

## P161 常用API-BigInteger

在Java中,整数有四种类型

|类型|占用字节大小|
|:-:|:-:|
|byte|1|
|short|2|
|int|4|
|long|8|

BigInteger构造方法
|方法名|说明|
|:-:|:-:|
|public BigInteger(int num,int Random rnd)|获取随机大整数:范围[0~2的num次方-1]|
|public BigInteger(String val)|获取指定大的整数|
|public BigInteger(String val,int radix)|获取指定进制的大整数|

有一个静态方法获取BigInteger对象，内部有优化的:
public static BigInteger valueOf(long val)
!注意:对象一旦创建内部的值无法改变不了

BigInteger常见成员方法

|方法名|说明|
|:-:|:-:|
|public BigIntegeradd(BigInteger val)|加法|
|public BigInteger subtract(BigInteger val)|减法|
|public BigInteger multiply(BigInteger val)|乘法|
|public BigInteger divide(BigInteger val)|除法,获取商|
|public BigInteger[] divideAndRemainder(BigInteger val)|除法,获取商和余数|
|public boolean equals(Object x)|比较是否相同|
|public BigInteger pow(int exponent)|次幂|
|public BigInteger max/min(BigInteger val)|返回较大/较小值|
|public int intValue(BigInteger val)|转为int类型整数,超出范围数据有误|

以下是代码笔记:

```java
   // BigInteger
        // 1.获取一个随机大整数
        BigInteger bigint = new BigInteger(100, new Random());
        System.out.println(bigint);

        // 2.获取一个指定的大整数
        BigInteger bigint2 = new BigInteger("4892570235");
        System.out.println(bigint2);

        // 3.获取一个指定进制的整数
        BigInteger bigint3 = new BigInteger("1000", 2);
        System.out.println(bigint3);

        // 4.有一个静态方法获取BigInteger对象，内部有优化的:
        // public static BigInteger valueOf(long val)
        // !注意:对象一旦创建内部的值无法改变
        // 细节:参数是long类型的,比较小
        // 这里可以获取long类型长度看一下
        System.out.println("Long类型最大" + Long.MAX_VALUE);
        // 这个方法对常用数字:-16~16做了优化 
        // 提前把这个范围以内的数字创建好BigInteger对象,如果多次获取不会创建新的对象

        BigInteger bigint4 = BigInteger.valueOf(5000);
        System.out.println(bigint4);
        // 只要计算就会产生一个新的BigInteger对象
```

## P162 常用API-BigDecimal

BigDecimal的作用

- 用于小数的精确计算
- 用来表示很大的小数

静态方法和成员方法斗鱼BigInteger类似
==细节==
> 1.如果要表示数字不大,没有超出double范围,建议使用BigDecimal的valueOf方法获取BigDecimal对象
> 2.如果要表示数字比较大,超出double范围，建议使用BigDecimal构造方法
> 3.如果我们创建的是0-10的整数,则会返回已经创建好的对象

BigDecimal常用方法
|方法名|说明|
|:-:|:-:|
|public BigDecimal divide(BigDecimal val)|除法|
|public BigDecimal devide(BigDecimal val,精确几位,舍入方式)|除法的重载方法|

其余都与BigInteger类似

**存储原理**:转成字符,转换为ascill,然后存入数组

要注意要会看Java的API帮助文档

## P163 正则表达式
