# 基础篇

## 基本功

##### 面向对象的特征

面向对象的三个基本特征是：封装、继承、多态。

- 封装

  封装就是将自己的数据属性抽象起来，让可信对象操作，对不可信的进行信息的隐藏，以保证信息的安全性。

  - 封装的好处

    - 私有化成员变量，使用private关键字修饰

    - 提供公有的get和set方法，在方法体中进行合理值的判断，使用public关键字修饰

  - 访问修饰符作用范围表

    |      作用域      | 当前类 | 同package | 子孙类 | 其他package |
    | :--------------: | :----: | :-------: | :----: | :---------: |
    |      public      |   ✓    |     ✓     |   ✓    |      ✓      |
    |    protected     |   ✓    |     ✓     |   ✓    |      x      |
    | friendly/default |   ✓    |     ✓     |   x    |      x      |
    |     private      |   ✓    |     x     |   x    |      x      |

- 继承

  继承就是子类继承父类特征和行为，使得子类对象（new关键字）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。

  - 继承的好处

    - 减少代码的冗余，提高代码的复用性

    - 便于功能的扩展

  - Java继承的特点

    - Java只允许单继承，即子类只能继承一个父类

    - 子类拥有父类的非private属性和方法

    - 子类可以拥有自己的属性和方法

    - 子类可以重写覆盖父类的方法

    - 构造方法是不能被继承的，但可以被子类调用

    - 调用父类构造方法的super语句必须写在子类构造方法的第一行，否则编译出错

  - 缺点

    - 打破封装，向子类暴露了细节，安全性不高

    - 父类更改之后子类也要同时更改

    - 继承是一种强耦合关系

- 多态

  是同一行为对不同类的对象产生不同的响应

  生活中，比如跑的动作，小猫、小狗和大象，跑起来是不一样的。再比如飞的动作，昆虫、鸟类和飞机，飞起来也是不一样的。可见，同一行为，通过不同的事物，可以体现出来的不同的形态。多态，描述的就是这样的状态。

  - 多态存在的三个必要条件

    - 要有继承

    - 要有重写

    - 父类引用指向子类对象

  - 多态的体现

    ```java
    父类类型 变量名 = new 子类对象;
    变量名.方法名();
    ```

##### final, finally, finalize的区别

这三者除了名字长得有点像，就是卡巴斯基、巴基斯坦和小丑巴基的关系，有个鸡巴关系！

- final

  final是很基础的关键字，final可以用来修饰类、方法、变量

  * final修饰的class，代表不可以继承扩展
  * final修饰的方法，代表不可以重写
  * final修饰的变量，代表不可以修改

  对于引用类型来说，只能说不能重新赋值。也就是不能改变引用地址，但是作为引用类型，它内部所包含的内容如果不是final则可以随意修改。例如：

  ```java
  final List<String> arrayList = new ArrayList<>();
  arrayList.add("AAA");
  arrayList.add("BBB")
  ```

  final声明的变量需要给它赋初始值。如果不想直接给它赋值，需要在构造函数里赋值，例如：

  ```java
  final int num;
  final int num2 = 666;
  public Test() {
      num = 666;
  }
  ```

- finally

  是Java保证重点代码一定要被执行的一种机制。最常用的地方：通过try-catch-finally来进行类似资源释放、保证解锁等动作。例如：

  ```java
  FileInputStream inputStream = null;
  try {
      inputStream = new FileInputStream(new File("test"));
      System.out.println(inputStream.read());
  } catch (IOException e) {
      throw new RuntimeException(e.getMessage(), e);
  } finally {
      if (inputStream != null) {
          try {
              inputStream.close();
          } catch (IOException e) {
              throw new RuntimeException(e.getMessage(), e);
          }
      }
  }
  ```

  什么情况下finally不执行，能导致程序停止的操作，finally都没办法执行。例如：

  ```java
  try {
      System.exit(1);
  } finally {
      System.out.println("程序都死，我还执行个毛线");
  }
  ```

- finalize

  在GC要回收某个对象时，让这个对象有底气地大喊一声：报告，我还能再抢救一下！finalize也就成为了GC回收的阻碍者，也就会导致这个对象经过多个垃圾回收周期才能被回收。用得少，不讨论。Java9已经被弃用。

##### Integer、new Integer()和int的比较

* 两个new Integer()变量比较，永远为false。

  因为new生成的是两个对象，其内存地址不同。

  ```java
  Integer i = new Integer(100);
  Integer j = new Integer(100);
  System.out.print(i == j); // false
  ```

- Integer变量和new Integer()变量比较，永远为false。

  因为Integer变量指向的是Java常量池中的对象，而new Integer的变量指向堆中新建的对象，两者在内存中的地址不同。

  ```java
  Integer i = new Integer(100);
  Integer j = 100;
  int k = 100
  System.out.print(i == j); // false
  System.out.print(j == k); // true
  ```

- 两个Integer变量比较，如果两个变量的值在区间-128到127之间，则比较结果为true，如果两个变量的值不在此区间，则比较结果为false。

  ```java
  Integer i = 100;
  Integer j = 100;
  System.out.print(i == j); // true
  
  Integer i = 128;
  Integer j = 128;
  System.out.print(i == j); // false
  ```

  分析，Integer i = 100在编译时，会翻译成为Integer i = Integer.valueOf(100)，而Java对Integer类型的valueOf的定义如下：

  ```java
  public static Integer valueOf(int i) {
      assert IntegerCashe.high >= 127;
      if (i >= IntegerCache.low && i <= IntegerCache.high) {
          return IntegerCache.cache[i + (-IntegerCache.low)];
      }
      return new Integer(i);
  }
  ```

  Java对于-128到127之间的数，会进行缓存。

- int变量与Integer、new Integer()比较时，只要两个值是相等，则为true。

  因为包装类Integer和基本数据类型int比较时，Java会自动拆包装为int，然后进行比较，实际上就变为两个int变量的比较。

  ```java
  Integer i = new Integer(100);
  int j = 100;
  System.out.print(i == j); // true 相当于 i.intValue() == j
  ```

##### String、StringBuffer、StringBuilder区别

- String (字符串常量)

  * String是类不是基本数据类型

  * String是final类，不能被继承。是不可变对象，一旦创建，就不能修改它的值。

- StringBuffer (字符串变量)

  * 一个类似于String的字符串缓冲区，对它的修改不会像String那样重新创建对象。

  * 使用append()方法修改StringBuffer的值，使用toString()方法转换为字符串。

  * 线程安全，建议多线程使用。

  * 注意：不能通过赋值符号对他进行赋值。

- StringBuilder (字符串变量)

  * StringBuilder是jdk1.5后用来替代StringBuffer的一个类，大多数时候可以替换StringBuffer。和StringBuffer的区别在于StringBuilder是一个单线程使用的类，不执行线程同步所以比StringBuffer速度快，效率高。

  * 线程非安全的，建议单线程使用。
  * 注意：不能通过赋值符号对他进行赋值。

##### 重载和重写的区别





