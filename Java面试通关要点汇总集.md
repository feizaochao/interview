# ·基础篇

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
    @Retendtion(RetentionPolicy.RUNTIME)
    @Target(ElementType.ANNOTATION_TYPE)
    public @interface Target {
        /**
        * Returns an array of the kinds of elements an
        * annotation can be applied to.
        */
        ElementType[] value();
    }
    ```
    
    

