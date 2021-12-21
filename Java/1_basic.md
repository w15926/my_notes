文档注释

```java
/**
 * <h1>文档注释: 生成网页文件（就是html）</h1><br>
 * 执行: javadoc -d 生成的文件夹名 -author -version 当前文件.java<br>
 * Author: pig<br>
 * EditTime: 2077-07-07<br>
 * Version: 99.0
 */

/*
 * 多行注释
 */

// 单行
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("HelloWorld"); // 换行
    System.out.print("HelloWorld"); // 不换行
    System.out.print("HelloWorld"); // 不换行
  }
}
```



## 模版

```java
/**
 * @Description This is description of method
 * @Author wangLeXiang
 * @Date $date$ $time$  date() time()
 * @Param $params$  methodPaameters()
 * @Return $return$  methodReturnType()
 * @Since version-1.0
 */
```



# 数据类型

### 整形

- 声明long型，值后面加 小写 l  或者 大写 L（不写默认int，声明long也没用）
- 默认int

| 类型  | 占用存储空间   | 表数范围                   |
| :---- | -------------- | -------------------------- |
| byte  | 1字节 = 8bit位 | -128 ~ 127                 |
| short | 2字节          | -2^15 ~ 2^15-1             |
| int   | 4字节          | -2^ ^1 ～ 2^31-1（约21亿） |
| long  | 8字节          | -2^-^3 ～ 2^63-1           |



### 浮点型

- float：单精度，尾数最多精确到7位。值后面加 小写 f  或者 大写 F （不写默认double，声明float也没用）
- double：双精度，是float两倍（默认）

| 类型         | 占用存储空间 | 表数范围               |
| ------------ | ------------ | ---------------------- |
| 单精度float  | 4字节        | -3.403E38 ~ 3.403E38   |
| 双精度double | 8字节        | -1.798E308 ~ 1.798E308 |



### Demo

#### 强制类型转换

- 可能会造成精度损失

```java
public class HelloWorld {
  public static void main(String[] args) {
    // 强制类型转换，表数范围大转小：
    double num = 12.3;
    int i = (int) num; // 括号内写强制转换的类型
    System.out.println(i); // 12（没有四舍五入，直接去掉小数位）
  }
}
```



#### 不同类型相加

- char和数值相加会优先转换成ASCII码（不论前后）
- char和字符串相加会直接拼接字符串（不论前后）
- char、short、byte三者任意相加必须是int，如果那其他类型接收必报错
  - 例：short  s = 3（byte） + 5（short）

```java
public class calculation {
  public static void main(String[] args) {
    // 练习一
    char c = 'a'; // ASCII a = 97 A = 105
    char c2 = 'b';
    int i = 10;
    String s = "hello";

    System.out.println(c + i + s); // 107hello
    System.out.println(c + (i + s)); // a10hello
    System.out.println((c + s) + i); // ahello10

    System.out.println(c + c2); // 195 (97 + 98)



    // 练习二
    // 打印出* *
    System.out.println("*  *"); // * *
    System.out.println('*' + '\t' + '*'); // 93(42 + 9 + 42)char转译成ASCII
    System.out.println('*' + "\t" + '*'); // * *
    System.out.println('*' + '\t' + "*"); // 51*(42 + 9 + “*”)
    System.out.println('*' + ('\t' + "*")); // *       *
  }
}
```



# 运算符

### 逻辑运算符

| &  逻辑与 | ｜ 逻辑或  | ! 逻辑非   |
| --------- | ---------- | ---------- |
| && 短路与 | \|\|短路或 | ^ 逻辑异或 |

- 逻辑异或是只要两者不一样（比如true、false）就是true，两者一样为false
- **逻辑与、或**  和   **短路与、或**  的区别
  - 逻辑与、或不论结果如何都会计算一次
  - 短路与、或不会

```java
    boolean a = true;
    int b = 10;
```

```java
    if (a | b++ > 1) { // if成立，但是右边还会计算一次，此时b = 11
      System.out.println("success");
    } else {
      System.out.println("error");
    }
```

```java
    if (a || b++ > 1) { // if成立，右边不管，b还是等于10
      System.out.println("success");
    } else {
      System.out.println("error");
    }
```



### 三元运算符

```java
    int a = 250;

    int success = 0;
    int error = 1;

    // 错误 不能写运算（垃圾java）
    // int c = (a>1) ? a = 11 : a=22;

    // 相比于JS，c的值必须要能接收成功或失败返回的值类型
    int c = (a > 1) ? success : error;
    System.out.println(c);

    // JS
    // a > 1 ? console.log('>') : console.log('<')
```



# Scanner

**扫描仪，扫描让你终端输入的信息**

```java
import java.util.Scanner;

class scanner04 {
  public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);

    System.out.println("请输入数字");
    int num = scan.nextInt(); // 扫描识别int型赋值给num
    System.out.println("您输入的是：" + num);
    scan.close(); // 关闭扫描器

    double d = scan.nextDouble();
    byte b = scan.nextByte();
    // ... 往下类似
  }
}
```



# switch循环区别

- 在Java中的switch中只支持以下六种数据类型
  1. byte
  2. short
  3. char
  4. int
  5. 枚举（JDK5.0新增）
  6. String（JDK7.0新增）



# Array区别

**数组初始化完成长度就确定无法修改**

```java
    // -------- 静态初始化 --------
    // 声明数组name1为int型
    int[] name1;
    // 为name1数组赋值，值为{}里的内容，长度为值的个数
    name1 = new int[] { 1, 2, 3, 4, 5, 5 };
    // 简写
    // int[] name1 = { 1, 2, 3, 4, 5, 5 };
    
    System.out.println("name1：" + name1[0]);

    
    // -------- 动态初始化 --------
    // 声明数组name2为string型，并指定长度为5
    String[] name2 = new String[5];

    name2[0] = "index0";
    System.out.println("name2：" + name2[0]);
```

**二维数组（就是数组内嵌套数组那一套，名字整的挺高大尚）**

```java
    // -------- 二维数组静态初始化 --------
    // 数组里嵌套数组
    // int[][] arr = new int[][] { { 123, 456 }, { 1, 2, 3 } };
    // 简写
    int[][] arr = { { 123, 456 }, { 1, 2, 3 } };
    System.out.println(arr[0][1]);

    // -------- 二维数组动态初始化 --------
    int[][] arr2 = new int[3][2];
    arr2[2][0] = 123;
    System.out.println(arr2[2][0]);
```



# Array.API（常用）

```java
import java.util.Arrays;

class test {
  public static void main(String[] args) {
    int[] arr1 = new int[] { 1, 2, 3 };
    int[] arr2 = new int[] { -1, -6, 06, 19, 11 };

    // 判断两数组是否相等
    boolean result = Arrays.equals(arr1, arr2); // 返回结果必须是布尔值
    System.out.println(result); // true

    // 查看数组信息（遍历）
    System.out.println(Arrays.toString(arr1)); // [1, 2, 3]
    // System.out.println(arr1.toString()); // JS写法，JS中用于数值转字符串

    // 替换数组中的每个值
    Arrays.fill(arr1, 666);
    System.out.println(Arrays.toString(arr1)); // [1, 2, 3] --> [666, 666, 666]

    // 数组排序
    Arrays.sort(arr2);
    System.out.println(Arrays.toString(arr2)); // [-1, -6, 06, 19, 11] -> [-6, -1, 6, 11, 19]
    // console.log(arr2.sort()); // js写法

    // 数组二分查找
    Arrays.sort(arr2); // 先排序
    int index = Arrays.binarySearch(arr2, 6); // 查找6在数组中的索引，找不到返回-10
    System.out.println(index); // 2
  }
}

```



# 面向对象写法

```java
class objectOriented {
  public static void main(String[] args) {
    // 调用对象Person赋值给p1
    Person p1 = new Person();

    // 调用对象.属性
    p1.name = "胖迪";
    System.out.println("姓名：" + p1.name + '\n' + "年龄：" + p1.age);

    // 调用对象.方法
    p1.eat("大公鸡");
  }
}

// 声明对象Person
class Person {
  // 属性
  String name;
  int age = 18;

  // 方法
  public void eat(String food) { // void返回值是空，没有返回值
    System.out.println("爱吃" + food);
  }

  // return
  public String hobby(String val) { // 返回值是String
    String info = val;
    return info;
  }
}
```



# 重载

```java
class chongzai08 {
  public static void main(String[] args) {
    Load l1 = new Load();
    l1.count(1); // 1 实际调用第一个count
    l1.count(1, 1); // 2 // 实际调用第二个count
    l1.count(1, 2, 3); // 1 2 3 实际调用第三个，可以无限写同类型形参
  }
}

// 重载 累名和方法名可以重复，但是参数不可以重复
class Load {
  public void count(int a) {
    System.out.println(a);
  }

  public void count(int a, int b) {
    System.out.println(a + b);
  }

  public void count(int... num) { // 三个点形式可以接收多个形参，必须写在最后一个，并且只能存在一个
    for (int i = 0; i < num.length; i++) {
      System.out.println(num[i]);
    }
  }

  // public void count(int[] num) { // 等价于 ... 只能使用其中一种

  // }
}
```



# construction构造器

```java
public class Construction {
  public static void main(String[] args) {
    // new的时候会直接执行构造器
    // Person p1 = new Person();

    // new的时候传参相当于给构造器传参
    Person p1 = new Person("胖迪");

    p1.eat(); // 这里面传参是给方法传参
  }
}

class Person {
  // 属性
  String name;
  int age;

  // 构造器 与类同名
  public Person() {
    System.out.println("构造器");
  }

  public Person(String n) { // 重载
    name = n;
  }

  // 方法
  public void eat() {
    System.out.println(name + "吃西瓜");
  }
}
```



# 继承

**A**

```java
package src.com.code.practice;

public class ExtendsA {
  String name = "胖迪";
  int age = 60;

  public ExtendsA() {
  }

  public ExtendsA(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public void eat(){
    System.out.println("吃饭");
  }
}
```

**B**

```java
package src.com.code.practice;

// B 继承 A
public class ExtendsB extends ExtendsA {
}
```

**use**

```java
package src.com.code.practice;

public class ExtendsTest {
  public static void main(String[] args) {
    ExtendsA ea = new ExtendsA();
    ea.eat();

    ExtendsB eb = new ExtendsB();
    // 此时eb里没有定义任何内容，但是eb继承ea，其实调用的是ea
    eb.eat();
    System.out.println(eb.name);
  }
}
```



# 重写

```java
// B 继承 A
public class ExtendsB extends ExtendsA {
  // 因为继承的父类有eat，我再写就是重写，用于覆盖
  public void eat(){
    System.out.println("因为继承的父类有eat，我再写就是重写");
  }
}
```



# super

```java
// B 继承 A
public class ExtendsB extends ExtendsA {
  int age = 80;

  public void getAge(){
    System.out.println(age); // 得到自己定义age，没有定义找父类的age
    System.out.println(super.age); // super关键字用于直接调用父类里的age
  }
}
```



# 多态性

```java
package src.com.code.practice;

public class ExtendsTest {
  public static void main(String[] args) {
    // 多态性
    ExtendsA e = new ExtendsB(); // B必须继承A
    System.out.println(e.age); // 输出A里的age
    e.eat("多态性"); // 在B里对A里的eat进行重写，输出B里重写的eat
  }
}
```



# 代码块

> 静态代码块
>
> > 生命周期最前，类似于Vue的created
> >
> > 内部可以有输出语句
> >
> > 随着类的加载而执行，并且只执行一次



> 非静态代码块
>
> > 生命周期类似于Vue的mounted
> >
> > 区别是于静态是每创建一个对象执行一次

```java
public class codeBlock {
  public static void main(String[] args) {
    // 打印静态代码块
    String my = Abc.name;
    System.out.println(my);

    // 非静态每new一次执行一次
    Abc person = new Abc();
    Abc person2 = new Abc();
  }
}

class Abc {
  static String name = "胖迪";
  int age = 18;

  static {
    System.out.println("static-codeBlock");
    // 生命周期最先执行无法调用
    // this.name = "迪丽热巴";
  }

  {
    System.out.println("codeBlock");
    this.name = "迪丽热巴";
  }
}
```



# interface接口

- 接口和类是两个并列结构

- 接口中不能定义构造器，也就是不能实例化

- 接口通过类去实现（implements）

