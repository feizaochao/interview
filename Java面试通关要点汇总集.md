# 基础篇

## 基本功

#### 面向对象的特征

面向对象的三个基本特征是：封装、继承、多态。

- **封装**

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

- **继承**

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

- **多态**

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

#### final, finally, finalize的区别

这三者除了名字长得有点像，就是卡巴斯基、巴基斯坦和小丑巴基的关系，有个鸡巴关系！

- **final**

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

- **finally**

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

- **finalize**

  在GC要回收某个对象时，让这个对象有底气地大喊一声：报告，我还能再抢救一下！finalize也就成为了GC回收的阻碍者，也就会导致这个对象经过多个垃圾回收周期才能被回收。用得少，不讨论。Java9已经被弃用。

#### == 和 equals 的区别

在Java中，== 和 equals的区别，是面试必问的问题，然后只有很少的面试者才能完全回答正确。

常见的错误回答就是：== 基础类型对比的是值是否相同，引用类型对比的是引用是否相同；而equals则是比较的值是否相同。

至于为什么说它是错的，看完本文对 == 和 equals 的解读，就知道了。

- **== 解读**

  对于基本类型和引用类型 == 的作用效果是不同的，如下所示：

  - 基本类型：比较的是值是否相同
  - 引用类型：比较的是引用是否相同

- **equals解读**

  equals本质上就是==，只不过String和Integer等重写了equals方法，把他变成了值比较。

#### Integer、new Integer()和int的比较

* **两个new Integer()变量比较，永远为false。**

  因为new生成的是两个对象，其内存地址不同。

  ```java
  Integer i = new Integer(100);
  Integer j = new Integer(100);
  System.out.print(i == j); // false
  ```

- **Integer变量和new Integer()变量比较，永远为false。**

  因为Integer变量指向的是Java常量池中的对象，而new Integer的变量指向堆中新建的对象，两者在内存中的地址不同。

  ```java
  Integer i = new Integer(100);
  Integer j = 100;
  int k = 100
  System.out.print(i == j); // false
  System.out.print(j == k); // true
  ```

- **两个Integer变量比较，如果两个变量的值在区间-128到127之间，则比较结果为true，如果两个变量的值不在此区间，则比较结果为false。**

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

- **int变量与Integer、new Integer()比较时，只要两个值是相等，则为true。**

  因为包装类Integer和基本数据类型int比较时，Java会自动拆包装为int，然后进行比较，实际上就变为两个int变量的比较。

  ```java
  Integer i = new Integer(100);
  int j = 100;
  System.out.print(i == j); // true 相当于 i.intValue() == j
  ```

#### String、StringBuffer、StringBuilder区别

- **String (字符串常量)**
* String是类不是基本数据类型
  
* String是final类，不能被继承。是不可变对象，一旦创建，就不能修改它的值。
  
- **StringBuffer (字符串变量)**
* 一个类似于String的字符串缓冲区，对它的修改不会像String那样重新创建对象。
  
* 使用append()方法修改StringBuffer的值，使用toString()方法转换为字符串。
  
* 线程安全，建议多线程使用。
  
* 注意：不能通过赋值符号对他进行赋值。
  
- **StringBuilder (字符串变量)**
* StringBuilder是jdk1.5后用来替代StringBuffer的一个类，大多数时候可以替换StringBuffer。和StringBuffer的区别在于StringBuilder是一个单线程使用的类，不执行线程同步所以比StringBuffer速度快，效率高。
  
* 线程非安全的，建议单线程使用。
  * 注意：不能通过赋值符号对他进行赋值。

#### 重载和重写的区别

|                       重载                       |                             重写                             |
| :----------------------------------------------: | :----------------------------------------------------------: |
|                 发生在同一个类中                 |                     发生在子类与父类之间                     |
| 方法名相同，参数列表不同（参数类型、个数、顺序） |    方法名、参数类型、参数个数、参数顺序与父类，返回值相同    |
|      可相同/不相同（返回类型、访问修饰符）       | 访问修饰符大于或等于父类方法，但父类方法访问修饰符是private时，子类不能重写该方法 |

从虚拟机角度来看重载和重写是怎么回事

- **重载**

  首先来看一段代码：

  ```java
  public abstract class Animal {
      
  }
  class Dog extends Animal {
      
  }
  class Lion extends Animal {
      
  }
  
  class Test {
      public void run(Animal animal) {
          System.out.println("动物跑啊跑");
      }
      public void run(Dog dog) {
          System.out.println("小狗跑啊跑");
      }
      public void run(Lion lion) {
          System.out.println("狮子跑啊跑");
      }
      // 测试
      public static void main(String[] args) {
          Animal dog = new Dog();
          Animal lion = new Lion();
          Test test = new Test();
          test.run(dog);
          test.run(lion);
      }
  }
  ```

  运行结果：

  ```java
  动物跑啊跑
  动物跑啊跑
  ```

  为什么会选择这个方法进行重载呢？虚拟机是如何选择的呢？

  先看一行代码：

  ```java
  Animal dog = new Dog();
  ```

  对于这一行代码，我们把Animal称之为变量dog的**静态类型**，而后面的Dog称为变量的**实际类型**。

  所谓静态类型，也就是说，在代码的编译期间就可以确定了，在编译期间就可以判断dog的静态类型是啥了。但在编译期间无法知道变量dog的实际类型是什么。

  对于静态类型相同，但实际类型不同的变量，虚拟机在重载的时候是根据参数的静态类型而不是实际类型作为判断选择的。并且静态类型在编译期间就是已知的，这也代表在编译阶段，就已经决定好了选择哪一个重载方法。

  由于dog和lion的静态类型都是Animal，所以选择了run(Animal animal)这个方法。

  不过需要注意的是，有时候是可以有多个重载版本的，也就是说，重载版本并非是唯一的。例如：

  ```java
  public class Test {
      public static void say(Object arg) {
          System.out.println("hello Object");
      }
      public static void say(int arg) {
          System.out.println("hello int");
      }
      public static void say(long arg) {
          System.out.println("hello long");
      }
      public static void say(Character arg) {
          System.out.println("hello Character");
      }
      public static void say(char arg) {
          System.out.println("hello char");
      }
      public static void say(char... arg) {
          System.out.println("hello char...");
      }
      public static void say(Serializable arg) {
          System.out.println("hello Serializable");
      }
      
      // 测试
      public static void main(String[] args) {
          char a = 'a';
          say(a); 
      }
  }
  ```

  运行代码，结果如下：

  ```java
  hello char
  ```

  因为a的静态类型是char，会匹配到say(char arg);

  但是，如果把say(char arg)这个方法注释掉，再运行下，结果如下：

  ```java
  hello int
  ```

   实际上这个时候由于方法中并没有静态类型为char的方法，它会自动进行类型转换，a除了可以是字符，还可以代表数字97。因此会选择int类型进行重载。

  我们继续注释掉say(int arg)这个方法。结果会输出：

  ```java
  hello long
  ```

  这个时候，a进行了两次类型转换，即‘a’ -> 97 -> 97L。所以匹配到了say(long arg)方法。

  实际上。a会按照char -> int -> long -> float -> double的顺序来转换。但并不会转换成byte或者short，因为从char到byte或者short的转换是不安全的。会丢失字节。

  继续注释掉long类型的方法。结果为：

  ```java
  hello Character
  ```

  这时发生了一次自动装箱，a被封装为Character类型。

  继续注释掉Character类型的方法，结果为：

  ```java
  hello Serializable
  ```

  实际上，因为Serializable是Character类实现的一个接口，当自动装箱后发现找不到装箱类，但是找到了装箱类实现了的接口类型，所以再一次发生了自动转型。

  我们继续注释掉Serializable，这个时候输出的结果是：

  ```java
  hello Object
  ```

  这是a装箱后转型为父类了。

  最后注释掉Object方法，这时候输出：

  ```java
  hello char...
  ```

  这个时候a被转换为一个数组元素。

  从上面的例子中，可以看出，元素的静态类型并非一定是固定的，**它在编译期间根据优先级原则进行转换。其实这也是Java语言实现重振的本质。**

- **重写**

  先看代码：

  ```java
  // 定义几个类
  public abstract class Animal {
      public abstract void run();
  }
  class Dog extends Animal {
      @Override
      public void run() {
          System.out.println("小狗跑啊跑");
      }
  }
  class Lion extends Animal {
      @Override
      public void run() {
          System.out.println("狮子跑啊跑");
      }
  }
  class Test {
      // 测试
      public static void main(String[] args) {
          Animal dog = new Dog();
          Animal lion = new Lion();
          dog.run();
          lion.run();
      }
  }
  ```

  运行结果：

  ```java
  小狗跑啊跑
  狮子跑啊跑
  ```

  显然，虚拟机是根据**实际类型**来执行方法的。我们来看看main()方法中的一部分字节码：

  ```java
  // 声明：我只是挑出了一部分关键的字节码
  public static void (java.lang.String[]);
      Code:
      Stack=2, Locals=3, Args_size=1;// 可以不用管这个
      // 下面的是关键
      0：new #16; // 即new Dog
      3: dup
      4: invokespecial #18; // 调用初始化方法
      7: astore_1
      8: new #19 ; // 即new Lion
      11: dup
      12: invokespecial #21; // 调用初始化方法
      15: astore_2
      
      16: aload_1; // 压入栈顶
      17: invokevirtual #22; // 调用run()方法
      20: aload_2; // 压入栈顶
      21: invokevirtual #22; // 调用run()方法
      24: return
  
  ```

  解释一下这段代码：

  0-15行的作用是创建Dog和Lion对象的内存空间，调用Dog，Lion类型的实例构造器。对应代码：

  ```java
  Animal dog = new Dog();
  Animal lion = new Lion();
  ```

  接下来的16-21句是关键部分，16、20两句分分别把刚刚创建的两个对象的引用压到栈顶。17和21是run()方法的调用指令。

  从指令可以看出，这两条方法的调用指令是完全一样的。可是最终执行的目标方法却并不相同。这是为啥？

  实际上：

  invokevirtual方法调用指令在执行的时候是这样的：

  1. 找到栈顶的第一个元素所指向的对象的实际类型，记作C.
  2. 如果类型C中找到run()这个方法，则进行访问权限的检验，如果可以访问，则方法这个方法的直接引用，查找结束；如果这个方法不可以访问，则抛出java.lang.IllegalAccessEror异常。
  3. 如果在该对象中没有找到run()方法，则按照继承关系从下往上对C的各个父类进行第二步的搜索和检验。
  4. 如果都没有找到，则抛出java.lang.AbstractMethodError异常。

  所以虽然指令的调用是相同的，但17行调用run方法时，此时栈顶存放的对象引用是Dog，21行则是Lion。

  这，就是java语言中方法重写的本质。