- -----------------------------------------------------------

  类和类之间 -> 继承

- 类和接口之间 -> 实现

- 接口和接口之间 -> (多)继承

```java
public class testOne {
  public static void main(String[] args) {
    Plane plane = new Plane();
    plane.fly();
    plane.stop();
  }
}

// 定义接口
interface Flyable {
  // 全局常量
  public static final int MAX_SPEED = 7900; // （第一宇宙速度，每秒7900米）
  // 简写
  int MIN_SPEED = 1;

  //  抽象方法
  public abstract void fly();
  // 简写
  void stop();
}

// 可定义多个接口
interface Attackable {
  void attack();
}

// 使用接口
class Plane implements Flyable {

  @Override
  public void fly() {
    System.out.println("起飞");
  }

  @Override
  public void stop() {
    System.out.println("停止");
  }
}

// 可使用多个接口（多继承），如果有父类先继承再实现接口
class Bullet extends Object implements Flyable,Attackable{
  @Override
  public void fly() {

  }

  @Override
  public void stop() {

  }

  @Override
  public void attack() {

  }
}
```

### JDK8

- 在JDK8中初恋定义全局常量和抽象方法之外，还能定义静态方法、默认方法

```java
public interface TestTwo {
  // 静态方法
  public static void method1() {
    System.out.println("method1");
  }

  // 默认方法
  public default void method2() {
    System.out.println("method2");
  }

  // //  简写
  // default void method2() {
  //
  // }
}
```

**use**

```java
public interface TestThree {
  public static void main(String[] args) {
    SubClass s = new SubClass();
    // s.method1(); // static静态方法只能在接口中调用
    TestTwo.method1(); // static静态方法
    s.method2(); // default默认方法
  }
}

class SubClass implements TestTwo {
  // 优先调用重写、父类
  // 重写TestTwo里method2
  // public void method2() {
  //   System.out.println("OverRide for method2");
  // }
}
```



# 异常

### 异常体系结构

![](/Users/lalala/Library/Application Support/typora-user-images/Java/1-basic/异常.png)

### try、catch、final

```java
    String str = "abc";

    try {
      int num = Integer.parseInt(str);
      System.out.println("switch successfully");
    } catch (NumberFormatException e) {
      System.out.println("switch fail");
    }finally { // optional
      System.out.println("类似于default，最后都会执行一次");
    }
```



### throws

- throws 抛出异常给父级处理
- 可一直向上抛直至main

```java
  public static void main(String[] args) {
    try {
      father();
    } catch (NumberFormatException e) {
      // e.printStackTrace();
      System.out.println("被执行了");
    }
  }

  public static void father() throws NumberFormatException {
    son();
  }

  public static void son() throws NumberFormatException {
    String str = "abc";

    int num = Integer.parseInt(str);
  }
```



### throw

手动抛出异常

```java
  public static void main(String[] args) {
    Student student = new Student();
    student.regist(0);
  }
}

class Student {
  private int id;

  public void regist(int id) { // methods1
    // public void regist(int id) throws Exception { // methods2
    if (id > 0) {
      this.id = id;
      System.out.println("当前id为" + this.id);
    } else {
      //  手动抛出异常

      // methods1 控制台打印错误信息
      throw new RuntimeException("给老子重输");

      // methods2 配合throws抛出异常给父级
      // public void regist(int id) throws Exception {
      // throw new Exception("给老子重输");
    }
  }
```




# ---- Thread多线程 ----

## 创建

**Java只能单继承，所以还是推荐方法三使用接口方法，因为接口可以多实现**

### 方法一：继承类Thread

```java
// 1.创建一个继承于Thread类的子类
class MyThread extends Thread {

  // 2.重写Thread类的run（此线程执行的操作）
  @Override
  public void run() {
    ...
  }

}

public class ThreadTest {
  public static void main(String[] args) {
    // 3.创建Thread类的子类对象
    MyThread t1 = new MyThread();

    // 4.调用对象的start()方法执行此线程
    t1.start();

    // 多线程，同时执行这行代码和strat。
    System.out.println("hello world"); 
    // 获取当前线程名
    System.out.println(Thread.currentThread().getName());
  }
}
```

#### 匿名子类

```java
    new Thread() {
      @Override
      public void run() {
        for (int i = 0; i <= 100; i++) {
          if (i % 2 == 0) {
            System.out.println(Thread.currentThread().getName() + "\t" + i);
          }
        }
      }
    }.start();
```



### 方法二：实现类Runnable

```java
public class ThreadTest2 {
  public static void main(String[] args) {
    // 3.创建实现类的对象
    MyThread_one m1 = new MyThread_one();
    // 4.将对象传递到Thread中
    Thread t1 = new Thread(m1);
    // 5.start执行
    t1.start();
  }
}

// 1.实现（继承）Runnable接口
class MyThread_one implements Runnable {

  // 2.重写Runnable中run方法
  @Override
  public void run() {
    for (int i = 0; i <= 100; i++) {
      if (i % 2 == 0) {
        System.out.println(i);
      }
    }
  }

}
```



### 方法三：实现类Callable

- 支持泛型，并带返回值。

```java
public class NewMethods {
  public static void main(String[] args) {
    // 3.创建Callable接口实现类对象
    NumThread numThread = new NumThread();
    // 4.创建的对象传递到FutureTask构造器中
    FutureTask futureTask = new FutureTask(numThread);
    // 5.最后将对象传递到Thread类中并执行此线程
    new Thread(futureTask).start();

    try {
      // 6.（可选）需要返回值的话可以在此调用
      Object sum = futureTask.get();
      System.out.println(sum);
    } catch (InterruptedException e) {
      e.printStackTrace();
    } catch (ExecutionException e) {
      e.printStackTrace();
    }

  }
}

// 1.创建一个实现Callable的实现类
class NumThread implements Callable {

  // 2.实现call方法，将此线程需要完成的操作声明在call()方法中
  @Override
  public Object call() throws Exception {
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
      sum += i;
      System.out.println(sum);
      sum++;
    }
    return sum; // call方法带返回值，不需要时写null
  }

}
```



### 方法四：线程池

> 多线程最终解决方案：线程池
>
> > service.execute(); // 适合适用于Runnable
> >
> > service.submit(); // 适合适用于Callable

```java
public class ThreadPoolOne {
  public static void main(String[] args) {
    // 1.提供指定线程数量的连接池
    ExecutorService service = Executors.newFixedThreadPool(10); // 最多10个线程

    // 这一步设置线程池的属性
    // ThreadPoolExecutor sevice1 = (ThreadPoolExecutor) service; // 向下强转
    // sevice1.setCorePoolSize(15); // 核心池的大小
    // sevice1.setMaximumPoolSize(15); // 最大线程数
    // sevice1.setKeepAliveTime(2000); // 线程没有任务时最多保持多长时间后会终止

    // 2.执行指定的线程的操作，可以是对应的适用于Runnable或者Callable
    service.execute(new NumberThread());
    service.execute(new NumberThread2());

    // 3.代码执行完毕关闭连接池
    service.shutdown();

  }
}

// 输出偶数
class NumberThread implements Runnable {
  @Override
  public void run() {
    Thread.currentThread().setName("线程一（偶数）");
    for (int i = 0; i <= 100; i++) {
      if (i % 2 == 0) {
        System.out.println(Thread.currentThread().getName() + "：" + i);
      }
    }
  }
}

// 输出奇数
class NumberThread2 implements Runnable {
  @Override
  public void run() {
    Thread.currentThread().setName("线程二（奇数）");
    for (int i = 0; i <= 100; i++) {
      if (i % 2 != 0) {
        System.out.println(Thread.currentThread().getName() + "：" + i);
      }
    }
  }
}
```



### 线程的命名

方法一：

```java
MethodTwo m2 = new MethodTwo();
m2.setName("线程二"); // 自定义线程名字，写在start之前
m2.start();
```

方法二：

```java
class MethodOne extends Thread {
  @Override
  public void run() {
    for (int i = 0; i <= 100; i++) {
      if (i % 2 == 0) {
        System.out.println(Thread.currentThread().getName() + "\t" + i);
      }
    }
  }

  // 构造函数命名线程
  public MethodOne(String name) {
    super(name);
  }
}
```

```java
// main
MethodOne m1 = new MethodOne("**** 线程一 ****");
m1.start();
```



### others

- yield

```java
class MethodTwo extends Thread {
  @Override
  public void run() {
    for (int i = 0; i <= 100; i++) {
      if (i % 2 != 0) {
        System.out.println(Thread.currentThread().getName() + "\t" + i);
      }
      if (i % 20 != 0) {
        yield(); // 释放当前CPU执行权（跳过此线程执行下一线程）（跳过了CPU下一次又分给你了，哎就是玩）
      }
    }
  }
}
```

- join

```java
    for (int i = 0; i <= 100; i++) {
      
      if (i % 2 == 0) {
        System.out.println(Thread.currentThread().getName() + "\t" + i);
      }
      
      if (i == 20) {
        try {
          m1.join(); // 当等于20的时候让权给m1线程执行，m1线程执行完毕后再执行此线程
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
      
    }
```

- sleep

```java
class MethodOne extends Thread {
  @Override
  public void run() {
    for (int i = 0; i <= 100; i++) {
      if (i % 2 == 0) {
        try {
          sleep(1000); // 让此线程睡眠一秒，一秒后等CPU分配继续执行
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + "\t" + i);
      }
    }
  }
```



- 优先级（提交一点概率）

```java
/*
 * 设置线程的优先级（只是CPU执行时提高一点概率执行）
 *     getPriority() 获取优先级
 *     setPriority() 设置优先级
 *       Thread.MAX_PRIORITY 10
 *       Thread.NORM_PRIORITY 5
 *       Thread.MIN_PRIORITY 1
 */
public class ThreadDemo2 {
  public static void main(String[] args) {
    MethodThree m1 = new MethodThree("线程一：");
    MethodFour m2 = new MethodFour("线程二：");

    // 设置优先级
    m1.setPriority(Thread.MIN_PRIORITY);
    m1.start();
    m2.start();
  }
}

class MethodThree extends Thread {
  public MethodThree(String name) {
    super(name);
  }

  @Override
  public void run() {
    for (int i = 0; i <= 100; i++) {
      if (i % 2 == 0) {
        // 获取优先级
        System.out.println(getName() + "\t优先级：" + getPriority() + "\t值：" + i);
      }
    }
  }
}
```

### 线程的安全

**使用synchronized或者lock**

#### 继承类

```java
public class Window extends Thread {
  // 票数（static随着类（Window）的加载而执行，并且只执行一次）
  private static int ticket = 100;

  public Window(String name) {
    super(name);
  }

  @Override
  public void run() {
    while (true) {
      show();
      if (ticket < 1) {
        break;
      }
    }
  }

  private static synchronized void show() {
    if (ticket > 0) {
      System.out.println(Thread.currentThread().getName() + "\t票号为：" + ticket);
      ticket--;
    }
  }

}

class windowTest {
  public static void main(String[] args) {
    Window w1 = new Window("窗口一："); // new三个对象
    Window w2 = new Window("窗口二：");
    Window w3 = new Window("窗口三：");

    w1.start();
    w2.start();
    w3.start();
  }
}
```

#### 实现接口

```java
// 方法二
public class Secure {
  public static void main(String[] args) {
    WR2 wr = new WR2();
    Thread t1 = new Thread(wr);
    Thread t2 = new Thread(wr);
    Thread t3 = new Thread(wr);

    t1.setName("窗口一：");
    t1.start();
    t2.setName("窗口二：");
    t2.start();
    t3.setName("窗口三：");
    t3.start();
  }
}

class WR2 implements Runnable {
  private int ticket = 100;

  @Override
  public void run() {
    while (true) {
      show();
      if (ticket < 1) {
        break;
      }
    }
  }

  private synchronized void show() {
    if (ticket > 0) {
      try {
        Thread.sleep(100);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println(Thread.currentThread().getName() + "\t" + ticket);
      ticket--;
    }
  }
}
```

#### notify、notifyAll、wait

**只能运行在同步synchronized中**

> 设两个线程：
>
> > 初始时第一个线程进入，此时没有锁定线程，notify不生效，执行到wait时当前线程锁定。
> >
> > 锁定时自然无法运行，只能被迫让第二个线程进入代码执行。
> >
> > 当第二个线程进入执行到notify时此时线程解锁，此时线程二已在代码里，线程一无法进来，当线程二执行到wait时当前线程锁定。
> >
> > 无限循环，直至退出条件成立。                                                           

```java
class Number implements Runnable {
  private int num = 1;

  @Override
  public void run() {
    while (true) {
      synchronized (this) {
        // 通知他别等了，开始执行
        notify(); // notifyAll();前者唤醒之前那个，后者唤醒所有

        if (num <= 100) {
          try {
            Thread.sleep(10);
          } catch (InterruptedException e) {
            e.printStackTrace();
          }
          System.out.println(Thread.currentThread().getName() + "：" + num);
          num++;

          // wait 让当前线程阻塞永远停止运行，等待notify来继续执行
          try {
            wait();
          } catch (InterruptedException e) {
            e.printStackTrace();
          }

        } else {
          break;
        }
      }
    }
  }

}
```



# ---- 常用类 ----

## String

```java
/*
 * String声明为final的，不可被继承
 * String实现了Serializable接口：可序列化
 *             Comparable接口：可比较大小
 * 内部定义了byte数组存放每一个字符，老版本是char
 */
public class StringTest {
  @Test
  public void test1() {
    String s1 = "ABS"; // 字面量声明赋值
    String s2 = "ABS";

    // 常用方法，和JS一样的就不写了
    // System.out.println(s1 == s2); // 比较地址值（常量池）
    // System.out.println(s1.isEmpty()); // 是否为空
    // System.out.println(s1.equals(s2)); // 比较里面的值

    // 等价于JS的slice（原数组不变那个）（因为Java中String不可变）
    // String newS1 = s1.substring(1);
    // System.out.println(s1);
    // System.out.println(newS1);

    // System.out.println(s1.matches("\\d+")); // 匹配正则表达式

    // String转char型数组，可用作遍历字符串（Java中String不可直接遍历）
    // char[] newStr = s1.toCharArray();
    // for (int i = 0; i < newStr.length; i++) {
    //   System.out.println(newStr[i]);
    // }

    // char型数组转String
    // char[] arr = new char[]{'a','1','2','c'};
    // String newArr = new String(arr);
    // System.out.println(newArr);

    //  String转byte[]（编码）
    //   byte[] bytes = s1.getBytes();
    //   System.out.println(Arrays.toString(bytes));

    // byte转String（解码）
    // byte[] bytes = s1.getBytes();
    // String str = new String(bytes);
    // System.out.println(str);
  }
}
```

### String、StringBuffer、StringBuilder

> 只要不处理线程问题就用StringBuilder

```java
/*
 * String、StringBuffer、StringBuilder三者区别
 * String：不可变的字符序列，底层使用byte（jdk1.9+），老版本使用char，效率最低
 * StringBuffer：可变的字符序列，线程安全、效率低
 * StringBuilder：可变的字符序列，jdk5.0+，线程安全，效率高
 */
public class StringBufferBuilder {

  @Test
  public void t1() {
    StringBuffer sb1 = new StringBuffer("abc");
    sb1.setCharAt(0, '1'); // 可以直接修改String

    // 可变字符序列默认创建16个长度的数组，当满16时自动扩容。
    // 打印长度时时当前当前值的长度
    // System.out.println(sb1.length()); // 3

    //  方法书写和String不同，结果相同
    //   sb1.append("1");
    // sb1.delete(1, 1); // String substring
    // 直接sb1.看提示就知道了

    // 常用总结
    // 增：append()
    // 删：delete()
    // 改：setCharAt(),replace()
    // 查：charAt()
    // 插：insert()
    // 长度：length()
    // 遍历：for() + charAt(),toString()
  }
}
```



## Date

### jdk8 before

Date

```java
    // 返回当前时间戳
    long time = System.currentTimeMillis();

    // Date date1 = new Date(new Date().getTime());
    // System.out.println(date1.toString()); // Fri Jul 16 17:18:20 CST 2021
    // System.out.println(date1.getTime()); // date1的当前时间戳
    // System.out.println(date1.getYear());
    // System.out.println(date1.getMonth());
    // System.out.println(date1.getDate()); // 日
    // System.out.println(date1.getDay()); // 周几

    // 创建java.sql.Date对象
    // java.sql.Date date2 = new java.sql.Date(time);
    // System.out.println(date2);// 2021-07-16

    // 将java.util.Date对象转换为java.sql.Date对象
    // Date date = new java.sql.Date(time);
    // java.sql.Date newDate = (java.sql.Date)date;
    // System.out.println(newDate);

    // SimpleDateFormat
    //     格式化：日期 --> 字符串
    //     解析：字符串 --> 日期
    // SimpleDateFormat sdf = new SimpleDateFormat();
    SimpleDateFormat sdf = new SimpleDateFormat("yyy-MM-dd hh:mm:ss");
    Date date = new Date();

    // 格式化
    String format = sdf.format(date);
    System.out.println(format); // 默认 2021/7/17 上午10:50
    System.out.println(format); // new传值 2021-07-17 11:03:57

    // 解析
    // String str = "2021/7/17 上午10:50";
    String str = "2021-07-17 11:03:57";
    try {
      Date parse = sdf.parse(str);
      System.out.println(parse); // Sat Jul 17 10:50:00 CST 2021
    } catch (ParseException e) {
      e.printStackTrace();
    }
```

日历类

```java
    // Calendar日历类（抽象类）的使用
    // 1.实例化
    // 调用静态方法
    Calendar calendar = Calendar.getInstance();

    // 2.常用方法
    // get()
    // System.out.println(calendar.get(Calendar.DAY_OF_YEAR)); // 这一年的第几天
    // System.out.println(calendar.get(Calendar.DAY_OF_MONTH)); // 这一月的第几天
    // System.out.println(calendar.get(Calendar.DAY_OF_WEEK) - 1); // 这一周的第几天（要减去1，老外以周日为一周第一天）

    // set()
    // calendar.set(Calendar.DAY_OF_MONTH, 6); // 今天为这个月的第几天改成第6天
    // System.out.println(calendar.get(Calendar.DAY_OF_MONTH));

    // add()
    // calendar.add(Calendar.DAY_OF_MONTH, 3); // 今天为这个月的第几天基础上加上3天
    // calendar.add(Calendar.DAY_OF_MONTH, -3); // 今天为这个月的第几天基础上加上-3天（减3天）
    // System.out.println(calendar.get(Calendar.DAY_OF_MONTH));

    // getTime()：日历类 -->  Date
    // Date date = calendar.getTime();                
    // System.out.println(date); // Sat Jul 17 20:05:52 CST 2021

    // setTime()：Date -->  日历类
    // calendar.setTime(new Date());
```

### jdk8 after

#### LocalDate、LocalTime、LocalDateTime（常用）

```java
    // now()获取当前的日期、时间、日期+时间
    // LocalDate localDate = LocalDate.now(); // 2021-07-17
    // LocalTime localTime = LocalTime.now(); // 21:48:33.490177
    // LocalDateTime localDateTime = LocalDateTime.now(); // 2021-07-17T21:48:33.490201

    // of()  自定义时间 2077-10-30T17:30:30
    // LocalDate
    // LocalTime
    // LocalDateTime of = LocalDateTime.of(2077, 10, 30,17,30,30);

    // get()
    // LocalDateTime localDateTime = LocalDateTime.now();
    // System.out.println(localDateTime.getDayOfMonth());// 获取当月的的第几天

    // set()
    // LocalDate localDate = LocalDate.now();
    // LocalDate localDate1 = localDate.withDayOfYear(20); // 修改当前日为20日
    // LocalDate localDate1 = localDate.withYear(2077); // 修改当前年为2077年
    // System.out.println(localDate); // 原对象本身不变，修改后返回返回值（不可变性）
    // System.out.println(localDate1);

    // plus()、minus()
    // LocalDate localDate = LocalDate.now();
    // LocalDate localDate1 = localDate.plusYears(1000); // 当前年加1000年
    // LocalDate localDate1 = localDate.minusMonths(2); // 当前月减2月
    // System.out.println(localDate1);
```

##### Instant

```java
    // now() 获取本初子午线对应的标准时间
    Instant instant = Instant.now();
    System.out.println(instant); // 2021-07-18T02:35:05.778463Z （时差不同，要手动加8小时）

    // 添加时间的偏移量
    OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));
    System.out.println(offsetDateTime); // 2021-07-18T10:39:49.077256+08:00（手动加上8小时）

    // 毫秒与转换
    System.out.println(instant.toEpochMilli()); // 获取当前毫秒
    System.out.println(Instant.ofEpochMilli(1626576323490L)); // 根据毫秒数返回时间（加L）
```

##### DateTimeFormatter

```java
    // method one
    DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
    // formatter: Date --> String
    LocalDateTime localDateTime = LocalDateTime.now(); // 2021-07-18T11:13:58.27574
    String format = formatter.format(localDateTime);
    System.out.println(format);
    // parser: String --> Date
    TemporalAccessor parse = formatter.parse("2021-07-18T11:02:47.75271");
    System.out.println(parse);

    // method two
    DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT);
    // formatter: Date --> String
    LocalDateTime localDateTime2 = LocalDateTime.now();
    String format1 = dateTimeFormatter.format(localDateTime2);
    System.out.println(format1); // 2021/7/18 上午11:13 (short)

    // method three(自定义格式)(常用)
    DateTimeFormatter formatter2 = DateTimeFormatter.ofPattern("yyy-MM-dd hh:mm:ss");
    // 格式化
    String format2 = formatter2.format(LocalDateTime.now());
    System.out.println(format2); // 2021-07-18 11:23:50
    // 解析
    TemporalAccessor parse1 = formatter2.parse("2021-07-18 11:23:50");
    System.out.println(parse1);
```



## Compare 

### Comparable（自然排序）（多重使用）

#### 无泛型写法

```java
  @Test // Comparable  自然排序
  public void test2() {
    // 按照价格从小到大对数组里的对象进行排序
    Goods[] arr = new Goods[4];
    arr[1] = new Goods("iphone3G", 666);
    arr[0] = new Goods("iphone4S", 7);
    arr[2] = new Goods("iphone4", 7);
    arr[3] = new Goods("iphone5", 3);

    Arrays.sort(arr); // Goods里必须先实现重写compareTo
    System.out.println(Arrays.toString(arr));
  }
```

```java
public class Goods implements Comparable {
  private String name;
  private double price;

  // ... 构造器、get set

  @Override
  public String toString() {
    return "Goods{" +
            "name='" + name + '\'' +
            ", price=" + price +
            '}';
  }

  // 实现接口重写方法
  // 实现价格从低到高排序
  @Override
  public int compareTo(Object o) {
    if (o instanceof Goods) {
      Goods goods = (Goods) o;

      // 方式一 （手动写方式二compare里的方法）
      if (this.price < goods.price) {
        return -1; // 传入价格比较大，返回-1，往后排
      } else if (this.price > goods.price) {
        return 1; // 传入价格比较小，返回1，往前排
      } else {
        // return 0;
        return this.name.compareTo(goods.name); // 如果价格一样按照名字字母排序
      }

      // 方式二 （compare里默认按照从小到大排序，如果按照从大到小等必须用方式一）
      // return Double.compare(this.price, goods.price);

    }
    throw new RuntimeException("传入的数据类型不一致");
  }
}
```



#### 有泛型写法

```java
public class GoodsFanXing implements Comparable<GoodsFanXing> {
  private String name;
  private double price;

  // ... 构造器、get set
  
    @Override
  public String toString() {
    return "Goods{" +
            "name='" + name + '\'' +
            ", price=" + price +
            '}';
  }

  @Override
  public int compareTo(GoodsFanXing o) {
    return this.name.compareTo(o.name); // 如果价格一样按照名字字母排序
  }
}
```





### Comparator（定制排序）（临时使用）

#### 无泛型写法

**两个排序中参杂了泛型，不要用，用下面真正的无泛型写法**

排序一

```java
  @Test // Comparator  定制排序
  public void test3() {
    String[] arr = new String[]{"a", "c", "b", "d"};
    Arrays.sort(arr, new Comparator<String>() {
      @Override
      public int compare(String o1, String o2) {
        if (o1 instanceof String && o2 instanceof String) {
          String s1 = (String) o1;
          String s2 = (String) o2;
          // return s1.compareTo(s2); // 从小到大
          return -s1.compareTo(s2); // 从大到小
        }
        // return 0;
        throw new RuntimeException("输入的类型不一致");
      }
    });
    System.out.println(Arrays.toString(arr));
  }
```