#### 抽象类和接口的区别

|     比较     |                            抽象类                            |                            接口                            |
| :----------: | :----------------------------------------------------------: | :--------------------------------------------------------: |
|   默认方法   |                  抽象类可以有默认的方法实现                  |              Java8之前，接口不存在方法的实现               |
|   实现方式   | 子类使用extends关键字来继承抽象类，如果子类不是抽象类，子类需要提供抽象类中所声明方法的实现 | 子类使用implements来实现接口，需要提供接口中所有声明的实现 |
|    构造器    |                     抽象类中可以有构造器                     |                         接口中不能                         |
| 和正常类区别 |                      抽象类不能被实例化                      |                   接口则是完全不同的类型                   |
|  访问修饰符  |        抽象方法可以有public，protected和default等修饰        |            接口默认是public，不能使用其他修饰符            |
|    多继承    |                   一个子类只能存在一个父类                   |                  一个子类可以存在多个接口                  |
|  添加新方法  | 向抽象类中添加新方法，可以提供默认的实现，因此可以不修改子类现有的代码 |       如果往接口中添加新方法，则子类中需要实现该方法       |

#### Java动态代理

- **什么是代理**

  举个例子，微商代理，简单地说就是代替厂家卖商品，厂家“委托”代理为其销售商品。厂家对我们来说是不可见的。

  * 优点一：可以隐藏委托类的实现
  * 优点二：可以实现客户与委托类间的解耦，在不修改委托类代码的情况下能够做一些额外的行为。

- **静态代理**

  若代理类在程序运行前就已经存在，那么这种代理方式被称为静态代理，这种情况下的代理类通常都是我们在Java代码中定义的。通常情况下，静态代理的代理类和委托类会实现同一接口或是派生自相同的父类。下面我们用Vendor类代表生产厂家，BusinessAgent类代表微商代理，来介绍静态代理的简单实现，委托类和代理类都实现了Sell接口，Sell接口的定义如下：

  ```java
  // 委托类和代理类都实现了Sell接口
  public interface Sell {
      void sell();
      void add();
  }
  ```

  生产厂家Vendor类的定义如下：

  ```java
  // 生成厂家
  public class Vendor implements Sell {
      @Override
      public void sell() {
          System.out.println("In sell method");
      }
      
      @Override
      public void add() {
          System.out.println("In add method");
      }
  }
  ```

  代理BusinessAgent类的定义如下：

  ```java
  // 代理类
  public class BusinessAgent implements Sell {
      private Sell vendor;
      
      public BusinessAgent(Sell vendor) {
          this.vendor = vendor;
      }
      
      @Override
      public void sell() {
          vendor.sell();
      }
      
      @Override
      public void add() {
          vendor.add();
      }
  }
  ```

  从BusinessAgent类的定义我们可以了解到，静态代理可以通过聚合来实现，让代理类持有一个委托类的引用即可。

  下面我们考虑一下这个需求：给Vendor类增加一个过滤功能，只卖货给大学生。通过静态代理，我们无需修改Vendor类的代码就可以实现，只需在BusinessAgent类中的sell方法中添加一个判断即可，如下所示：

  ```java
  // 代理类
  public class BusinessAgent implements Sell {
      private Sell vendor;
      
      public BusinessAgent(Sell vendor) {
          this.vendor = vendor;
      }
      
      @Override
      public void sell() {
          if (isCollegeStudent()) {
              vendor.sell();
          }
      }
      
      public void add() {
          vendor.add();
      }
      
      private boolean isCollegeStudent() {
          System.out.println("In isCollegeStudent method");
          return true;
      }
  }
  ```

  这对应这我们上面提到的使用代理的第二个优点：可以实现客户与委托类间的解耦，在不修改委托类代码的情况下能够做一些额外的处理。静态代理的局限在于运行前必须编写好代理类。

- **动态代理**

  * 什么是动态代理

    代理类在程序运行时创建的代理方法被称为动态代理。也就是说，这种情况下，代理类并不是在Java代码中定义的，而是在运行时根据我们在Java代码中的“指示”动态生成的。相对于静态代理，动态代理的优势在于可以很方便的对代理类的方法进行统一处理，而不用修改每个代理类的方法。

    现在，我们假设要实现一个这样的需求：在执行委托类中的方法之前输出“before”，在执行完毕后输出“after”。我们还是以上面例子中的Vendor类作为委托类，BusinessAgent类作为代理类来进行介绍。首先我们使用静态代理来实现这一需求，相关代码如下：

    ```java
    // 代理类
    public class BusinessAgent implements Sell {
        private Sell vendor;
        
        public BusinessAgent(Sell vendor) {
            this.vendor = vendor;
        }
        
        public void sell() {
            System.out.println("before");
            vendor.sell();
            System.out.println("after");
        }
        
        public void add() {
            System.out.println("before");
            vendor.add();
            System.out.println("after");
        }
    }
    ```

    假如Sell接口中包含上百个方法呢？这时候使用静态代理就会编写许多冗余代码。通过使用动态代理，我们可以做一个“统一指示”，从而对所有代理类的方法进行统一处理，而不用主意修改每个方法。

  - 使用动态代理

    * InvocationHandler接口

      在使用动态代理时，我们需要定义一个位于代理类与委托类之间的中介类，这个中介类被要求实现InvocationHandler接口，这个接口的定义如下：

      ```java
      // 调用处理程序
      public interface InvocationHandler {
          Object invoke(Object proxy, Method method, Object[] args);
      }
      ```

      从InvocationHandler这个名称我们就可以知道，实现了这个接口的中介类用作”调用处理器“。当我们调用代理类对象的方法时，这个”调用“会转送到invoke方法中，代理类对象作为proxy参数传入，参数method标识了我们具体调用的是代理类的哪个方法，args为这个方法的参数。这样一来，我们对代理类中的所有方法的调用都会变为对invoke的调用，我们就可以在invoke方法中添加统一的处理逻辑（ 也可以根据method参数对不同的代理类方法做不同的处理）。因此我们只需要在中介类的invoke方法实现中输出”before“，然后调用委托类的invoke方法，再输出“after”。

    - 委托类的定义

      动态代理方式下，要求委托类必须实现某个接口，这里我们实现的是Sell接口。委托类Vendor类的定义如下：

      ```java
      // 委托类
      public class Vendor implements Sell {
          public void sell() {
              System.out.println("In sell method");
          }
          
          public void add() {
              System.out.println("In add method");
          }
      }
      ```

    - 中介类

      上面我们提到过，中介类必须实现InvocationHandler接口，作为调用处理器“拦截”对代理类方法的调用。定义如下：

      ```java
      public class DynamicProxy implements InvocationHandler {
          // obj为委托类对象
          private Object obj;
          
          public DynamicProxy(Object obj) {
              this.obj = obj;
          }
          
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
              System.out.println("before");
              Object result = method.invoke(obj, args);
              System.out.println("afer");
              return result;
          }
      }
      ```

    - 动态生成代理类

      ```java
      public class Test {
          public static void main(String[] args) {
              // 创建中介类实例
              DynamicProxy inter = new DynamicProxy(new Vendor());
              // 加上这句将会产生一个$Proxy.class文件，这个文件即为动态生成的代理类文件
              			System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles","true"); 
      
              //获取代理类实例sell 
              Sell sell = (Sell)(Proxy.newProxyInstance(Sell.class.getClassLoader(), new Class[] {Sell.class}, inter)); 
       
              //通过代理类对象调用代理类方法，实际上会转到invoke方法调用 
              sell.sell(); 
              sell.ad();
          }
      }
      ```

      在以上代码中，我们调用Proxy类的newProxyInstance方法来获取一个代理类实例。这个代理类实现了我们指定的接口并且会把方法调用分发到指定的调用处理器。这个方法的声明如下：

      ```java
      public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) throws IllegalArgumentException 
      ```

      方法的三个参数含义分别如下：

      loader：定义了代理类的ClassLoder; interfaces：代理类实现的接口列表 h：调用处理器，也就是我们上面定义的实现了InvocationHandler接口的类实例

      我们运行一下，看看我们的动态代理是否能正常工作。我这里运行后的输出为：

      ```java
      before
      In sell method
      after
      before
      In add method
      after
      ```

      上面我们已经简单提到过动态代理的原理，这里再简单的总结下：首先通过newProxyInstance方法获取代理类实例，而后我们便可以通过这个代理类实例调用代理类的方法，对代理类的方法的调用实际上都会调用中介类(调用处理器)的invoke方法，在invoke方法中我们调用委托类的相应方法，并且可以添加自己的处理逻辑。

#### 序列化和反序列化

- **概念**
  * 序列化：把对象转换为字节序列的过程称为对象的序列化
  * 反序列化：把字节序列恢复为对象的过程称为对象的反序列化

- **为什么序列化**
  * 持久存储时
  * 网络传输时
  * 通过RMI传输对象时