排序二

```java
  @Test // Comparator  定制排序2
  public void test4() {
    // 先按照name从小到大排序，再按照price从高到低排序
    Goods[] arr = new Goods[4];
    arr[1] = new Goods("iphone3G", 666);
    arr[0] = new Goods("iphone4S", 7);
    arr[2] = new Goods("iphone4", 7);
    arr[3] = new Goods("iphone5", 3);

    Arrays.sort(arr, new Comparator<Object>() {
      @Override
      public int compare(Object o1, Object o2) {
        if (o1 instanceof Goods && o2 instanceof Goods) {
          Goods g1 = (Goods) o1;
          Goods g2 = (Goods) o2;
          if (g1.getName().equals(g2.getName())) { // 如果名字一样
            return -Double.compare(g1.getPrice(), g2.getPrice());
          } else {
            return g1.getName().compareTo(g2.getName());
          }
        }
        throw new RuntimeException("输入的类型不一致");
      }
    });
    System.out.println(Arrays.toString(arr));
  }
```



#### 有泛型写法

排序一

```java
    String[] arr = new String[]{"a", "c", "b", "d"};
    Arrays.sort(arr, new Comparator<String>() {
      @Override
      public int compare(String o1, String o2) {
        return -o1.compareTo(o2); // 从大到小
      }
    });
    System.out.println(Arrays.toString(arr));
```

排序二

```java
    // 先按照name从小到大排序，再按照price从高到低排序
    GoodsFanXing[] arr = new GoodsFanXing[4];
    arr[1] = new GoodsFanXing("iphone3G", 666);
    arr[0] = new GoodsFanXing("iphone4S", 7);
    arr[2] = new GoodsFanXing("iphone4", 7);
    arr[3] = new GoodsFanXing("iphone5", 3);

    Arrays.sort(arr, new Comparator<GoodsFanXing>() {
      @Override
      public int compare(GoodsFanXing o1, GoodsFanXing o2) {
        if (o1.getName().equals(o2.getName())) { // 如果名字一样
          return -Double.compare(o1.getPrice(), o2.getPrice()); // 按照价格从高到低  反之去掉负号
        } else {
          // return o1.getName().compareTo(o2.getName()); // 否则按照名字排序
          return o2.getName().compareTo(o1.getName()); // 按照名字倒序排序
        }
      }
    });
    System.out.println(Arrays.toString(arr));
```





## System

```java
    // java版本
    String javaVersion = System.getProperty("java.version");
    System.out.println(javaVersion);

    // java路径
    String javaHome = System.getProperty("java.home");
    System.out.println(javaHome);

    // 电脑系统名称
    String osName = System.getProperty("os.name");
    System.out.println(osName);

    // 电脑系统版本
    String osVersion = System.getProperty("os.version");
    System.out.println(osVersion);

    // 电脑用户名
    String userName = System.getProperty("user.name");
    System.out.println(userName);

    // 电脑用户所在路径
    String userHome = System.getProperty("user.home");
    System.out.println(userHome);

    // 当前文件所在的路径
    String userDir = System.getProperty("user.dir");
    System.out.println(userDir);
```



## Match

```java
    int num = -12;
    int num2 = 12;

    // 绝对值
    System.out.println(Math.abs(num));
    // 最大值
    System.out.println(Math.max(num,num2));
    // 最小值
    System.out.println(Math.min(num,num2));
    // 0.0 - 1.0的随机数
    System.out.println(Math.random());
    // 向下取整 double
    System.out.println(Math.floor(6.66));
    // 向上取整 double
    System.out.println(Math.ceil(6.66));
    // 四舍五入 double --> long
    System.out.println(Math.round(6.66));
```



## BigInteger与BigDecimal

```java	
  // BigInteger
  @Test
  public void t() {
    // 表示不可变的任意精度的整数
    BigInteger bi = new BigInteger("123124124124123123123112312323123123");
  }

  // BigDecimal
  @Test
  public void t(){
    // 表示不可变的任意精度的浮点数
    BigDecimal bd = new BigDecimal("0.1241284124513532575856865756756");
  }
```



# ---- Enumeration枚举 ----

## 常用方法

> jdk5.0后使用enum定义
>
> > 定义的枚举类默认继承java.lang.Enum类

```java
public class SeasonTest {
  public static void main(String[] args) {
    // toString() 拿到里面某个枚举对象的值
    Season summer = Season.SUMMER;
    System.out.println(summer.toString());

    // valueOf 拿到里面某个枚举对象的值（等价于toString）
    Season winter = Season.valueOf("WINTER");
    System.out.println(winter);

    // values 拿到里面所有枚举对象
    Season[] values = Season.values();
    for (int i = 0; i < values.length; i++) {
      System.out.println(values[i]);
    }

  }
}

// 使用enum关键字枚举类
enum Season {
  // 1.提供当前枚举类的对象（常量），多个对象之间用,隔开，最后一个;结束
  SPRING("spring", "春天"),
  SUMMER("summer", "夏天"),
  AUTUMN("autumn", "秋天"),
  WINTER("winter", "冬天");

  // 2.声明season对象的属性
  private final String seasonName;
  private final String seasonDesc;

  // 3.初始化构造器
  Season(String seasonName, String seasonDesc) {
    this.seasonName = seasonName;
    this.seasonDesc = seasonDesc;
  }

  // 4.其他操作：1.获取枚举类的属性
  public String getSeasonName() {
    return seasonName;
  }

  public String getSeasonDesc() {
    return seasonDesc;
  }

  // 4.其他操作：2.重写toString() 与Season.valueOf()等价
  @Override
  public String toString() {
    return "Season{" +
            "seasonName='" + seasonName + '\'' +
            ", seasonDesc='" + seasonDesc + '\'' +
            '}';
  }
}
```



## 接口使用

### 方式一

```java
    // 方式一：实现接口，每个对象调用同一个接口
    Season spring = Season.SPRING;
    Season autumn = Season.AUTUMN;
    spring.show();
    autumn.show();
```

```java
interface Info {
  void show();
}

// 使用enum关键字枚举类
enum Season implements Info {
  
  ...
    
  @Override
  public void show() {
    System.out.println("hello");
  }
}
```



### 方式二

```java
    // 方式二：实现接口，每个对象调用各自重写同一个接口
    Season spring = Season.SPRING;
    Season autumn = Season.AUTUMN;
    spring.show();
    autumn.show();
```

```java
interface Info {
  void show();
}

// 使用enum关键字枚举类
enum Season implements Info {
  // 1.提供当前枚举类的对象（常量），多个对象之间用,隔开，最后一个;结束
  SPRING("spring", "春天") {
    @Override
    public void show() {
      System.out.println("这是春天");
    }
  },
  SUMMER("summer", "夏天") {
    @Override
    public void show() {
      System.out.println("这是夏天");
    }
  },
  AUTUMN("autumn", "秋天") {
    @Override
    public void show() {
      System.out.println("这是秋天");
    }
  },
  WINTER("winter", "冬天") {
    @Override
    public void show() {
      System.out.println("这是冬天");
    }
  };
  
  ...
    
}
```



# ---- Annotation注解 ----

## 自定义注解

```java
// 自定义注解
// @MyAnnotation(value = "hello")
@MyAnnotation
```

```java
// 元注解 注解注解的注解
@Retention(RetentionPolicy.RUNTIME)
// 声明注解只能在哪生效
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
// @interface声明注解
public @interface MyAnnotation {
  String value() default "world";
}
```



# ---- Collection集合 ----

> 集合、数组都是对多个数据进行存储操作的结构，简称java容器
>
> 
>
> 数组：
>
> > 初始化后长度固定，元素类型固定。
> >
> > 比如String[] arr 其元素类型固定为String，不可改变
> >
> > 增删改查不方便，远不如JS push来pop去
>
> 
>
> 集合：
>
> > 数组升级版，但源码是用数组书写
> >
> > Collection接口（单列集合，用来存储一个个对象）
> >
> > > 接口（存储有序、可重复的数据）（动态数组）
> > >
> > > > ArrayList、LinkedList、Vector
> > >
> > > Set接口（存储无序、不可重复的数据）
> > >
> > > > HashSet、LinkedHashSet、TreeSet
> >
> > Map接口（双列集合，用来存储一对一对（key value）的数据）
> >
> > > HashMap、LinkedHashMap、TreeMap、Hashtable、Properties
>
> 
>
> **有序无序是对底层存放进数组的排序，有序按照书写顺序进行排序，无序自动排序并自动过滤重复的值**



## collection接口

### basic

```java
    Collection collection = new ArrayList();

    // add (Object e)
    collection.add("A");
    collection.add("b");
    collection.add(123); // 自动装箱
    collection.add(456);
    collection.add(new Date());
    System.out.println(collection); // [A, b, 123, 456, Mon Jul 19 17:25:33 CST 2021]

    // size (length)
    System.out.println(collection.size()); // 5

    // addAll （定义的一组集合添加到另一组集合中）
    Collection collection1 = new ArrayList();
    collection1.addAll(collection);
    System.out.println(collection1);

    // isEmpty（根据size来判断是否为空）
    System.out.println(collection1.isEmpty()); // false

    // clear
    collection1.clear();
    System.out.println(collection1); // []
```



### contains()

```java
  @Test
  public void test() {
    Collection collection = new ArrayList<>();
    collection.add(1);
    collection.add(false);
    collection.add(new String("3"));
    collection.add(new Person("胖迪", 18));
    System.out.println(collection);

    // contains() 判断是否包含
    System.out.println(collection.contains(1)); // true
    System.out.println(collection.contains(new String("3"))); // true
    System.out.println(collection.contains("3")); // true
    System.out.println(collection.contains(new Person("胖迪", 18))); // false （Person内重写equals后变成true，让他们比内容）

    // containsAll() 另一个对象里的内容该数组里是否都存在
    Collection coll = Arrays.asList(1, "3", new String("3"));
    System.out.println(coll); // [1, 3, 3] 不管写多少个"3"，只要包含就为true
    System.out.println(collection.containsAll(coll)); // true coll里的内容collection里都存在
  }
```

```java
// Person
public class Person {
  private String name;
  private int age;
  
  // ... 构造器，get set

  @Override
  public String toString() {
    return "Person{" +
            "name='" + name + '\'' +
            ", age=" + age +
            '}';
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Person person = (Person) o;
    return getAge() == person.getAge() && Objects.equals(getName(), person.getName());
  }

  @Override
  public int hashCode() {
    return Objects.hash(getName(), getAge());
  }
}
```



### remove()、removeAll()

```java
  @Test
  public void test2() {
    Collection collection = new ArrayList<>();
    collection.add(1);
    collection.add(false);
    collection.add(new String("3"));
    collection.add(new Person("胖迪", 18));
    System.out.println(collection); // [1, false, 3, Person{name='胖迪', age=18}]

    // remoe
    // collection.remove(false);
    // collection.remove(new Person("胖迪", 18));
    // System.out.println(collection); // [1, 3]

    // removeAll
    Collection coll = Arrays.asList(1, "3", "3", "3");
    collection.removeAll(coll); // coll里的东西只要collection里存在就删掉
    System.out.println(collection);
  }
```



### retainAll()

```java
  @Test
  public void test3() {
    Collection collection = new ArrayList<>();
    collection.add(1);
    collection.add(false);
    collection.add(new String("3"));
    collection.add(new Person("胖迪", 18));
    System.out.println(collection);

    // retainAll 交集 检测coll和collection里有哪些一样的值
    Collection coll = Arrays.asList(1, 2, "3", 4, 5);
    collection.retainAll(coll);
    System.out.println(collection); // [1, 3]
  }
```



### equals

```java
  @Test
  public void test4() {
    Collection collection = new ArrayList();
    collection.add(1);
    collection.add(false);
    collection.add(new String("3"));
    collection.add(new Person("胖迪", 18));

    Collection collection1 = new ArrayList();
    collection1.add(1);
    collection1.add(false);
    collection1.add(new String("3"));
    collection1.add(new Person("胖迪", 18));

    // 检测两个集合是否相等
    System.out.println(collection.equals(collection1)); // true ArrayList是有序列表，两个列表瞬间不可变
  }
```



### hashCode()、集合 --> 数组、数组 --> 集合

```java
@Test
public void test5() {
  Collection collection = new ArrayList();
  collection.add(1);
  collection.add(false);
  collection.add(new String("3"));
  collection.add(new Person("胖迪", 18));

  // hashCode() 返回当前对象的哈希值
  System.out.println(collection.hashCode());

  // 集合 --> 数组
  Object[] arr = collection.toArray();
  System.out.println(Arrays.toString(arr));

  // 数组 --> 集合
  List<String> list = Arrays.asList(new String[]{"a", "b"});
  System.out.println(list);
}
```



### iterator迭代器

```java
  @Test
  public void test() {
    Collection collection = new ArrayList();
    collection.add(1);
    collection.add(false);
    collection.add(new String("3"));
    collection.add(new Person("胖迪", 18));

    Iterator iterator = collection.iterator();

    // 方式一
    // System.out.println(iterator.next());
    // System.out.println(iterator.next());
    // System.out.println(iterator.next());
    // System.out.println(iterator.next());
    // System.out.println(iterator.next()); // 这行报错，一共四条数据

    // 方式二
    // for (int i = 0; i < collection.size(); i++) {
    //   System.out.println(iterator.next());
    // }

    // 方式三 推荐 hashNext检测当前是否有元素，有就执行
    // while (iterator.hasNext()){
    //   System.out.println(iterator.next());
    // }

    // iterator里的remove
    Iterator iterator1 = collection.iterator(); // 需要再声明一次让指针返回初始
    while (iterator1.hasNext()) {
      Object next = iterator1.next();
      if ("3".equals(next)) {
        iterator1.remove(); // next之前不能调用
      }
    }
    System.out.println(collection);

  }
```



## List接口

> List接口：存储有序、可重复
>
> > ArrayList：List接口主要实现类，线程不安全，效率高，底层使用数组存储
> >
> > LinkedList，频繁使用插入和删除时效率最高，底层使用双向链表存储
> >
> > Vector：古老实现类，线程安全，效率低，底层使用数组存储

```java

  @Test
  public void test() {
    ArrayList list = new ArrayList();

    list.add(123);
    list.add("123");
    list.add(new Person("胖迪", 18));

    // addAll
    List<Integer> newList = Arrays.asList(6, 7, 8);
    // 当成一个对象添加 [123, 123, Person{name='胖迪', age=18}, [6, 7, 8]]
    // list.add(newList);
    // 把该对象里的各个内容逐个添加 [123, 123, Person{name='胖迪', age=18}, 6, 7, 8]
    list.addAll(newList);
    System.out.println(list);
  }
```

```java
  @Test
  public void test2() {
    ArrayList list = new ArrayList();

    list.add(123);
    list.add(123);
    list.add(123);

    // indexOf
    // System.out.println(list.indexOf(123)); // 该值第一次出现的索引

    // lastIndexOf
    // System.out.println(list.lastIndexOf(123)); // 该值最后一次出现的索引

    // remove
    // Object remove = list.remove(0); // 删除指定索引上的值并返回，等价于JS的slice
    // System.out.println(remove);

    // get
    // System.out.println(list.get(0)); // 获取索引0的值

    // set
    // list.set(0, "666"); // 索引0的值改成"666"
    // System.out.println(list);

    // subList
    // List list1 = list.subList(0, 2);
    // System.out.println(list1); // 返回 index 0 到 length 2 之间的值
  }
```



## set接口

```java
public class t {
  @Test // HashSet
  public void tt() {
    Set set = new HashSet();
    set.add(3);
    set.add("2");
    set.add(1);
    set.add(1);
    set.add(1);
    set.add(1);
    set.add(1);
    set.add("b");
    set.add("a");

    System.out.println(set); // [1, a, 2, b, 3] 不可重复 顺序按照自动排序
  }

  @Test // LinkedHashSet
  public void ttt() {
    LinkedHashSet set = new LinkedHashSet();
    set.add(3);
    set.add("2");
    set.add(1);
    set.add(1);
    set.add(1);
    set.add(1);
    set.add(1);
    set.add("b");
    set.add("a");

    System.out.println(set); // [3, 2, 1, b, a] 不可重复 顺序按照书写顺序排序
  }

  @Test // TreeSet
  public void tttt() {
    TreeSet set = new TreeSet();
    set.add(3);
    set.add(2);
    set.add(1);
    set.add(4);

    System.out.println(set); // [1, 2, 3, 4] TreeSet里的类型必须是同一类型
  }
}
```



## Map接口

> Map：双列数据：存储key-value对的数据
>
> > HashMap：Map的主要实现类；线程不安全，效率高；存储null的key和value
> >
> > > LinkedHashMap：按照添加顺序遍历，对于频繁操作，效率最高
> >
> > ThreeMap：按书写顺序进行排序，只能用一类型
> >
> > Hashtable：作为古老的实现类；线程安全，效率低；不能存储null的key和value
> >
> > > Properties：常用来处理配置文件，key和value都是String类型



### HashMap

> 结构
>
> > key：无序、不可重复，set存储
> >
> > > key所在类要重写equals()和hashCode()（以HashMap为例）
> >
> > value：无序、可重复、Collection存储
> >
> > > value类要重写equals()类
> >
> > 一个键值对：key-value构成了一个Entry对象
> >
> > > entry：无序、不可重复，set存储

#### 增删改查

```java
    // put 添加
    Map map = new HashMap();
    map.put(null, null);
    map.put("name", "胖迪");
    map.put("age", 18);
    map.put("age", 18);
    map.put("age", 18);
    map.put("age", 18);
    System.out.println(map); // {null=null, name=胖迪, age=18}

    // put 修改
    map.put("age", 14);
    System.out.println(map); // {null=null, name=胖迪, age=14}

    // putAll 一个map对象添加到另一个map中
    HashMap map1 = new HashMap();
    map1.put("gender", "woman");
    map1.put("hobby", "kissMe");
    map.putAll(map1);
    System.out.println(map); // {null=null, gender=woman, name=胖迪, age=14, hobby=kissMe}

    // remove
    map.remove("hobby"); // 返回被删除的value
    System.out.println(map); // {null=null, gender=woman, name=胖迪, age=14}

    // clear 清空
    map.clear();
    System.out.println(map); // {}
    System.out.println(map.size()); // 0 清空map数据，并不是重置map，清空后数据length = 0
```

```java
    Map map = new HashMap();
    map.put(null, null);
    map.put("name", "胖迪");
    map.put("age", 18);

    // get 查询
    System.out.println(map.get("name")); // 胖迪

    // contains 包含
    System.out.println(map.containsKey("name")); // true
    System.out.println(map.containsValue("胖迪")); // true

    // isEmpty
    System.out.println(map.isEmpty()); // false

    // equals 判断两个map是否相等
```



#### 遍历

```java
  @Test
  public void test3() {
    Map map = new HashMap();
    map.put(null, null);
    map.put("name", "胖迪");
    map.put("age", 18);

    // 遍历（元视图操作方法）

    // key
    Set keySet = map.keySet();
    System.out.println(keySet); // [null, name, age]
    // 方式一
    Iterator iterator = keySet.iterator(); // key是用set存储，所以要这样调用迭代器才能遍历
    while (iterator.hasNext()) {
      System.out.println(iterator.next()); // 遍历每一项key
    }
    // 方式二
    // for(Object item:keySet){
    //   System.out.println(item);
    // }

    // value
    Collection values = map.values();
    System.out.println(values); // [null, 胖迪, 18]
    // 方式一
    // Iterator iterator1 = values.iterator();
    // while (iterator1.hasNext()){
    //   System.out.println(iterator1.next()); // 遍历每一项value
    // }
    // 方式二
    for (Object item : values) {
      System.out.println(item);
    }

    // key-value
    Set entrySet = map.entrySet();
    System.out.println(entrySet); // [null=null, name=胖迪, age=18]
    // 方式一 iterator
    Iterator iterator1 = entrySet.iterator();
    while (iterator1.hasNext()) {
      Object obj = iterator1.next();
      Map.Entry entry = (Map.Entry) obj;
      System.out.println("key = " + entry.getKey() + " ---- value = " + entry.getValue());
    }
    // 方式二 forEach
    for (Object item : entrySet) {
      System.out.println(item); // 拿到每一项key=value
    }
    
    // ----------------- 使用泛型 ----------------
    HashMap<String, Integer> map = new HashMap<>(); // 定义key和value的类型
    map.put("age", 14);
    map.put("age2", 18);

    Set<Map.Entry<String, Integer>> entrySet = map.entrySet();
    Iterator<Map.Entry<String, Integer>> iterator = entrySet.iterator();

    while (iterator.hasNext()) {
      Map.Entry<String, Integer> next = iterator.next();
      System.out.println(next.getKey() + "\t" + next.getValue());
    }

  }
```



### TreeMap

> 存储key-value，要求key必须是同一类型
>
> key排序：自然排序、定制排序

```java
public class t_TreeMap {
  @Test
  public void test() {
    TreeMap treeMap = new TreeMap();
    treeMap.put("name", "胖迪");
    // treeMap.put(123,"value"); // 报错，key类型不一样
    System.out.println(treeMap);
  }

  @Test // 自然排序 Goods里重写compareTo
  public void test2() {
    TreeMap treeMap = new TreeMap();
    Goods goods1 = new Goods("name2", 124);
    Goods goods = new Goods("name", 123);
    Goods goods2 = new Goods("name3", 125);

    treeMap.put(goods1, 1002);
    treeMap.put(goods, 1001);
    treeMap.put(goods2, 1003);

    for (Object item : treeMap.entrySet()) {
      System.out.println(item);
    }

  }

  @Test // 定制排序
  public void test3() {
    TreeMap treeMap = new TreeMap(new Comparator() {
      @Override
      public int compare(Object o1, Object o2) {
        if (o1 instanceof Goods && o2 instanceof Goods) {
          Goods g1 = (Goods) o1;
          Goods g2 = (Goods) o2;
          return -g1.compareTo(g2); // 从大到小
        }
        throw new RuntimeException("输入的类型不匹配");
      }
    });
    Goods goods = new Goods("name", 123);
    Goods goods1 = new Goods("name2", 124);
    Goods goods2 = new Goods("name3", 125);

    treeMap.put(goods, 1001);
    treeMap.put(goods1, 1002);
    treeMap.put(goods2, 1003);

    for (Object item : treeMap.entrySet()) {
      System.out.println(item);
    }

  }
}
```



## Collections工具类

> Collections是操作Collection接口的工具类

```java
public class t_Collections {
  @Test
  public void t2() {
    List list = new ArrayList();
    list.add(123);
    list.add(127);
    list.add(121);

    // copy
    List newList = Arrays.asList(new Object[list.size()]); // 复制的数组长度要大于等于被复制数组的长度
    Collections.copy(newList, list); // list复制给newList

    // synchronizedList 返回安全的线程
    List list1 = Collections.synchronizedList(list);

    System.out.println(newList);
  }

  @Test
  public void t() {
    List list = new ArrayList();
    list.add(123);
    list.add(127);
    list.add(121);
    System.out.println(list);

    // reverse 反转
    // Collections.reverse(list);

    // shuffle 顺序随机化
    // Collections.shuffle(list);

    // sort 顺序排序
    // Collections.sort(list);

    // swap 交换
    // Collections.swap(list,0,2); // index 0与2交换位置

    // 值在集合中出现的个数
    // System.out.println(Collections.frequency(list, 123));

    System.out.println(list);
  }
}
```



# ---- 泛型 ----

> 泛型类，泛型接口；泛型方法

## 基本使用

```java
    HashMap<String, Integer> map = new HashMap<>(); // 定义key和value的类型
    map.put("age", 14);
    map.put("age2", 18);

    Set<Map.Entry<String, Integer>> entrySet = map.entrySet();

    // 方法一
    // while (iterator.hasNext()) {
    //   Map.Entry<String, Integer> next = iterator.next();
    //   System.out.println(next.getKey() + "\t" + next.getValue());
    // }
    
    // 方法二
    for (Map.Entry<String, Integer> next : entrySet) {
      System.out.println(next.getKey() + "\t" + next.getValue());
    }
```



## 泛型类

### 自定义泛型类