- **实现方式**

  实现Serializable接口。

  * transient修饰的属性，不会被序列化
  * 静态static的属性，不会序列化
  * 实现这个Serializable接口的时候，一定要给这个serialVersionUID赋值
  * 当属性是对象的时候，对象也要实现序列化接口。

- **关于序列化的一些好文**

  何为序列化：[Java Serializable：明明就一个空的接口嘛](https://juejin.im/post/5d0c4bebf265da1bc23f7f1d)

  private方法调用：[readObject，writeObject，readResolve是怎么被调用的](https://blog.csdn.net/u014653197/article/details/78114041)

#### 反射的用途和实现

* **Java反射机制**

  在程序运行时，对于任意一个类，都能够知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性，这种**动态获取信息**以及**动态调用对象的方法**的功能称为Java的反射机制。

  反射机制很重要的一点就是**运行时**，其使得我们可以在程序运行时加载、探索以及使用编译期间完全未知的.class文件。换句话说，Java程序可以加载一个运行时才得知道名称的.class文件，然后获悉其完整构造，并生成其对象实体、或对其fields（变量）设值、或调用其methods（方法）。

* **使用反射获取类的信息**

  首先定义一个FatherClass类，然后定义一个继承自FatherClass类的SonClass类，如下所示：

  ```java
  public class FatherClass {
      public String mFatherName;
      public int mFatherAge;
      
      public void printFatherMsg() {
          
      }
  }
  ```

  ```java
  public class SonClass extends FatherClass {
      private String mSonName;
      protected int mSonAge;
      public String mSonBirthday;
      
      public void printSonMsg() {
          System.out.println("Son Msg - name : " + mSonName + "; age : " + mSonAge);
      }
      
      private void setSonName(String name){
          mSonName = name;
      }
  
      private void setSonAge(int age){
          mSonAge = age;
      }
  
      private int getSonAge(){
          return mSonAge;
      }
  
      private String getSonName(){
          return mSonName;
      }
  }
  ```

  - 获取类的所有变量信息

    ```java
    /**
     * 通过反射获取类的所有变量
     */
    private static void printFields(){
        //1.获取并输出类的名称
        Class mClass = SonClass.class;
        System.out.println("类的名称：" + mClass.getName());
        
        //2.1 获取所有 public 访问权限的变量
        // 包括本类声明的和从父类继承的
        Field[] fields = mClass.getFields();
        
        //2.2 获取所有本类声明的变量（不问访问权限）
        //Field[] fields = mClass.getDeclaredFields();
        
        //3. 遍历变量并输出变量信息
        for (Field field :
                fields) {
            //获取访问权限并输出
            int modifiers = field.getModifiers();
            System.out.print(Modifier.toString(modifiers) + " ");
            //输出变量的类型及变量名
            System.out.println(field.getType().getName()
    		         + " " + field.getName());
        }
    }
    
    ```

    需要注意的是注释2.1中的getFileds()与2.2的getDeclaredFields()之间的区别。

    - 调用getFields()方法，输出sonClass类以及其所继承的父类（包括FatherClass和Object）的public方法。注：Object类中没有成员变量，所以没有输出。

      ```java
      类的名称：obj.SonClass
      public java.lang.String mSonBirthday
      public java.lang.String mFatherName
      public int mFatherAge
      ```

    - 调用getDeclaredFields()，输出SonClass类的所有成员变量，不问访问权限。

      ```java
      类的名称：obj.SonClass
      private java.lang.String mSonName
      protected int mSonAge
      public java.lang.String mSonBirthday
      ```

  - 获取类的所有方法信息

    ```java
    /**
     * 通过反射获取类的所有方法
     */
    private static void printMethods(){
        //1.获取并输出类的名称
        Class mClass = SonClass.class;
        System.out.println("类的名称：" + mClass.getName());
        
        //2.1 获取所有 public 访问权限的方法
        //包括自己声明和从父类继承的
        Method[] mMethods = mClass.getMethods();
        
        //2.2 获取所有本类的的方法（不问访问权限）
        //Method[] mMethods = mClass.getDeclaredMethods();
        
        //3.遍历所有方法
        for (Method method :
                mMethods) {
            //获取并输出方法的访问权限（Modifiers：修饰符）
            int modifiers = method.getModifiers();
            System.out.print(Modifier.toString(modifiers) + " ");
            //获取并输出方法的返回值类型
            Class returnType = method.getReturnType();
            System.out.print(returnType.getName() + " "
                    + method.getName() + "( ");
            //获取并输出方法的所有参数
            Parameter[] parameters = method.getParameters();
            for (Parameter parameter:
                 parameters) {
                System.out.print(parameter.getType().getName()
    		            + " " + parameter.getName() + ",");
            }
            //获取并输出方法抛出的异常
            Class[] exceptionTypes = method.getExceptionTypes();
            if (exceptionTypes.length == 0){
                System.out.println(" )");
            }
            else {
                for (Class c : exceptionTypes) {
                    System.out.println(" ) throws "
                            + c.getName());
                }
            }
        }
    }
    ```

  - 访问或操纵类的私有变量和方法

#### 注解的本质和用途

- **注解的基本原理**

  以前，XML是各大框架的青睐者，它以松耦合的方式完成了框架中几乎所有的配置，但是随着项目越来越庞大，XML的内容也越来越复杂，维护成本变高。

  于是就有人提出来一种标记式高耦合的配置方式，注解。

  - 注解的本质

    java.lang.annotation.Annotation接口中有这么一句话，用来描述注解。

    > The common interface extended by all annotation types
    >
    > 所有的注解类型都继承自这个普通的类（Annotation）

    这句话有点抽象，但却说出了注解的本质。我们看一个JDK内置注解的定义：

    ```java
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.SOURCE)
    public @interface Override {
        
    }
    ```

    这是注解@Override的定义，其实它本质上就是：

    ```java
    public interface Override extends Annotation {
        
    }
    ```

    这是我们@Override注解的定义，你可以看到其中的@Target，@Retention两个注解就是我们所谓的元注解，元注解一般用于指定某个注解的生命周期以及作用目标等信息。
    
    Java中有以下几个元注解：
    
    - @Target：注解的作用目标
    - @Retention：注解的生命周期
    - @Documented：注解是否应当被包含在JavaDoc文档中
    - @Inherited：是否允许子类继承该注解
    
    其中，@Target用于指定被修饰的注解最终可以作用的目标是谁，也就是指明，你的注解到底是用来修饰方法的？修饰类的还是用来修饰字段属性的。
    
    @Target的定义如下：
    
    ```java
    @Documented
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.ANNOTATION_TYPE)
    public @interface Target {
        /**
         * Returns an array of the kinds of elements        * an annotation type can be applied to.
         * @return an array of the kinds of elements 	  * an annotation type can be applied to
         */
        ElementType[] value();
    }
    ```
    
    我们可以通过以下的方式来为这个value传值：
    
    ```java
    @Target(value = {ElementType.FIELD})
    ```
    
    被这个@Target注解修饰的注解将只能作用在成员字段上，不能用于修饰方法或者类。其中，ElementType是一个枚举类型，有以下一些值：
    
    - ElementType.TYPE：允许被修饰的注解作用在类、接口和枚举上
    - ElementType.FIELD：允许作用在属性字段上
    - ElementType.METHOD：允许作用在方法上
    - ElementType.PARAMETER：允许作用在方法参数上
    - ElementType.CONSTRUCTOR：允许作用在构造器上
    - ElementType.LOCAL_VARIABLE：允许作用在本地局部变量上
    - ElementType.ANNOTATION_TYPE：允许作用在注解上
    - ElementType.PACKAGE：允许作用在包上
    
    @Retention用于指定当前注解的生命周期，它的基本定义如下：
    
    ```java
    @Documented
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.ANNOTATION_TYPE)
    public @interface Retention {
        /**
         * Returns the retention policy.
         * @return the retention policy
         */
        RetentionPolicy value();
    }
    ```
    
    同样的它也有一个value属性：
    
    ```java
    @Retention(value = RetentionPolicy.RUNTIME)
    ```
    
    这里的RetentionPolicy依然是一个枚举类型，它有以下几个枚举值可取：
    
    - RetentionPolicy.SOURCE：当前注解编译期可见，不会写入class文件
    - RetentionPolicy.CLASS：类加载阶段丢弃，会写入class文件
    - RetentionPolicy.RUNTIME：永久保存，可以反射获取
    
    @Retention注解指定了被修饰的注解的生命周期，一种是只能在编译期可见，编译后会被丢弃，一种会被编译器编译进class文件中，无论是类或是方法，乃至字段，他们都是有属性表的，而Java虚拟机也定义了几种注解属性表用于存储注解信息，但是这种可见性不能带到方法区，类加载时会予以丢弃，最后一种则是永久存在的可见性。
    
    @Documented注解修饰的注解，当我们执行JavaDoc文档打包时会被保存进doc文档，反之将在打包时丢弃。
    
    @Inherited注解修饰的注解是具有可继承性的，也就说我们的注解修饰了一个类，而该类的子类将自动继承父类的该注解。

- **Java的内置三大注解**

  除了上述四种元注解外，JDK还为我们预定义了另外三种注解，它们是：

  - @Override
  - @Deprecated
  - @SuppressWarnings

  @Override注解想必是大家很熟悉的了，它的定义如下：

  ```java
  @Target(ElementType:METHOD)
  @Retention(RetentionPolicy.SOURCE)
  public @interface Override {
      
  }
  ```

  它没有任何的属性，所以并不能存储任何其他信息。它只能作用于方法之上，编译结束后将被丢弃。

  它就是一种典型的标记式注解，仅被编译器可知，编译器在对Java文件进行编译成字节码的过程中，一旦检测到某个方法上被修饰了该注解，就会去匹对父类中是否具有一个同样方法签名的函数，如果不是，自然不能通过编译。

  @Deprecated的基本定义如下：

  ```java
  @Documented
  @Retention(RetentionPolicy.RUNTIME)
  @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
  public @interface Deprecated {
      
  }
  ```

  依然是一种标记式注解，永久存在，可以修饰所有的类型，作用是标记当前的类或者方法或者字段等已经不再被推荐使用了，可能下一次的JDK版本就会被删除。

  @SuppressWarnings主要用来压制Java的警告。

- **注解与反射**

  从虚拟机的层面看看，注解的本质到底是什么。

  首先，我们自定义一个注解类型：

  ```java
  @Target(value = {ElementType.FIELD, ElementType.METHOD})
  @Retention(value = RetentionPolicy.RUNTIME)
  public @interface Hello {
      String value();
  }
  ```

  这里我们指定了Hello这个注解只能修饰字段和方法，并且该注解永久保存，以便我们反射获取。

  之前我们说过，虚拟机规范定义了一系列和注解相关的属性表，也就是说，无论是字段、方法或是类本身，如果被注解修饰了，就可以被写进字节码文件。属性表有以下几种：

  - RuntimeVisibleAnnotations：运行时可见的注解
  - RuntimeInVisibleAnnotations：运行时不可见的注解
  - RuntimeVisibleParameterAnnotations：运行时可见的方法参数注解
  - RuntimeInVisibleParameterAnnotations：运行时不可见的方法参数注解
  - AnnotationDefault：注解类元素的默认值

  给大家看虚拟机的这几个注解相关的属性表的目的在于，让大家从整体上构建一个基本的印象，注解在字节码文件中是如何存储的。

  所以，对于一个类或者接口来说，Class 类中提供了以下一些方法用于反射注解。

  - getAnnotation：返回指定的注解
  - isAnnotationPresent：判定当前元素是否被指定注解修饰
  - getAnnotations：返回所有的注解
  - getDeclaredAnnotation：返回本元素的指定注解
  - getDeclaredAnnotations：返回本元素的所有注解，不包含父类继承而来的

  方法、字段中相关反射注解的方法基本是类似的，这里不再赘述，我们下面看一个完整的例子。

  首先，设置一个虚拟机启动参数，用于捕获 JDK 动态代理类。

  > -Dsun.misc.ProxyGenerator.saveGeneratedFiles=true

  然后main函数：

  ```java
  public class Test {
      @Hello("hello")
      public static void main(String[] args) throws NoSuchMethodException {
          Class cls = Test.class;
          Method method = cls.getMethod("main", String[].class);
          Hello hello = method.getAnnatation(Hello.class);
      }
  }
  ```

  我们说过，注解本质上是继承了 Annotation 接口的接口，而当你通过反射，也就是我们这里的 getAnnotation 方法去获取一个注解类实例的时候，其实 JDK 是通过动态代理机制生成一个实现我们注解（接口）的代理类。

  我们运行程序后，会看到输出目录里有这么一个代理类，反编译之后是这样的：

  ```java
  public final class $Proxy1 extends Proxy implements Hello {
      public $Proxy1(InvocationHandler invocationHandler) {
          super(invocationHandler);
      }
  }
  ```

  ```java
  public final String valus() {
      try {
          return (String)super.h.invoke(this, m3, null);
      }
      catch(Error_ex) {}
      catch(Throwable throwable) {
          throw new UndeclaredThrowableException(throwable);
      }
  }
  ```

  代理类实现接口 Hello 并重写其所有方法，包括 value 方法以及接口 Hello 从 Annotation 接口继承而来的方法。

  而这个关键的 InvocationHandler 实例是谁？

  AnnotationInvocationHandler 是 JAVA 中专门用于处理注解的 Handler， 这个类的设计也非常有意思。

  ```java
  class AnnotationInvocationHandler implements InvocationHandler, Serializable {
      private static final long serialVersionUID = 6182022883658399397L;
      private final Class<? extends Annotation> type;
      private final Map<String, Object> memberValues;
      private transient volatile Method[] memberMethods = null;
  
      AnnotationInvocationHandler(Class<? extends Annotation> var1, Map<String, Object> var2) {
          Class[] var3 = var1.getInterfaces();
          if (var1.isAnnotation() && var3.length == 1 && var3[0] == Annotation.class) {
              this.type = var1;
              this.memberValues = var2;
          } else {
              throw new AnnotationFormatError("Attempt to create proxy for a non-annotation type.");
          }
      }
      ...
  }
  ```

  这里有一个 memberValues，它是一个 Map 键值对，键是我们注解属性名称，值就是该属性当初被赋上的值。

  ```java
  public Object invoke(Object var1, Method var2, Object[] var3) {
          String var4 = var2.getName();
          Class[] var5 = var2.getParameterTypes();
          if (var4.equals("equals") && var5.length == 1 && var5[0] == Object.class) {
              return this.equalsImpl(var3[0]);
          } else if (var5.length != 0) {
              throw new AssertionError("Too many parameters for an annotation method");
          } else {
              byte var7 = -1;
              switch(var4.hashCode()) {
              case -1776922004:
                  if (var4.equals("toString")) {
                      var7 = 0;
                  }
                  break;
              case 147696667:
                  if (var4.equals("hashCode")) {
                      var7 = 1;
                  }
                  break;
              case 1444986633:
                  if (var4.equals("annotationType")) {
                      var7 = 2;
                  }
              }
  
              switch(var7) {
              case 0:
                  return this.toStringImpl();
              case 1:
                  return this.hashCodeImpl();
              case 2:
                  return this.type;
              default:
                  Object var6 = this.memberValues.get(var4);
                  if (var6 == null) {
                      throw new IncompleteAnnotationException(this.type, var4);
                  } else if (var6 instanceof ExceptionProxy) {
                      throw ((ExceptionProxy)var6).generateException();
                  } else {
                      if (var6.getClass().isArray() && Array.getLength(var6) != 0) {
                          var6 = this.cloneArray(var6);
                      }
  
                      return var6;
                  }
              }
          }
      }
  ```

  而这个 invoke 方法就很有意思了，大家注意看，我们的代理类代理了 Hello 接口中所有的方法，所以对于代理类中任何方法的调用都会被转到这里来。

  var2 指向被调用的方法实例，而这里首先用变量 var4 获取该方法的简明名称，接着 switch 结构判断当前的调用方法是谁，如果是 Annotation 中的四大方法，将 var7 赋上特定的值。

  如果当前调用的方法是 toString，equals，hashCode，annotationType 的话，AnnotationInvocationHandler 实例中已经预定义好了这些方法的实现，直接调用即可。

  那么假如 var7 没有匹配上这四种方法，说明当前的方法调用的是自定义注解字节声明的方法，例如我们 Hello 注解的 value 方法。**这种情况下，将从我们的注解 map 中获取这个注解属性对应的值。**

  其实，JAVA 中的注解设计个人觉得有点反人类，明明是属性的操作，非要用方法来实现。当然，如果你有不同的见解，欢迎留言探讨。

  最后我们再总结一下整个反射注解的工作原理：

  首先，我们通过键值对的形式可以为注解属性赋值，像这样：@Hello（value = "hello"）。

  接着，你用注解修饰某个元素，编译器将在编译期扫描每个类或者方法上的注解，会做一个基本的检查，你的这个注解是否允许作用在当前位置，最后会将注解信息写入元素的属性表。

  然后，当你进行反射的时候，虚拟机将所有生命周期在 RUNTIME 的注解取出来放到一个 map 中，并创建一个 AnnotationInvocationHandler 实例，把这个 map 传递给它。

  最后，虚拟机将采用 JDK 动态代理机制生成一个目标注解的代理类，并初始化好处理器。

  那么这样，一个注解的实例就创建出来了，它本质上就是一个代理类，你应当去理解好 AnnotationInvocationHandler 中 invoke 方法的实现逻辑，这是核心。一句话概括就是，**通过方法名返回注解属性值**。

  可以看[Java注解](https://www.bilibili.com/video/av62102209?p=6)

## 集合

#### 泛型

- **泛型是什么**

  泛型最精准的定义：参数化类型。具体点说就是处理的数据类型不是固定的，而是可以作为参数传入。定义泛型类、泛型接口、泛型方法，这样，同一套代码，可以用于多种数据类型。

- **泛型带来的好处**

  在没有泛型的情况下，通过对类型Object的引用来实现参数的“任意化”，“任意化”带来的缺点是要做显示的强制类型转换，而这种转换是要求开发者对实际参数类型可以预知的情况下进行的。对于强制类型转换错误的情况，编译器可能不提示错误，在运行时才出现异常，这本身就是一个安全隐患。

  那么泛型的好处就是在编译的时候能够检查类型安全，并且所有的强制转换都是自动和隐式的。

- **泛型中通配符**

  我们在定义泛型类，泛型方法，泛型接口的时候经常会碰见很多不同的通配符，比如T，E，K，V，?等等，这些通配符又都是些什么意思呢？

  - 常用的T，E，K，V，?

    本质上都是通配符，没啥区别，只不过是编码时的一种约定俗成的东西。

    - ? 表示不确定的Java类型
    - T（type）表示具体的一个Java类型
    - K（key）V（value）分别代表Java键值中的key和value
    - E（element）代表Element

    - ? 无界通配符

      先从一个小例子看起，我有一个父类Animal和几个子类，如狗、猫等，现在我需要一个动物的列表，我的第一个想法是像这样的：

      ```java
      List<Animal> listAnimals;
      ```

      但是老板的想法却是这样的：

      ```java
      List<? extends Animal> listAnimals;
      ```

      为什么要使用通配符而不是简单的泛型呢？通配符其实在声明局部变量时是没有什么意义的，但是当你为一个方法声明一个参数时，它是非常重要的。

      ```java
      static int countLegs(List<? extends Animal> animals) {
          int retVal = 0;
          for(Animal animal : animals) {
              retVal += animal.countLegs();
          }
          return retVal;
      }
      
      static int countLegs1(List<Animal> animals) {
          int retVal = 0;
          for(Animal animal : animals) {
              retVal += animals.countLegs();
          }
          return retVal;
      }
      
      public static void main(String[] args) {
          List<Dog> dogs = new ArrayList<>();
          // 不会报错
          countLegs(dogs);
          // 报错
          countLegs1(dogs);
      }
      ```

      所以，对与不确定或者不关心实际要操作的类型，可以使用无限制通配符（尖括号里一个问好，即<?>），表示可以持有任何类型。像countLegs方法中，限定了上届，但是不关心具体类型是什么，所以对于传入的Animal的所有子类都可以支持，并且不会报错。

    - 上界通配符<? extends E>

      > 上界：用extends关键字声明，表示参数化的类型可能是所指定的类型，或者是此类型的子类。

      在类型参数中使用extends表示这个泛型中的参数必须是E或者E的子类，这样有两个好处：

      - 如果传入的类型不是E或者E的子类，编译不成功。
      - 泛型中可以使用E的方法，要不然还得强制成E才能使用。

      ```java
      private <K extends A, E extends B> E test(K arg1, E arg2) {
          E result = arg2;
          arg2.compareTo(arg1);
          // ..
          return result;
      }
      ```

      > 类型参数列表中如果有多个类型参数上限，用逗号分开

    - 下界通配符<? super E>

      > 下界：用super进行声明，表示参数化的类型可能是所指定的类型，或者是此类型的父类型，直至Object

      在类型参数中使用super表示这个泛型中的参数必须是E或者E的父类。

      ```java
      private <T> void test(List<? super T> dst, List<T> src) {
          for(T t : src) {
              dst.add(t);
          }
      }
      
      public static void main(String[] args) {
          List<Dog> dogs = new ArrayList<>();
          List<Animal> animals = new ArrayList<>();
          new Test3().test(animals, dogs);
      }
      ```

      dst 类型“大于等于”src的类型，这里的“大于等于”是指dst表示的范围比src要大，因此装得下dst的容器也就能装src。

      [Java-泛型类型擦除](https://blog.csdn.net/fw0124/article/details/42295463)

#### Collections总览

- **Java集合框架主要结构图**

  ![img](https://user-gold-cdn.xitu.io/2016/11/29/9c549ffd99ca2505da782d00b6a0a27a?imageslim)

  如上图所示，Java的集合主要按两种接口分类：Collection，Map。

  - Collection接口

    Collection作为集合的一个根接口，定义了一组对象和它的子类需要实现的15个方法：

    ![这里写图片描述](https://user-gold-cdn.xitu.io/2016/11/29/90037ec30b20e7935af3566e71a04cf7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

    对集合的基础操作，比如：

    ```java
    int size(); // 获取元素个数
    boolean isEmpty(); // 是否个数为0
    boolean contains(Object element); // 是否包含指定元素
    boolean add(E element); // 添加元素，成功时返回true
    boolean remove(Object element); // 删除元素，成功时返回true
    Iterator iterator(); // 获取迭代器
    ```

    还有一些操作整个集合的方法，比如：

    ```java
    boolean containsAll(Collection c); // 比较
    boolean addAll(Collection c); // 添加集合C中所有的元素到本集合中，如果集合有改变就返回true
    boolean removeAll(Collection c); // 删除本集合中和c集合中一致的元素，如果集合有改变就返回true
    boolean retainAll(Collection c); // 保留本集合中c集合中没有的元素，如果集合有改变就返回true
    void clear();
    ```

    还有对数组操作的方法：

    ```java 
    Object[] toArray();
    T[] toArray(T[] a); // 返回一个包含集合中所有元素的数组，运行时根据集合元素的类型指定数组的类型。
    ```

    在JDK8以后，Collection接口还提供了从集合获取连续的或者并发的流。

    ```java
    Stream stream();
    Stream parallelStream();
    ```

    遍历Collection的几种方式：

    - for-each语法

      ```java
      Collection persons = new ArrayList<>();
      for(Person person: persons) {
          Sout(person.name);
      }
      ```

    - 使用Iterator迭代器

      ```java
      Collecton persons = new ArrayList<>();
      Iterator iterator = persons.iterator();
      while(iterator.hasNext()) {
          Sout(iterator.next.name);
      }
      ```

    - 使用aggregate operations聚合操作

      ```java
      Collection persons = new ArrayList<>();
      persons.stream().forEach(new Consumer() {
          @Override
          public void accept(Person person) {
              Sout(person.name);
          }
      })
      ```

  - Aggregate Operations 聚合操作

    在JDK8以后，推荐使用聚合操作对一个集合进行操作。聚合操作通常和lambda表达式结合使用，让代码看起来更简洁。

    - 使用流来遍历一个ShapesCollection，然后输出红色的元素：

      ```java
      myShapesCollection.stream()
          .filter(e -> e.getColor() == Color.RED)
          .forEach(e -> Sout(e.getName()));
      ```

    - 可以获取一个并发流（parallelStream）,当集合元素很多时使用并发可以提高效率：

      ```java
      myShapesCollection.parallelStream()
          .filter(e -> e.getColor() == Color.RED)
          .forEach(e -> Sout(e.getName()));
      ```

    - 聚合操作还有很多操作集合的方法，比如说你想把Collection中的元素都转成String对象，然后把它们连起来：

      ```java
      String joined = elements.stream()
          .map(Object::toString)
          .collect(Collectors.joining(", "));
      ```

  - Iterator迭代器

    Collection继承自Iterator迭代器。

    结合Collection和Iterator可以实现一些复用性很强的方法，比如：

    ```java
    public static void filter(Collection c) {
        for(Iterator it = c.iterator; it.hasNext()l;) {
            if(!condition(it.next())) {
                it.remove();
            }
        }
    }
    ```

    这个filter方法是多态的，可以用于所有Collection的子类，实现类。

#### Iterator迭代器

- **Iterator概览**

  ```java
  public interface Iterable<T> {
  
      /**
      * Returns an {@link Iterator} for the elements in 	  * this object.
      *
      * @return An {@code Iterator} instance.
      */
      Iterator<T> iterator();
  }
  ```

  Iterator接口内只有一个iterator方法，返回一个Iterator迭代器。

  根据官方文档：

  > Iterators differ from enumerations in two ways:
  >
  > Iterators allow the caller to remove elements from the underlying collection during the iteration with well-defined semantics.
  > Method names have been improved.

  大致意思就是说替代了Enumerations，方法名字缩短了。其次是允许调用者在遍历过程中语法正确地删除元素。

  注意这个【语法正确】，事实上我们在使用Iterator对容器进行迭代时如果修改容器，可能会报ConcurrentModificationException的错。官方称这种情况下的迭代器时fail-fast迭代器。

- **fail-fast与ConcurrentModificationException**

  以ArrayList为例，在调用迭代器的next，remove方法时：

  ```java
  public E next() {
      if (expectedModCount == modCount) {
          try {
              E result = get(pos + 1);
              lastPosition = ++pos;
              return result;
          } catch (IndexOutOfBoundsException e) {
              throw new NoSuchElementException();
          }
      }
      throw new ConcurrentModificationException();
  }
  
  public void remove() {
      if (this.lastPosition == -1) {
          throw new IllegalStateException();
      }
  
      if (expectedModCount != modCount) {
          throw new ConcurrentModificationException();
      }
  
      try {
          AbstractList.this.remove(lastPosition);
      } catch (IndexOutOfBoundsException e) {
          throw new ConcurrentModificationException();
      }
  
      expectedModCount = modCount;
      if (pos == lastPosition) {
          pos--;
      }
      lastPosition = -1;
  }
  ```

  可以看到在调用迭代器的next，remove方法时都会比较modCount和

  expectedModCount是否相等，如果不相等就会抛出ConcurrentModificationException，也就是成为了fail-fast。

  而modCount在add，clear，remove时都会被修改：

  ```java
  public boolean add(E object) {
      //...
      modCount++;
      return true;
  }
  
  public void clear() {
      if (size != 0) {
          //...
          modCount++;
      }
  }
  
  public boolean remove(Object object) {
      Object[] a = array;
      int s = size;
      if (object != null) {
          for (int i = 0; i < s; i++) {
              if (object.equals(a[i])) {
                  //...
                  modCount++;
                  return true;
              }
          }
      } else {
          for (int i = 0; i < s; i++) {
              if (a[i] == null) {
                  //...
                  modCount++;
                  return true;
              }
          }
      }
      return false;
  }
  ```

  因此我们知道了fail-fast即ConcurrentModificationException出现的原因。

- **解决fail-fast**

  - 方法一:
    用 CopyOnWriteArrayList，ConcurrentHashMap 替换 ArrayList， HashMap，它们的功能和名字一样，在写入时会创建一个 copy，然后在这个 copy 版本上进行修改操作，这样就不会影响原来的迭代。不过坏处就是浪费内存。

  - 方法二：
    使用 Collections.synchronizedList加同步锁，不过这样有点粗暴。

#### ArrayList

- **什么是ArrayList**

  ArrayList是Java集合框架中List接口的一个实现类。有以下特点：

  - 容量不固定，想放多少放多少（当然有最大阈值，但一般达不到）
  - 有序的（元素输出顺序与输入顺粗一致）
  - 元素可以为null
  - 效率高
    - size()，isEmpty()，get()，set()，iterator()，ListIterator()方法的时间复杂度都是O(1)
    - add()添加操作的时间复杂度平均为O(n)
    - 其他所有操作的时间复杂度几乎都是O(n)

  - 占用空间更小

    对比LinkedList，不用占用额外空间维护链表结构。

- **ArrayList的成员变量**

  ![这里写图片描述](https://img-blog.csdn.net/20161018135844873)

  - 底层数据结构，数组：

    ```java
    transient Object[] elementData
    ```

    由于数组类型为Object，所以允许添加null。

    transient说明这个数组无法序列化。

    初始时为DEFAULTCAPACITY_EMPTY_ELEMENTDATA 

  - 默认的空数组：

    ```jav
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    
    private static final Object[] EMPTY_ELEMENTDATA = {};
    
    ```

  - 数组初始容量为10：

    ```java
    private static final int DEFAULT_CAPACITY = 10;
    ```

  - 数组中当前元素个数：

    ```java
    private int size;
    ```

    size <= capacity

  - 数组最大容量：

    ```java
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    ```

    Integer.MAX_VALUE = 0x7fffffff

    换算成二进制：2^31 - 1，十进制就是2147483647，二十一亿多

    一些虚拟机需要在数组前加上头标签，所以减去8。

    当想要分配比MAX_ARRAY_SIZE大的个数就会报OutOfMemoryError。

- **ArrayList的关键方法**

  - 构造函数

    ArrayList有三种构造方法：

    ```java
    //初始为空数组
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
    
    //根据指定容量，创建个对象数组
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
    
    //直接创建和指定集合一样内容的 ArrayList
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray 有可能不返回一个 Object 数组
            if (elementData.getClass() != Object[].class)
                //使用 Arrays.copy 方法拷创建一个 Object 数组
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
    
    ```

  - 添加元素：

    ```java
    public boolean add(E e) {
        //对数组的容量进行调整
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
    
    //在指定位置添加一个元素
    public void add(int index, E element) {
        rangeCheckForAdd(index);
    
        //对数组的容量进行调整
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //整体后移一位，效率不太好啊
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }
    
    
    //添加一个集合
    public boolean addAll(Collection<? extends E> c) {
        //把该集合转为对象数组
        Object[] a = c.toArray();
        int numNew = a.length;
        //增加容量
        ensureCapacityInternal(size + numNew);  // Increments modCount
        //挨个向后迁移
        System.arraycopy(a, 0, elementData, size, numNew);
        size += numNew;
        //新数组有元素，就返回 true
        return numNew != 0;
    }
    
    //在指定位置，添加一个集合
    public boolean addAll(int index, Collection<? extends E> c) {
        rangeCheckForAdd(index);
    
        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // Increments modCount
    
        int numMoved = size - index;
        //原来的数组挨个向后迁移
        if (numMoved > 0)
            System.arraycopy(elementData, index, elementData, index + numNew,
                             numMoved);
        //把新的集合数组 添加到指定位置
        System.arraycopy(a, 0, elementData, index, numNew);
        size += numNew;
        return numNew != 0;
    }
    ```

  - 对数组的容量进行调整

    ```java
    public void ensureCapacity(int minCapacity) {
        int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            // 不是默认的数组，说明已经添加了元素
            ? 0
            // 默认的容量
            : DEFAULT_CAPACITY;
    
        if (minCapacity > minExpand) {
            //当前元素个数比默认容量大
            ensureExplicitCapacity(minCapacity);
        }
    }
    
    private void ensureCapacityInternal(int minCapacity) {
        //还没有添加元素
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            //最小容量取默认容量和 当前元素个数 最大值
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }
    
        ensureExplicitCapacity(minCapacity);
    }
    
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
    
        // 容量不够了，需要扩容
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    ```

    我们可以主动调用ensureCapcity来增加ArrayList对象的容量，这样就避免添加元素满了时扩容、挨个复制后移等消耗。

  - 扩容

    ```java
    private void grow(int minCapacity) {
        int oldCapacity = elementData.length;
        // 1.5 倍 原来容量
        int newCapacity = oldCapacity + (oldCapacity >> 1);
    
        //如果当前容量还没达到 1.5 倍旧容量，就使用当前容量，省的站那么多地方
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
    
        //新的容量居然超出了 MAX_ARRAY_SIZE
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            //最大容量可以是 Integer.MAX_VALUE
            newCapacity = hugeCapacity(minCapacity);
    
        // minCapacity 一般跟元素个数 size 很接近，所以新建的数组容量为 newCapacity 更宽松些
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
    ```

  - 查询，修改等操作，直接根据角标对数组操作，都很快：

    ```java
    E elementData(int index) {
        return (E) elementData[index];
    }
    
    //获取
    public E get(int index) {
        rangeCheck(index);
        //直接根据数组角标返回元素，快的一比
        return elementData(index);
    }
    
    //修改
    public E set(int index, E element) {
        rangeCheck(index);
        E oldValue = elementData(index);
    
        //直接对数组操作
        elementData[index] = element;
        //返回原来的值
        return oldValue;
    }
    ```

  - 删除，还是有点慢

    ```java
    //根据位置删除
    public E remove(int index) {
        rangeCheck(index);
    
        modCount++;
        E oldValue = elementData(index);
    
        //挨个往前移一位
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        //原数组中最后一个元素删掉
        elementData[--size] = null; // clear to let GC do its work
    
        return oldValue;
    }
    
    //删除某个元素
    public boolean remove(Object o) {
        if (o == null) {
            //挨个遍历找到目标
            for (int index = 0; index < size; index++)
                if (elementData[index] == null) {
                    //快速删除
                    fastRemove(index);
                    return true;
                }
        } else {
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }
    
    //内部方法，“快速删除”，就是把重复的代码移到一个方法里
    //没看出来比其他 remove 哪儿快了 - -
    private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }
    
    //保留公共的
    public boolean retainAll(Collection<?> c) {
        Objects.requireNonNull(c);
        return batchRemove(c, true);
    }
    
    //删除或者保留指定集合中的元素
    private boolean batchRemove(Collection<?> c, boolean complement) {
        final Object[] elementData = this.elementData;
        //使用两个变量，一个负责向后扫描，一个从 0 开始，等待覆盖操作
        int r = 0, w = 0;
        boolean modified = false;
        try {
            //遍历 ArrayList 集合
            for (; r < size; r++)
                //如果指定集合中是否有这个元素，根据 complement 判断是否往前覆盖删除
                if (c.contains(elementData[r]) == complement)
                    elementData[w++] = elementData[r];
        } finally {
            //发生了异常，直接把 r 后面的复制到 w 后面
            if (r != size) {
                System.arraycopy(elementData, r,
                                 elementData, w,
                                 size - r);
                w += size - r;
            }
            if (w != size) {
                // 清除多余的元素，clear to let GC do its work
                for (int i = w; i < size; i++)
                    elementData[i] = null;
                modCount += size - w;
                size = w;
                modified = true;
            }
        }
        return modified;
    }
    
    //清楚全部
    public void clear() {
        modCount++;
        //并没有直接使数组指向 null,而是逐个把元素置为空
        //下次使用时就不用重新 new 了
        for (int i = 0; i < size; i++)
            elementData[i] = null;
    
        size = 0;
    }
    ```

  - 判断状态：

    ```java
    public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
    
    //遍历，第一次找到就返回
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
    
    //倒着遍历
    public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size-1; i >= 0; i--)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = size-1; i >= 0; i--)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
    ```

  - 转换成数组：

    ```java
    public Object[] toArray() {
        return Arrays.copyOf(elementData, size);
    }
    
    public <T> T[] toArray(T[] a) {
        //如果只是要把一部分转换成数组
        if (a.length < size)
            // Make a new array of a's runtime type, but my contents:
            return (T[]) Arrays.copyOf(elementData, size, a.getClass());
        //全部元素拷贝到 数组 a
        System.arraycopy(elementData, 0, a, 0, size);
        if (a.length > size)
            a[size] = null;
        return a;
    }
    ```

    看下Arrays.copyOf()方法：

    ```java
    public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
        @SuppressWarnings("unchecked")
        T[] copy = ((Object)newType == (Object)Object[].class)
            ? (T[]) new Object[newLength]
            : (T[]) Array.newInstance(newType.getComponentType(), newLength);
        System.arraycopy(original, 0, copy, 0,
                         Math.min(original.length, newLength));
        return copy;
    }
    ```

    如果newType是一个对象数组，就直接把original的元素拷贝到对象数组中；否则新建一个newType类型的数组。

- **ArrayList的内部类**

  迭代器Iterator，ListIterator没什么特别，直接使用角标访问数组的元素：

  ```java
  private class ListItr extends Itr implements ListIterator<E> {
      ListItr(int index) {
          super();
          cursor = index;
      }
  
      public boolean hasPrevious() {
          return cursor != 0;
      }
  
      public int nextIndex() {
          return cursor;
      }
  
      public int previousIndex() {
          return cursor - 1;
      }
  
      @SuppressWarnings("unchecked")
      public E previous() {
          checkForComodification();
          int i = cursor - 1;
          if (i < 0)
              throw new NoSuchElementException();
          Object[] elementData = ArrayList.this.elementData;
          if (i >= elementData.length)
              throw new ConcurrentModificationException();
          cursor = i;
          return (E) elementData[lastRet = i];
      }
  
      public void set(E e) {
          if (lastRet < 0)
              throw new IllegalStateException();
          checkForComodification();
  
          try {
              ArrayList.this.set(lastRet, e);
          } catch (IndexOutOfBoundsException ex) {
              throw new ConcurrentModificationException();
          }
      }
  
      public void add(E e) {
          checkForComodification();
  
          try {
              int i = cursor;
              ArrayList.this.add(i, e);
              cursor = i + 1;
              lastRet = -1;
              expectedModCount = modCount;
          } catch (IndexOutOfBoundsException ex) {
              throw new ConcurrentModificationException();
          }
      }
  }
  ```

#### LinkedList

- **为什么选择LinkedList**

  我们知道ArrayList是以数组实现的，遍历时很快，但是插入、删除时都需要移动后面的元素，效率略差些。

  而LinkedList是以链表实现的，插入、删除时只需要改变前后两个节点指针指向即可，省事不少。

- **LinkedList双向链表实现**

  LinkedList继承自AbstractSequentialList接口，同时还实现了Deque，Queue接口。

  ```java
  public class LinkedList<E>
      extends AbstractSequentialList<E>
      implements List<E>, Deque<E>, Cloneable, java.io.Serializable
  {
      transient int size = 0;
  
      /**
       * Pointer to first node.
       * Invariant: (first == null && last == null) ||
       *            (first.prev == null && first.item != null)
       */
      transient Node<E> first;
  
      /**
       * Pointer to last node.
       * Invariant: (first == null && last == null) ||
       *            (last.next == null && last.item != null)
       */
      transient Node<E> last;
  }
  ```

  可以看到，LinkedList的成员变量只有三个：

  - 头节点first
  - 尾节点last
  - 容量size

  节点是一个双向节点：

  ```java
  private static class Node<E> {
      E item;
      Node<E> next;
      Node<E> prev;
  
      Node(Node<E> prev, E element, Node<E> next) {
          this.item = element;
          this.next = next;
          this.prev = prev;
      }
  }
  ```

  用一副图表示节点：

  ![这里写图片描述](https://img-blog.csdn.net/20161020161332351)

- **LinkedList的方法**

  关键的几个内部方法

  头部添加删除

  ```java
  //插入到头部
  private void linkFirst(E e) {
      //获取头节点
      final Node<E> f = first;
      //新建一个节点，尾部指向之前的 头元素 first
      final Node<E> newNode = new Node<>(null, e, f);
      //first 指向新建的节点
      first = newNode;
      //如果之前是空链表，新建的节点 也是最后一个节点
      if (f == null)
          last = newNode;
      else
          //原来的第一个节点（现在的第二个）头部指向新建的头结点
          f.prev = newNode;
      size++;
      modCount++;
  }
  
  //删除头节点并返回该节点上的数据，假设不为 null
  private E unlinkFirst(Node<E> f) {
      // 获取数据，一会儿返回
      final E element = f.item;
      //获取头节点后面一个节点
      final Node<E> next = f.next;
      //使头节点上数据为空，尾部指向空
      f.item = null;
      f.next = null; // help GC
      //现在头节点后边的节点变成第一个了
      first = next;
      //如果头节点后面的节点为 null，说明移除这个节点后，链表里没节点了
      if (next == null)
          last = null;
      else
          next.prev = null;
      size--;
      modCount++;
      return element;
  }
  ```

  尾部添加删除

  ```java
  //插入到尾部
  void linkLast(E e) {
      //获取尾部节点
      final Node<E> l = last;
      //新建一个节点，头部指向之前的 尾节点 last
      final Node<E> newNode = new Node<>(l, e, null);
      //last 指向新建的节点
      last = newNode;
      //如果之前是空链表， 新建的节点也是第一个节点
      if (l == null)
          first = newNode;
      else
          //原来的尾节点尾部指向新建的尾节点
          l.next = newNode;
      size++;
      modCount++;
  }
  
  //删除尾部节点并返回，假设不为空
  private E unlinkLast(Node<E> l) {
  
      final E element = l.item;
      //获取倒数第二个节点
      final Node<E> prev = l.prev;
      //尾节点数据、尾指针置为空
      l.item = null;
      l.prev = null; // help GC
      //现在倒数第二变成倒数第一了
      last = prev;
      if (prev == null)
          first = null;
      else
          prev.next = null;
      size--;
      modCount++;
      return element;
  }
  ```

  获取指定节点，指定节点的添加删除

  ```java
  //获取指定位置的节点
  Node<E> node(int index) {
      // 假设指定位置有元素
  
      //二分一下，如果小于 size 的一半，从头开始遍历
      if (index < (size >> 1)) {
          Node<E> x = first;
          for (int i = 0; i < index; i++)
              x = x.next;
          return x;
      } else {
          //大于 size 一半，从尾部倒着遍历
          Node<E> x = last;
          for (int i = size - 1; i > index; i--)
              x = x.prev;
          return x;
      }
  }
  
  //在 指定节点 前插入一个元素，这里假设 指定节点不为 null
  void linkBefore(E e, Node<E> succ) {
      // 获取指定节点 succ 前面的一个节点
      final Node<E> pred = succ.prev;
      //新建一个节点，头部指向 succ 前面的节点，尾部指向 succ 节点，数据为 e
      final Node<E> newNode = new Node<>(pred, e, succ);
      //让 succ 节点头部指向 新建的节点
      succ.prev = newNode;
      //如果 succ 前面的节点为空，说明 succ 就是第一个节点，那现在新建的节点就变成第一个节点了
      if (pred == null)
          first = newNode;
      else
          //如果前面有节点，让前面的节点
          pred.next = newNode;
      size++;
      modCount++;
  }
  
  //删除某个指定节点
  E unlink(Node<E> x) {
      // 假设 x 不为空
      final E element = x.item;
      //获取指定节点前面、后面的节点
      final Node<E> next = x.next;
      final Node<E> prev = x.prev;
  
      //如果前面没有节点，说明 x 是第一个
      if (prev == null) {
          first = next;
      } else {
          //前面有节点，让前面节点跨过 x 直接指向 x 后面的节点
          prev.next = next;
          x.prev = null;
      }
  
      //如果后面没有节点，说 x 是最后一个节点
      if (next == null) {
          last = prev;
      } else {
          //后面有节点，让后面的节点指向 x 前面的
          next.prev = prev;
          x.next = null;
      }
  
      x.item = null;
      size--;
      modCount++;
      return element;
  }
  ```

  这些内部方法实现了对链表节点的基本修改操作，每次操作都只要修改前后节点的指针，时间复杂度为O(1)。很多公开方法都是通过调用它们实现的。

  公开的添加方法：

  ```java
  //普通的在尾部添加元素
  public boolean add(E e) {
      linkLast(e);
      return true;
  }
  
  //在指定位置添加元素
  public void add(int index, E element) {
      checkPositionIndex(index);
      //指定位置也有可能是在尾部
      if (index == size)
          linkLast(element);
      else
          linkBefore(element, node(index));
  }
  
  //添加一个集合的元素
  public boolean addAll(Collection<? extends E> c) {
      return addAll(size, c);
  }
  
  public boolean addAll(int index, Collection<? extends E> c) {
      checkPositionIndex(index);
  
      //把 要添加的集合转成一个 数组
      Object[] a = c.toArray();
      int numNew = a.length;
      if (numNew == 0)
          return false;
  
      //创建两个节点，分别指向要插入位置前面和后面的节点
      Node<E> pred, succ;
      //要添加到尾部
      if (index == size) {
          succ = null;
          pred = last;
      } else {
          //要添加到中间， succ 指向 index 位置的节点，pred 指向它前一个
          succ = node(index);
          pred = succ.prev;
      }
  
      //遍历要添加内容的数组
      for (Object o : a) {
          @SuppressWarnings("unchecked") E e = (E) o;
          //创建新节点，头指针指向 pred
          Node<E> newNode = new Node<>(pred, e, null);
          //如果 pred 为空，说明新建的这个是头节点
          if (pred == null)
              first = newNode;
          else
              //pred 指向新建的节点
              pred.next = newNode;
          //pred 后移一位
          pred = newNode;
      }
  
      //添加完后需要修改尾指针 last
      if (succ == null) {
          //如果 succ 为空，说明要插入的位置就是尾部，现在 pred 已经到最后了
          last = pred;
      } else {
          //否则 pred 指向后面的元素
          pred.next = succ;
          succ.prev = pred;
      }
  
      //元素个数增加
      size += numNew;
      modCount++;
      return true;
  }
  
  //添加到头部，时间复杂度为 O(1)
  public void addFirst(E e) {
      linkFirst(e);
  }
  
  //添加到尾部，时间复杂度为 O(1)
  public void addLast(E e) {
      linkLast(e);
  }
  ```

  继承自双端队列的添加方法

  ```java
  //入栈，其实就是在头部添加元素
  public void push(E e) {
      addFirst(e);
  }
  
  //安全的添加操作，在尾部添加
  public boolean offer(E e) {
      return add(e);
  }
  
  //在头部添加
  public boolean offerFirst(E e) {
      addFirst(e);
      return true;
  }
  
  //尾部添加
  public boolean offerLast(E e) {
      addLast(e);
      return true;
  }
  ```

  删除方法：

  ```java
  //删除头部节点
  public E remove() {
      return removeFirst();
  }
  
  //删除指定位置节点
  public E remove(int index) {
      checkElementIndex(index);
      return unlink(node(index));
  }
  
  //删除包含指定元素的节点，这就得遍历了
  public boolean remove(Object o) {
      if (o == null) {
          //遍历终止条件，不等于 null
          for (Node<E> x = first; x != null; x = x.next) {
              if (x.item == null) {
                  unlink(x);
                  return true;
              }
          }
      } else {
          for (Node<E> x = first; x != null; x = x.next) {
              if (o.equals(x.item)) {
                  unlink(x);
                  return true;
              }
          }
      }
      return false;
  }
  
  //删除头部元素
  public E removeFirst() {
      final Node<E> f = first;
      if (f == null)
          throw new NoSuchElementException();
      return unlinkFirst(f);
  }
  
  //删除尾部元素
  public E removeLast() {
      final Node<E> l = last;
      if (l == null)
          throw new NoSuchElementException();
      return unlinkLast(l);
  }
  
  //删除首次出现的指定元素，从头遍历
  public boolean removeFirstOccurrence(Object o) {
      return remove(o);
  }
  
  //删除最后一次出现的指定元素，倒过来遍历
  public boolean removeLastOccurrence(Object o) {
      if (o == null) {
          for (Node<E> x = last; x != null; x = x.prev) {
              if (x.item == null) {
                  unlink(x);
                  return true;
              }
          }
      } else {
          for (Node<E> x = last; x != null; x = x.prev) {
              if (o.equals(x.item)) {
                  unlink(x);
                  return true;
              }
          }
      }
      return false;
  }
  ```

  继承自双端队列的删除方法：

  ```java
  public E pop() {
      return removeFirst();
  }
  
  public E pollFirst() {
      final Node<E> f = first;
      return (f == null) ? null : unlinkFirst(f);
  }
  
  public E pollLast() {
      final Node<E> l = last;
      return (l == null) ? null : unlinkLast(l);
  }
  ```

  清除全部元素其实只需要把首位都置为null，这个链表就已经是空的，因为无法访问元素。但是为了避免浪费空间，需要把中间节点都置为null：

  ```java
  public void clear() {
      for (Node<E> x = first; x != null;) {
          Node<E> next = x.next;
          x.item = null;
          x.next = null;
          x.prev = null;
          x = next;
      }
      first = last = null;
      size = 0;
      modCount ++;
  }
  ```

  公开的修改方法，只有一个set：

  ```java
  // set很简单，找到这个节点，替换数据就好了
  public E set(int index, E element) {
      checkElementIndex(index);
      Node<E> x = node(index);
      E oldVal = x.item;
      x.item = element;
      return oldVal;
  }
  ```

  公开的查询方法：

  ```java
  //挨个遍历，获取第一次出现位置
  public int indexOf(Object o) {
      int index = 0;
      if (o == null) {
          for (Node<E> x = first; x != null; x = x.next) {
              if (x.item == null)
                  return index;
              index++;
          }
      } else {
          for (Node<E> x = first; x != null; x = x.next) {
              if (o.equals(x.item))
                  return index;
              index++;
          }
      }
      return -1;
  }
  
  //倒着遍历，查询最后一次出现的位置
  public int lastIndexOf(Object o) {
      int index = size;
      if (o == null) {
          for (Node<E> x = last; x != null; x = x.prev) {
              index--;
              if (x.item == null)
                  return index;
          }
      } else {
          for (Node<E> x = last; x != null; x = x.prev) {
              index--;
              if (o.equals(x.item))
                  return index;
          }
      }
      return -1;
  }
  
  //是否包含指定元素
  public boolean contains(Object o) {
      return indexOf(o) != -1;
  }
  
  //获取指定位置的元素，需要遍历
  public E get(int index) {
      checkElementIndex(index);
      return node(index).item;
  }
  
  //获取第一个元素，很快
  public E getFirst() {
      final Node<E> f = first;
      if (f == null)
          throw new NoSuchElementException();
      return f.item;
  }
  
  //获取第一个，同时删除它
  public E poll() {
      final Node<E> f = first;
      return (f == null) ? null : unlinkFirst(f);
  }
  
  //也是获取第一个，和 poll 不同的是不删除
  public E peek() {
      final Node<E> f = first;
      return (f == null) ? null : f.item;
  }
  
  //长得一样嘛
  public E peekFirst() {
      final Node<E> f = first;
      return (f == null) ? null : f.item;
   }
  
  //最后一个元素，也很快
  public E getLast() {
      final Node<E> l = last;
      if (l == null)
          throw new NoSuchElementException();
      return l.item;
  }
  
  public E peekLast() {
      final Node<E> l = last;
      return (l == null) ? null : l.item;
  }
  ```

  关键方法介绍完了，接下来是内部实现的迭代器，需要注意的是LinkedList实现了一个倒序迭代器DescendingIterator，还实现了一个ListIterator，名叫ListItr。

  DescendingIterator倒叙迭代器：

  ```java
  //很简单，就是游标直接在 迭代器尾部，然后颠倒黑白，说是向后遍历，实际是向前遍历
  private class DescendingIterator implements Iterator<E> {
      private final ListItr itr = new ListItr(size());
      public boolean hasNext() {
          return itr.hasPrevious();
      }
      public E next() {
          return itr.previous();
      }
      public void remove() {
          itr.remove();
      }
  }
  ```

  ListItr操作基本都是调用的内部关键方法：

  ```java
  private class ListItr implements ListIterator<E> {
      private Node<E> lastReturned;
      private Node<E> next;
      private int nextIndex;
      private int expectedModCount = modCount;
  
      ListItr(int index) {
          // 二分遍历，指定游标位置
          next = (index == size) ? null : node(index);
          nextIndex = index;
      }
  
      public boolean hasNext() {
          return nextIndex < size;
      }
  
      public E next() {
          checkForComodification();
          if (!hasNext())
              throw new NoSuchElementException();
          //很简单，后移一位
          lastReturned = next;
          next = next.next;
          nextIndex++;
          return lastReturned.item;
      }
  
      public boolean hasPrevious() {
          return nextIndex > 0;
      }
  
      public E previous() {
          checkForComodification();
          if (!hasPrevious())
              throw new NoSuchElementException();
  
          lastReturned = next = (next == null) ? last : next.prev;
          nextIndex--;
          return lastReturned.item;
      }
  
      public int nextIndex() {
          return nextIndex;
      }
  
      public int previousIndex() {
          return nextIndex - 1;
      }
  
      public void remove() {
          checkForComodification();
          if (lastReturned == null)
              throw new IllegalStateException();
  
          Node<E> lastNext = lastReturned.next;
          unlink(lastReturned);
          if (next == lastReturned)
              next = lastNext;
          else
              nextIndex--;
          lastReturned = null;
          expectedModCount++;
      }
  
      public void set(E e) {
          if (lastReturned == null)
              throw new IllegalStateException();
          checkForComodification();
          lastReturned.item = e;
      }
  
      public void add(E e) {
          checkForComodification();
          lastReturned = null;
          if (next == null)
              linkLast(e);
          else
              linkBefore(e, next);
          nextIndex++;
          expectedModCount++;
      }
  
      public void forEachRemaining(Consumer<? super E> action) {
          Objects.requireNonNull(action);
          while (modCount == expectedModCount && nextIndex < size) {
              action.accept(next.item);
              lastReturned = next;
              next = next.next;
              nextIndex++;
          }
          checkForComodification();
      }
  
      final void checkForComodification() {
          if (modCount != expectedModCount)
              throw new ConcurrentModificationException();
      }
  }
  ```

  还有个LLSpIiterator继承自SpIiterator，JDK8出来的新东西：

  > Spliterator 是 Java 8 引入的新接口，顾名思义，Spliterator 可以理解为 Iterator 的 Split 版本（但用途要丰富很多）。
  >
  > 使用 Iterator 的时候，我们可以顺序地遍历容器中的元素，使用 Spliterator 的时候，我们可以将元素分割成多份，分别交于不于的线程去遍历，以提高效率。
  >
  > 使用 Spliterator 每次可以处理某个元素集合中的一个元素 — 不是从 Spliterator 中获取元素，而是使用 tryAdvance() 或 forEachRemaining() 方法对元素应用操作。
  >
  > 但 Spliterator 还可以用于估计其中保存的元素数量，而且还可以像细胞分裂一样变为一分为二。这些新增加的能力让流并行处理代码可以很方便地将工作分布到多个可用线程上完成。

  LinkedList特点：

  - 双向链表实现
  - 元素是有序的，输出顺序与输入顺序一致
  - 允许元素为null
  - 所有指定位置的操作都是从头开始遍历进行的
  - 和ArrayList一样，不是同步容器。

#### ArrayList与LinkedList区别

- **是否保证线程安全**

  ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全。

- **底层数据结构**

  ArrayList 底层使用的是 Object 数组。

  LinkedList 底层使用的是双向链表数据结构。

- **插入和删除是否受元素位置的影响**

  ArrayList采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。

  LinkedList采用链表存储，所以插入和删除元素时间复杂度不受元素位置的影响。

- **是否支持快速随机访问**

  LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。快速随机访问就是通过元素的序号快速获取元素对象（对应get(int index) 方法）。

- **内存空间占用**

  ArrayList 的空间浪费主要体现在数组的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

**补充内容：RandomAccess接口**

```java
public interface RandomAccess {
    
}
```

查看源码我们发现实际上RandomAccess接口中什么都没有定义。所以，在我看来RandomAccess接口不过是一个标识罢了。标识什么？标识实现这个接口的类具有随机访问功能。

在binarySearch()方法中，它要判断传入的list是否RandomAccess的实例，如果是，调用indexedBinarySearch()方法，如果不是，那么调用iteratorBinarySearch()方法

```java
public static <T>
    int binarySearch(List<? extends Comparable<? super T>> list, T key) {
        if (list instanceof RandomAccess || list.size()<BINARYSEARCH_THRESHOLD)
            return Collections.indexedBinarySearch(list, key);
        else
            return Collections.iteratorBinarySearch(list, key);
    }
```

ArrayList 实现了 RandomAccess 接口，而 LinkedList 没有实现。为什么呢？我觉得还是和底层数据结构有关！ArrayList 底层是数组，而LinkedList 底层是链表。数组天然支持随机访问，时间复杂度为O(1)，所以称为快速随机访问。链表需要遍历到特定位置才能访问特定位置的元素，时间复杂度O(n)，所以不支持快速随机访问，ArrayList 实现了 RandomAccess 接口，就表明了它具有快速随机访问功能。RandomAccess 接口只是标识，并不是说 ArrayList 实现了 RandomAccess 接口才具有快速随机访问功能的。

下面总结一下list的遍历方式：

- 实现了RandomAccess 接口的list，优先选择普通for循环，其次foreach。
- 未实现RandomAccess接口的list，优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大size的数据，千万不要使用普通for循环。

#### ArrayList与Vector 区别

Vector类的所有方法都是同步的。一个线程访问Vector的话，代码要在同步操作上耗费大量时间。

ArrayList不是同步的，所以在不需要保证线程安全时建议使用ArrayList。

另外需要同步的话，使用：

```java
List list = Collections.synchronizedList(List list);
```