```java
public class Order<T> { // T 类的泛型
  String orderName;
  int orderId;

  // 类的内部结构可以使用类的泛型
  T orderT;

  public Order() {
  }

  public Order(String orderName, int orderId, T orderT) {
    this.orderName = orderName;
    this.orderId = orderId;
    this.orderT = orderT;
  }

  public T getOrderT() {
    return orderT;
  }

  public void setOrderT(T orderT) {
    this.orderT = orderT;
  }

  @Override
  public String toString() {
    return "Order{" +
            "orderName='" + orderName + '\'' +
            ", orderId=" + orderId +
            ", orderT=" + orderT +
            '}';
  }

}
```



### 自定义泛型类实例化

```java
  @Test
  public void test1() {
    // Order内定义了泛型，实例化中必须要跟上泛型
    Order<Integer> order = new Order<>();
    order.setOrderT(123);

    // 传惨时给T赋值字符串，此时该实例化对象的泛型就是String
    Order<String> order1 = new Order<>("pd", 1001, "order");
    order1.setOrderT("1");
  }
```



### 继承自定义泛型类

继承

```java
// 继承时添加泛型类型，之后new SubOrder的时候不需要跟泛型，默认继承类泛型
public class SubOrder extends Order<Integer> {
}

// 继承时添加泛型，但未指明类型，实例化的时候要写泛型类型
public class SubOrder2<T> extends Order<T> {
}
```

实例化

```java
  @Test
  public void test2() {
    // SubOrder里继承了泛型，此时实例化时默认跟着泛型
    SubOrder subOrder = new SubOrder();
    
    // 如果继承时未指定泛型类型，则实例化的时候可以自定义的泛型
    SubOrder2<String> subOrder2 = new SubOrder2<>();
  }
```



## 泛型方法

### 自定义泛型方法

泛型方法和泛型类没有关系

自定义

```java
  // 泛型方法
  public <E> List<E> copyFromToList(E[] arr) {
    ArrayList<E> list = new ArrayList<>();
    for (E e : arr) { // 遍历形参arr
      list.add(e);
    }
    return list;
  }
```

使用

```java
    // 泛型方法
    Order<String> order = new Order<>();
    Integer[] arr = {1, 2, 3, 4};
    List<Integer> integers = order.copyFromToList(arr);
    System.out.println(integers);
```



## 通配符

```java
  @Test
  public void t2() {
    // 通配符的使用 <?>
    List<Object> list1 = null;
    List<String> list2 = null;

    List<?> list = null;

    list = list1;
    list = list2;
  }

  public void print(List<?> list) {
    for (Object o : list) {
      System.out.println(o);
    }
  }
```

```java
    // Student extends Person
    
    // ？ <= Person
    List<? extends Person> list1 = null;
    // ？ >= Person
    List<? super Person> list2 = null;

    List<Student> list3 = null;
    List<Person> list4 = null;
    List<Object> list5 = null ;

    // list1 = list3;
    // list1 = list4;
    // list1 = list5; // 报错 > Person

    // list2 = list3; // 报错 < Person
    // list2 = list4;
    // list2 = list5;
```



# ---- IO流 ----

## File类的使用

### path

```java
    // 相对（当前模块）
    File file = new File("test.txt");
    // 绝对
    File file2 = new File("/Users/lalala/Documents/process/2021-backend/java/02-basic/javaBasic/src/com/java/IO/FileTest/test.txt");
    // 父子
    File file3 = new File("/Users/lalala/Documents", "process/2021-backend/java/02-basic/javaBasic/src/com/java/IO/FileTest/test.txt");
    // 父子2
    String p = "/Users/lalala/Documents";
    File file4 = new File(p, "process/2021-backend/java/02-basic/javaBasic/src/com/java/IO/FileTest/test.txt");
```



### get

```java
    File file = new File("test.txt");
    File file2 = new File("/Users/lalala/Documents/process/2021-backend/java/02-basic/javaBasic/test.txt");

    // 获取相对路径的绝对路径
    System.out.println(file.getAbsolutePath());
    // 获取当前文件存放的相对或者绝对路径
    System.out.println(file.getPath());
    // 获取该绝对路径的上一级  a/b/c --> a/b
    System.out.println(file2.getParent());
    // 获取文件名
    System.out.println(file.getName());
    // 获取当前文件的长度
    System.out.println(file.length());
    // 获取最近一次修改的时间
    System.out.println(new Date(file.lastModified()));

    File file3 = new File("/Users/lalala/Documents");
    // 获取当前目录下的所有文件目录
    String[] list = file3.list();
    for (String item : list) {
      System.out.println(item);
    }
    // 获取当前目录下的所有文件目录和文件的绝对路径加文件后缀
    File[] files = file3.listFiles();
    for (File item : files) {
      System.out.println(item);
    }
```



### set

```java
    // 删除one文件并将其内容覆盖到two文件
    File file = new File("one.txt");
    File file2 = new File("two.txt");

    boolean rename = file.renameTo(file2);
    System.out.println(rename);
```



### 判断文件

```java
    File file = new File("test.txt");

    // 判断当前文件是否为目录
    System.out.println(file.isDirectory());
    // 判断当前文件是否为文件
    System.out.println(file.isFile());
    // 判断当前文件是否存在
    System.out.println(file.exists());
    // 判断当前文件是否能读
    System.out.println(file.canRead());
    // 判断当前文件是否能写
    System.out.println(file.canWrite());
    // 判断当前文件是否隐藏
    System.out.println(file.isHidden());
```



### 创建、删除文件

```java
    File file = new File("newFile.txt");
    if (!file.exists()) { // 如果文件不存在
      try {
        file.createNewFile();     // 创建文件
        System.out.println("创建成功");
      } catch (IOException e) {
        e.printStackTrace();
      }
    } else { // 如果文件存在
      file.delete(); // 删除文件
      System.out.println("删除成功");
    }
```



### 创建、删除目录

```java
    // 创建目录 - mkdir
    File file = new File("/Users/lalala/Desktop/tt/test");
    boolean mkdir = file.mkdir(); // 在 /Users/lalala/Desktop/tt 目录下创建 test 目录
    if(mkdir){
      System.out.println("创建成功");
    }else {
      System.out.println("创建失败");
    }


    // 创建目录 - mkdirs
    File file = new File("/Users/lalala/Desktop/tt/test");
    // 在 /Users/lalala/Desktop/tt 目录下创建 test 目录
    // 如果要创建的 test 目前前面没有目录（不存在 tt 目录），则自动一起创建 tt 目录
    boolean mkdirs = file.mkdirs();
    if(mkdirs){
      System.out.println("创建成功");
    }else {
      System.out.println("创建失败");
    }
```





## IO流的原理及流的分类

```java
/*
 * 一、流的分类：
 * 1.操作数据单位：字节流、字符流
 * 2.数据的流向：输入流、输出流
 * 3.流的角色：节点流、处理流
 *
 * 二、流的体系结构
 * 抽象基类         节点流（或文件流）                                缓冲流（处理流的一种）
 * InputStream     FileInputStream   (read(byte[] buffer))        BufferedInputStream (read(byte[] buffer))
 * OutputStream    FileOutputStream  (write(byte[] buffer,0,len)  BufferedOutputStream (write(byte[] buffer,0,len) / flush()
 * Reader          FileReader (read(char[] cbuf))                 BufferedReader (read(char[] cbuf) / readLine())
 * Writer          FileWriter (write(char[] cbuf,0,len)           BufferedWriter (write(char[] cbuf,0,len) / flush()
 */
```

### 节点流（或文件流）

```java
// 1. 对于文本文件(.txt,.java,.c,.cpp)，使用字符流处理
// 2. 对于非文本文件(.jpg,.mp3,.mp4,.avi,.doc,.ppt,...)，使用字节流处理
```

#### 读（字符流）

> FileReader (read(char[] cbuf))

*将 src 下 test2.txt 文件读入程序中，并输出到控制台*

```java
  // 对 read()操作升级：使用read的重载方法
  @Test
  public void test2() {
    FileReader fr = null;
    try {

      // 1。File类的实例化
      File file = new File("src/test2.txt");

      // 2。FileRead流的实例化
      fr = new FileReader(file);

      // 3。读入操作
      char[] cbuf = new char[5]; // 每次读取5个   // 开发中常用1024
      int len; // 读取到文件末尾返回 -1
      while ((len = fr.read(cbuf)) != -1) {
        // ------------ 方式一 -------------
        // 错误写法
        // for (int i = 0; i < cbuf.length; i++) {
        //   System.out.println(cbuf[i]);
        // }
        // 正确写法：每次只遍历存进去的个数
        // for (int i = 0; i < len; i++) {
        //   System.out.print(cbuf[i]);
        // }
        // ------------ 方式二 -------------
        // 错误写法
        // String str = new String(cbuf);
        // System.out.println(str);
        // 正确写法
        String str = new String(cbuf, 0, len); // 每次遍历的时候输出 0-len 的字符
        System.out.print(str);
      }

    } catch (IOException e) {

      e.printStackTrace();

    } finally {

      // 4。资源的关闭
      try {
        assert fr != null;
        fr.close();
      } catch (IOException e) {
        e.printStackTrace();
      }

    }

  }
```



#### 写（字符流）

> FileWriter (write(char[] cbuf,0,len)

```java
  // 从内存中写入数据到硬盘
  @Test
  public void test() {
    FileWriter fw = null; // 在该文件里追加内容，默认为覆盖
    try {
      // 1。提供File类到对象，并指明写出到的文件
      File file = new File("test2.txt");

      // 2。提供FileWriter的对象，用于数据的写出
      fw = new FileWriter(file, true);
      // FileWriter fw = new FileWriter(file); // 默认为false覆盖原文件

      // 3。写出的操作
      fw.write("I hava a pig~ \n");
      fw.write("I hava a pig to ...");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 4。流资源的关闭
      try {
        assert fw != null;
        fw.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }

  }
```



#### 读写练习（字符流）

```java
  // 练习：读取本地文件到内存再到硬盘（复制）
  @Test
  public void test2() {
    FileReader fr = null;
    FileWriter fw = null;
    try {
      // 1。提供File类到对象，指明读入和写出文件
      File srcFile = new File("test.txt");
      File destFile = new File("test2.txt");

      // 2。创建输入、输出流的对象
      fr = new FileReader(srcFile);
      fw = new FileWriter(destFile);

      // 3。数据的读入写出操作
      char[] cbuf = new char[5]; // 开发中常用1024
      int len;
      while ((len = fr.read(cbuf)) != -1) {
        fw.write(cbuf, 0, len); // 每次写出len个字符
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 4。关闭流资源
      try {
        if (fw != null)
          fw.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
      try {
        if (fr != null)
          fr.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }

  }
```



#### 读、写（字节流）

> FileInputStream   (read(byte[] buffer))
>
> FileOutputStream  (write(byte[] buffer,0,len)

无非是把char改成byte，new FileReader改成new FileInputStream

```java
  // 实现对图片对复制操作
  @Test
  public void test2() {
    FileInputStream fis = null;
    FileOutputStream fos = null;
    try {
      // 1。提供File类到对象，指明读入和写出文件
      File srcFile = new File("src/com/java/img/爱情与友情.jpg");
      File destFile = new File("src/com/java/img/爱情与友情2.jpg");

      // 2。创建输入、输出流的对象
      fis = new FileInputStream(srcFile);
      fos = new FileOutputStream(destFile);

      // 3。数据的读入写出操作
      byte[] buffer = new byte[5]; // 开发中常用1024
      int len;
      while ((len = fis.read(buffer)) != -1) {
        fos.write(buffer, 0, len); // 写
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 4。关闭流资源
      try {
        if (fos != null)
          fos.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
      try {
        if (fis != null)
          fis.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }

  }
```



### 缓冲流（处理流的一种）

> 提高字节流的速度的方式，开发常用

#### 非文本文件复制

```java
@Test
public void test() {
  BufferedInputStream bis = null;
  BufferedOutputStream bos = null;
  try {
    // 1。造文件
    File srcFile = new File("src/com/java/img/爱情与友情.jpg");
    File destFile = new File("src/com/java/img/爱情与友情3.jpg");

    // 2。造流
    // 2-1 造节点流
    FileInputStream fis = new FileInputStream(srcFile);
    FileOutputStream fos = new FileOutputStream(destFile);
    // 2-2 造缓冲流
    bis = new BufferedInputStream(fis);
    bos = new BufferedOutputStream(fos);

    // 3。复制的细节：读取、写入
    byte[] buffer = new byte[10];
    int len;
    while ((len = bis.read(buffer)) != -1) {
      bos.write(buffer, 0, len);
    }
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    //4。资源的关闭  关闭外层内层自动关闭
    if (bos != null) {
      try {
        bos.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
    if (bis != null) {
      try {
        bis.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```



#### 文本文件的复制 * 简写

```java
  // 文本文件的复制
  @Test
  public void test() {
    BufferedReader br = null;
    BufferedWriter bw = null;
    try {
      
      br = new BufferedReader(new FileReader(new File("test.txt")));
      bw = new BufferedWriter(new FileWriter(new File("test3.txt")));

      // method one
      // char[] cbuf = new char[1024];
      // int len;
      // while ((len = br.read(cbuf)) != -1) {
      //   bw.write(cbuf, 0, len);
      // }

      // method two
      String data;
      while ((data = br.readLine()) != null) { // 一行一行读取
        bw.write(data + "\n"); // 默认一行读取不换行继续拼接
        // bw.newLine(); // 或者去掉\n加neeLine
      }
      
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      if (bw != null) {
        try {
          bw.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (br != null) {
        try {
          br.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
  }
```



### 转换流

```java
/*
 * 转换流
 *
 * 1.转换流：属于字符流
 *   InputStreamReader：将一个字节的输入流转换为字符的输入流
 *   OutputStreamWriter：将一个字符的输出流转换为字节的输出流
 *
 * 2.作用：提供字节流与字符流之间的转换
 *
 * 3. 解码：字节、字节数组  --->字符数组、字符串
 *    编码：字符数组、字符串 ---> 字节、字节数组
 *
 *
 * 4.字符集
 */
```



**InputStreamReader的使用，实现字节的输入流到字符的输入流的转换**

```java
  @Test
  public void test() {
    InputStreamReader isr = null;

    try {
      // FileInputStream fis = new FileInputStream("test.txt");
      // InputStreamReader isr = new InputStreamReader(fis,"UTF-8"); // 默认使用系统使用，可自定义
      // InputStreamReader isr = new InputStreamReader(fis); // 当前系统默认 UTF-8

      isr = new InputStreamReader(new FileInputStream("test.txt"));

      char[] cbuf = new char[1024];
      int len;
      while ((len = isr.read(cbuf)) != -1) {
        System.out.println(new String(cbuf, 0, len));
      }

    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      if (isr != null) {
        try {
          isr.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
  }
```

**综合使用InputStreamReader和OutputStreamWriter**

```java
@Test
public void test2() {
  InputStreamReader isr = null;
  OutputStreamWriter osw = null;
  try {
    isr = new InputStreamReader(new FileInputStream(new File("test.txt")), "UTF-8");
    osw = new OutputStreamWriter(new FileOutputStream(new File("test_gbk.txt")), "gbk");

    char[] cbuf = new char[1024];
    int len;
    while ((len = isr.read(cbuf)) != -1) {
      osw.write(cbuf, 0, len);
    }
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    if (isr != null) {
      try {
        isr.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
    if (osw != null) {
      try {
        osw.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```



## 标准输入、输出流

p603 - 619

# ---- 网络编程 ----

## InetAddress

```java
      InetAddress inet = Inet4Address.getByName("192.168.10.14");
      System.out.println(inet); // /192.168.10.14

      InetAddress inet2 = Inet4Address.getByName("www.goole.com");
      System.out.println(inet2); // www.goole.com/217.160.0.201  域名+IP
      System.out.println(inet2.getHostName()); // www.goole.com
      System.out.println(inet2.getHostAddress()); // 217.160.0.201

      InetAddress inet3 = Inet4Address.getLocalHost();
      System.out.println(inet3); // lalaladeMacBook-Air.local/127.0.0.1
```



## TCP

### 栗子一

**客户端发送信息给服务端，服务端将数据显示在控制台**

client

```java
/*
 * 栗子：客户端发送信息给服务端，服务端将数据显示在控制台
 */
public class TcpTest {
  @Test
  public void client() {
    Socket socket = null;
    OutputStream os = null;
    try {
      socket = new Socket(InetAddress.getByName("127.0.0.1"), 8081); // 设置socket端口
      os = socket.getOutputStream();
      os.write("你好，我是客户端迪丽热巴".getBytes()); // 发送
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      if (os != null) {
        try {
          os.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (socket != null) {
        try {
          socket.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
  }
```

server

```java
  @Test
  public void server() {
    ServerSocket ss = null;
    Socket socket = null;
    InputStream is = null;
    ByteArrayOutputStream baos = null;
    try {
      ss = new ServerSocket(8081);
      socket = ss.accept();
      is = socket.getInputStream();

      // 可能会有乱码 当文件超出new byte时会乱码
      // byte[] buffer = new byte[1024];
      // int len;
      // while ((len = is.read(buffer)) != -1) {
      //   String str = new String(buffer, 0, len);
      //   System.out.print(str);
      // }

      // 不会出现乱码
      baos = new ByteArrayOutputStream();
      byte[] buffer = new byte[1024];
      int len;
      while ((len = is.read(buffer)) != -1) {
        baos.write(buffer, 0, len);
      }

      System.out.println("收到了来自以下端口的消息：" + socket.getInetAddress().getHostName());
      System.out.println("消息内容为：" + baos.toString());

    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      if (baos != null) {
        try {
          baos.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (is != null) {
        try {
          is.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (socket != null) {
        try {
          socket.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (ss != null) {
        try {
          ss.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
  }
```



### 栗子二

**客户端发送文件给服务端，服务端将文件保存在本地并返回"发送成功"给客户端后关闭相应的链接**

client

```java
  @Test
  public void client() {
    Socket socket = null;
    OutputStream os = null;
    BufferedInputStream bis = null;
    InputStream is = null;
    ByteArrayOutputStream baos = null;
    try {
      // 1.造socket
      socket = new Socket(InetAddress.getByName("127.0.0.1"), 8088);
      // 2.获取输出流
      os = socket.getOutputStream();
      // 3.获取输入流，读取文件数据
      bis = new BufferedInputStream(new FileInputStream("src/com/java/img/爱情与友情3.jpg"));
      // 4.读写过程
      byte[] buffer = new byte[1024];
      int len;
      while ((len = bis.read(buffer)) != -1) {
        os.write(buffer, 0, len);
      }
      socket.shutdownOutput(); // 关闭数据输出，告诉服务器端已经传输完毕，否则服务端一直卡在接收状态
      // 5.接收服务器端反馈
      is = socket.getInputStream();
      baos = new ByteArrayOutputStream();
      byte[] receiveBuffer = new byte[1024];
      int receiveLen;
      while ((receiveLen = is.read(receiveBuffer)) != -1) {
        baos.write(receiveBuffer, 0, receiveLen);
      }
      System.out.println(baos);

    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 6  .资源关闭
      if (bis != null) {
        try {
          bis.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (os != null) {
        try {
          os.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (socket != null) {
        try {
          socket.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (baos != null) {
        try {
          baos.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (is != null) {
        try {
          is.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
  }
```

server

```java
  @Test
  public void server() {
    ServerSocket ss = null;
    Socket socket = null;
    InputStream is = null;
    BufferedOutputStream bos = null;
    try {
      ss = new ServerSocket(8088);

      socket = ss.accept();

      is = socket.getInputStream();

      bos = new BufferedOutputStream(new FileOutputStream("爱情与友情.jpg"));

      byte[] buffer = new byte[1024];
      int len;
      while ((len = is.read(buffer)) != -1) {
        bos.write(buffer, 0, len);
      }

      // 服务端给予的反馈
      OutputStream os = socket.getOutputStream();
      os.write("commit successfully!!!".getBytes());

    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      if (bos != null) {
        try {
          bos.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (is != null) {
        try {
          is.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (socket != null) {
        try {
          socket.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
      if (ss != null) {
        try {
          ss.close();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
  }
```



## UDP

sender

```java
@Test // UDP只管发，TCP需要握手
public void sendr() throws IOException {
  // 声明数据报
  DatagramSocket socket = new DatagramSocket(); // 数据报socket

  // 封装数据报
  String str = "I'm UDP";
  byte[] data = str.getBytes();
  InetAddress inet = InetAddress.getLocalHost();
  DatagramPacket packet = new DatagramPacket(data, 0, data.length, inet, 8083); // 接收数据报的数据报包

  // 发送数据报
  socket.send(packet);

  // 关闭发送资源
  socket.close();
}
```

receiver

```java
@Test
public void receiver() throws IOException {
  // 声明数据报
  DatagramSocket socket = new DatagramSocket(8083); // 指明端口号

  // 接收数据
  byte[] buffer = new byte[1024];
  DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);
  socket.receive(packet);

  // 输出数据
  System.out.println(new String(packet.getData(), 0, packet.getLength()));
}
```



## URL

```java
try {
  URL url = new URL("https://www.bilibili.com/video/BV1Kb411W75N?p=629&spm_id_from=pageDriver");

  System.out.println(url.getProtocol()); // protocol协议 https
  System.out.println(url.getHost()); // 主机名 www.bilibili.com
  System.out.println(url.getPort()); // 端口号 -1
  System.out.println(url.getPath()); // 文件路径 /video/BV1Kb411W75N
  System.out.println(url.getFile()); // 文件名 /video/BV1Kb411W75N?p=629&spm_id_from=pageDriver
  System.out.println(url.getQuery()); // 查询名

} catch (MalformedURLException e) {
  e.printStackTrace();
}
```

**下载**

```java
HttpURLConnection urlConnection = null;
InputStream is = null;
FileOutputStream fos = null;
try {
  URL url = new URL("https://www.xxx.com/xxx.jpg");

  // 获取连接
  urlConnection = (HttpURLConnection) url.openConnection();
  urlConnection.connect();

  is = urlConnection.getInputStream();
  fos = new FileOutputStream("xxx.jpg");

  byte[] buffer = new byte[1024];
  int len;
  while ((len = is.read(buffer)) != -1) {
    fos.write(buffer, 0, len);
  }

  System.out.println("下载完成");

} catch (IOException e) {
  e.printStackTrace();
} finally {
  if (fos != null) {
    try {
      fos.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
  if (is != null) {
    try {
      is.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
  if (urlConnection != null) {
    urlConnection.disconnect();
  }
}
```



# ---- 反射 ----

## ⭐️ 获取Class实例

**反射之前对Person类的操作**

```java
  @Test
  public void t() {
    // 1.创建Person类对象
    Person p1 = new Person("胖迪", 18);
    // 2.通过对象调用内部的属性、方法
    p1.age = 14;
    System.out.println(p1.toString());
    p1.show();

    // 外部不可以调用内部私有结构

  }
```

**反射之后对Person类的操作**

```java
// 有get就是有set，有fields就有filed；举一反三，直接略过  
@Test
  public void t2() throws Exception {
    // 1.创建Person类对象
    Class<Person> personClass = Person.class;
    Constructor<Person> constructor = personClass.getConstructor(String.class, int.class);
    Person p1 = constructor.newInstance("迪丽热巴", 25);
    System.out.println(p1);
    // 2.调用内部的属性、方法
    // 属性
    Field age = personClass.getDeclaredField("age"); // 拿到Person里的age变量
    age.set(p1, 99); // 把p1里的age改成10
    System.out.println(p1);
    // 方法
    Method show = personClass.getDeclaredMethod("show");
    show.invoke(p1); // 调用p1里的show方法

    // 外部可以调用内部私有结构
    // 私有构造器
    Constructor<Person> constructor2 = personClass.getDeclaredConstructor(String.class);
    constructor2.setAccessible(true);
    Person p2 = constructor2.newInstance("貂蝉");
    System.out.println(p2);
    // 私有属性
    Field name = personClass.getDeclaredField("name");
    name.setAccessible(true);
    name.set(p2, "GieGie~");
    System.out.println(p2);
    // 私有方法
    Method showNation = personClass.getDeclaredMethod("showNation", String.class);
    showNation.setAccessible(true);
    // showNation.invoke(p2, "China"); // 来自国家：China （invoke调用的意思）
    String nation = (String) showNation.invoke(p2, "China");
    System.out.println(nation); // China return返回值

  }
```



### 读取配置文件

Properties

```java
  @Test
  public void t() throws Exception {
    Properties props = new Properties();

    // 方式一
    props.load(new InputStreamReader(new FileInputStream("jdbc.properties"), StandardCharsets.UTF_8));

    // 方式二
    // ClassLoader classLoader = r3_properties.class.getClassLoader(); // r3_properties为当前文件名
    // props.load(classLoader.getResourceAsStream("jdbc1.properties")); // 这里默认src下

    String name = props.getProperty("name");
    String age = props.getProperty("age");
    System.out.println(name + age);
  }
```



## 创建运行时类的对象

### 通过反射创建运行时类的对象

```java
  @Test
  public void t() throws InstantiationException, IllegalAccessException {
    Class<Person> personClass = Person.class;
    Person obj = personClass.newInstance(); // 默认调用空参构造器
    System.out.println(obj);
  }
```



### 反射的动态性

```java
@Test
public void t2() {
  int num = new Random().nextInt(3); // 0 1 2
  String classPath = "";
  switch (num) {
    case 0:
      classPath = "java.util.Date";
      break;
    case 1:
      classPath = "java.lang.Object";
      break;
    case 2:
      classPath = "com.java.reflection.Person";
      break;
  }

  try {
    Object instance = getInstance(classPath);
    System.out.println(instance);
  } catch (Exception e) {
    e.printStackTrace();
  }

}
```

```java
/*
 * 创建一个指定类的对象；classPath为指定类的全类名
 */
public Object getInstance(String classPath) throws Exception {
  Class<?> aClass = Class.forName(classPath);
  return aClass.newInstance();
}
```



## 获取运行时类的指定结构

### 获取运行时类的属性

```java
/*
 * 获取Person类的 权限修饰符 数据类型 变量名
 */
@Test
public void t() {
  Class<Person> clazz = Person.class;
  Field[] declaredFields = clazz.getDeclaredFields();
  for (Field item : declaredFields) {
    // 1.权限修饰符
    int modifiers = item.getModifiers();
    System.out.println("modifiers: " + Modifier.toString(modifiers));
    // 2.数据类型
    Class<?> type = item.getType();
    System.out.println("type: " + type.getName());
    // 3.变量名
    String name = item.getName();
    System.out.println("variable: " + name);
  }
}
```



### 获取运行时类的方法结构

```java
  @Test
  public void t() {
    Class<Person> clazz = Person.class;

    // getMethods：获取运行时类所有public权限方法
    // Method[] methods = clazz.getMethods();
    // for (Method item : methods) {
    //   System.out.println(item);
    // }

    // getDeclaredMethods：获取运行类所有声明的方法
    Method[] declaredMethods = clazz.getDeclaredMethods();
    for (Method item : declaredMethods) {
      System.out.println(item);
    }

    // 构造器。。。
    // clazz.getConstructor()
    // clazz.getDeclaredConstructor()
  }
```



### 获取运行时类的方法 内部 结构

```java
    /*
    @Xxxx
    权限修饰符  返回值类型  方法名(参数类型1 形参名1,...) throws XxxException{}
     */
    @Test
    public void test2(){
        Class clazz = Person.class;
        Method[] declaredMethods = clazz.getDeclaredMethods();
        for(Method m : declaredMethods){
            //1.获取方法声明的注解
            Annotation[] annos = m.getAnnotations();
            for(Annotation a : annos){
                System.out.println(a);
            }

            //2.权限修饰符
            System.out.print(Modifier.toString(m.getModifiers()) + "\t");

            //3.返回值类型
            System.out.print(m.getReturnType().getName() + "\t");

            //4.方法名
            System.out.print(m.getName());
            System.out.print("(");
            //5.形参列表
            Class[] parameterTypes = m.getParameterTypes();
            if(!(parameterTypes == null && parameterTypes.length == 0)){
                for(int i = 0;i < parameterTypes.length;i++){

                    if(i == parameterTypes.length - 1){
                        System.out.print(parameterTypes[i].getName() + " args_" + i);
                        break;
                    }

                    System.out.print(parameterTypes[i].getName() + " args_" + i + ",");
                }
            }

            System.out.print(")");

            //6.抛出的异常
            Class[] exceptionTypes = m.getExceptionTypes();
            if(exceptionTypes.length > 0){
                System.out.print("throws ");
                for(int i = 0;i < exceptionTypes.length;i++){
                    if(i == exceptionTypes.length - 1){
                        System.out.print(exceptionTypes[i].getName());
                        break;
                    }

                    System.out.print(exceptionTypes[i].getName() + ",");
                }
            }


            System.out.println();
        }

    }
```



### 获取运行时类的带泛型的父类

```java
@Test
public void test3(){
    Class clazz = Person.class;

    Type genericSuperclass = clazz.getGenericSuperclass();
    System.out.println(genericSuperclass);
}
```



### 获取运行时类实现的接口

```java
@Test
public void test5(){
    Class clazz = Person.class;

    Class[] interfaces = clazz.getInterfaces();
    for(Class c : interfaces){
        System.out.println(c);
    }

    System.out.println();
    //获取运行时类的父类实现的接口
    Class[] interfaces1 = clazz.getSuperclass().getInterfaces();
    for(Class c : interfaces1){
        System.out.println(c);
    }

}
```



### 获取运行时类所在的包

```java
@Test
public void test6(){
    Class clazz = Person.class;

    Package pack = clazz.getPackage();
    System.out.println(pack);
}
```



### 获取运行时类声明的注解

```java
@Test
public void test7(){
    Class clazz = Person.class;

    Annotation[] annotations = clazz.getAnnotations();
    for(Annotation annos : annotations){
        System.out.println(annos);
    }
}
```



## 调用运行时类的指定结构

***看 获取Class实例***



## 静态代理

```java
public class p1 {
  public static void main(String[] args) {
    // 创建被代理类对象
    NikeClothFactory nike = new NikeClothFactory();
    // 创建代理类的对象
    ProxyClothFactory proxyClothFactory = new ProxyClothFactory(nike);

    proxyClothFactory.produceCloth();
  }
}

interface ClothFactory {
  void produceCloth();
}

// 代理类
class ProxyClothFactory implements ClothFactory {
  private ClothFactory fatcory; // 用被代理类对象进行实例化

  public ProxyClothFactory(ClothFactory fatcory) {
    this.fatcory = fatcory;
  }

  @Override
  public void produceCloth() {
    System.out.println("代理工厂准备开始");
    fatcory.produceCloth();
    System.out.println("代理工厂准备结束");
  }
}

// 被代理类
class NikeClothFactory implements ClothFactory {
  @Override
  public void produceCloth() {
    System.out.println("Nike工厂倒闭前生产的最后一批衣服");
  }
}
```



## 动态代理

```java
public class p2 {
  public static void main(String[] args) {
    SuperMan superMan = new SuperMan();
    //proxyInstance:代理类的对象
    Human proxyInstance = (Human) ProxyFactory.getProxyInstance(superMan);
    //当通过代理类对象调用方法时，会自动的调用被代理类中同名的方法
    String belief = proxyInstance.getBelief();
    System.out.println(belief);
    proxyInstance.eat("四川麻辣烫");

    System.out.println("*****************************");

    NikeClothFactory nikeClothFactory = new NikeClothFactory();

    ClothFactory proxyClothFactory = (ClothFactory) ProxyFactory.getProxyInstance(nikeClothFactory);

    proxyClothFactory.produceCloth();

  }
}

interface Human {
  String getBelief();

  void eat(String food);
}

// 被代理类
class SuperMan implements Human {
  @Override
  public String getBelief() {
    return "I believe i can fly?";
  }

  @Override
  public void eat(String food) {
    System.out.println("吃：" + food);
  }
}

/*
 * 要想实现动态代理，需要解决的问题？
 * 问题一：如何根据加载到内存中的被代理类，动态的创建一个代理类及其对象。
 * 当通过代理类的对象调用方法a时，如何动态的去调用被代理类中的同名方法a。
 */
class ProxyFactory {
  //调用此方法，返回一个代理类的对象。解决问题一
  public static Object getProxyInstance(Object obj) {//obj:被代理类的对象
    MyInvocationHandler handler = new MyInvocationHandler();

    handler.bind(obj);

    return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), handler);
    // Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), handler);
  }

}

class MyInvocationHandler implements InvocationHandler {

  private Object obj;//需要使用被代理类的对象进行赋值

  public void bind(Object obj) {
    this.obj = obj;
  }

  //当我们通过代理类的对象，调用方法a时，就会自动的调用如下的方法：invoke()
  //将被代理类要执行的方法a的功能就声明在invoke()中
  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

    HumanUtil util = new HumanUtil();
    util.method1();

    //method:即为代理类对象调用的方法，此方法也就作为了被代理类对象要调用的方法
    //obj:被代理类的对象
    Object returnValue = method.invoke(obj, args);

    util.method2();

    //上述方法的返回值就作为当前类中的invoke()的返回值。
    return returnValue;

  }
}

class HumanUtil {

  public void method1() {
    System.out.println("====================通用方法一====================");

  }

  public void method2() {
    System.out.println("====================通用方法二====================");
  }

}
```



# ---- Java8的其他新特性 ----

## Lambda表达式

> 本质：作为函数式接口的实例

### 箭头函数、方法引用、构造器

```java
    // 原生
    Runnable r1 = new Runnable() {
      @Override
      public void run() {
        System.out.println("你好");
      }
    };
    r1.run();

    // 抄袭的箭头函数  （无参写法）
    Runnable r2 = () -> System.out.println("你也好");
    r2.run();
```

```java
    // 原生
    Comparator<Integer> comparator = new Comparator<>() {
      @Override
      public int compare(Integer o1, Integer o2) {
        return Integer.compare(o1, o2);
      }
    };
    System.out.println(comparator.compare(1, 2)); // 比较大小，前小-1前大1一样0

    // 箭头函数  （带参写法）（声明了泛型形参不需要写类型 - 类型推断）
    Comparator<Integer> comparator1 = (o1, o2) -> Integer.compare(o1, o2);
    System.out.println(comparator1.compare(1, 2));
    
    // 方法引用  类或对象 :: 方法名
    Comparator<Integer> comparator2 = Integer :: compare;
    System.out.println(comparator2.compare(3, 2));
```

```java
    // 方法引用 构造器
    // Supplier<Person> person = () -> new Person(); // 空参
    // Supplier<Person> person = () -> new Person("胖迪",18); // 带参

    Supplier<Person> person = Person::new; // 只要调用空参
    
    System.out.println(person.get());
```



## 函数式（Functional）接口

```java
@FunctionalInterface // 检测是否只定义了一个抽象方法
public interface i {
  void method();
  // void method2(); // 定义两个报错
}
```

> java内置的四大核心函数式接口
>
> > 消费型接口 Consumer<T> void accept(T t) ：只进不出
> >
> > 供给型接口 Supperlier<T> T get() ：只出不进
> >
> > 函数型接口 Function<T, R> R apply(T t) ：即进即出
> >
> > 断定型接口 Predicate<T> boolean test(T t) ：判断

```java
/*
 * 消费型接口
 */
@Test
public void t() {
  // 原生
  happyTime(1000, new Consumer<Double>() {
    @Override
    public void accept(Double aDouble) {
      System.out.println("上海路天上人间今日消费：" + aDouble);
    }
  });

  // 箭头函数
  happyTime(999.99, xingCan -> System.out.println("上海路天上人间今日消费：" + xingCan));
}

public void happyTime(double money, Consumer<Double> consumer) {
  consumer.accept(money);
}
```

```java
/*
 * 断定型接口
 */
@Test
public void t2() {
  List<String> list = Arrays.asList("北京", "天津", "南京");
  // 原生
  List<String> result = filterString(list, new Predicate<String>() {
    @Override
    public boolean test(String s) {
      return s.contains("京"); // 拿到包含 京 的字符串
    }
  });
  System.out.println(result);
  // 箭头函数
  List<String> result1 = filterString(list, s -> s.contains("京"));
  System.out.println(result1);
}

// 根据给定的规则，过滤集合中的字符串。此规则由Predicate的方法决定
public List<String> filterString(List<String> list, Predicate<String> predicate) {
  ArrayList<String> filterList = new ArrayList<>();
  for (String item : list) {
    if (predicate.test(item)) {
      filterList.add(item);
    }
  }
  return filterList;
}
```



## Stream API

p677 - 684



## Optional类

p685 - 686
