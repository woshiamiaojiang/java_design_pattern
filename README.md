
## 1.设计模式七大原则

1. 单一职责原则
2. 接口隔离原则
3. 依赖倒转(倒置)原则
4. 里氏替换原则
5. 开闭原则
6. 迪米特法则
7. 合成复用原则

### 1.1.单一职责原则

#### 1.1.1.基本介绍

一个类只应负责一项职责。

#### 1.1.2.应用实例

```java
package com.atguigu.principle.singleresponsibility;

public class SingleResponsibility3 {

    public static void main(String[] args) {
        Vehicle2 vehicle2 = new Vehicle2();
        vehicle2.run("汽车");
        vehicle2.runWater("轮船");
        vehicle2.runAir("飞机");
    }
}

// 方式3的分析
// 1. 这种修改方法没有对原来的类做大的修改，只是增加方法
// 2. 这里虽然没有在类这个级别上遵守单一职责原则，但是在方法级别上，仍然是遵守单一职责
class Vehicle2 {

    public void run(String vehicle) {
        System.out.println(vehicle + " 在公路上运行....");
    }

    public void runAir(String vehicle) {
        System.out.println(vehicle + " 在天空上运行....");
    }

    public void runWater(String vehicle) {
        System.out.println(vehicle + " 在水中行....");
    }
}
```

#### 1.1.3.注意事项和细节

1. 一个类只负责一项职责。
2. 提高类的可读性与维护性。
3. 降低变更引起的风险。
4. 只有逻辑足够简单，才可以在代码级违反单一职责原则；只有类中方法数量足够少，可以在方法级别保持单一职责原则。

### 1.2.接口隔离原则(Interface Segregation Principle)

#### 1.2.1.基本介绍

一个类对另一个类的依赖应该建立在最小的接口上。

#### 1.2.2.应用实例

```java
package com.atguigu.principle.segregation.improve;

// 将接口Interface1拆分为独立的几个接口，类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则

/**
 * 接口1
 */
interface Interface1 {
    void operation1();
}

/**
 * 接口2
 */
interface Interface2 {
    void operation2();

    void operation3();
}

/**
 * 接口3
 */
interface Interface3 {
    void operation4();

    void operation5();
}

public class Segregation1 {

    public static void main(String[] args) {
        // 使用一把
        A a = new A();
        // A类通过接口去依赖B类
        a.depend1(new B());
        a.depend2(new B());
        a.depend3(new B());
        C c = new C();
        // C类通过接口去依赖(使用)D类
        c.depend1(new D());
        c.depend4(new D());
        c.depend5(new D());
    }
}

/**
 * A 类通过接口Interface1,Interface2 依赖(使用) B类，但是只会用到1,2,3方法
 */
class A {
    public void depend1(Interface1 i) {
        i.operation1();
    }

    public void depend2(Interface2 i) {
        i.operation2();
    }

    public void depend3(Interface2 i) {
        i.operation3();
    }
}

class B implements Interface1, Interface2 {
    @Override public void operation1() {
        System.out.println("B 实现了 operation1");
    }

    @Override public void operation2() {
        System.out.println("B 实现了 operation2");
    }

    @Override public void operation3() {
        System.out.println("B 实现了 operation3");
    }
}

/**
 * C 类通过接口Interface1,Interface3 依赖(使用) D类，但是只会用到1,4,5方法
 */
class C {
    public void depend1(Interface1 i) {
        i.operation1();
    }

    public void depend4(Interface3 i) {
        i.operation4();
    }

    public void depend5(Interface3 i) {
        i.operation5();
    }
}

class D implements Interface1, Interface3 {
    @Override public void operation1() {
        System.out.println("D 实现了 operation1");
    }

    @Override public void operation4() {
        System.out.println("D 实现了 operation4");
    }

    @Override public void operation5() {
        System.out.println("D 实现了 operation5");
    }
}
```

1. 类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须去实现他们不需要的方法
2. 将接口Interface1拆分为独立的几个接口，类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则
3. 接口Interface1中出现的方法，根据实际情况拆分为三个接口

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWRiZjM3MzlhYjk3YmY2MmEucG5n?x-oss-process=image/format,png)

### 1.3.依赖倒转原则(Dependence Inversion Principle)

#### 1.3.1.基本介绍

1. 高层模块不应该依赖低层模块，二者都应该依赖其抽象
2. **抽象不应该依赖细节，细节应该依赖抽象**
3. 依赖倒转(倒置)的中心思想是**面向接口编程**
4. 依赖倒转原则是基于这样的设计理念：相对于细节的多变性，抽象的东西要稳定的
   多。以抽象为基础搭建的架构比以细节为基础的架构要稳定的多。在java中，抽象
   指的是接口或抽象类，细节就是具体的实现类
5. 使用**接口或抽象类**的目的是制定好**规范**，而不涉及任何具体的操作，把**展现细节的**
   **任务交给他们的实现类**去完成

#### 1.3.2.应用实例

```java
package com.atguigu.principle.inversion.improve;

//定义接口
interface IReceiver {
    String getInfo();
}

public class DependecyInversion {

    public static void main(String[] args) {
        //客户端无需改变
        Person person = new Person();
        person.receive(new Email());
        person.receive(new WeiXin());
    }
}

class Email implements IReceiver {
    @Override public String getInfo() {
        return "电子邮件信息: hello,world";
    }
}

//增加微信
class WeiXin implements IReceiver {
    @Override public String getInfo() {
        return "微信信息: hello,ok";
    }
}

//方式2
class Person {
    //这里我们是对接口的依赖
    public void receive(IReceiver receiver) {
        System.out.println(receiver.getInfo());
    }
}
```

#### 1.3.3.三种方式和应用案例

```java
package com.atguigu.principle.inversion.improve;

// 方式3 , 通过setter方法传递
interface IOpenAndClose {
    
    void open(); // 抽象方法

    void setTv(ITV tv);
}

// 方式1： 通过接口传递实现依赖
// 开关的接口
// interface IOpenAndClose {
// public void open(ITV tv); //抽象方法,接收接口
// }
//
// interface ITV { //ITV接口
// public void play();
// }
//
// class ChangHong implements ITV {
//
//	@Override
//	public void play() {
//		System.out.println("长虹电视机，打开");
//	}
//
// }
//// 实现接口
// class OpenAndClose implements IOpenAndClose{
// public void open(ITV tv){
// tv.play();
// }
// }

// 方式2: 通过构造方法依赖传递
// interface IOpenAndClose {
// public void open(); //抽象方法
// }
// interface ITV { //ITV接口
// public void play();
// }
// class OpenAndClose implements IOpenAndClose{
// public ITV tv; //成员
// public OpenAndClose(ITV tv){ //构造器
// this.tv = tv;
// }
// public void open(){
// this.tv.play();
// }
// }

interface ITV { // ITV接口
    void play();
}

public class DependencyPass {

    public static void main(String[] args) {
        ChangHong changHong = new ChangHong();
        //		OpenAndClose openAndClose = new OpenAndClose();
        //		openAndClose.open(changHong);
        // 通过构造器进行依赖传递
        //		OpenAndClose openAndClose = new OpenAndClose(changHong);
        //		openAndClose.open();
        // 通过setter方法进行依赖传递
        OpenAndClose openAndClose = new OpenAndClose();
        openAndClose.setTv(changHong);
        openAndClose.open();
    }
}

class OpenAndClose implements IOpenAndClose {

    private ITV tv;

    public void setTv(ITV tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}

class ChangHong implements ITV {

    @Override public void play() {
        System.out.println("长虹电视机，打开");
    }
}
```

#### 1.3.4.注意事项和细节

1. 低层模块尽量都要有抽象类或接口，或者两者都有，程序稳定性更好.
2. 变量的声明类型尽量是抽象类或接口, 这样我们的变量引用和实际对象间，就存在
   一个缓冲层，利于程序扩展和优化
3. 继承时遵循里氏替换原则

### 1.4.里氏替换原则

#### 1.4.1.基本介绍

所有引用基类的地方必须能透明地使用其子类的对象。

子类中尽量不要重写父类的方法。

通过聚合，组合，依赖来解决问题。

#### 1.4.2.应用实例

```java
package com.atguigu.principle.liskov.improve;

public class Liskov {

    public static void main(String[] args) {
        A a = new A();
        System.out.println("11-3=" + a.func1(11, 3));
        System.out.println("1-8=" + a.func1(1, 8));
        System.out.println("-----------------------");
        B b = new B();
        //因为B类不再继承A类，因此调用者，不会再func1是求减法
        //调用完成的功能就会很明确
        System.out.println("11+3=" + b.func1(11, 3));//这里本意是求出11+3
        System.out.println("1+8=" + b.func1(1, 8));// 1+8
        System.out.println("11+3+9=" + b.func2(11, 3));
        //使用组合仍然可以使用到A类相关方法
        System.out.println("11-3=" + b.func3(11, 3));// 这里本意是求出11-3
    }
}

//创建一个更加基础的基类
class Base {
    //把更加基础的方法和成员写到Base类
}

// A类
class A extends Base {
    // 返回两个数的差
    public int func1(int num1, int num2) {
        return num1 - num2;
    }
}

// B类继承了A
// 增加了一个新功能：完成两个数相加,然后和9求和
class B extends Base {
    //如果B需要使用A类的方法,使用组合关系
    private A a = new A();

    //这里，重写了A类的方法, 可能是无意识
    public int func1(int a, int b) {
        return a + b;
    }

    public int func2(int a, int b) {
        return func1(a, b) + 9;
    }

    //我们仍然想使用A的方法
    public int func3(int a, int b) {
        return this.a.func1(a, b);
    }
}
```

### 1.5.开闭原则（Open Closed Principle）

#### 1.5.1.基本介绍

1. 开闭原则（Open Closed Principle）是编程中最基础、最重要的设计原则
2. 一个软件实体如类，模块和函数应该对扩展开放(对提供方)，对修改关闭(对使用
   方)。用抽象构建框架，用实现扩展细节。
3. 当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已
   有的代码来实现变化。

#### 1.5.2.应用实例

```java
package com.atguigu.principle.ocp.improve;

public class Ocp {

    public static void main(String[] args) {
        //使用看看存在的问题
        GraphicEditor graphicEditor = new GraphicEditor();
        graphicEditor.drawShape(new Rectangle());
        graphicEditor.drawShape(new Circle());
        graphicEditor.drawShape(new Triangle());
        graphicEditor.drawShape(new OtherGraphic());
    }
}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
    //接收Shape对象，调用draw方法
    public void drawShape(Shape s) {
        s.draw();
    }
}

//Shape类，基类
abstract class Shape {
    int m_type;

    public abstract void draw();//抽象方法
}

class Rectangle extends Shape {
    Rectangle() {
        super.m_type = 1;
    }

    @Override public void draw() {
        System.out.println(" 绘制矩形 ");
    }
}

class Circle extends Shape {
    Circle() {
        super.m_type = 2;
    }

    @Override public void draw() {
        System.out.println(" 绘制圆形 ");
    }
}

//新增画三角形
class Triangle extends Shape {
    Triangle() {
        super.m_type = 3;
    }

    @Override public void draw() {
        System.out.println(" 绘制三角形 ");
    }
}

//新增一个图形
class OtherGraphic extends Shape {
    OtherGraphic() {
        super.m_type = 4;
    }

    @Override public void draw() {
        System.out.println(" 绘制其它图形 ");
    }
}
```

### 1.6.迪米特法则则(Demeter Principle)

#### 1.6.1.基本介绍

1. 迪米特法则(Demeter Principle)又叫最少知道原则，即一个类对自己依赖的类知道的
   越少越好。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内
   部。对外除了提供的public 方法，不对外泄露任何信息

#### 1.6.2.应用实例

```java
package com.atguigu.principle.demeter.improve;

import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter1 {

    public static void main(String[] args) {
        System.out.println("~~~使用迪米特法则的改进~~~");
        //创建了一个 SchoolManager 对象
        SchoolManager schoolManager = new SchoolManager();
        //输出学院的员工id 和  学校总部的员工信息
        schoolManager.printAllEmployee(new CollegeManager());
    }
}

//学校总部员工类
class Employee {
    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}

//学院的员工类
class CollegeEmployee {
    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}

//管理学院员工的管理类
class CollegeManager {
    //返回学院的所有员工
    public List<CollegeEmployee> getAllEmployee() {
        List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
        //这里我们增加了10个员工到 list
        for (int i = 0; i < 10; i++) {
            CollegeEmployee emp = new CollegeEmployee();
            emp.setId("学院员工id= " + i);
            list.add(emp);
        }
        return list;
    }

    //输出学院员工的信息
    public void printEmployee() {
        //获取到学院员工
        List<CollegeEmployee> list1 = getAllEmployee();
        System.out.println("------------学院员工------------");
        for (CollegeEmployee e : list1) {
            System.out.println(e.getId());
        }
    }
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
    //返回学校总部的员工
    public List<Employee> getAllEmployee() {
        List<Employee> list = new ArrayList<Employee>();
        //这里我们增加了5个员工到 list
        for (int i = 0; i < 5; i++) {
            Employee emp = new Employee();
            emp.setId("学校总部员工id= " + i);
            list.add(emp);
        }
        return list;
    }

    //该方法完成输出学校总部和学院员工信息(id)
    void printAllEmployee(CollegeManager sub) {
        //分析问题
        //1. 将输出学院的员工方法，封装到CollegeManager
        sub.printEmployee();
        //获取到学校总部员工
        List<Employee> list2 = this.getAllEmployee();
        System.out.println("------------学校总部员工------------");
        for (Employee e : list2) {
            System.out.println(e.getId());
        }
    }
}
```

#### 1.6.3.注意事项和细节

1. 迪米特法则的核心是降低类之间的耦合
2. 但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低
   类间(对象间)耦合关系， 并不是要求完全没有依赖关系

### 1.7.合成复用原则（Composite Reuse Principle）

#### 1.7.1.基本介绍
原则是尽量使用合成/聚合的方式，而不是使用继承

#### 1.7.2.核心思想

1. 找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代
   码混在一起。
2. 针对接口编程，而不是针对实现编程。
3. 为了交互对象之间的松耦合设计而努力

## 2.UML类图

### 2.1.基本介绍

1. UML——Unified modeling language UML
   (统一建模语言)，是一种用于软件系统
   分析和设计的语言工具，它用于帮助软
   件开发人员进行思考和记录思路的结果
2. UML本身是一套符号的规定，就像数学
   符号和化学符号一样，这些符号用于描
   述软件模型中的各个元素和他们之间的
   关系，比如类、接口、实现、泛化、依
   赖、组合、聚合等，如右图:
3. 使用UML来建模，常用的工具有 Rational
   Rose , 也可以使用一些插件来建模

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTViNDkxOGU2OTFiZWFmNGMucG5n?x-oss-process=image/format,png)

### 2.2.分类

1. **用例图**(use case)
2. **静态结构图**：类图、对象图、包图、组件图、部署图
3. **动态行为图**：交互图（时序图与协作图）、状态图、活动图

### 2.3.类图

1. 用于描述系统中的类(对象)本身的组成和类(对象)之间的各种静态关系。
2. 类之间的关系：依赖、泛化（继承）、实现、关联、聚合与组合

#### 2.3.1.类图—依赖关系（Dependence）

只要是在类中用到了对方，那么他们之间就存在依赖关系。如果没有对方，连编
绎都通过不了。

1. 类中用到了对方
2. 如果是类的成员属性
3. 如果是方法的返回类型
4. 是方法接收的参数类型
5. 方法中使用到

#### 2.3.2.类图—泛化关系(generalization）

泛化关系实际上就是**继承关系**，他是依赖关系的特例

#### 2.3.3.类图—实现关系（Implementation）

实现关系实际上就是A类**实现**B**接口**，他是依赖关系的特例

#### 2.3.4类图—关联关系（Association）

- 关联关系实际上就是类与类之间的联系，他是依赖关系的特例
- 关联具有导航性：即双向关系或单向关系
- 关系具有多重性：如“1”（表示有且仅有一个），“0...”（表示0个或者多个），
  “0，1”（表示0个或者一个），“n...m”(表示n到 m个都可以),“m...*”（表示至少m
  个）。

#### 2.3.5.类图—聚合关系（Aggregation）

聚合关系（Aggregation）表示的是整体和部分的关系，整体与部分可以分开。聚
合关系是关联关系的特例，所以他具有关联的导航性与多重性。

如：一台电脑由键盘(keyboard)、显示器(monitor)，鼠标等组成；组成电脑的各个
配件是可以从电脑上分离出来的，使用**带空心菱形的实线**来表示：

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTdkZTg2Y2ViMzM1MjgxNTYucG5n?x-oss-process=image/format,png)

#### 2.3.6.类图—组合关系（Composition）

- 组合关系：也是整体与部分的关系，但是整体与部分不可以分开。
- 再看一个案例：在程序中我们定义实体：Person与IDCard、Head, 那么 Head 和
  Person 就是 组合，IDCard 和 Person 就是聚合。
- 但是如果在程序中Person实体中定义了对IDCard进行**级联删除**，即删除Person时
  连同IDCard一起删除，那么IDCard 和 Person 就是**组合**了.

## 3.设计模式概述

### 3.1.设计模式类型

设计模式分为三种类型，共23种

1. **创建型模式**：单例模式、抽象工厂模式、原型模式、建造者模式、工厂模式。
2. **结构型模式**：适配器模式、桥接模式、装饰模式、组合模式、外观模式、享
   元模式、代理模式。
3. **行为型模式**：模版方法模式、命令模式、访问者模式、迭代器模式、观察者
   模式、中介者模式、备忘录模式、解释器模式（Interpreter模式）、状态模
   式、策略模式、职责链模式(责任链模式)。

## 4.单例模式

### 4.0.1.单例模式介绍

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类
**只能存在一个对象实例**，并且该类只提供一个取得其对象实例的方法(静态方法)。

### 4.0.2.单例模式八种方式：

1. 饿汉式(静态常量)
2. 饿汉式（静态代码块）
3. 懒汉式(线程不安全)
4. 懒汉式(线程安全，同步方法)
5. 懒汉式(线程安全，同步代码块)
6. 双重检查
7. 静态内部类
8. 枚举

### 4.1.饿汉式(静态常量)

#### 4.1.1.步骤如下：

1. 构造器私有化 (防止 new )
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法。getInstance
4. 代码实现

```java
//饿汉式(静态变量)
class Singleton {
    
    //2.本类内部创建对象实例
    private final static Singleton instance = new Singleton();

    //1. 构造器私有化, 外部能new
    private Singleton() {
    }

    //3. 提供一个公有的静态方法，返回实例对象
    public static Singleton getInstance() {
        return instance;
    }
}
```

####  4.1.2.优缺点说明：

1. **优点：**这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同
   步问题。
2. **缺点：**在类装载的时候就完成实例化，没有达到Lazy Loading的效果。如果从始
   至终从未使用过这个实例，则会造成内存的浪费
3. 这种方式基于classloder机制避免了多线程的同步问题，不过，instance在类装载
   时就实例化，在单例模式中大多数都是调用getInstance方法， 但是导致类装载
   的原因有很多种，因此不能确定有其他的方式（或者其他的静态方法）导致类
   装载，这时候初始化instance就没有达到lazy loading的效果
4. **结论：**这种单例模式可用，可能造成内存浪费

### 4.2.饿汉式(静态代码块)

```java
//饿汉式(静态变量)
class Singleton {

    //2.本类内部创建对象实例
    private static Singleton instance;

    static { // 在静态代码块中，创建单例对象
        instance = new Singleton();
    }

    //1. 构造器私有化, 外部能new
    private Singleton() {
    }

    //3. 提供一个公有的静态方法，返回实例对象
    public static Singleton getInstance() {
        return instance;
    }
}
```

#### 4.2.1.优缺点说明：

1. 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块
   中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。优
   缺点和上面是一样的。
2. **结论：**这种单例模式可用，但是可能造成内存浪费

### 4.3.懒汉式(线程不安全)

```java
class Singleton {
    private static Singleton instance;

    private Singleton() {
    }

    //提供一个静态的公有方法，当使用到该方法时，才去创建 instance
    //即懒汉式
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 4.3.1.优缺点说明：
1. 起到了Lazy Loading的效果，但是只能在单线程下使用。
2. 如果在多线程下，一个线程进入了if (singleton == null)判断语句块，还未来得及
   往下执行，另一个线程也通过了这个判断语句，这时便会产生多个实例。所以
   在多线程环境下不可使用这种方式
3. **结论：**在实际开发中，不要使用这种方式

### 4.4.懒汉式(线程安全，同步方法)

```java
// 懒汉式(线程安全，同步方法)
class Singleton {
    private static Singleton instance;

    private Singleton() {
    }

    //提供一个静态的公有方法，加入同步处理的代码，解决线程安全问题
    //即懒汉式
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 4.4.1.优缺点说明：
1. 解决了线程不安全问题
2. 效率太低了，每个线程在想获得类的实例时候，执行getInstance()方法都要进行
   同步。而其实这个方法只执行一次实例化代码就够了，后面的想获得该类实例，
   直接return就行了。方法进行同步效率太低
3. 结论：在实际开发中，不推荐使用这种方式

### 4.5.懒汉式(线程安全，同步代码块)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWFmMzVlYjc3MTAwNGVmZWQucG5n?x-oss-process=image/format,png)

#### 4.5.1. 优缺点说明： 

1. 这种方式，本意是想对第四种实现方式的改进，因为前面同步方法效率太低， 改为同步产生实例化的的代码块
2. 但是这种同步并不能起到线程同步的作用。跟第3种实现方式遇到的情形一 致，假如一个线程进入了if (singleton == null)判断语句块，还未来得及往下执行， 另一个线程也通过了这个判断语句，这时便会产生多个实例 
3. **结论：**在实际开发中，不能使用这种方式 

### 4.6. 双重检查  

```java
// 懒汉式(线程安全，同步方法)
class Singleton {
	private static volatile Singleton instance;

	private Singleton() {
	}

	//提供一个静态的公有方法，加入双重检查代码，解决线程安全问题, 同时解决懒加载问题
	//同时保证了效率, 推荐使用
	public static synchronized Singleton getInstance() {
		if (instance == null) {
			synchronized (Singleton.class) {
				if (instance == null) {
					instance = new Singleton();
				}
			}
		}
		return instance;
	}
}
```

#### 4.6.1.优缺点说明： 

1. Double-Check概念是多线程开发中常使用到的，如代码中所示，我们进行了两 次if (singleton == null)检查，这样就可以保证线程安全了。 
2. 这样，实例化代码只用执行一次，后面再次访问时，判断if (singleton == null)， 直接return实例化对象，也避免的反复进行方法同步. 
3. 线程安全；延迟加载；效率较高 
4. **结论：**在实际开发中，推荐使用这种单例设计模式 

### 4.7. 静态内部类 

```java
// 静态内部类完成， 推荐使用
class Singleton {
	private static volatile Singleton instance;

	//构造器私有化
	private Singleton() {
	}

	public static synchronized Singleton getInstance() {
		return SingletonInstance.INSTANCE;
	}

	//提供一个静态的公有方法，直接返回SingletonInstance.INSTANCE
	//写一个静态内部类,该类中有一个静态属性 Singleton
	private static class SingletonInstance {
		private static final Singleton INSTANCE = new Singleton();
	}
}
```

#### 4.7.1. 优缺点说明： 

1. 这种方式采用了类装载的机制来保证初始化实例时只有一个线程。 
2. 静态内部类方式在Singleton类被装载时并不会立即实例化，而是在需要实例化 时，调用getInstance方法，才会装载SingletonInstance类，从而完成Singleton的 实例化。 
3. 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们 保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。 
4. 优点：避免了线程不安全，利用静态内部类特点实现延迟加载，效率高 
5. 结论：推荐使用. 

### 4.8. 枚举 

```java
//使用枚举，可以实现单例, 推荐
enum Singleton {
	INSTANCE; //属性

	public void sayOK() {
		System.out.println("ok~");
	}
}
```

#### 4.8.1优缺点说明： 

1.  这借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而 且还能防止反序列化重新创建新的对象。 
2. 这种方式是Effective Java作者Josh Bloch 提倡的方式 
3. 结论：推荐使用 

### 4.9. 单例模式在JDK 应用的源码分析 

 我们JDK中，java.lang.Runtime就是经典的单例模式(饿汉式) 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTIwYzRiNjQ3NGQ5NWQ4ZjQucG5n?x-oss-process=image/format,png)

### 4.10. 单例模式注意事项和细节说明 

1. 单例模式保证了 系统内存中该类只存在一个对象，节省了系统资源，对于一些需 要频繁创建销毁的对象，使用单例模式可以提高系统性能 
2. 当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用new 
3. 单例模式使用的场景：需要频繁的进行创建和销毁的对象、创建对象时耗时过多或 耗费资源过多(即：**重量级对象**)，但又经常用到的对象、**工具类对象**、频繁访问数 据库或文件的对象(比如**数据源、session工厂**等) 

## 5. 工厂模式 

### 5.1.简单工厂模式

#### 5.1.1基本介绍

1. 简单工厂模式是属于创建型模式，是工厂模式的一种。**简单工厂模式是由一**
   **个工厂对象决定创建出哪一种产品类的实例**。简单工厂模式是工厂模式家族
   中最简单实用的模式
2. 简单工厂模式：定义了一个创建对象的类，由这个类来**封装实例化对象的行为**(代码)
3. 在软件开发中，当我们会用到大量的创建某种、某类或者某批对象时，就会
   使用到工厂模式.

```java
//编写简单工厂类
public class SimpleFactory {

    public static Pizza createPizza2(String orderType) {

        Pizza pizza = null;

        System.out.println("使用简单工厂模式2");
        if (orderType.equals("greek")) {
            pizza = new GreekPizza();
            pizza.setName(" 希腊披萨 ");
        } else if (orderType.equals("cheese")) {
            pizza = new CheesePizza();
            pizza.setName(" 奶酪披萨 ");
        } else if (orderType.equals("pepper")) {
            pizza = new PepperPizza();
            pizza.setName("胡椒披萨");
        }

        return pizza;
    }

    //简单工厂模式 也叫 静态工厂模式
    //更加orderType 返回对应的Pizza 对象
    public Pizza createPizza(String orderType) {

        Pizza pizza = null;

        System.out.println("使用简单工厂模式");
        if (orderType.equals("greek")) {
            pizza = new GreekPizza();
            pizza.setName(" 希腊披萨 ");
        } else if (orderType.equals("cheese")) {
            pizza = new CheesePizza();
            pizza.setName(" 奶酪披萨 ");
        } else if (orderType.equals("pepper")) {
            pizza = new PepperPizza();
            pizza.setName("胡椒披萨");
        }
        return pizza;
    }
}
```

```java
public class OrderPizza {

    //定义一个简单工厂对象
    SimpleFactory simpleFactory;
    Pizza pizza = null;

    //构造器
    public OrderPizza(SimpleFactory simpleFactory) {
        setFactory(simpleFactory);
    }

    public void setFactory(SimpleFactory simpleFactory) {
        String orderType = ""; //用户输入的

        this.simpleFactory = simpleFactory; //设置简单工厂对象

        do {
            orderType = getType();
            pizza = this.simpleFactory.createPizza(orderType);

            //输出pizza
            if (pizza != null) { //订购成功
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            } else {
                System.out.println(" 订购披萨失败 ");
                break;
            }
        } while (true);
    }

    // 写一个方法，可以获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza 种类:");
            String str = strin.readLine();
            return str;
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }

}
```

### 5.2.工厂方法模式

#### 5.2.1.基本介绍

**定义了一个创建对象的抽象方法**，由**子类决定要实例化的类**。工厂方
法模式将**对象的实例化推迟到子类**。

#### 5.2.2.应用案例

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTJiN2QzMzA5M2JkZWRhZjEucG5n?x-oss-process=image/format,png)

```java
public abstract class OrderPizza {

    // 构造器
    public OrderPizza() {
        Pizza pizza = null;
        String orderType; // 订购披萨的类型
        do {
            orderType = getType();
            pizza = createPizza(orderType); //抽象方法，由工厂子类完成
            //输出pizza 制作过程
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
        } while (true);
    }

    //定义一个抽象方法，createPizza , 让各个工厂子类自己实现
    abstract Pizza createPizza(String orderType);

    // 写一个方法，可以获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza 种类:");
            String str = strin.readLine();
            return str;
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }
}
```

```java
public class BJOrderPizza extends OrderPizza {

    @Override Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if (orderType.equals("cheese")) {
            pizza = new BJCheesePizza();
        } else if (orderType.equals("pepper")) {
            pizza = new BJPepperPizza();
        }
        return pizza;
    }
}
```

```java
public class LDOrderPizza extends OrderPizza {

    @Override Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if (orderType.equals("cheese")) {
            pizza = new LDCheesePizza();
        } else if (orderType.equals("pepper")) {
            pizza = new LDPepperPizza();
        }
        return pizza;
    }
}
```

### 5.3.抽象工厂模式

#### 5.3.1.基本介绍

1. 抽象工厂模式：定义了一个interface用于创建相关或有依赖关系的对象簇，而无需
   指明具体的类

2. 抽象工厂模式可以将简单工厂模式和工厂方法模式进行整合。

3. 从设计层面看，抽象工厂模式就是对简单工厂模式的改进(或者称为进一步的抽象)。

4. 将工厂抽象成两层，AbsFactory(抽象工厂) 和 具体实现的工厂子类。程序员可以
   根据创建对象类型使用对应的工厂子类。这样将单个的简单工厂类变成了工厂簇，

   更利于代码的维护和扩展。

5. 类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWViYzFiOWY2MTQ2OGI0MzcucG5n?x-oss-process=image/format,png)

#### 5.3.2.应用实例

```java
//一个抽象工厂模式的抽象层(接口)
public interface AbsFactory {
	//让下面的工厂子类来 具体实现
	Pizza createPizza(String orderType);
}
```

```java
//这是工厂子类
public class BJFactory implements AbsFactory {

	@Override public Pizza createPizza(String orderType) {
		System.out.println("~使用的是抽象工厂模式~");
		Pizza pizza = null;
		if (orderType.equals("cheese")) {
			pizza = new BJCheesePizza();
		} else if (orderType.equals("pepper")) {
			pizza = new BJPepperPizza();
		}
		return pizza;
	}
}
```

```java
public class LDFactory implements AbsFactory {

    @Override public Pizza createPizza(String orderType) {
        System.out.println("~使用的是抽象工厂模式~");
        Pizza pizza = null;
        if (orderType.equals("cheese")) {
            pizza = new LDCheesePizza();
        } else if (orderType.equals("pepper")) {
            pizza = new LDPepperPizza();
        }
        return pizza;
    }
}
```

```java
public class OrderPizza {

    AbsFactory factory;

    // 构造器
    public OrderPizza(AbsFactory factory) {
        setFactory(factory);
    }

    private void setFactory(AbsFactory factory) {
        Pizza pizza = null;
        String orderType = ""; // 用户输入
        this.factory = factory;
        do {
            orderType = getType();
            // factory 可能是北京的工厂子类，也可能是伦敦的工厂子类
            pizza = factory.createPizza(orderType);
            if (pizza != null) { // 订购ok
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            } else {
                System.out.println("订购失败");
                break;
            }
        } while (true);
    }

    // 写一个方法，可以获取客户希望订购的披萨种类
    private String getType() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza 种类:");
            String str = strin.readLine();
            return str;
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }
}
```

### 5.4.工厂模式在JDK-Calendar 应用的源码分析

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTdlMzQ4MjFhZjVlM2MxMDQucG5n?x-oss-process=image/format,png)

### 5.5.工厂模式小结

1. 工厂模式的意义
   将实例化对象的代码提取出来，放到一个类中统一管理和维护，达到和主项目的
   依赖关系的解耦。从而提高项目的扩展和维护性。
2. 三种工厂模式 (简单工厂模式、工厂方法模式、抽象工厂模式)
3. 设计模式的依赖抽象**原则**
   1. 创建对象实例时，不要直接 new 类, 而是把这个new 类的动作放在一个工厂的方法
      中，并返回。有的书上说，变量不要直接持有具体类的引用。
   2. 不要让类继承具体类，而是继承抽象类或者是实现interface(接口)
   3. 不要覆盖基类中已经实现的方法。

## 6.原型模式

Java中Object类是所有类的根类，Object类提供了一个clone()方法，该方法可以
将一个Java对象复制一份，但是需要实现clone的Java类必须要实现一个接口Cloneable，
该接口表示该类能够复制且具有复制的能力 => 原型模式

### 6.1.基本介绍

1) 原型模式(Prototype模式)是指：用原型实例指定创建对象的种类，并且通过拷
贝这些原型，创建新的对象
2) 原型模式是一种创建型设计模式，允许一个对象再创建另外一个可定制的对象，
无需知道如何创建的细节
3) 工作原理是:通过将一个原型对象传给那个要发动创建的对象，这个要发动创建
的对象通过请求原型对象拷贝它们自己来实施创建，即 对象.clone()
4) 形象的理解：孙大圣拔出猴毛， 变出其它孙大圣

### 6.2.原型模式-原理结构图(UML类图)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWRkYTE5ODk2NDFiMTE3MDUucG5n?x-oss-process=image/format,png)

原理结构图说明
1) Prototype : 原型类，声明一个克隆自己的接口
2) ConcretePrototype: 具体的原型类, 实现一个克隆自己的操作
3) Client: 让一个原型对象克隆自己，从而创建一个新的对象(属性一样)

### 6.3.原型模式解决克隆羊问题的应用实例

```java
public class Sheep implements Cloneable {
    public Sheep friend; //是对象, 克隆是会如何处理
    private String name;
    private int age;
    private String color;
    private String address = "蒙古羊";

    public Sheep(String name, int age, String color) {
        super();
        this.name = name;
        this.age = age;
        this.color = color;
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

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    @Override public String toString() {
        return "Sheep [name=" + name + ", age=" + age + ", color=" + color + ", address=" + address + "]";
    }

    //克隆该实例，使用默认的clone方法来完成
    @Override protected Object clone() {
        Sheep sheep = null;
        try {
            sheep = (Sheep)super.clone();
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
        return sheep;
    }
}
```

```java
public class Client {

    public static void main(String[] args) {
        System.out.println("原型模式完成对象的创建");
        Sheep sheep = new Sheep("tom", 1, "白色");
        sheep.friend = new Sheep("jack", 2, "黑色");
        Sheep sheep2 = (Sheep)sheep.clone(); //克隆
        Sheep sheep3 = (Sheep)sheep.clone(); //克隆
        Sheep sheep4 = (Sheep)sheep.clone(); //克隆
        Sheep sheep5 = (Sheep)sheep.clone(); //克隆
        System.out.println("sheep2 =" + sheep2 + "sheep2.friend=" + sheep2.friend.hashCode());
        System.out.println("sheep3 =" + sheep3 + "sheep3.friend=" + sheep3.friend.hashCode());
        System.out.println("sheep4 =" + sheep4 + "sheep4.friend=" + sheep4.friend.hashCode());
        System.out.println("sheep5 =" + sheep5 + "sheep5.friend=" + sheep5.friend.hashCode());
    }
}
```

### 6.4.原型模式在Spring框架中源码分析

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWVjOTU3ZDNlN2I5Y2ZmZjIucG5n?x-oss-process=image/format,png)

### 6.5.深入讨论-浅拷贝和深拷贝

#### 6.5.1浅拷贝的介绍
1) 对于数据类型是基本数据类型的成员变量，浅拷贝会直接进行值传递，也就是将
该属性值复制一份给新的对象。
2) 对于数据类型是引用数据类型的成员变量，比如说成员变量是某个数组、某个类
的对象等，那么浅拷贝会进行引用传递，也就是只是将该成员变量的引用值（内
存地址）复制一份给新的对象。因为实际上两个对象的该成员变量都指向同一个
实例。在这种情况下，在一个对象中修改该成员变量会影响到另一个对象的该成
员变量值
3) 前面我们克隆羊就是浅拷贝
4) 浅拷贝是使用默认的 clone()方法来实现
sheep = (Sheep) super.clone(); 

#### 6.5.2.深拷贝基本介绍
1) 复制对象的所有基本数据类型的成员变量值
2) 为所有引用数据类型的成员变量申请存储空间，并复制每个引用数据类型成员变
量所引用的对象，直到该对象可达的所有对象。也就是说，对象进行深拷贝要对
整个对象进行拷贝
3) 深拷贝实现方式1：重写clone方法来实现深拷贝
4) 深拷贝实现方式2：通过对象序列化实现深拷贝(推荐)

#### 6.5.3.深拷贝应用实例

```java
public class DeepCloneableTarget implements Serializable, Cloneable {
    private static final long serialVersionUID = 1L;

    private String cloneName;

    private String cloneClass;

    //构造器
    public DeepCloneableTarget(String cloneName, String cloneClass) {
        this.cloneName = cloneName;
        this.cloneClass = cloneClass;
    }

    //因为该类的属性，都是String , 因此我们这里使用默认的clone完成即可
    @Override protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

```java
public class DeepProtoType implements Serializable, Cloneable {

    public String name; //String 属性
    public DeepCloneableTarget deepCloneableTarget;// 引用类型

    public DeepProtoType() {
        super();
    }

    //深拷贝 - 方式 1 使用clone 方法
    @Override protected Object clone() throws CloneNotSupportedException {
        Object deep = null;
        //这里完成对基本数据类型(属性)和String的克隆
        deep = super.clone();
        //对引用类型的属性，进行单独处理
        DeepProtoType deepProtoType = (DeepProtoType)deep;
        deepProtoType.deepCloneableTarget = (DeepCloneableTarget)deepCloneableTarget.clone();
        return deepProtoType;
    }

    //深拷贝 - 方式2 通过对象的序列化实现 (推荐)
    public Object deepClone() {
        //创建流对象
        ByteArrayOutputStream bos = null;
        ObjectOutputStream oos = null;
        ByteArrayInputStream bis = null;
        ObjectInputStream ois = null;
        try {
            //序列化
            bos = new ByteArrayOutputStream();
            oos = new ObjectOutputStream(bos);
            oos.writeObject(this); //当前这个对象以对象流的方式输出
            //反序列化
            bis = new ByteArrayInputStream(bos.toByteArray());
            ois = new ObjectInputStream(bis);
            DeepProtoType copyObj = (DeepProtoType)ois.readObject();
            return copyObj;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        } finally {
            //关闭流
            try {
                bos.close();
                oos.close();
                bis.close();
                ois.close();
            } catch (Exception e2) {
                System.out.println(e2.getMessage());
            }
        }
    }
}
```

```java
public class Client {

    public static void main(String[] args) throws Exception {
        DeepProtoType p = new DeepProtoType();
        p.name = "宋江";
        p.deepCloneableTarget = new DeepCloneableTarget("大牛", "小牛");
        //方式1 完成深拷贝
        //		DeepProtoType p2 = (DeepProtoType) p.clone();
        //		System.out.println("p.name=" + p.name + "p.deepCloneableTarget=" + p.deepCloneableTarget.hashCode());
        //		System.out.println("p2.name=" + p.name + "p2.deepCloneableTarget=" + p2.deepCloneableTarget.hashCode());
        //方式2 完成深拷贝
        DeepProtoType p2 = (DeepProtoType)p.deepClone();
        System.out.println("p.name=" + p.name + "p.deepCloneableTarget=" + p.deepCloneableTarget.hashCode());
        System.out.println("p2.name=" + p.name + "p2.deepCloneableTarget=" + p2.deepCloneableTarget.hashCode());
    }
}
```

### 6.6.原型模式的注意事项和细节

1) 创建新的对象比较复杂时，可以利用原型模式简化对象的创建过程，同时也能够提
高效率
2) 不用重新初始化对象，而是动态地获得对象运行时的状态
3) 如果原始对象发生变化(增加或者减少属性)，其它克隆对象的也会发生相应的变化，
无需修改代码
4) 在实现深克隆的时候可能需要比较复杂的代码
5) 缺点：需要为每一个类配备一个克隆方法，这对全新的类来说不是很难，但对已有
的类进行改造时，需要修改其源代码，违背了ocp原则，这点请同学们注意.

## 7.建造者模式

### 7.1.基本介绍

1) 建造者模式（Builder Pattern） 又叫生成器模式，是一种对象构建模式。它可以
将复杂对象的建造过程抽象出来（抽象类别），使这个抽象过程的不同实现方
法可以构造出不同表现（属性）的对象。
2) 建造者模式 是一步一步创建一个复杂的对象，它允许用户只通过指定复杂对象
的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。

### 7.2.建造者模式的四个角色

1) Product（产品角色）： 一个具体的产品对象。
2) Builder（抽象建造者）： 创建一个Product对象的各个部件指定的 接口/抽象类。
3) ConcreteBuilder（具体建造者）： 实现接口，构建和装配各个部件。
4) Director（指挥者）： 构建一个使用Builder接口的对象。它主要是用于创建一个
复杂的对象。它主要有两个作用，一是：隔离了客户与对象的生产过程，二是：
负责控制产品对象的生产过程。

### 7.3.建造者模式原理类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTM2OGI2MWFkMmJhM2JiNWQucG5n?x-oss-process=image/format,png)

### 7.4.建造者模式解决盖房需求应用实例

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWUxMTQ2MzM3NGRjYjg1ZTYucG5n?x-oss-process=image/format,png)

```java
public class Client {
    public static void main(String[] args) {

        //盖普通房子
        CommonHouse commonHouse = new CommonHouse();
        //准备创建房子的指挥者
        HouseDirector houseDirector = new HouseDirector(commonHouse);
        //完成盖房子，返回产品(普通房子)
        House house = houseDirector.constructHouse();
        //System.out.println("输出流程");
        System.out.println("--------------------------");
        //盖高楼
        HighBuilding highBuilding = new HighBuilding();
        //重置建造者
        houseDirector.setHouseBuilder(highBuilding);
        //完成盖房子，返回产品(高楼)
        houseDirector.constructHouse();
    }
}
```

```java
public class CommonHouse extends HouseBuilder {

    @Override public void buildBasic() {
        System.out.println(" 普通房子打地基5米 ");
    }

    @Override public void buildWalls() {
        System.out.println(" 普通房子砌墙10cm ");
    }

    @Override public void roofed() {
        System.out.println(" 普通房子屋顶 ");
    }
}
```

```java
//产品->Product
public class House {
    private String baise;
    private String wall;
    private String roofed;

    public String getBaise() {
        return baise;
    }

    public void setBaise(String baise) {
        this.baise = baise;
    }

    public String getWall() {
        return wall;
    }

    public void setWall(String wall) {
        this.wall = wall;
    }

    public String getRoofed() {
        return roofed;
    }

    public void setRoofed(String roofed) {
        this.roofed = roofed;
    }
}
```

```java
// 抽象的建造者
public abstract class HouseBuilder {

    protected House house = new House();

    //将建造的流程写好, 抽象的方法
    public abstract void buildBasic();

    public abstract void buildWalls();

    public abstract void roofed();

    //建造房子好， 将产品(房子) 返回
    public House buildHouse() {
        return house;
    }

}
```

```java
//指挥者，这里去指定制作流程，返回产品
public class HouseDirector {

    HouseBuilder houseBuilder = null;

    //构造器传入 houseBuilder
    public HouseDirector(HouseBuilder houseBuilder) {
        this.houseBuilder = houseBuilder;
    }

    //通过setter 传入 houseBuilder
    public void setHouseBuilder(HouseBuilder houseBuilder) {
        this.houseBuilder = houseBuilder;
    }

    //如何处理建造房子的流程，交给指挥者
    public House constructHouse() {
        houseBuilder.buildBasic();
        houseBuilder.buildWalls();
        houseBuilder.roofed();
        return houseBuilder.buildHouse();
    }
}
```

### 7.5.建造者模式在JDK的应用和源码分析

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTFkYmFhY2ZmYzBmODEyNjUucG5n?x-oss-process=image/format,png)

3) 源码中建造者模式角色分析

- Appendable 接口定义了多个append方法(抽象方法), 即Appendable 为抽象建
  造者, 定义了抽象方法
- AbstractStringBuilder 实现了 Appendable 接口方法，这里的
  AbstractStringBuilder 已经是建造者，只是不能实例化
- StringBuilder 即充当了指挥者角色，同时充当了具体的建造者，建造方法的
  实现是由 AbstractStringBuilder 完成, 而StringBuilder 继承了
  AbstractStringBuilder

### 7.6.建造者模式的注意事项和细节

1) 客户端(使用程序)**不必知道产品内部组成的细节，将产品本身与产品的创建过程解**
**耦，使得相同的创建过程可以创建不同的产品对象**
2) 每一个具体建造者都相对独立，而与其他的具体建造者无关，因此可以很方便地替
换具体建造者或增加新的具体建造者， **用户使用不同的具体建造者即可得到不同**
**的产品对象**
3) **可以更加精细地控制产品的创建过程 。**将复杂产品的创建步骤分解在不同的方法
中，使得创建过程更加清晰，也更方便使用程序来控制创建过程
4) **增加新的具体建造者无须修改原有类库的代码，**指挥者类针对抽象建造者类编程，
系统扩展方便，符合 **“开闭原则”**

5) 建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，**如果产品之间**
**的差异性很大，则不适合使用建造者模式，**因此其使用范围受到一定的限制。
6) 如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，
导致系统变得很庞大，因此在这种情况下，要考虑是否选择建造者模式.
7) **抽象工厂模式VS建造者模式**
抽象工厂模式实现对产品家族的创建，一个产品家族是这样的一系列产品：具有不
同分类维度的产品组合，采用抽象工厂模式不需要关心构建过程，只关心什么产品
由什么工厂生产即可。而建造者模式则是要求按照指定的蓝图建造产品，它的主要
目的是通过组装零配件而产生一个新产品

## 8.适配器模式

### 8.1.基本介绍
1) 适配器模式(Adapter Pattern)将某个类的接口转换成客户端期望的另一个接口表
示，主的目的是兼容性，让原本因接口不匹配不能一起工作的两个类可以协同
工作。其别名为包装器(Wrapper)
2) 适配器模式属于结构型模式
3) 主要分为三类：**类适配器模式、对象适配器模式、接口适配器模式**

### 8.2.工作原理
1) 适配器模式：将一个类的接口转换成另一种接口.让原本接口不兼容的类可以兼
容
2) 从用户的角度看不到被适配者，是解耦的
3) 用户调用适配器转化出来的目标接口方法，适配器再调用被适配者的相关接口
方法
4) 用户收到反馈结果，感觉只是和目标接口交互，如图

### 8.3.类适配器模式

#### 8.3.1.基本介绍：

Adapter类，通过继承 src类，实现 dst 类接口，完成src->dst的适配。

#### 8.3.2应用实例
1) 应用实例说明
以生活中充电器的例子来讲解适配器，充电器本身相当于Adapter，220V交流电
相当于src (即被适配者)，我们的目dst(即 目标)是5V直流电
2) 思路分析(类图)
3) 代码实现

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTkzYWE1ODA0M2VjODU4N2QucG5n?x-oss-process=image/format,png)

```java
public class Client {

    public static void main(String[] args) {
        System.out.println(" === 类适配器模式 ====");
        Phone phone = new Phone();
        phone.charging(new VoltageAdapter());
    }

}
```

```java
//适配接口
public interface IVoltage5V {
	public int output5V();
}
```

```java
public class Phone {
    //充电
    public void charging(IVoltage5V iVoltage5V) {
        if (iVoltage5V.output5V() == 5) {
            System.out.println("电压为5V, 可以充电~~");
        } else if (iVoltage5V.output5V() > 5) {
            System.out.println("电压大于5V, 不能充电~~");
        }
    }
}
```

```java
//被适配的类
public class Voltage220V {
	//输出220V的电压
	public int output220V() {
		int src = 220;
		System.out.println("电压=" + src + "伏");
		return src;
	}
}
```

```java
//适配器类
public class VoltageAdapter extends Voltage220V implements IVoltage5V {
    @Override public int output5V() {
        // 获取到220V电压
        int srcV = output220V();
        // 转成 5v
        int dstV = srcV / 44;
        return dstV;
    }
}
```

#### 8.3.3.类适配器模式注意事项和细节
1) Java是单继承机制，所以类适配器需要继承src类这一点算是一个缺点, 因为这要
求dst必须是接口，有一定局限性;
2) src类的方法在Adapter中都会暴露出来，也增加了使用的成本。
3) 由于其继承了src类，所以它可以根据需求重写src类的方法，使得Adapter的灵
活性增强了。

### 8.4.对象适配器模式

#### 8.4.1.对象适配器模式介绍
1) 基本思路和类的适配器模式相同，只是将Adapter类作修改，不是继承src类，而
是持有src类的实例，以解决兼容性的问题。 即：持有 src类，实现 dst 类接口，
完成src->dst的适配
2) 根据**“合成复用原则”**，在系统中尽量使用**关联关系**来替代**继承关系**。
3) 对象适配器模式是适配器模式常用的一种

#### 8.4.2.应用实例
1) 应用实例说明
以生活中充电器的例子来讲解适配器，充电器本身相当于Adapter，220V交流电
相当于src (即被适配者)，我们的目dst(即目标)是5V直流电，使用**对象适配器模**
**式完成。**
2) 思路分析(类图)：只需修改适配器即可, 如下:

```java
public class VoltageAdapter2 implements Voltage5 {
	private Voltage220 voltage220; //持有Voltage220对象,不是继承了
}
```

3) 代码实现

```java
public class Client {

    public static void main(String[] args) {
        System.out.println(" === 对象适配器模式 ====");
        Phone phone = new Phone();
        phone.charging(new VoltageAdapter(new Voltage220V()));
    }
}
```

```java
//适配接口
public interface IVoltage5V {
	public int output5V();
}
```

```java
public class Phone {

    //充电
    public void charging(IVoltage5V iVoltage5V) {
        if (iVoltage5V.output5V() == 5) {
            System.out.println("电压为5V, 可以充电~~");
        } else if (iVoltage5V.output5V() > 5) {
            System.out.println("电压大于5V, 不能充电~~");
        }
    }
}
```

```java
//被适配的类
public class Voltage220V {
    //输出220V的电压，不变
    public int output220V() {
        int src = 220;
        System.out.println("电压=" + src + "伏");
        return src;
    }
}
```

```java
/**
 * 适配器类
 */
public class VoltageAdapter implements IVoltage5V {

    /**
     * 关联关系-聚合
     */
    private Voltage220V voltage220V;

    /**
     * 通过构造器，传入一个 Voltage220V 实例
     *
     * @param voltage220v
     */
    public VoltageAdapter(Voltage220V voltage220v) {
        this.voltage220V = voltage220v;
    }

    @Override public int output5V() {
        int dst = 0;
        if (null != voltage220V) {
            int src = voltage220V.output220V();//获取220V 电压
            System.out.println("使用对象适配器，进行适配~~");
            dst = src / 44;
            System.out.println("适配完成，输出的电压为=" + dst);
        }
        return dst;
    }
}
```

#### 8.4.3.对象适配器模式注意事项和细节
1) 对象适配器和类适配器其实算是同一种思想，只不过实现方式不同。
根据合成复用原则，使用组合替代继承， 所以它解决了类适配器必须继承src的
局限性问题，也不再要求dst必须是接口。
2) 使用成本更低，更灵活。

### 8.5.接口适配器模式

#### 8.5.1.接口适配器模式介绍
1) 一些书籍称为：适配器模式(Default Adapter Pattern)或缺省适配器模式。
2) 当不需要全部实现接口提供的方法时，可先设计一个抽象类实现接口，并为该接
口中每个方法提供一个默认实现（空方法），那么该抽象类的子类可有选择地覆
盖父类的某些方法来实现需求
3) 适用于一个接口不想使用其所有的方法的情况。

#### 8.5.2.接口适配器模式应用实例

1) Android中的属性动画ValueAnimator类可以
通过addListener(AnimatorListener listener)方
法添加监听器， 那么常规写法如右：
2) 有时候我们不想实现
Animator.AnimatorListener接口的全部方法，
我们只想监听onAnimationStart，我们会如
下写

```java
ValueAnimator valueAnimator=ValueAnimator.ofInt(0,100);
valueAnimator.addListener(new AnimatorListenerAdapter(){
    @Override public void onAnimationStart(Animator animation){
        //xxxx具体实现
    }
});
valueAnimator.start();
```

```java
ValueAnimator valueAnimator = ValueAnimator.ofInt(0,100);
valueAnimator.addListener(new Animator.AnimatorListener() {
    @Override
    public void onAnimationStart(Animator animation) {
    }
    @Override
    public void onAnimationEnd(Animator animation) {
    }
    @Override
    public void onAnimationCancel(Animator animation) {
    }
    @Override
    public void onAnimationRepeat(Animator animation) {
    }
});
valueAnimator.start();
```

3) AnimatorListenerAdapter类，就是一个
接口适配器，代码如右图:它空实现了
Animator.AnimatorListener类(src)的所
有方法.
4) AnimatorListener是一个接口. 

```java
public static interface AnimatorListener {
void onAnimationStart(Animator animation);
void onAnimationEnd(Animator animation);
void onAnimationCancel(Animator animation);
void onAnimationRepeat(Animator animation);
}
```

```java
public abstract class AnimatorListenerAdapter implements Animator.AnimatorListener
Animator.AnimatorPauseListener {
    @Override //默认实现
    public void onAnimationCancel(Animator animation) {
    }
    @Override
    public void onAnimationEnd(Animator animation) {
    }
    @Override
    public void onAnimationRepeat(Animator animation) {
    }
    @Override
    public void onAnimationStart(Animator animation) {
    }
    @Override
    public void onAnimationPause(Animator animation) {
    }
    @Override
    public void onAnimationResume(Animator animation) {
    }
}
```

5) 程序里的匿名内部类就是Listener 具体实现类

```java
new AnimatorListenerAdapter() {
    @Override
    public void onAnimationStart(Animator animation) {
        //xxxx具体实现
    }
}
```

6)案例说明

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWRhZjg3OTQ5YzNjOTJmMjYucG5n?x-oss-process=image/format,png)

```java
public interface Interface4 {
    void m1();

    void m2();

    void m3();

    void m4();
}
```

```java
//在AbsAdapter 我们将 Interface4 的方法进行默认实现
public abstract class AbsAdapter implements Interface4 {

    //默认实现
    public void m1() {
    }

    public void m2() {
    }

    public void m3() {
    }

    public void m4() {
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        AbsAdapter absAdapter = new AbsAdapter() {
            //只需要去覆盖我们 需要使用 接口方法
            @Override public void m1() {
                System.out.println("使用了m1的方法");
            }
        };
        absAdapter.m1();
    }
}
```

### 8.6.适配器模式在SpringMVC框架应用的源码分析

1) SpringMvc中的HandlerAdapter, 就使用了适配器模式
2) SpringMVC处理请求的流程回顾
3) 使用HandlerAdapter 的原因分析:
可以看到处理器的类型不同，有多重实现方式，那么调用方式就不是确定的，如果需要直接调用
Controller方法，需要调用的时候就得不断是使用if else来进行判断是哪一种子类然后执行。那么
如果后面要扩展Controller，就得修改原来的代码，这样违背了OCP原则。

4) 代码分析+Debug源码

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTg4MTNiZWRkZWRkN2Q1M2EucG5n?x-oss-process=image/format,png)

5) 动手写SpringMVC通过适配器设计模式获取到对应的Controller的源码
说明：
• Spring定义了一个适配接口，使得每一种Controller有一种对应的适配器实现类
• 适配器代替controller执行相应的方法
• 扩展Controller 时，只需要增加一个适配器类就完成了SpringMVC的扩展了,
• 这就是设计模式的力量

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTc4NmM0YTNhYmVmYzlmMWQucG5n?x-oss-process=image/format,png)

```java
public class DispatchServlet {

    public static List<HandlerAdapter> handlerAdapters = new ArrayList<HandlerAdapter>();

    public DispatchServlet() {
        handlerAdapters.add(new AnnotationHandlerAdapter());
        handlerAdapters.add(new HttpHandlerAdapter());
        handlerAdapters.add(new SimpleHandlerAdapter());
    }

    public static void main(String[] args) {
        new DispatchServlet().doDispatch(); // http...
    }

    public void doDispatch() {
        // 此处模拟SpringMVC从request取handler的对象，
        // 适配器可以获取到希望的Controller
        HttpController controller = new HttpController();
        // AnnotationController controller = new AnnotationController();
        //SimpleController controller = new SimpleController();
        // 得到对应适配器
        HandlerAdapter adapter = getHandler(controller);
        // 通过适配器执行对应的controller对应方法
        adapter.handle(controller);

    }

    public HandlerAdapter getHandler(Controller controller) {
        // 遍历：根据得到的controller(handler), 返回对应适配器
        for (HandlerAdapter adapter : handlerAdapters) {
            if (adapter.supports(controller)) {
                return adapter;
            }
        }
        return null;
    }
}
```

```java
///定义一个Adapter接口 
public interface HandlerAdapter {
    boolean supports(Object handler);
    void handle(Object handler);
}

// 多种适配器类
class SimpleHandlerAdapter implements HandlerAdapter {

    public void handle(Object handler) {
        ((SimpleController) handler).doSimplerHandler();
    }

    public boolean supports(Object handler) {
        return (handler instanceof SimpleController);
    }
}

class HttpHandlerAdapter implements HandlerAdapter {

    public void handle(Object handler) {
        ((HttpController) handler).doHttpHandler();
    }

    public boolean supports(Object handler) {
        return (handler instanceof HttpController);
    }
}

class AnnotationHandlerAdapter implements HandlerAdapter {

    public void handle(Object handler) {
        ((AnnotationController) handler).doAnnotationHandler();
    }

    public boolean supports(Object handler) {

        return (handler instanceof AnnotationController);
    }
}
```

```java
//多种Controller实现  
public interface Controller {
}

class HttpController implements Controller {
    public void doHttpHandler() {
        System.out.println("http...");
    }
}

class SimpleController implements Controller {
    public void doSimplerHandler() {
        System.out.println("simple...");
    }
}

class AnnotationController implements Controller {
    public void doAnnotationHandler() {
        System.out.println("annotation...");
    }
}
```

### 8.7.适配器模式的注意事项和细节

1) 三种命名方式，是根据 src是以怎样的形式给到Adapter（在Adapter里的形式）来
命名的。
2) 类适配器：以类给到，在Adapter里，就是将src当做类，继承
对象适配器：以对象给到，在Adapter里，将src作为一个对象，持有
接口适配器：以接口给到，在Adapter里，将src作为一个接口，实现
3) Adapter模式最大的作用还是将原本不兼容的接口融合在一起工作。
4) 实际开发中，实现起来不拘泥于我们讲解的三种经典形式

## 9.桥接模式

### 9.1.基本介绍
1) 桥接模式(Bridge模式)是指：将实现与抽象放在两个不同的类层次中，使两个层
次可以独立改变。
2) 是一种结构型设计模式
3) Bridge模式基于类的最小设计原则，通过使用封装、聚合及继承等行为让不同
的类承担不同的职责。它的主要特点是把抽象(Abstraction)与行为实现
(Implementation)分离开来，从而可以保持各部分的独立性以及应对他们的功能
扩展

### 9.2.桥接模式原理类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWQ4YjBmYzBlYWI5M2E5MGUucG5n?x-oss-process=image/format,png)

原理类图说明：
1) Client类：桥接模式的调用者
2) 抽象类(Abstraction) :维护了 Implementor / 即它的实现类ConcreteImplementorA.., 二者是聚合关系, Abstraction 充当
桥接类

### 9.3.桥接模式解决手机操作问题

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTcwZTg4NGUxNjdkNDVmZjMucG5n?x-oss-process=image/format,png)

```java
//接口
public interface Brand {
    void open();

    void close();

    void call();
}
```

```java
public class Client {
    public static void main(String[] args) {
        //获取折叠式手机 (样式 + 品牌 )
        Phone phone1 = new FoldedPhone(new XiaoMi());
        phone1.open();
        phone1.call();
        phone1.close();
        System.out.println("=======================");
        Phone phone2 = new FoldedPhone(new Vivo());
        phone2.open();
        phone2.call();
        phone2.close();
        System.out.println("==============");
        UpRightPhone phone3 = new UpRightPhone(new XiaoMi());
        phone3.open();
        phone3.call();
        phone3.close();
        System.out.println("==============");
        UpRightPhone phone4 = new UpRightPhone(new Vivo());
        phone4.open();
        phone4.call();
        phone4.close();
    }
}
```

```java
//折叠式手机类，继承 抽象类 Phone
public class FoldedPhone extends Phone {
    //构造器
    public FoldedPhone(Brand brand) {
        super(brand);
    }

    public void open() {
        super.open();
        System.out.println(" 折叠样式手机 ");
    }

    public void close() {
        super.close();
        System.out.println(" 折叠样式手机 ");
    }

    public void call() {
        super.call();
        System.out.println(" 折叠样式手机 ");
    }
}
```

```java
public abstract class Phone {

    //组合品牌
    private Brand brand;

    //构造器
    public Phone(Brand brand) {
        super();
        this.brand = brand;
    }

    protected void open() {
        this.brand.open();
    }

    protected void close() {
        brand.close();
    }

    protected void call() {
        brand.call();
    }

}
```

```java
public class UpRightPhone extends Phone {
    //构造器
    public UpRightPhone(Brand brand) {
        super(brand);
    }

    public void open() {
        super.open();
        System.out.println(" 直立样式手机 ");
    }

    public void close() {
        super.close();
        System.out.println(" 直立样式手机 ");
    }

    public void call() {
        super.call();
        System.out.println(" 直立样式手机 ");
    }
}
```

```java
public class Vivo implements Brand {

    @Override public void open() {
        System.out.println(" Vivo手机开机 ");
    }

    @Override public void close() {
        System.out.println(" Vivo手机关机 ");
    }

    @Override public void call() {
        System.out.println(" Vivo手机打电话 ");
    }
}
```

```java
public class XiaoMi implements Brand {

    @Override public void open() {
        System.out.println(" 小米手机开机 ");
    }

    @Override public void close() {
        System.out.println(" 小米手机关机 ");
    }

    @Override public void call() {
        System.out.println(" 小米手机打电话 ");
    }
}
```

### 9.4.桥接模式在JDBC的源码剖析

1) Jdbc 的 Driver接口，如果从桥接模式来看，Driver就是一个接口，下面可以有
MySQL的Driver，Oracle的Driver，这些就可以当做实现接口类
2) 代码分析+Debug源码

```java
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    static {
        try {
            //1.注册驱动
            //2. 调用DriverManager中的getConnection
            DriverManager.registerDriver(new Driver());
        } catch (SQLException var1) {
            throw new RuntimeException("Can't register driver!");
        }
    }

    public Driver() throws SQLException {
    }
}..
```

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWZlMDRiNzEyZmE1NDUwZGUucG5n?x-oss-process=image/format,png)

```java
public class ConnectionImpl extends ConnectionPropertiesImpl implements MySQLConnection
```

说明：
1. MySQL有自己的ConnectionImpl类，同样Oracle也有对应的实现类
2. Driver和Connection之间是通过DriverManager类进行桥连接的
3. 简单示意图[待]

### 9.5.桥接模式的注意事项和细节
1) 实现了抽象和实现部分的分离，从而极大的提供了系统的灵活性，让抽象部分和实
现部分独立开来，这有助于系统进行分层设计，从而产生更好的结构化系统。
2) 对于系统的高层部分，只需要知道抽象部分和实现部分的接口就可以了，其它的部
分由具体业务来完成。
3) 桥接模式替代多层继承方案，可以减少子类的个数，降低系统的管理和维护成本。
4) 桥接模式的引入增加了系统的理解和设计难度，由于聚合关联关系建立在抽象层，
要求开发者针对抽象进行设计和编程
5) 桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围有一定的局
限性，即需要有这样的应用场景。

### 9.6.桥接模式其它应用场景
1) 对于那些不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统，桥
接模式尤为适用.
2) 常见的应用场景:
-JDBC驱动程序
-银行转账系统
转账分类: 网上转账，柜台转账，AMT转账
转账用户类型：普通用户，银卡用户，金卡用户..
-消息管理
消息类型：即时消息，延时消息
消息分类：手机短信，邮件消息，QQ消息...

## 10.装饰者设计模式

### 10.1.装饰者模式定义
1) 装饰者模式：动态的将新功能附加到对象上。在对象功能扩展方面，它比继承更
有弹性，装饰者模式也体现了开闭原则(ocp)
2) 这里提到的动态的将新功能附加到对象和ocp原则，在后面的应用实例上会以代
码的形式体现，请同学们注意体会。

### 10.2.装饰者模式原理
1) 装饰者模式就像打包一个快递
\> 主体：比如：陶瓷、衣服 (Component) // 被装饰者
\> 包装：比如：报纸填充、塑料泡沫、纸板、木板(Decorator)
2) Component
主体：比如类似前面的Drink
3) ConcreteComponent和Decorator
ConcreteComponent：具体的主体，
比如前面的各个单品咖啡
Decorator: 装饰者，比如各调料.
4) 在如图的Component与ConcreteComponent之间，如果
ConcreteComponent类很多,还可以设计一个缓冲层，将共有的部分提取出来，
抽象层一个类。

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWEwOWNmMDg3MDZkY2ViYTYucG5n?x-oss-process=image/format,png)

### 10.3.用装饰者模式设计的方案

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLThiNmJlNDg4NDYxNTQzNDMucG5n?x-oss-process=image/format,png)

### 10.4.装饰者模式咖啡订单项目应用实例

```java
//具体的Decorator， 这里就是调味品
public class Chocolate extends Decorator {

    public Chocolate(Drink obj) {
        super(obj);
        setDes(" 巧克力 ");
        setPrice(3.0f); // 调味品 的价格
    }
}
```

```java
public class Coffee extends Drink {

    @Override public float cost() {
        return super.getPrice();
    }
}
```

```java
public class CoffeeBar {

    public static void main(String[] args) {
        // 装饰者模式下的订单：2份巧克力+一份牛奶的LongBlack
        // 1. 点一份 LongBlack
        Drink order = new LongBlack();
        System.out.println("费用1=" + order.cost());
        System.out.println("描述=" + order.getDes());
        // 2. order 加入一份牛奶
        order = new Milk(order);
        System.out.println("order 加入一份牛奶 费用 =" + order.cost());
        System.out.println("order 加入一份牛奶 描述 = " + order.getDes());
        // 3. order 加入一份巧克力
        order = new Chocolate(order);
        System.out.println("order 加入一份牛奶 加入一份巧克力  费用 =" + order.cost());
        System.out.println("order 加入一份牛奶 加入一份巧克力 描述 = " + order.getDes());
        // 3. order 加入一份巧克力
        order = new Chocolate(order);
        System.out.println("order 加入一份牛奶 加入2份巧克力   费用 =" + order.cost());
        System.out.println("order 加入一份牛奶 加入2份巧克力 描述 = " + order.getDes());
        System.out.println("===========================");
        Drink order2 = new DeCaf();
        System.out.println("order2 无因咖啡  费用 =" + order2.cost());
        System.out.println("order2 无因咖啡 描述 = " + order2.getDes());
        order2 = new Milk(order2);
        System.out.println("order2 无因咖啡 加入一份牛奶  费用 =" + order2.cost());
        System.out.println("order2 无因咖啡 加入一份牛奶 描述 = " + order2.getDes());
    }
}
```

```java
public class DeCaf extends Coffee {

    public DeCaf() {
        setDes(" 无因咖啡 ");
        setPrice(1.0f);
    }
}
```

```java
public class Decorator extends Drink {
    private Drink obj;

    public Decorator(Drink obj) { //组合
        this.obj = obj;
    }

    @Override public float cost() {
        // getPrice 自己价格
        return super.getPrice() + obj.cost();
    }

    @Override public String getDes() {
        // obj.getDes() 输出被装饰者的信息
        return des + " " + getPrice() + " && " + obj.getDes();
    }
}
```

```java
public abstract class Drink {

    public String des; // 描述
    private float price = 0.0f;

    public String getDes() {
        return des;
    }

    public void setDes(String des) {
        this.des = des;
    }

    public float getPrice() {
        return price;
    }

    public void setPrice(float price) {
        this.price = price;
    }

    //计算费用的抽象方法
    //子类来实现
    public abstract float cost();

}
```

```java
public class Espresso extends Coffee {

    public Espresso() {
        setDes(" 意大利咖啡 ");
        setPrice(6.0f);
    }
}
```

```java
public class LongBlack extends Coffee {

    public LongBlack() {
        setDes(" longblack ");
        setPrice(5.0f);
    }
}
```

```java
public class Milk extends Decorator {

    public Milk(Drink obj) {
        super(obj);
        setDes(" 牛奶 ");
        setPrice(2.0f);
    }
}
```

```java
public class ShortBlack extends Coffee {

    public ShortBlack() {
        setDes(" shortblack ");
        setPrice(4.0f);
    }
}
```

```java
public class Soy extends Decorator{

	public Soy(Drink obj) {
		super(obj);
		setDes(" 豆浆  ");
		setPrice(1.5f);
	}
}
```

### 10.5.装饰者模式在JDK应用的源码分析

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTMwMGU0ODZmMjBmN2Y3YmQucG5n?x-oss-process=image/format,png)

```java
public class Decorator {

    public static void main(String[] args) throws Exception {
        //说明
        //1. InputStream 是抽象类, 类似我们前面讲的 Drink
        //2. FileInputStream 是  InputStream 子类，类似我们前面的 DeCaf, LongBlack
        //3. FilterInputStream  是  InputStream 子类：类似我们前面 的 Decorator 修饰者
        //4. DataInputStream 是 FilterInputStream 子类，具体的修饰者，类似前面的 Milk, Soy 等
        //5. FilterInputStream 类 有  protected volatile InputStream in; 即含被装饰者
        //6. 分析得出在jdk 的io体系中，就是使用装饰者模式
        DataInputStream dis = new DataInputStream(new FileInputStream("d:\\abc.txt"));
        System.out.println(dis.read());
        dis.close();
    }
}
```

## 11.组合模式

### 11.1.基本介绍
1) 组合模式（Composite Pattern），又叫部分整体模式，它创建了对象组的树形结
构，将对象组合成树状结构以表示“整体-部分”的层次关系。
2) 组合模式依据树形结构来组合对象，用来表示部分以及整体层次。
3) 这种类型的设计模式属于结构型模式。
4) 组合模式使得用户对单个对象和组合对象的访问具有一致性，即：组合能让客
户以一致的方式处理个别对象以及组合对象

### 11.2.组合模式的原理类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTM2OTBkM2M0YjkyMjRmYTIucG5n?x-oss-process=image/format,png)

对原理结构图的说明-即(组合模式的角色及职责)
1) Component :这是组合中对象声明接口，在适当情况下，实现所有类共有的接口默认行为,用于访问和管理Component 子
部件, Component 可以是抽象类或者接口
2) Leaf : 在组合中表示叶子节点，叶子节点没有子节点

### 11.3.组合模式解决的问题
1) 组合模式解决这样的问题，当我们的要处理的对象可以生成一颗树形结构，而
我们要对树上的节点和叶子进行操作时，它能够提供一致的方式，而不用考虑
它是节点还是叶子
2) 对应的示意图
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWNjMWZiYWYyZmIwNTc2YTcucG5n?x-oss-process=image/format,png)
### 11.4.组合模式解决学校院系展示的 应用实例
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWIwZjU5ZjZmMTE4NjlkNGQucG5n?x-oss-process=image/format,png)
```java
public class Client {

    public static void main(String[] args) {
        //从大到小创建对象 学校
        OrganizationComponent university = new University("清华大学", " 中国顶级大学 ");

        //创建 学院
        OrganizationComponent computerCollege = new College("计算机学院", " 计算机学院 ");
        OrganizationComponent infoEngineercollege = new College("信息工程学院", " 信息工程学院 ");

        //创建各个学院下面的系(专业)
        computerCollege.add(new Department("软件工程", " 软件工程不错 "));
        computerCollege.add(new Department("网络工程", " 网络工程不错 "));
        computerCollege.add(new Department("计算机科学与技术", " 计算机科学与技术是老牌的专业 "));

        //
        infoEngineercollege.add(new Department("通信工程", " 通信工程不好学 "));
        infoEngineercollege.add(new Department("信息工程", " 信息工程好学 "));

        //将学院加入到 学校
        university.add(computerCollege);
        university.add(infoEngineercollege);

        //university.print();
        infoEngineercollege.print();
    }
}
```
```java
public class College extends OrganizationComponent {

    // List 中 存放的Department
    List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

    // 构造器
    public College(String name, String des) {
        super(name, des);
    }

    // 重写add
    @Override protected void add(OrganizationComponent organizationComponent) {
        organizationComponents.add(organizationComponent);
    }

    // 重写remove
    @Override protected void remove(OrganizationComponent organizationComponent) {
        organizationComponents.remove(organizationComponent);
    }

    @Override public String getName() {
        return super.getName();
    }

    @Override public String getDes() {
        return super.getDes();
    }

    // print方法，就是输出University 包含的学院
    @Override protected void print() {
        System.out.println("--------------" + getName() + "--------------");
        //遍历 organizationComponents
        for (OrganizationComponent organizationComponent : organizationComponents) {
            organizationComponent.print();
        }
    }
}
```
```java
public class Department extends OrganizationComponent {

    //没有集合
    public Department(String name, String des) {
        super(name, des);
    }

    //add , remove 就不用写了，因为他是叶子节点
    @Override public String getName() {
        return super.getName();
    }

    @Override public String getDes() {
        return super.getDes();
    }

    @Override protected void print() {
        System.out.println(getName());
    }
}
```

```java
public abstract class OrganizationComponent {

    private String name; // 名字
    private String des; // 说明

    //构造器
    public OrganizationComponent(String name, String des) {
        super();
        this.name = name;
        this.des = des;
    }
	
    protected void add(OrganizationComponent organizationComponent) {
        //默认实现
        throw new UnsupportedOperationException();
    }

    protected void remove(OrganizationComponent organizationComponent) {
        //默认实现
        throw new UnsupportedOperationException();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDes() {
        return des;
    }

    public void setDes(String des) {
        this.des = des;
    }

    //方法print, 做成抽象的, 子类都需要实现
    protected abstract void print();
}
```

```java
//University 就是 Composite , 可以管理College
public class University extends OrganizationComponent {

    List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

    // 构造器
    public University(String name, String des) {
        super(name, des);
    }

    // 重写add
    @Override protected void add(OrganizationComponent organizationComponent) {
        organizationComponents.add(organizationComponent);
    }

    // 重写remove
    @Override protected void remove(OrganizationComponent organizationComponent) {
        organizationComponents.remove(organizationComponent);
    }

    @Override public String getName() {
        return super.getName();
    }

    @Override public String getDes() {
        return super.getDes();
    }

    // print方法，就是输出University 包含的学院
    @Override protected void print() {
        System.out.println("--------------" + getName() + "--------------");
        //遍历 organizationComponents 
        for (OrganizationComponent organizationComponent : organizationComponents) {
            organizationComponent.print();
        }
    }
}
```

### 11.5.组合模式在JDK集合的源码分析

组合模式在JDK集合的源码分析
1) Java的集合类-HashMap就使用了组合模式
2) 代码分析+Debug 源码

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTJlZTBmNWM5YzExMWMxM2QucG5n?x-oss-process=image/format,png)



### 11.6.组合模式的注意事项和细节
1) 简化客户端操作。客户端只需要面对一致的对象而不用考虑整体部分或者节点叶子
的问题。
2) 具有较强的扩展性。当我们要更改组合对象时，我们只需要调整内部的层次关系，
客户端不用做出任何改动.
3) 方便创建出复杂的层次结构。客户端不用理会组合里面的组成细节，容易添加节点
或者叶子从而创建出复杂的树形结构
4) 需要遍历组织机构，或者处理的对象具有树形结构时, 非常适合使用组合模式.
5) 要求较高的抽象性，如果节点和叶子有很多差异性的话，比如很多方法和属性
都不一样，不适合使用组合模式

## 12.外观模式

### 12.1.基本介绍
1) 外观模式（Facade），也叫“过程模式：外观模式为子系统中的一组接口提供
一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更加
容易使用
2) 外观模式通过定义一个一致的接口，用以屏蔽内部子系统的细节，使得调用端
只需跟这个接口发生调用，而无需关心这个子系统的内部细节

### 12.2.外观模式的原理类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWJiYmU0ZjE4OTk2ZWM5NjgucG5n?x-oss-process=image/format,png)

原理类图的说明(外观模式的角色)
1) 外观类(Facade): 为调用端提供统一的调用接口, 外观类知道哪些子系统负责处理请求,从而将调用端的请求代理给适当
子系统对象
2) 调用者(Client): 外观接口的调用者
3) 子系统的集合：指模块或者子系统，处理Facade 对象指派的任务，他是功能的实际提供者

### 12.3.外观模式解决影院管理

1) 外观模式可以理解为转换一群接口，客户只
要调用一个接口，而不用调用多个接口才能
达到目的。比如：在pc上安装软件的时候经
常有一键安装选项（省去选择安装目录、安
装的组件等等），还有就是手机的重启功能
（把关机和启动合为一个操作）。
2) 外观模式就是解决多个复杂接口带来的使用
困难，起到简化用户操作的作用
3) 示意图说明

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWQ0ZWRmMTNjYWU0NjZmZjQucG5n?x-oss-process=image/format,png)

### 12.4.外观模式应用实例

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTBmNDJjMGMzZjRiNWQ2YjQucG5n?x-oss-process=image/format,png)

```java
public class Client {

    public static void main(String[] args) {
        //这里直接调用。。 很麻烦
        HomeTheaterFacade homeTheaterFacade = new HomeTheaterFacade();
        homeTheaterFacade.ready();
        homeTheaterFacade.play();
        homeTheaterFacade.end();
    }
}
```

```java
public class DVDPlayer {

    //使用单例模式, 使用饿汉式
    private static DVDPlayer instance = new DVDPlayer();

    public static DVDPlayer getInstanc() {
        return instance;
    }

    public void on() {
        System.out.println(" dvd on ");
    }

    public void off() {
        System.out.println(" dvd off ");
    }

    public void play() {
        System.out.println(" dvd is playing ");
    }

    public void pause() {
        System.out.println(" dvd pause ..");
    }
}
```

```java
public class HomeTheaterFacade {

    //定义各个子系统对象
    private TheaterLight theaterLight;
    private Popcorn popcorn;
    private Stereo stereo;
    private Projector projector;
    private Screen screen;
    private DVDPlayer dVDPlayer;

    //构造器
    public HomeTheaterFacade() {
        super();
        this.theaterLight = TheaterLight.getInstance();
        this.popcorn = Popcorn.getInstance();
        this.stereo = Stereo.getInstance();
        this.projector = Projector.getInstance();
        this.screen = Screen.getInstance();
        this.dVDPlayer = DVDPlayer.getInstanc();
    }

    //操作分成 4 步
    public void ready() {
        popcorn.on();
        popcorn.pop();
        screen.down();
        projector.on();
        stereo.on();
        dVDPlayer.on();
        theaterLight.dim();
    }

    public void play() {
        dVDPlayer.play();
    }

    public void pause() {
        dVDPlayer.pause();
    }

    public void end() {
        popcorn.off();
        theaterLight.bright();
        screen.up();
        projector.off();
        stereo.off();
        dVDPlayer.off();
    }
}
```

```java
public class Popcorn {

    private static Popcorn instance = new Popcorn();

    public static Popcorn getInstance() {
        return instance;
    }

    public void on() {
        System.out.println(" popcorn on ");
    }

    public void off() {
        System.out.println(" popcorn ff ");
    }

    public void pop() {
        System.out.println(" popcorn is poping  ");
    }
}
```

```java
public class Projector {

	private static Projector instance = new Projector();
	
	public static Projector getInstance() {
		return instance;
	}
	
	public void on() {
		System.out.println(" Projector on ");
	}
	
	public void off() {
		System.out.println(" Projector ff ");
	}
	
	public void focus() {
		System.out.println(" Projector is Projector  ");
	}
}
```

```java
public class Screen {

	private static Screen instance = new Screen();
	
	public static Screen getInstance() {
		return instance;
	}
	
	public void up() {
		System.out.println(" Screen up ");
	}
	
	public void down() {
		System.out.println(" Screen down ");
	}
}
```

```java
public class Stereo {

    private static Stereo instance = new Stereo();

    public static Stereo getInstance() {
        return instance;
    }

    public void on() {
        System.out.println(" Stereo on ");
    }

    public void off() {
        System.out.println(" Screen off ");
    }

    public void up() {
        System.out.println(" Screen up.. ");
    }
}
```

```java
public class TheaterLight {

	private static TheaterLight instance = new TheaterLight();

	public static TheaterLight getInstance() {
		return instance;
	}

	public void on() {
		System.out.println(" TheaterLight on ");
	}

	public void off() {
		System.out.println(" TheaterLight off ");
	}

	public void dim() {
		System.out.println(" TheaterLight dim.. ");
	}

	public void bright() {
		System.out.println(" TheaterLight bright.. ");
	}
}
```

### 12.5.外观模式在MyBatis框架应用的源码分析

1) MyBatis 中的Configuration 去创建MetaObject 对象使用到外观模式
2) 代码分析+Debug源码+示意图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTMzYzAyMzZiMmUzZTExMmUucG5n?x-oss-process=image/format,png)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWIxZTMyOThlZmI3OGIxZDgucG5n?x-oss-process=image/format,png)

### 12.6.外观模式的注意事项和细节
1) 外观模式对外屏蔽了子系统的细节，因此外观模式降低了客户端对子系统使用的复
杂性
2) 外观模式对客户端与子系统的耦合关系，让子系统内部的模块更易维护和扩展
3) 通过合理的使用外观模式，可以帮我们更好的划分访问的层次
4) 当系统需要进行分层设计时，可以考虑使用Facade模式
5) 在维护一个遗留的大型系统时，可能这个系统已经变得非常难以维护和扩展，此时
可以考虑为新系统开发一个Facade类，来提供遗留系统的比较清晰简单的接口，
让新系统与Facade类交互，提高复用性
6) 不能过多的或者不合理的使用外观模式，使用外观模式好，还是直接调用模块好。
要以让系统有层次，利于维护为目的。

## 13.享元模式

### 13.1.基本介绍
1) 享元模式（Flyweight Pattern） 也叫 蝇量模式: 运
用共享技术有效地支持大量细粒度的对象
2) 常用于系统底层开发，解决系统的性能问题。像
**数据库连接池**，里面都是创建好的连接对象，在
这些连接对象中有我们需要的则直接拿来用，避
免重新创建，如果没有我们需要的，则创建一个
3) 享元模式能够解决**重复对象的内存浪费的问题**，
当系统中有大量相似对象，需要缓冲池时。不需
总是创建新对象，可以从缓冲池里拿。这样可以
降低系统内存，同时提高效率
4) 享元模式**经典的应用场景**就是池技术了，String常
量池、数据库连接池、缓冲池等等都是享元模式
的应用，享元模式是池技术的重要实现方式

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTAyY2M3YmEwYTYyYTYyMzMucG5n?x-oss-process=image/format,png)

### 13.2.享元模式的原理类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWJiOWZkYzg4ODE3ODBhNjEucG5n?x-oss-process=image/format,png)

对原理图的说明-即(模式的角色及职责)
1) FlyWeight 是抽象的享元角色, 他是产品的抽象类, 同时定义出对象的外部状态和内部状态(后面介绍) 的接口或实现
2) ConcreteFlyWeight 是具体的享元角色，是具体的产品类，实现抽象角色定义相关业务
3) UnSharedConcreteFlyWeight 是不可共享的角色，一般不会出现在享元工厂

### 13.3.内部状态和外部状态
比如围棋、五子棋、跳棋，它们都有大量的棋子对象，围棋和五子棋只有黑白两色，跳棋颜色多一
点，所以棋子颜色就是棋子的内部状态；而各个棋子之间的差别就是位置的不同，当我们落子后，
落子颜色是定的，但位置是变化的，所以棋子坐标就是棋子的外部状态
1) 享元模式提出了两个要求：细粒度和共享对象。这里就涉及到内部状态和外部状态
了，即将对象的信息分为两个部分：内部状态和外部状态
2) 内部状态指对象共享出来的信息，存储在享元对象内部且不会随环境的改变而改变
3) 外部状态指对象得以依赖的一个标记，是随环境改变而改变的、不可共享的状态。
4) 举个例子：围棋理论上有361个空位可以放棋子，每盘棋都有可能有两三百个棋子对
象产生，因为内存空间有限，一台服务器很难支持更多的玩家玩围棋游戏，如果用
享元模式来处理棋子，那么棋子对象就可以减少到只有两个实例，这样就很好的解
决了对象的开销问题

### 13.4.享元模式应用实例
1) 应用实例要求
使用享元模式完成，前面提出的网站外包问题
2) 思路分析和图解(类图)
3) 代码实现

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTAzYzIyOTI4ZWQyMmYzNTQucG5n?x-oss-process=image/format,png)

```java
public class Client {

    public static void main(String[] args) {
        // 创建一个工厂类
        WebSiteFactory factory = new WebSiteFactory();
        // 客户要一个以新闻形式发布的网站
        WebSite webSite1 = factory.getWebSiteCategory("新闻");
        webSite1.use(new User("tom"));
        // 客户要一个以博客形式发布的网站
        WebSite webSite2 = factory.getWebSiteCategory("博客");
        webSite2.use(new User("jack"));
        // 客户要一个以博客形式发布的网站
        WebSite webSite3 = factory.getWebSiteCategory("博客");
        webSite3.use(new User("smith"));
        // 客户要一个以博客形式发布的网站
        WebSite webSite4 = factory.getWebSiteCategory("博客");
        webSite4.use(new User("king"));
        System.out.println("网站的分类共=" + factory.getWebSiteCount());
    }
}
```

```java
//具体网站
public class ConcreteWebSite extends WebSite {

    //共享的部分，内部状态
    private String type = ""; //网站发布的形式(类型)

    //构造器
    public ConcreteWebSite(String type) {
        this.type = type;
    }

    @Override public void use(User user) {
        System.out.println("网站的发布形式为:" + type + " 在使用中 .. 使用者是" + user.getName());
    }
}
```

```java
public class User {

    private String name;

    public User(String name) {
        super();
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

```java
public abstract class WebSite {

    public abstract void use(User user);//抽象方法
}
```

```java
// 网站工厂类，根据需要返回压一个网站
public class WebSiteFactory {

    //集合， 充当池的作用
    private HashMap<String, ConcreteWebSite> pool = new HashMap<>();

    //根据网站的类型，返回一个网站, 如果没有就创建一个网站，并放入到池中,并返回
    public WebSite getWebSiteCategory(String type) {
        if (!pool.containsKey(type)) {
            //就创建一个网站，并放入到池中
            pool.put(type, new ConcreteWebSite(type));
        }
        return pool.get(type);
    }

    //获取网站分类的总数 (池中有多少个网站类型)
    public int getWebSiteCount() {
        return pool.size();
    }
}
```

### 13.5.享元模式在JDK-Interger的应用源码分析

1) Integer中的享元模式
2) 代码分析+Debug源码+说明

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWVlMzFjY2JmODBhNzljZDEucG5n?x-oss-process=image/format,png)

```java
public class FlyWeight {

    public static void main(String[] args) {
        //如果 Integer.valueOf(x) x 在  -128 --- 127 直接，就是使用享元模式返回,如果不在
        //范围类，则仍然 new
        //小结:
        //1. 在valueOf 方法中，先判断值是否在 IntegerCache 中，如果不在，就创建新的Integer(new), 否则，就直接从 缓存池返回
        //2. valueOf 方法，就使用到享元模式
        //3. 如果使用valueOf 方法得到一个Integer 实例，范围在 -128 - 127 ，执行速度比 new 快
        Integer x = Integer.valueOf(127); // 得到 x实例，类型 Integer
        Integer y = new Integer(127); // 得到 y 实例，类型 Integer
        Integer z = Integer.valueOf(127);//..
        Integer w = new Integer(127);
        System.out.println(x.equals(y)); // 大小，true
        System.out.println(x == y); //  false
        System.out.println(x == z); // true
        System.out.println(w == x); // false
        System.out.println(w == y); // false
        Integer x1 = Integer.valueOf(200);
        Integer x2 = Integer.valueOf(200);
        System.out.println("x1==x2" + (x1 == x2)); // false
    }
}
```

## 14.代理模式
### 14.1.代理模式的基本介绍
1) 代理模式：为一个对象提供一个替身，以控制对这个对象的访问。即通过代理
对象访问目标对象.这样做的好处是:可以在目标对象实现的基础上,增强额外的
功能操作,即扩展目标对象的功能。
2) 被代理的对象可以是远程对象、创建开销大的对象或需要安全控制的对象
3) 代理模式有不同的形式, 主要有三种 静态代理、动态代理 (JDK代理、接口代
理)和 Cglib代理 (可以在内存动态的创建对象，而不需要实现接口， 他是属于
动态代理的范畴) 。
4) 代理模式示意图
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWViMzA2ZDkzZmIyYTc0M2IucG5n?x-oss-process=image/format,png)
### 14.2.静态代理
#### 14.2.1.静态代码模式的基本介绍
静态代理在使用时,需要定义接口或者父类,被代理对象(即目标对象)与代理对象一
起实现相同的接口或者是继承相同父类
#### 14.2.2.应用实例
\> 具体要求
1) 定义一个接口:ITeacherDao
2) 目标对象TeacherDAO实现接口ITeacherDAO
3) 使用静态代理方式,就需要在代理对象TeacherDAOProxy中也实现ITeacherDAO
4) 调用的时候通过调用代理对象的方法来调用目标对象.
5) **特别提醒：**代理对象与目标对象要实现相同的接口,然后通过调用相同的方法来
调用目标对象的方法。

思路分析图解
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTA0ZmIxZTMwMzQxZmQ4MDkucG5n?x-oss-process=image/format,png)

代码实现
```java
public class Client {

    public static void main(String[] args) {
        //创建目标对象(被代理对象)
        TeacherDao teacherDao = new TeacherDao();
        //创建代理对象, 同时将被代理对象传递给代理对象
        TeacherDaoProxy teacherDaoProxy = new TeacherDaoProxy(teacherDao);
        //通过代理对象，调用到被代理对象的方法
        //即：执行的是代理对象的方法，代理对象再去调用目标对象的方法
        teacherDaoProxy.teach();
    }
}
```

```java
//接口
public interface ITeacherDao {

    void teach(); // 授课的方法
}
```

```java
public class TeacherDao implements ITeacherDao {

    @Override public void teach() {
        System.out.println(" 老师授课中  。。。。。");
    }
}
```

```java
//代理对象,静态代理
public class TeacherDaoProxy implements ITeacherDao {

    private ITeacherDao target; // 目标对象，通过接口来聚合

    //构造器
    public TeacherDaoProxy(ITeacherDao target) {
        this.target = target;
    }

    @Override public void teach() {
        System.out.println("开始代理  完成某些操作。。。。。 ");//方法
        target.teach();
        System.out.println("提交。。。。。");//方法
    }
}
```

#### 14.2.3.静态代理优缺点
1) 优点：在不修改目标对象的功能前提下, 能通过代理对象对目标功能扩展
2) 缺点：因为代理对象需要与目标对象实现一样的接口,所以会有很多代理类
3) 一旦接口增加方法,目标对象与代理对象都要维护

### 14.3.动态代理
#### 14.3.1.动态代理模式的基本介绍
1) 代理对象,不需要实现接口，但是目标对象要实现接口，否则不能用动态代理
2) 代理对象的生成，是利用JDK的API，动态的在内存中构建代理对象
3) 动态代理也叫做：JDK代理、接口代理

#### 14.3.2.JDK中生成代理对象的API
1) 代理类所在包:java.lang.reflect.Proxy
2) JDK实现代理只需要使用newProxyInstance方法,但是该方法需要接收三个参数,完
整的写法是:
static Object newProxyInstance(ClassLoader loader, Class<?>[]
interfaces,InvocationHandler h )

#### 14.3.3.动态代理应用实例
\> 应用实例要求
将前面的静态代理改进成动态代理模式(即：JDK代理模式)
\> 思路图解(类图)
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWZkZjgwNzU5ZTFhN2I1M2YucG5n?x-oss-process=image/format,png)
\> 代码实现
```java
public class Client {

    public static void main(String[] args) {
        //创建目标对象
        ITeacherDao target = new TeacherDao();
        //给目标对象，创建代理对象, 可以转成 ITeacherDao
        ITeacherDao proxyInstance = (ITeacherDao)new ProxyFactory(target).getProxyInstance();
        // proxyInstance=class com.sun.proxy.$Proxy0 内存中动态生成了代理对象
        System.out.println("proxyInstance=" + proxyInstance.getClass());
        //通过代理对象，调用目标对象的方法
        //proxyInstance.teach();
        proxyInstance.sayHello(" tom ");
    }
}
```
```java
//接口
public interface ITeacherDao {
    void teach(); // 授课方法
    void sayHello(String name);
}
```
```java
public class ProxyFactory {

    //维护一个目标对象 , Object
    private Object target;

    //构造器 ， 对target 进行初始化
    public ProxyFactory(Object target) {
        this.target = target;
    }

    //给目标对象 生成一个代理对象
    public Object getProxyInstance() {
        //说明
		/*
		 *  public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
            //1. ClassLoader loader ： 指定当前目标对象使用的类加载器, 获取加载器的方法固定
            //2. Class<?>[] interfaces: 目标对象实现的接口类型，使用泛型方法确认类型
            //3. InvocationHandler h : 事情处理，执行目标对象的方法时，会触发事情处理器方法, 会把当前执行的目标对象方法作为参数传入
		 */
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(),
            new InvocationHandler() {
                @Override public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    System.out.println("JDK代理开始~~");
                    //反射机制调用目标对象的方法
                    Object returnVal = method.invoke(target, args);
                    System.out.println("JDK代理提交");
                    return returnVal;
                }
            });
    }
}

```
```java
public class TeacherDao implements ITeacherDao {

    @Override public void teach() {
        System.out.println(" 老师授课中.... ");
    }

    @Override public void sayHello(String name) {
        System.out.println("hello " + name);
    }
}
```
### 14.4.Cglib代理
#### 14.4.1.Cglib代理模式的基本介绍
1) 静态代理和JDK代理模式都要求目标对象是实现一个接口,但是有时候目标对象只
是一个单独的对象,并没有实现任何的接口,这个时候可使用目标对象子类来实现
代理-这就是Cglib代理
2) Cglib代理也叫作子类代理,它是在内存中构建一个子类对象从而实现对目标对象功
能扩展, 有些书也将Cglib代理归属到动态代理。
3) Cglib是一个强大的高性能的代码生成包,它可以在运行期扩展java类与实现java接
口.它广泛的被许多AOP的框架使用,例如Spring AOP，实现方法拦截
4) 在AOP编程中如何选择代理模式：

1. 目标对象需要实现接口，用JDK代理
2. 目标对象不需要实现接口，用Cglib代理

5) Cglib包的底层是通过使用字节码处理框架ASM来转换字节码并生成新的类

#### 14.4.2.Cglib代理模式实现步骤
1) 需要引入cglib的jar文件
2) 在内存中动态构建子类，注意代理的类不能为final，否则报错
java.lang.IllegalArgumentException:
3) 目标对象的方法如果为final/static,那么就不会被拦截,即不会执行目标对象额外的
业务方法.

#### 14.4.3.Cglib代理模式应用实例
\> 应用实例要求
将前面的案例用Cglib代理模式实现
\> 思路图解(类图)
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWFhMmI4OTQ0YWU0YmQ0ZTkucG5n?x-oss-process=image/format,png)
\> 代码实现+Debug源码[待 debug]
```java
public class Client {

    public static void main(String[] args) {
        //创建目标对象
        TeacherDao target = new TeacherDao();
        //获取到代理对象，并且将目标对象传递给代理对象
        TeacherDao proxyInstance = (TeacherDao)new ProxyFactory(target).getProxyInstance();
        //执行代理对象的方法，触发intecept 方法，从而实现 对目标对象的调用
        String res = proxyInstance.teach();
        System.out.println("res=" + res);
    }
}
```
```java
public class ProxyFactory implements MethodInterceptor {

    //维护一个目标对象
    private Object target;

    //构造器，传入一个被代理的对象
    public ProxyFactory(Object target) {
        this.target = target;
    }

    //返回一个代理对象:  是 target 对象的代理对象
    public Object getProxyInstance() {
        //1. 创建一个工具类
        Enhancer enhancer = new Enhancer();
        //2. 设置父类
        enhancer.setSuperclass(target.getClass());
        //3. 设置回调函数
        enhancer.setCallback(this);
        //4. 创建子类对象，即代理对象
        return enhancer.create();

    }

    //重写  intercept 方法，会调用目标对象的方法
    @Override public Object intercept(Object arg0, Method method, Object[] args, MethodProxy arg3) throws Throwable {
        System.out.println("Cglib代理模式 ~~ 开始");
        Object returnVal = method.invoke(target, args);
        System.out.println("Cglib代理模式 ~~ 提交");
        return returnVal;
    }
}
```
```java
public class TeacherDao {

    public String teach() {
        System.out.println(" 老师授课中  ， 我是cglib代理，不需要实现接口 ");
        return "hello";
    }
}
```
### 14.5.几种常见的代理模式介绍— 几种变体
1) 防火墙代理
内网通过代理穿透防火墙，实现对公网的访问。
2) 缓存代理
比如：当请求图片文件等资源时，先到缓存代理取，如果取到资源则ok,如果取不到资源，
再到公网或者数据库取，然后缓存。
3) 远程代理
远程对象的本地代表，通过它可以把远程对象当本地对象来调用。远程代理通过网络和
真正的远程对象沟通信息。

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTNmOWZmNWJkN2YxMmRlYmQucG5n?x-oss-process=image/format,png)

4) 同步代理：主要使用在多线程编程中，完成多线程间同步工作

## 15.模板方法模式

### 15.1.基本介绍
1) 模板方法模式（Template Method Pattern），又叫模板模式(Template Pattern)，z
在一个抽象类公开定义了执行它的方法的模板。它的子类可以按需要重写方法
实现，但调用将以抽象类中定义的方式进行。
2) 简单说，模板方法模式 定义一个操作中的算法的骨架，而将一些步骤延迟到子
类中，使得子类可以不改变一个算法的结构，就可以重定义该算法的某些特定
步骤
3) 这种类型的设计模式属于行为型模式。

### 15.2.模板方法模式的原理类图

对原理类图的说明-即(模板方法模式的角色及职责)
1) AbstractClass 抽象类， 类中实现了模板方法(template)，定义了算法的骨
架，具体子类需要去实现 其它的抽象方法operationr2,3,4
2) ConcreteClass 实现抽象方法operationr2,3,4, 以完成算法中特点子类的步
骤

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWY4OTc0NDAyNTg4YmEyNjAucG5n?x-oss-process=image/format,png)

### 15.3.模板方法模式解决豆浆制作问题
1) 应用实例要求
编写制作豆浆的程序，说明如下:
• 制作豆浆的流程 选材--->添加配料--->浸泡--->放到豆浆机打碎
• 通过添加不同的配料，可以制作出不同口味的豆浆
• 选材、浸泡和放到豆浆机打碎这几个步骤对于制作每种口味的豆浆都是一样的(红
豆、花生豆浆。。。)
2) 思路分析和图解(类图)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTUwNTYxMWQwZWY5NjM4NDgucG5n?x-oss-process=image/format,png)3) 代码实现

```java
public class Client {

	public static void main(String[] args) {
		//制作红豆豆浆
		System.out.println("----制作红豆豆浆----");
		SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
		redBeanSoyaMilk.make();
		System.out.println("----制作花生豆浆----");
		SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
		peanutSoyaMilk.make();
	}
}
```

```java
public class PeanutSoyaMilk extends SoyaMilk {

    @Override void addCondiments() {
        System.out.println(" 加入上好的花生 ");
    }
}
```

```java
public class RedBeanSoyaMilk extends SoyaMilk {

    @Override void addCondiments() {
        System.out.println(" 加入上好的红豆 ");
    }
}
```

```java
//抽象类，表示豆浆
public abstract class SoyaMilk {

    //模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
    final void make() {
        select();
        addCondiments();
        soak();
        beat();
    }

    //选材料
    void select() {
        System.out.println("第一步：选择好的新鲜黄豆  ");
    }

    //添加不同的配料， 抽象方法, 子类具体实现
    abstract void addCondiments();

    //浸泡
    void soak() {
        System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
    }

    void beat() {
        System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
    }
}
```

### 15.4.模板方法模式的钩子方法

1) 在模板方法模式的父类中，我们可以定义一个方法，它默认不做任何事，子类可以
视情况要不要覆盖它，该方法称为“钩子”。
2) 还是用上面做豆浆的例子来讲解，比如，我们还希望制作纯豆浆，不添加任何的配
料，请使用钩子方法对前面的模板方法进行改造
3) 代码演示：

```java
//抽象类，表示豆浆
public abstract class SoyaMilk {

    //模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
    final void make() {

        select();
        if (customerWantCondiments()) {
            addCondiments();
        }
        soak();
        beat();

    }

    //选材料
    void select() {
        System.out.println("第一步：选择好的新鲜黄豆  ");
    }

    //添加不同的配料， 抽象方法, 子类具体实现
    abstract void addCondiments();

    //浸泡
    void soak() {
        System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
    }

    void beat() {
        System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
    }

    //钩子方法，决定是否需要添加配料
    boolean customerWantCondiments() {
        return true;
    }
}
```

### 15.5.模板方法模式在Spring框架应用的源码分析
1) Spring IOC容器初始化时运用到的模板方法模式
2) 代码分析+角色分析+说明类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWQyZmZiZGMxYzU5ZmNkYjgucG5n?x-oss-process=image/format,png)

3) 类图

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTcwMTU3YTdjNWI1MmI5MWUucG5n?x-oss-process=image/format,png)

### 15.6.模板方法模式的注意事项和细节
1) 基本思想是：算法只存在于一个地方，也就是在父类中，容易修改。需要修改算
法时，只要修改父类的模板方法或者已经实现的某些步骤，子类就会继承这些修改
2) 实现了最大化代码复用。父类的模板方法和已实现的某些步骤会被子类继承而直接
使用。
3) 既统一了算法，也提供了很大的灵活性。父类的模板方法确保了算法的结构保持不
变，同时由子类提供部分步骤的实现。
4) 该模式的不足之处：每一个不同的实现都需要一个子类实现，导致类的个数增加，
使得系统更加庞大
5) 一般模板方法都加上final关键字， 防止子类重写模板方法.
6) 模板方法模式使用场景：当要完成在某个过程，该过程要执行一系列步骤 ，这一
系列的步骤基本相同，但其个别步骤在实现时 可能不同，通常考虑用模板方法模
式来处理

## 16.命令模式

### 16.1.基本介绍
1) 命令模式（Command Pattern）：在软件设计中，我们经常需要
向某些对象发送请求，但是并不知道请求的接收者是谁，也不知
道被请求的操作是哪个，
我们只需在程序运行时指定具体的请求接收者即可，此时，可以
使用命令模式来进行设计
2) 命名模式使得请求发送者与请求接收者消除彼此之间的耦合，让
对象之间的调用关系更加灵活，实现解耦。
3) 在命名模式中，会将一个请求封装为一个对象，以便使用不同参
数来表示不同的请求(即命名)，同时命令模式也支持可撤销的操作。
4) 通俗易懂的理解：将军发布命令，士兵去执行。其中有几个角色：
将军（命令发布者）、士兵（命令的具体执行者）、命令(连接将
军和士兵)。
Invoker是调用者（将军），Receiver是被调用者（士兵），
MyCommand是命令，实现了Command接口，持有接收对象

### 16.2.命令模式的原理类图
对原理类图的说明-即(命名模式的角色及职责)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWNmMWQyMTQyYjIyNDUwODUucG5n?x-oss-process=image/format,png)

1) Invoker 是调用者角色
2) Command: 是命令角色，需要执行的所有命令都在这里，可以是接口或抽象类
3) Receiver: 接受者角色，知道如何实施和执行一个请求相关的操作
4) ConcreteCommand: 将一个接受者对象与一个动作绑定，调用接受者相应的操作，实现execute

### 16.3.命令模式应用实例
1) 应用实例要求
编写程序，使用命令模式 完成前面的智能家电项目
2) 思路分析和图解
3) 代码实现

```java
public class Client {

    public static void main(String[] args) {
        //使用命令设计模式，完成通过遥控器，对电灯的操作
        //创建电灯的对象(接受者)
        LightReceiver lightReceiver = new LightReceiver();
        //创建电灯相关的开关命令
        LightOnCommand lightOnCommand = new LightOnCommand(lightReceiver);
        LightOffCommand lightOffCommand = new LightOffCommand(lightReceiver);
        //需要一个遥控器
        RemoteController remoteController = new RemoteController();
        //给我们的遥控器设置命令, 比如 no = 0 是电灯的开和关的操作
        remoteController.setCommand(0, lightOnCommand, lightOffCommand);
        System.out.println("--------按下灯的开按钮-----------");
        remoteController.onButtonWasPushed(0);
        System.out.println("--------按下灯的关按钮-----------");
        remoteController.offButtonWasPushed(0);
        System.out.println("--------按下撤销按钮-----------");
        remoteController.undoButtonWasPushed();
        System.out.println("=========使用遥控器操作电视机==========");
        TVReceiver tvReceiver = new TVReceiver();
        TVOffCommand tvOffCommand = new TVOffCommand(tvReceiver);
        TVOnCommand tvOnCommand = new TVOnCommand(tvReceiver);
        //给我们的遥控器设置命令, 比如 no = 1 是电视机的开和关的操作
        remoteController.setCommand(1, tvOnCommand, tvOffCommand);
        System.out.println("--------按下电视机的开按钮-----------");
        remoteController.onButtonWasPushed(1);
        System.out.println("--------按下电视机的关按钮-----------");
        remoteController.offButtonWasPushed(1);
        System.out.println("--------按下撤销按钮-----------");
        remoteController.undoButtonWasPushed();
    }
}
```

```java
//创建命令接口
public interface Command {

    //执行动作(操作)
	void execute();

    //撤销动作(操作)
	void undo();
}
```

```java
public class LightOffCommand implements Command {

    // 聚合LightReceiver
    LightReceiver light;

    // 构造器
    public LightOffCommand(LightReceiver light) {
        super();
        this.light = light;
    }

    @Override public void execute() {
        // 调用接收者的方法
        light.off();
    }

    @Override public void undo() {
        // 调用接收者的方法
        light.on();
    }
}
```

```java
public class LightOnCommand implements Command {

    //聚合LightReceiver
    LightReceiver light;

    //构造器
    public LightOnCommand(LightReceiver light) {
        super();
        this.light = light;
    }

    @Override public void execute() {
        //调用接收者的方法
        light.on();
    }

    @Override public void undo() {
        //调用接收者的方法
        light.off();
    }
}
```

```java
public class LightReceiver {

    public void on() {
        System.out.println(" 电灯打开了.. ");
    }

    public void off() {
        System.out.println(" 电灯关闭了.. ");
    }
}
```

```java
/**
 * 没有任何命令，即空执行: 用于初始化每个按钮, 当调用空命令时，对象什么都不做
 * 其实，这样是一种设计模式, 可以省掉对空判断
 */
public class NoCommand implements Command {

    @Override public void execute() {
    }

    @Override public void undo() {
    }
}
```

```java
public class RemoteController {

    // 开 按钮的命令数组
    Command[] onCommands;
    Command[] offCommands;

    // 执行撤销的命令
    Command undoCommand;

    // 构造器，完成对按钮初始化
    public RemoteController() {
        onCommands = new Command[5];
        offCommands = new Command[5];
        for (int i = 0; i < 5; i++) {
            onCommands[i] = new NoCommand();
            offCommands[i] = new NoCommand();
        }
    }

    // 给我们的按钮设置你需要的命令
    public void setCommand(int no, Command onCommand, Command offCommand) {
        onCommands[no] = onCommand;
        offCommands[no] = offCommand;
    }

    // 按下开按钮
    public void onButtonWasPushed(int no) { // no 0
        // 找到你按下的开的按钮， 并调用对应方法
        onCommands[no].execute();
        // 记录这次的操作，用于撤销
        undoCommand = onCommands[no];
    }

    // 按下开按钮
    public void offButtonWasPushed(int no) { // no 0
        // 找到你按下的关的按钮， 并调用对应方法
        offCommands[no].execute();
        // 记录这次的操作，用于撤销
        undoCommand = offCommands[no];
    }

    // 按下撤销按钮
    public void undoButtonWasPushed() {
        undoCommand.undo();
    }
}
```

```java
public class TVOffCommand implements Command {

    // 聚合TVReceiver
    TVReceiver tv;

    // 构造器
    public TVOffCommand(TVReceiver tv) {
        super();
        this.tv = tv;
    }

    @Override public void execute() {
        // 调用接收者的方法
        tv.off();
    }

    @Override public void undo() {
        // 调用接收者的方法
        tv.on();
    }
}
```

```java
public class TVOnCommand implements Command {

    // 聚合TVReceiver
    TVReceiver tv;

    // 构造器
    public TVOnCommand(TVReceiver tv) {
        super();
        this.tv = tv;
    }

    @Override public void execute() {
        // 调用接收者的方法
        tv.on();
    }

    @Override public void undo() {
        // 调用接收者的方法
        tv.off();
    }
}
```

```java
public class TVReceiver {

    public void on() {
        System.out.println(" 电视机打开了.. ");
    }

    public void off() {
        System.out.println(" 电视机关闭了.. ");
    }
}
```

### 16.5.命令模式在Spring框架JdbcTemplate应用的源码分析

1) Spring框架的JdbcTemplate就使用到了命令模式
2) 代码分析

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWE4YzQyZmQyZTcyYmI2YjUucG5n?x-oss-process=image/format,png)

3) 模式角色分析说明
- StatementCallback 接口 ,类似命令接口(Command)
- class QueryStatementCallback implements StatementCallback, SqlProvider , 匿名内
部类， 实现了命令接口， 同时也充当命令接收者
- 命令调用者 是 JdbcTemplate , 其中execute(StatementCallback action) 方法中，调
用action.doInStatement 方法. 不同的 实现 StatementCallback 接口的对象，对应不同
的doInStatemnt 实现逻辑
- 另外实现 StatementCallback 命令接口的子类还有 QueryStatementCallback、

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWRiMzIyZTZiMWMwYzQ4MDgucG5n?x-oss-process=image/format,png)

### 16.6.命令模式的注意事项和细节
1) 将发起请求的对象与执行请求的对象解耦。发起请求的对象是调用者，调用者只要
调用命令对象的execute()方法就可以让接收者工作，而不必知道具体的接收者对
象是谁、是如何实现的，命令对象会负责让接收者执行请求的动作，也就是说：”
请求发起者”和“请求执行者”之间的解耦是通过命令对象实现的，命令对象起到
了纽带桥梁的作用。
2) 容易设计一个命令队列。只要把命令对象放到列队，就可以多线程的执行命令
3) 容易实现对请求的撤销和重做
4) 命令模式不足：可能导致某些系统有过多的具体命令类，增加了系统的复杂度，这
点在在使用的时候要注意
5) 空命令也是一种设计模式，它为我们省去了判空的操作。在上面的实例中，如果没
有用空命令，我们每按下一个按键都要判空，这给我们编码带来一定的麻烦。
6) 命令模式经典的应用场景：界面的一个按钮都是一条命令、模拟CMD（DOS命令）
订单的撤销/恢复、触发-反馈机制

## 17.访问者模式

### 17.1.访问者模式基本介绍
1) 访问者模式（Visitor Pattern），封装一些作用于某种数据结构的各元素的操作，
它可以在不改变数据结构的前提下定义作用于这些元素的新的操作。
2) 主要将数据结构与数据操作分离，解决 数据结构和操作耦合性问题
3) 访问者模式的基本工作原理是：在被访问的类里面加一个对外提供接待访问者
的接口
4) 访问者模式主要应用场景是：需要对一个对象结构中的对象进行很多不同操作
(这些操作彼此没有关联)，同时需要避免让这些操作"污染"这些对象的类，可以
选用访问者模式解决

### 17.2.访问者模式的原理类图
对原理类图的说明即(访问者模式的角色及职责)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTFkZTQ0NmY0ZjBlNjkyOGIucG5n?x-oss-process=image/format,png)1) Visitor 是抽象访问者，为该对象结构中的ConcreteElement的每一个类声明一个visit操作
2) ConcreteVisitor ：是一个具体的访问值 实现每个有Visitor 声明的操作，是每个操作实现的部分.
3) ObjectStructure 能枚举它的元素， 可以提供一个高层的接口，用来允许访问者访问元素
4) Element 定义一个accept 方法，接收一个访问者对象
5) ConcreteElement 为具体元素，实现了accept 方法

### 17.3.访问者模式应用实例
访问者模式应用实例
1) 应用实例要求
将人分为男人和女人，对歌手进行测评，当看完某个歌手表演后，得到他们对该歌手
不同的评价(评价 有不同的种类，比如 成功、失败 等)，请使用访问者模式来说实现
2) 思路分析和图解(类图)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWEyYzYzNTkwZGE2NDU5N2IucG5n?x-oss-process=image/format,png)3) 代码实现

```java
public abstract class Action {

    //得到男性 的测评
    public abstract void getManResult(Man man);

    //得到女的 测评
    public abstract void getWomanResult(Woman woman);
}
```

```java
public class Client {

    public static void main(String[] args) {
        //创建ObjectStructure
        ObjectStructure objectStructure = new ObjectStructure();
        objectStructure.attach(new Man());
        objectStructure.attach(new Woman());
        //成功
        Success success = new Success();
        objectStructure.display(success);
        System.out.println("===============");
        Fail fail = new Fail();
        objectStructure.display(fail);
        System.out.println("=======给的是待定的测评========");
        Wait wait = new Wait();
        objectStructure.display(wait);
    }
}
```

```java
public class Fail extends Action {

    @Override public void getManResult(Man man) {
        System.out.println(" 男人给的评价该歌手失败 !");
    }

    @Override public void getWomanResult(Woman woman) {
        System.out.println(" 女人给的评价该歌手失败 !");
    }
}
```

```java
public class Man extends Person {

    @Override public void accept(Action action) {
        action.getManResult(this);
    }
}
```

```java
//数据结构，管理很多人（Man , Woman）
public class ObjectStructure {

    //维护了一个集合
    private List<Person> persons = new LinkedList<>();

    //增加到list
    public void attach(Person p) {
        persons.add(p);
    }

    //移除
    public void detach(Person p) {
        persons.remove(p);
    }

    //显示测评情况
    public void display(Action action) {
        for (Person p : persons) {
            p.accept(action);
        }
    }
}
```

```java
public abstract class Person {
	
	//提供一个方法，让访问者可以访问
	public abstract void accept(Action action);
}
```

```java
public class Success extends Action {

    @Override public void getManResult(Man man) {
        System.out.println(" 男人给的评价该歌手很成功 !");
    }

    @Override public void getWomanResult(Woman woman) {
        System.out.println(" 女人给的评价该歌手很成功 !");
    }
}
```

```java
public class Wait extends Action {

    @Override public void getManResult(Man man) {
        System.out.println(" 男人给的评价是该歌手待定 ..");
    }

    @Override public void getWomanResult(Woman woman) {
        System.out.println(" 女人给的评价是该歌手待定 ..");
    }
}
```

```java
 //说明
//1. 这里我们使用到了双分派, 即首先在客户端程序中，将具体状态作为参数传递Woman中(第一次分派)
//2. 然后Woman 类调用作为参数的 "具体方法" 中方法getWomanResult, 同时将自己(this)作为参数
//   传入，完成第二次的分派
public class Woman extends Person {

    @Override public void accept(Action action) {
        action.getWomanResult(this);
    }
}
```

4) 应用案例的小结

- 上面提到了双分派，所谓双分派是指不管类怎么变化，我们都能找到期望的方法运行。
  双分派意味着得到执行的操作取决于请求的种类和两个接收者的类型

- 以上述实例为例，假设我们要添加一个Wait的状态类，考察Man类和Woman类的反
应，由于使用了双分派，只需增加一个Action子类即可在客户端调用即可，不
需要改动任何其他类的代码。

### 17.4.访问者模式的注意事项和细节
\> 优点
1) 访问者模式符合单一职责原则、让程序具有优秀的扩展性、灵活性非常高
2) 访问者模式可以对功能进行统一，可以做报表、UI、拦截器与过滤器，适用于数据
结构相对稳定的系统
\> 缺点
1) 具体元素对访问者公布细节，也就是说访问者关注了其他类的内部细节，这是迪米
特法则所不建议的, 这样造成了具体元素变更比较困难
2) 违背了依赖倒转原则。访问者依赖的是具体元素，而不是抽象元素
3) 因此，如果一个系统有比较稳定的数据结构，又有经常变化的功能需求，那么访问
者模式就是比较合适的.

## 18.迭代器模式

### 18.1.基本介绍
1) 迭代器模式（Iterator Pattern）是常用的设计模式，属于行为型模式
2) 如果我们的集合元素是用不同的方式实现的，有数组，还有java的集合类，
或者还有其他方式，当客户端要遍历这些集合元素的时候就要使用多种遍历
方式，而且还会暴露元素的内部结构，可以考虑使用迭代器模式解决。
3) 迭代器模式，提供一种遍历集合元素的统一接口，用一致的方法遍历集合元素，
不需要知道集合对象的底层表示，即：不暴露其内部的结构。

### 18.2.迭代器模式的原理类图
对原理类图的说明-即(迭代器模式的角色及职责)

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTM1MTAzNTM0OWJlYzI5NTkucG5n?x-oss-process=image/format,png)1) Iterator ： 迭代器接口，是系统提供，含义 hasNext, next, remove
2) ConcreteIterator : 具体的迭代器类，管理迭代
3) Aggregate :一个统一的聚合接口， 将客户端和具体聚合解耦

### 18.3.迭代器模式应用实例

1) 应用实例要求
编写程序展示一个学校院系结构：需求是这样，要在一个页面中展示出学校的院系组
成，一个学校有多个学院，一个学院有多个系。
2) 设计思路分析
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTk4NTdhYjAyYzBhMzNlOWEucG5n?x-oss-process=image/format,png)
3) 代码实现
```java
public class Client {

    public static void main(String[] args) {
        //创建学院
        List<College> collegeList = new ArrayList<College>();
        ComputerCollege computerCollege = new ComputerCollege();
        InfoCollege infoCollege = new InfoCollege();
        collegeList.add(computerCollege);
        //collegeList.add(infoCollege);
        OutPutImpl outPutImpl = new OutPutImpl(collegeList);
        outPutImpl.printCollege();
    }
}
```
```java
public interface College {

    String getName();

    //增加系的方法
	void addDepartment(String name, String desc);

    //返回一个迭代器,遍历
	Iterator createIterator();
}
```
```java
public class ComputerCollege implements College {

    Department[] departments;
    int numOfDepartment = 0;// 保存当前数组的对象个数

    public ComputerCollege() {
        departments = new Department[5];
        addDepartment("Java专业", " Java专业 ");
        addDepartment("PHP专业", " PHP专业 ");
        addDepartment("大数据专业", " 大数据专业 ");
    }

    @Override public String getName() {
        return "计算机学院";
    }

    @Override public void addDepartment(String name, String desc) {
        Department department = new Department(name, desc);
        departments[numOfDepartment] = department;
        numOfDepartment += 1;
    }

    @Override public Iterator createIterator() {
        return new ComputerCollegeIterator(departments);
    }
}
```
```java
public class ComputerCollegeIterator implements Iterator {

    //这里我们需要Department 是以怎样的方式存放=>数组
    Department[] departments;
    int position = 0; //遍历的位置

    public ComputerCollegeIterator(Department[] departments) {
        this.departments = departments;
    }

    //判断是否还有下一个元素
    @Override public boolean hasNext() {
		return position < departments.length && departments[position] != null;
    }

    @Override public Object next() {
        Department department = departments[position];
        position += 1;
        return department;
    }

    //删除的方法，默认空实现
    public void remove() {
    }
}
```
```java
//系
public class Department {

    private String name;
    private String desc;

    public Department(String name, String desc) {
        super();
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }
}
```
```java
public class InfoColleageIterator implements Iterator {

    List<Department> departmentList; // 信息工程学院是以List方式存放系
    int index = -1;//索引

    public InfoColleageIterator(List<Department> departmentList) {
        this.departmentList = departmentList;
    }

    //判断list中还有没有下一个元素
    @Override public boolean hasNext() {
        if (index >= departmentList.size() - 1) {
            return false;
        } else {
            index += 1;
            return true;
        }
    }

    @Override public Object next() {
        return departmentList.get(index);
    }

    //空实现remove
    public void remove() {
    }
}
```
```java
public class InfoCollege implements College {

    List<Department> departmentList;

    public InfoCollege() {
        departmentList = new ArrayList<Department>();
        addDepartment("信息安全专业", " 信息安全专业 ");
        addDepartment("网络安全专业", " 网络安全专业 ");
        addDepartment("服务器安全专业", " 服务器安全专业 ");
    }

    @Override public String getName() {
        return "信息工程学院";
    }

    @Override public void addDepartment(String name, String desc) {
        Department department = new Department(name, desc);
        departmentList.add(department);
    }

    @Override public Iterator createIterator() {
        return new InfoColleageIterator(departmentList);
    }
}
```
```java
public class OutPutImpl {

    //学院集合
    List<College> collegeList;

    public OutPutImpl(List<College> collegeList) {
        this.collegeList = collegeList;
    }

    //遍历所有学院,然后调用printDepartment 输出各个学院的系
    public void printCollege() {
        //从collegeList 取出所有学院, Java 中的 List 已经实现Iterator
        Iterator<College> iterator = collegeList.iterator();
        while (iterator.hasNext()) {
            //取出一个学院
            College college = iterator.next();
            System.out.println("=== " + college.getName() + "=====");
            printDepartment(college.createIterator()); //得到对应迭代器
        }
    }
    
    //输出 学院输出 系
    public void printDepartment(Iterator iterator) {
        while (iterator.hasNext()) {
            Department d = (Department)iterator.next();
            System.out.println(d.getName());
        }
    }
}
```
### 18.4.迭代器模式在JDK-ArrayList集合应用的源码分析
1) JDK的ArrayList 集合中就使用了迭代器模式
2) 代码分析+类图+说明
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWNmYjdhZDhlMDRmNTI0MGEucG5n?x-oss-process=image/format,png)
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTY5YzU3NGUyODczODg4NWEucG5n?x-oss-process=image/format,png)
3) 对类图的角色分析说明
- 内部类Itr 充当具体实现迭代器Iterator 的类， 作为ArrayList 内部类
- List 就是充当了聚合接口，含有一个iterator() 方法，返回一个迭代器对象
- ArrayList 是实现聚合接口List 的子类，实现了iterator()
- Iterator 接口系统提供
- 迭代器模式解决了 不同集合(ArrayList ,LinkedList) 统一遍历问题

### 18.5.迭代器模式的注意事项和细节
\> 优点
1) 提供一个统一的方法遍历对象，客户不用再考虑聚合的类型，使用一种方法就可以
遍历对象了。
2) 隐藏了聚合的内部结构，客户端要遍历聚合的时候只能取到迭代器，而不会知道聚
合的具体组成。
3) 提供了一种设计思想，就是一个类应该只有一个引起变化的原因（叫做单一责任
原则）。在聚合类中，我们把迭代器分开，就是要把管理对象集合和遍历对象集
合的责任分开，这样一来集合改变的话，只影响到聚合对象。而如果遍历方式改变
的话，只影响到了迭代器。
4) 当要展示一组相似对象，或者遍历一组相同对象时使用, 适合使用迭代器模式
\> 缺点
每个聚合对象都要一个迭代器，会生成多个迭代器不好管理类

## 19.观察者模式
### 19.1.观察者模式(Observer)原理
\> 观察者模式类似订牛奶业务
1) 奶站/气象局：Subject
2) 用户/第三方网站：Observer
\> Subject：登记注册、移除和通知
1) registerObserver 注册
2) removeObserver 移除
3) notifyObservers() 通知所有的注册的用户，根据不同需求，可以是更新数据，让用
户来取，也可能是实施推送，看具体需求定
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTliM2FjYThmZmE5M2E5Y2UucG5n?x-oss-process=image/format,png)
\> Observer：接收输入
\> 观察者模式：对象之间多对一依赖的一种设计方案，被依赖的对象为Subject，
依赖的对象为Observer，Subject通知Observer变化,比如这里的奶站是
Subject，是1的一方。用户时Observer，是多的一方。
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWQyNTMwZTY5MTg4MzBjMGQucG5n?x-oss-process=image/format,png)
### 19.2.观察者模式解决天气预报需求
思路分析图解(类图)
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTRjODRjNjYzNDViMzQ4ZGUucG5n?x-oss-process=image/format,png)
### 19.3.观察者模式的好处
1) 观察者模式设计后，会以集合的方式来管理用户(Observer)，包括注册，移除
和通知。
2) 这样，我们增加观察者(这里可以理解成一个新的公告板)，就不需要去修改核
心类WeatherData不会修改代码，遵守了ocp原则。
### 19.4.代码实现
```java
public class BaiduSite implements Observer {

    // 温度，气压，湿度
    private float temperature;
    private float pressure;
    private float humidity;

    // 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
    public void update(float temperature, float pressure, float humidity) {
        this.temperature = temperature;
        this.pressure = pressure;
        this.humidity = humidity;
        display();
    }

    // 显示
    public void display() {
        System.out.println("===百度网站====");
        System.out.println("***百度网站 气温 : " + temperature + "***");
        System.out.println("***百度网站 气压: " + pressure + "***");
        System.out.println("***百度网站 湿度: " + humidity + "***");
    }
}
```
```java
public class Client {

    public static void main(String[] args) {
        //创建一个WeatherData
        WeatherData weatherData = new WeatherData();
        //创建观察者
        CurrentConditions currentConditions = new CurrentConditions();
        BaiduSite baiduSite = new BaiduSite();
        //注册到weatherData
        weatherData.registerObserver(currentConditions);
        weatherData.registerObserver(baiduSite);
        //测试
        System.out.println("通知各个注册的观察者, 看看信息");
        weatherData.setData(10f, 100f, 30.3f);
        weatherData.removeObserver(currentConditions);
        //测试
        System.out.println();
        System.out.println("通知各个注册的观察者, 看看信息");
        weatherData.setData(10f, 100f, 30.3f);
    }
}
```
```java
public class CurrentConditions implements Observer {
    // 温度，气压，湿度
    private float temperature;
    private float pressure;
    private float humidity;
    // 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
    public void update(float temperature, float pressure, float humidity) {
        this.temperature = temperature;
        this.pressure = pressure;
        this.humidity = humidity;
        display();
    }
    // 显示
    public void display() {
        System.out.println("***Today mTemperature: " + temperature + "***");
        System.out.println("***Today mPressure: " + pressure + "***");
        System.out.println("***Today mHumidity: " + humidity + "***");
    }
}
```
```java
//观察者接口，有观察者来实现
public interface Observer {

    void update(float temperature, float pressure, float humidity);
}
```
```java
//接口, 让WeatherData 来实现 
public interface Subject {

    void registerObserver(Observer o);

    void removeObserver(Observer o);

    void notifyObservers();
}
```
```java
/**
 * 类是核心
 * 1. 包含最新的天气情况信息
 * 2. 含有 CurrentConditions 对象
 * 3. 当数据有更新时，就主动的调用   CurrentConditions对象update方法(含 display), 这样他们（接入方）就看到最新的信息
 */
public class WeatherData {
    private float temperatrue;
    private float pressure;
    private float humidity;
    private CurrentConditions currentConditions;
    //加入新的第三方
    public WeatherData(CurrentConditions currentConditions) {
        this.currentConditions = currentConditions;
    }
    public float getTemperature() {
        return temperatrue;
    }
    public float getPressure() {
        return pressure;
    }
    public float getHumidity() {
        return humidity;
    }
    public void dataChange() {
        //调用 接入方的 update
        currentConditions.update(getTemperature(), getPressure(), getHumidity());
    }
    //当数据有更新时，就调用 setData
    public void setData(float temperature, float pressure, float humidity) {
        this.temperatrue = temperature;
        this.pressure = pressure;
        this.humidity = humidity;
        //调用dataChange， 将最新的信息 推送给 接入方 currentConditions
        dataChange();
    }
}
```
### 19.5.观察者模式在Jdk应用的源码分析
1) Jdk的Observable类就使用了观察者模式
2) 代码分析+模式角色分析
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTJjZjFjZDVjZTdkNTU2M2QucG5n?x-oss-process=image/format,png)
3) 模式角色分析
- Observable 的作用和地位等价于 我们前面讲过Subject
- Observable 是类，不是接口，类中已经实现了核心的方法 ,即管理Observer
的方法 add.. delete .. notify...
- Observer 的作用和地位等价于我们前面讲过的 Observer, 有update
- Observable 和 Observer 的使用方法和前面讲过的一样，只是Observable 是
类，通过继承来实现观察者模式

## 20.中介者模式
### 20.1.基本介绍
1) 中介者模式（Mediator Pattern），用一个中介对象来封装一系列的对象交互。
中介者使各个对象不需要显式地相互引用，从而使其耦合松散，而且可以独立
地改变它们之间的交互
2) 中介者模式属于行为型模式，使代码易于维护
3) 比如MVC模式，C（Controller控制器）是M（Model模型）和V（View视图）的中
介者，在前后端交互时起到了中间人的作用
### 20.2.中介者模式的原理类图
对原理类图的说明-即(中介者模式的角色及职责)
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWE3MGEzNmZkZjFjOGI3ZGIucG5n?x-oss-process=image/format,png)
1) Mediator 就是抽象中介者,定义了同事对象到中介者对象的接口
2) Colleague 是抽象同事类
3) ConcreteMediator 具体的中介者对象, 实现抽象方法, 他需要知道所有的具体的同事类,即以一个集合来管理
### 20.3.中介者模式应用实例
1) 应用实例要求
完成前面的智能家庭的项目，使用中介者模式
2) 思路分析和图解(类图)
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTM4MDY5NjNjZGEyYjY3OTQucG5n?x-oss-process=image/format,png)
3) 代码实现
```java
//具体的同事类
public class Alarm extends Colleague {

    //构造器
    public Alarm(Mediator mediator, String name) {
        super(mediator, name);
        //在创建Alarm 同事对象时，将自己放入到ConcreteMediator 对象中[集合]
        mediator.Register(name, this);
    }

    public void SendAlarm(int stateChange) {
        SendMessage(stateChange);
    }

    @Override public void SendMessage(int stateChange) {
        //调用的中介者对象的getMessage
        this.GetMediator().GetMessage(stateChange, this.name);
    }
}
```
```java
public class ClientTest {

    public static void main(String[] args) {
        //创建一个中介者对象
        Mediator mediator = new ConcreteMediator();
        //创建Alarm 并且加入到  ConcreteMediator 对象的HashMap
        Alarm alarm = new Alarm(mediator, "alarm");
        //创建了CoffeeMachine 对象，并  且加入到  ConcreteMediator 对象的HashMap
        CoffeeMachine coffeeMachine = new CoffeeMachine(mediator, "coffeeMachine");
        //创建 Curtains , 并  且加入到  ConcreteMediator 对象的HashMap
        Curtains curtains = new Curtains(mediator, "curtains");
        TV tV = new TV(mediator, "TV");
        //让闹钟发出消息
        alarm.SendAlarm(0);
        coffeeMachine.FinishCoffee();
        alarm.SendAlarm(1);
    }
}
```
```java
public class CoffeeMachine extends Colleague {

    public CoffeeMachine(Mediator mediator, String name) {
        super(mediator, name);
        mediator.Register(name, this);
    }

    @Override public void SendMessage(int stateChange) {
        this.GetMediator().GetMessage(stateChange, this.name);
    }

    public void StartCoffee() {
        System.out.println("It's time to startcoffee!");
    }

    public void FinishCoffee() {
        System.out.println("After 5 minutes!");
        System.out.println("Coffee is ok!");
        SendMessage(0);
    }
}
```

```java
//同事抽象类
public abstract class Colleague {
    public String name;
    private Mediator mediator;

    public Colleague(Mediator mediator, String name) {
        this.mediator = mediator;
        this.name = name;

    }

    public Mediator GetMediator() {
        return this.mediator;
    }

    public abstract void SendMessage(int stateChange);
}
```

```java
package com.atguigu.mediator.smarthouse;

import java.util.HashMap;

//具体的中介者类
public class ConcreteMediator extends Mediator {
    //集合，放入所有的同事对象
    private HashMap<String, Colleague> colleagueMap;
    private HashMap<String, String> interMap;

    public ConcreteMediator() {
        colleagueMap = new HashMap<String, Colleague>();
        interMap = new HashMap<String, String>();
    }

    @Override public void Register(String colleagueName, Colleague colleague) {
        colleagueMap.put(colleagueName, colleague);
        if (colleague instanceof Alarm) {
            interMap.put("Alarm", colleagueName);
        } else if (colleague instanceof CoffeeMachine) {
            interMap.put("CoffeeMachine", colleagueName);
        } else if (colleague instanceof TV) {
            interMap.put("TV", colleagueName);
        } else if (colleague instanceof Curtains) {
            interMap.put("Curtains", colleagueName);
        }
    }

    //具体中介者的核心方法
    //1. 根据得到消息，完成对应任务
    //2. 中介者在这个方法，协调各个具体的同事对象，完成任务
    @Override public void GetMessage(int stateChange, String colleagueName) {
        //处理闹钟发出的消息
        if (colleagueMap.get(colleagueName) instanceof Alarm) {
            if (stateChange == 0) {
                ((CoffeeMachine)(colleagueMap.get(interMap.get("CoffeeMachine")))).StartCoffee();
                ((TV)(colleagueMap.get(interMap.get("TV")))).StartTv();
            } else if (stateChange == 1) {
                ((TV)(colleagueMap.get(interMap.get("TV")))).StopTv();
            }
        } else if (colleagueMap.get(colleagueName) instanceof CoffeeMachine) {
            ((Curtains)(colleagueMap.get(interMap.get("Curtains")))).UpCurtains();
        } else if (colleagueMap.get(colleagueName) instanceof TV) {//如果TV发现消息
        } else if (colleagueMap.get(colleagueName) instanceof Curtains) {
            //如果是以窗帘发出的消息，这里处理...
        }
    }

    @Override public void SendMessage() {
    }
}
```

```java
public class Curtains extends Colleague {

    public Curtains(Mediator mediator, String name) {
        super(mediator, name);
        mediator.Register(name, this);
    }

    @Override public void SendMessage(int stateChange) {
        this.GetMediator().GetMessage(stateChange, this.name);
    }

    public void UpCurtains() {
        System.out.println("I am holding Up Curtains!");
    }
}
```

```java
public abstract class Mediator {
    //将给中介者对象，加入到集合中
    public abstract void Register(String colleagueName, Colleague colleague);

    //接收消息, 具体的同事对象发出
    public abstract void GetMessage(int stateChange, String colleagueName);

    public abstract void SendMessage();
}
```

```java
public class TV extends Colleague {

    public TV(Mediator mediator, String name) {
        super(mediator, name);
        mediator.Register(name, this);
    }

    @Override public void SendMessage(int stateChange) {
        this.GetMediator().GetMessage(stateChange, this.name);
    }

    public void StartTv() {
        System.out.println("It's time to StartTv!");
    }

    public void StopTv() {
        System.out.println("StopTv!");
    }
}
```

### 20.4. 中介者模式的注意事项和细节 

1) 多个类相互耦合，会形成网状结构, 使用中介者模式将网状结构分离为星型结构， 进行解耦 

2) 减少类间依赖，降低了耦合，符合迪米特原则 

3) 中介者承担了较多的责任，一旦中介者出现了问题，整个系统就会受到影响 

4) 如果设计不当，中介者对象本身变得过于复杂，这点在实际使用时，要特别注意 

## 21. 备忘录模式 

### 21.1.基本介绍 

1) 备忘录模式（Memento Pattern）在不破坏封装性的前提下，捕获一个对象的内 部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保 存的状态 

2) 可以这里理解备忘录模式：现实生活中的备忘录是用来记录某些要去做的事情， 或者是记录已经达成的共同意见的事情，以防忘记了。而在软件层面，备忘录 模式有着相同的含义，备忘录对象主要用来记录一个对象的某种状态，或者某 些数据，当要做回退时，可以从备忘录对象里获取原来的数据进行恢复操作 

3) 备忘录模式属于行为型模式 

### 21.2. 备忘录模式的原理类图 

对原理类图的说明-即 (备忘录模式的角色及职责) 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWM3ZjNiMDFiMWQ3ZjlkODIucG5n?x-oss-process=image/format,png)

1) originator : 对象(需要保存 状态的对象) 

2) Memento ： 备忘录对象,负责 保存好记录，即Originator内部 状态 

3) Caretaker: 守护者对象,负责保存多个备忘录对象， 使用集合管理，提高效 率 

4) 说明：如果希望保存多个originator对象的不同时间的状态，也可以，只需要 要 HashMap\<String，集合\>

### 21.3. 备忘录模式原理代码实现 

```java
public class Caretaker {

    //在List 集合中会有很多的备忘录对象
    private List<Memento> mementoList = new ArrayList<Memento>();

    public void add(Memento memento) {
        mementoList.add(memento);
    }

    //获取到第index个Originator 的 备忘录对象(即保存状态)
    public Memento get(int index) {
        return mementoList.get(index);
    }
}
```

```java
public class Client {

    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();
        originator.setState(" 状态#1 攻击力 100 ");
        //保存了当前的状态
        caretaker.add(originator.saveStateMemento());
        originator.setState(" 状态#2 攻击力 80 ");
        caretaker.add(originator.saveStateMemento());
        originator.setState(" 状态#3 攻击力 50 ");
        caretaker.add(originator.saveStateMemento());
        System.out.println("当前的状态是 =" + originator.getState());
        //希望得到状态 1, 将 originator 恢复到状态1
        originator.getStateFromMemento(caretaker.get(0));
        System.out.println("恢复到状态1 , 当前的状态是");
        System.out.println("当前的状态是 =" + originator.getState());
    }
}
```

```java
public class Memento {
    private String state;

    //构造器
    public Memento(String state) {
        super();
        this.state = state;
    }

    public String getState() {
        return state;
    }
}
```

```java
public class Originator {

    private String state;//状态信息

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    //编写一个方法，可以保存一个状态对象 Memento
    //因此编写一个方法，返回 Memento
    public Memento saveStateMemento() {
        return new Memento(state);
    }

    //通过备忘录对象，恢复状态
    public void getStateFromMemento(Memento memento) {
        state = memento.getState();
    }
}
```

### 21.4. 游戏角色恢复状态实例 

1) 应用实例要求 游戏角色有攻击力和防御力，在大战Boss前保存自身的状态(攻击力和防御力)，当大战 Boss后攻击力和防御力下降，从备忘录对象恢复到大战前的状态 

2) 思路分析和图解(类图) 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTJkZDA3YjMzN2JhZDk3MTgucG5n?x-oss-process=image/format,png)

3) 代码实现 

```java
//守护者对象, 保存游戏角色的状态
public class Caretaker {

    //如果只保存一次状态
    private Memento memento;
    //对GameRole 保存多次状态
    //private ArrayList<Memento> mementos;
    //对多个游戏角色保存多个状态
    //private HashMap<String, ArrayList<Memento>> rolesMementos;

    public Memento getMemento() {
        return memento;
    }

    public void setMemento(Memento memento) {
        this.memento = memento;
    }
}
```

```java
public class Client {

    public static void main(String[] args) {
        //创建游戏角色
        GameRole gameRole = new GameRole();
        gameRole.setVit(100);
        gameRole.setDef(100);
        System.out.println("和boss大战前的状态");
        gameRole.display();
        //把当前状态保存caretaker
        Caretaker caretaker = new Caretaker();
        caretaker.setMemento(gameRole.createMemento());
        System.out.println("和boss大战~~~");
        gameRole.setDef(30);
        gameRole.setVit(30);
        gameRole.display();
        System.out.println("大战后，使用备忘录对象恢复到站前");
        gameRole.recoverGameRoleFromMemento(caretaker.getMemento());
        System.out.println("恢复后的状态");
        gameRole.display();
    }
}
```

```java
public class GameRole {

    private int vit;
    private int def;

    //创建Memento ,即根据当前的状态得到Memento
    public Memento createMemento() {
        return new Memento(vit, def);
    }

    //从备忘录对象，恢复GameRole的状态
    public void recoverGameRoleFromMemento(Memento memento) {
        this.vit = memento.getVit();
        this.def = memento.getDef();
    }

    //显示当前游戏角色的状态
    public void display() {
        System.out.println("游戏角色当前的攻击力：" + this.vit + " 防御力: " + this.def);
    }

    public int getVit() {
        return vit;
    }

    public void setVit(int vit) {
        this.vit = vit;
    }

    public int getDef() {
        return def;
    }

    public void setDef(int def) {
        this.def = def;
    }
}
```

```java
public class Memento {
    //攻击力
    private int vit;
    //防御力
    private int def;

    public Memento(int vit, int def) {
        super();
        this.vit = vit;
        this.def = def;
    }

    public int getVit() {
        return vit;
    }

    public void setVit(int vit) {
        this.vit = vit;
    }

    public int getDef() {
        return def;
    }

    public void setDef(int def) {
        this.def = def;
    }
}
```

### 21.5. 备忘录模式的注意事项和细节 

1) 给用户提供了一种可以恢复状态的机制，可以使用户能够比较方便地回到某个历史 的状态 

2) 实现了信息的封装，使得用户不需要关心状态的保存细节 

3) 如果类的成员变量过多，势必会占用比较大的资源，而且每一次保存都会消耗一定 的内存, 这个需要注意 

4) 适用的应用场景：1、后悔药。 2、打游戏时的存档。 3、Windows 里的 ctri + z。 4、IE 中的后退。 4、数据库的事务管理 

5) 为了节约内存，备忘录模式可以和原型模式配合使用 

## 22. 解释器模式 

### 22.1. 基本介绍 

1) 在编译原理中，一个算术表达式通过词法分析器形成词法单元，而后这些词法 单元再通过语法分析器构建语法分析树，最终形成一颗抽象的语法分析树。这 里的词法分析器和语法分析器都可以看做是解释器 

2) 解释器模式（Interpreter Pattern）：是指给定一个语言(表达式)，定义它的文法 的一种表示，并定义一个解释器，使用该解释器来解释语言中的句子(表达式) 

3) 应用场景 

• 应用可以将一个需要解释执行的语言中的句子表示为一个抽象语法树 

• 一些重复出现的问题可以用一种简单的语言来表达 

• 一个简单语法需要解释的场景 

4) 这样的例子还有，比如编译器、运算表达式计算、正则表达式、机器人等 

### 22.2. 解释器模式的原理类图 

对原理类图的说明-即(解释器模式的角色及职责) 

<img src="https://upload-images.jianshu.io/upload_images/3128142-8a351e73384b0acf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" style="zoom:50%;" />

1) Context: 是环境角色,含有解释器之外的全局信息. 

2) AbstractExpression: 抽象表达式， 声明一个抽象的解释操作,这个方法为抽象语法树中所有的节点所 共享 

3) TerminalExpression: 为终结符表达式, 实现与文法中的终结符相关的解释操作 

4) NonTermialExpression: 为非终结符表达式，为文法中的非终结符实现解释操作. 

5) 说明： 输入Context he TerminalExpression 信息通过Client 输入即可 

### 22.2. 解释器模式来实现四则 

1) 应用实例要求 通过解释器模式来实现四则运算， 如计算a+b-c的值 

2) 思路分析和图解(类图) 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTdlMTViNTRkNTQ4ODBjMTcucG5n?x-oss-process=image/format,png)

3) 代码实现 

```java
/**
 * 加法解释器
 */
public class AddExpression extends SymbolExpression {

    public AddExpression(Expression left, Expression right) {
        super(left, right);
    }

    //处理相加
    //var 仍然是 {a=10,b=20}..
    //一会我们debug 源码,就ok
    public int interpreter(HashMap<String, Integer> var) {
        //super.left.interpreter(var) ： 返回 left 表达式对应的值 a = 10
        //super.right.interpreter(var): 返回right 表达式对应值 b = 20
        return super.left.interpreter(var) + super.right.interpreter(var);
    }
}
```

```java
public class Calculator {

    // 定义表达式
    private Expression expression;

    // 构造函数传参，并解析
    public Calculator(String expStr) { // expStr = a+b
        // 安排运算先后顺序
        Stack<Expression> stack = new Stack<>();
        // 表达式拆分成字符数组 
        char[] charArray = expStr.toCharArray();// [a, +, b]
        Expression left = null;
        Expression right = null;
        //遍历我们的字符数组， 即遍历  [a, +, b]
        //针对不同的情况，做处理
        for (int i = 0; i < charArray.length; i++) {
            switch (charArray[i]) {
                case '+': //
                    left = stack.pop();// 从stack取出left => "a"
                    right = new VarExpression(String.valueOf(charArray[++i]));// 取出右表达式 "b"
                    stack.push(new AddExpression(left, right));// 然后根据得到left 和 right 构建 AddExpresson加入stack
                    break;
                case '-': // 
                    left = stack.pop();
                    right = new VarExpression(String.valueOf(charArray[++i]));
                    stack.push(new SubExpression(left, right));
                    break;
                default:
                    //如果是一个 Var 就创建要给 VarExpression 对象，并push到 stack
                    stack.push(new VarExpression(String.valueOf(charArray[i])));
                    break;
            }
        }
        //当遍历完整个 charArray 数组后，stack 就得到最后Expression
        this.expression = stack.pop();
    }

    public int run(HashMap<String, Integer> var) {
        //最后将表达式a+b和 var = {a=10,b=20}
        //然后传递给expression的interpreter进行解释执行
        return this.expression.interpreter(var);
    }
}
```

```java
public class ClientTest {

    public static void main(String[] args) throws IOException {
        String expStr = getExpStr(); // a+b
        HashMap<String, Integer> var = getValue(expStr);// var {a=10, b=20}
        Calculator calculator = new Calculator(expStr);
        System.out.println("运算结果：" + expStr + "=" + calculator.run(var));
    }

    // 获得表达式
    public static String getExpStr() throws IOException {
        System.out.print("请输入表达式：");
        return (new BufferedReader(new InputStreamReader(System.in))).readLine();
    }

    // 获得值映射
    public static HashMap<String, Integer> getValue(String expStr) throws IOException {
        HashMap<String, Integer> map = new HashMap<>();
        for (char ch : expStr.toCharArray()) {
            if (ch != '+' && ch != '-') {
                if (!map.containsKey(String.valueOf(ch))) {
                    System.out.print("请输入" + ch + "的值：");
                    String in = (new BufferedReader(new InputStreamReader(System.in))).readLine();
                    map.put(String.valueOf(ch), Integer.valueOf(in));
                }
            }
        }
        return map;
    }
}
```

```java
/**
 * 抽象类表达式，通过HashMap 键值对, 可以获取到变量的值
 */
public abstract class Expression {
    // a + b - c
    // 解释公式和数值, key 就是公式(表达式) 参数[a,b,c], value就是就是具体值
    // HashMap {a=10, b=20}
    public abstract int interpreter(HashMap<String, Integer> var);
}
```

```java
public class SubExpression extends SymbolExpression {

	public SubExpression(Expression left, Expression right) {
		super(left, right);
	}

	//求出left 和 right 表达式相减后的结果
	public int interpreter(HashMap<String, Integer> var) {
		return super.left.interpreter(var) - super.right.interpreter(var);
	}
}
```

```java
/**
 * 抽象运算符号解析器 这里，每个运算符号，都只和自己左右两个数字有关系，
 * 但左右两个数字有可能也是一个解析的结果，无论何种类型，都是Expression类的实现类
 */
public class SymbolExpression extends Expression {

    protected Expression left;
    protected Expression right;

    public SymbolExpression(Expression left, Expression right) {
        this.left = left;
        this.right = right;
    }

    //因为 SymbolExpression 是让其子类来实现，因此 interpreter 是一个默认实现
    @Override public int interpreter(HashMap<String, Integer> var) {
        return 0;
    }
}
```

```java
/**
 * 变量的解释器
 */
public class VarExpression extends Expression {

    private String key; // key=a,key=b,key=c

    public VarExpression(String key) {
        this.key = key;
    }

    // var 就是{a=10, b=20}
    // interpreter 根据 变量名称，返回对应值
    @Override public int interpreter(HashMap<String, Integer> var) {
        return var.get(this.key);
    }
}
```

### 22.3. 解释器模式在Spring框架应用的源码剖析 

1) Spring框架中 SpelExpressionParser就使用到解释器模式 

2) 代码分析+Debug源码 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWYyNzUwYTQ5NDVmNGM2MDIucG5n?x-oss-process=image/format,png)

 3) 说明 

- Expression 接口 表达式接口 
- 下面有不同的实现类，比如SpelExpression, 或者CompositeStringExpression。 
- 使用时候，根据你创建的不同的Parser 对象，返回不同的 Expression 对象 
- ![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWFkNzc5MGNjYTFiYmEwODYucG5n?x-oss-process=image/format,png)
- 使用得当 Expression对象，调用getValue 解释执行 表达式，最后得到结果 

### 22.4. 解释器模式的注意事项和细节 

1) 当有一个语言需要解释执行，可将该语言中的句子表示为一个抽象语法树，就可以 考虑使用解释器模式，让程序具有良好的扩展性 

2) 应用场景：编译器、运算表达式计算、正则表达式、机器人等 

3) 使用解释器可能带来的问题：解释器模式会引起类膨胀、解释器模式采用递归调用 方法，将会导致调试非常复杂、效率可能降低. 

## 23. 状态模式 

### 23.1. 基本介绍 

1) 状态模式（State Pattern）：它主要用来解决对象在多种状态转换时，需要对外 输出不同的行为的问题。状态和行为是一一对应的，状态之间可以相互转换 

2) 当一个对象的内在状态改变时，允许改变其行为，这个对象看起来像是改变了 其类 

### 23.2. 状态模式的原理类图 对原理类图的说明-即(状态模式的角色及职责) 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTdmZDlhMWI5YTBlODgwNzAucG5n?x-oss-process=image/format,png)

1) Context 类为环境角色, 用于维护State实例,这个实例定义当前状态 

2) State 是抽象状态角色,定义一个接口封装与Context 的一个特点接口相关行为 

3) ConcreteState 具体的状态角色，每个子类实现一个与Context 的一个状态相关行为 

### 23.3. 状态模式在实际项目-借贷平台 源码剖析 

1) 借贷平台的订单，有审核-发布-抢单 等等 步骤，随着操作的不同，会改变订单的 状态, 项目中的这个模块实现就会使用到状态模式 

2) 通常通过if/else判断订单的状态，从而实现不同的逻辑，伪代码如下 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTU0YWEwNjIzYWNjMjUzOWUucG5n?x-oss-process=image/format,png)

 3) 使用状态模式完成 借贷平台项目的审核模块 [设计+代码] 

```java
/**
 * 可以抽奖的状态
 */
public class CanRaffleState extends State {

    RaffleActivity activity;

    public CanRaffleState(RaffleActivity activity) {
        this.activity = activity;
    }

    //已经扣除了积分，不能再扣
    @Override public void deductMoney() {
        System.out.println("已经扣取过了积分");
    }

    //可以抽奖, 抽完奖后，根据实际情况，改成新的状态
    @Override public boolean raffle() {
        System.out.println("正在抽奖，请稍等！");
        Random r = new Random();
        int num = r.nextInt(10);
        // 10%中奖机会
        if (num == 0) {
            // 改变活动状态为发放奖品 context
            activity.setState(activity.getDispenseState());
            return true;
        } else {
            System.out.println("很遗憾没有抽中奖品！");
            // 改变状态为不能抽奖
            activity.setState(activity.getNoRafflleState());
            return false;
        }
    }

    // 不能发放奖品
    @Override public void dispensePrize() {
        System.out.println("没中奖，不能发放奖品");
    }
}
```

```java
/**
 * 状态模式测试类
 */
public class ClientTest {

    public static void main(String[] args) {
        // 创建活动对象，奖品有1个奖品
        RaffleActivity activity = new RaffleActivity(1);
        // 我们连续抽300次奖
        for (int i = 0; i < 30; i++) {
            System.out.println("--------第" + (i + 1) + "次抽奖----------");
            // 参加抽奖，第一步点击扣除积分
            activity.debuctMoney();
            // 第二步抽奖
            activity.raffle();
        }
    }
}
```

```java
/**
 * 奖品发放完毕状态
 * 说明，当我们activity 改变成 DispenseOutState， 抽奖活动结束
 */
public class DispenseOutState extends State {

    // 初始化时传入活动引用
    RaffleActivity activity;

    public DispenseOutState(RaffleActivity activity) {
        this.activity = activity;
    }

    @Override public void deductMoney() {
        System.out.println("奖品发送完了，请下次再参加");
    }

    @Override public boolean raffle() {
        System.out.println("奖品发送完了，请下次再参加");
        return false;
    }

    @Override public void dispensePrize() {
        System.out.println("奖品发送完了，请下次再参加");
    }
}
```

```java
/**
 * 发放奖品的状态
 */
public class DispenseState extends State {

    // 初始化时传入活动引用，发放奖品后改变其状态
    RaffleActivity activity;

    public DispenseState(RaffleActivity activity) {
        this.activity = activity;
    }

    @Override public void deductMoney() {
        System.out.println("不能扣除积分");
    }

    @Override public boolean raffle() {
        System.out.println("不能抽奖");
        return false;
    }

    //发放奖品
    @Override public void dispensePrize() {
        if (activity.getCount() > 0) {
            System.out.println("恭喜中奖了");
            // 改变状态为不能抽奖
            activity.setState(activity.getNoRafflleState());
        } else {
            System.out.println("很遗憾，奖品发送完了");
            // 改变状态为奖品发送完毕, 后面我们就不可以抽奖
            activity.setState(activity.getDispensOutState());
            //System.out.println("抽奖活动结束");
            //System.exit(0);
        }
    }
}
```

```java
/**
 * 不能抽奖状态
 */
public class NoRaffleState extends State {

    // 初始化时传入活动引用，扣除积分后改变其状态
    RaffleActivity activity;

    public NoRaffleState(RaffleActivity activity) {
        this.activity = activity;
    }

    // 当前状态可以扣积分 , 扣除后，将状态设置成可以抽奖状态
    @Override public void deductMoney() {
        System.out.println("扣除50积分成功，您可以抽奖了");
        activity.setState(activity.getCanRaffleState());
    }

    // 当前状态不能抽奖
    @Override public boolean raffle() {
        System.out.println("扣了积分才能抽奖喔！");
        return false;
    }

    // 当前状态不能发奖品
    @Override public void dispensePrize() {
        System.out.println("不能发放奖品");
    }
}
```

```java
/**
 * 抽奖活动
 */
public class RaffleActivity {

    // state 表示活动当前的状态，是变化
    State state = null;
    // 奖品数量
    int count = 0;

    // 四个属性，表示四种状态
    State noRafflleState = new NoRaffleState(this);
    State canRaffleState = new CanRaffleState(this);

    State dispenseState = new DispenseState(this);
    State dispensOutState = new DispenseOutState(this);

    //构造器
    //1. 初始化当前的状态为 noRafflleState（即不能抽奖的状态）
    //2. 初始化奖品的数量 
    public RaffleActivity(int count) {
        this.state = getNoRafflleState();
        this.count = count;
    }

    //扣分, 调用当前状态的 deductMoney
    public void debuctMoney() {
        state.deductMoney();
    }

    //抽奖 
    public void raffle() {
        // 如果当前的状态是抽奖成功
        if (state.raffle()) {
            //领取奖品
            state.dispensePrize();
        }
    }

    public State getState() {
        return state;
    }

    public void setState(State state) {
        this.state = state;
    }

    //这里请大家注意，每领取一次奖品，count--
    public int getCount() {
        int curCount = count;
        count--;
        return curCount;
    }

    public void setCount(int count) {
        this.count = count;
    }

    public State getNoRafflleState() {
        return noRafflleState;
    }

    public void setNoRafflleState(State noRafflleState) {
        this.noRafflleState = noRafflleState;
    }

    public State getCanRaffleState() {
        return canRaffleState;
    }

    public void setCanRaffleState(State canRaffleState) {
        this.canRaffleState = canRaffleState;
    }

    public State getDispenseState() {
        return dispenseState;
    }

    public void setDispenseState(State dispenseState) {
        this.dispenseState = dispenseState;
    }

    public State getDispensOutState() {
        return dispensOutState;
    }

    public void setDispensOutState(State dispensOutState) {
        this.dispensOutState = dispensOutState;
    }
}
```

```java
/**
 * 状态抽象类
 */
public abstract class State {

    // 扣除积分 - 50
    public abstract void deductMoney();

    // 是否抽中奖品
    public abstract boolean raffle();

    // 发放奖品
    public abstract void dispensePrize();
}
```

### 23.4. 状态模式的注意事项和细节 

1) 代码有很强的可读性。状态模式将每个状态的行为封装到对应的一个类中 

2) 方便维护。将容易产生问题的if-else语句删除了，如果把每个状态的行为都放到一 个类中，每次调用方法时都要判断当前是什么状态，不但会产出很多if-else语句， 而且容易出错 

3) 符合“开闭原则”。容易增删状态 

4) 会产生很多类。每个状态都要一个对应的类，当状态过多时会产生很多类，加大维 护难度 

5) 应用场景：当一个事件或者对象有很多种状态，状态之间会相互转换，对不同的状 态要求有不同的行为的时候，可以考虑使用状态模式 

## 24. 策略模式 

### 24.1. 基本介绍 

1) 策略模式（Strategy Pattern）中，定义算法族，分别封装起来，让他们之间可以 互相替换，此模式让算法的变化独立于使用算法的客户 

2) 这算法体现了几个设计原则，第一、把变化的代码从不变的代码中分离出来； 第二、针对接口编程而不是具体类（定义了策略接口）；第三、多用组合/聚合， 少用继承（客户通过组合方式使用策略）。 

### 24.2. 策略模式的原理类图 

<img src="https://upload-images.jianshu.io/upload_images/3128142-ed1ee28e245df6f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" style="zoom:50%;" />

说明：从上图可以看到，客户context 有成员变量strategy或者其他的策略接口 ,至于需要使用到哪个策略，我们可以在构造器中指定 

### 24.3. 策略模式解决鸭子问题 

1) 应用实例要求 编写程序完成前面的鸭子项目，要求使用策略模式 

2) 思路分析(类图) 策略模式：分别封装行为接口，实现算法族，超类里放行为接口对象，在子类里具体 设定行为对象。原则就是：分离变化部分，封装接口，基于接口编程各种功能。此模 式让行为的变化独立于算法的使用者 

<img src="https://upload-images.jianshu.io/upload_images/3128142-25e20c0aaea0a0fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" style="zoom:50%;" />

3) 代码实现 

```java
public class BadFlyBehavior implements FlyBehavior {

    @Override public void fly() {
        System.out.println(" 飞翔技术一般 ");
    }
}
```

```java
public class Client {

    public static void main(String[] args) {
        WildDuck wildDuck = new WildDuck();
        wildDuck.fly();//
        ToyDuck toyDuck = new ToyDuck();
        toyDuck.fly();
        PekingDuck pekingDuck = new PekingDuck();
        pekingDuck.fly();
        //动态改变某个对象的行为, 北京鸭 不能飞
        pekingDuck.setFlyBehavior(new NoFlyBehavior());
        System.out.println("北京鸭的实际飞翔能力");
        pekingDuck.fly();
    }
}
```

```java
public abstract class Duck {

    //属性, 策略接口
    FlyBehavior flyBehavior;
    //其它属性<->策略接口
    QuackBehavior quackBehavior;

    public Duck() {
    }

    public abstract void display();//显示鸭子信息

    public void quack() {
        System.out.println("鸭子嘎嘎叫~~");
    }

    public void swim() {
        System.out.println("鸭子会游泳~~");
    }

    public void fly() {
        //改进
        if (flyBehavior != null) {
            flyBehavior.fly();
        }
    }

    public void setFlyBehavior(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }

    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }
}
```

```java
public interface FlyBehavior {

    void fly(); // 子类具体实现
}
```

```java
public class GoodFlyBehavior implements FlyBehavior {

    @Override public void fly() {
        System.out.println(" 飞翔技术高超 ~~~");
    }
}
```

```java
public class NoFlyBehavior implements FlyBehavior {

	@Override public void fly() {
		System.out.println(" 不会飞翔  ");
	}
}
```

```java
public class PekingDuck extends Duck {

    //假如北京鸭可以飞翔，但是飞翔技术一般
    public PekingDuck() {
        flyBehavior = new BadFlyBehavior();

    }

    @Override public void display() {
        System.out.println("~~北京鸭~~~");
    }
}
```

```java
public interface QuackBehavior {
    void quack();//子类实现
}
```

```java
public class ToyDuck extends Duck {

    public ToyDuck() {
        flyBehavior = new NoFlyBehavior();
    }

    @Override public void display() {
        System.out.println("玩具鸭");
    }

    //需要重写父类的所有方法
    public void quack() {
        System.out.println("玩具鸭不能叫~~");
    }

    public void swim() {
        System.out.println("玩具鸭不会游泳~~");
    }
}
```

```java
public class WildDuck extends Duck {

    //构造器，传入FlyBehavor 的对象
    public WildDuck() {
        flyBehavior = new GoodFlyBehavior();
    }

    @Override public void display() {
        System.out.println(" 这是野鸭 ");
    }
}
```

### 24.4. 策略模式在JDK-Arrays 应用的源码分析 

1) JDK的 Arrays 的Comparator就使用了策略模式 

2) 代码分析+Debug源码+模式角色分析  

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLWUxODc2ZDZhZTBiODczZjMucG5n?x-oss-process=image/format,png)

```java
public class Decorator {

    public static void main(String[] args) throws Exception {
        //说明
        //1. InputStream 是抽象类, 类似我们前面讲的 Drink
        //2. FileInputStream 是  InputStream 子类，类似我们前面的 DeCaf, LongBlack
        //3. FilterInputStream  是  InputStream 子类：类似我们前面 的 Decorator 修饰者
        //4. DataInputStream 是 FilterInputStream 子类，具体的修饰者，类似前面的 Milk, Soy 等
        //5. FilterInputStream 类 有  protected volatile InputStream in; 即含被装饰者
        //6. 分析得出在jdk 的io体系中，就是使用装饰者模式
        DataInputStream dis = new DataInputStream(new FileInputStream("d:\\abc.txt"));
        System.out.println(dis.read());
        dis.close();
    }
}
```

### 24.5. 策略模式的注意事项和细节 

1) 策略模式的关键是：分析项目中变化部分与不变部分 

2) 策略模式的核心思想是：多用组合/聚合 少用继承；用行为类组合，而不是行为的 继承。更有弹性 

3) 体现了“对修改关闭，对扩展开放”原则，客户端增加行为不用修改原有代码，只 要添加一种策略（或者行为）即可，避免了使用多重转移语句（if..else if..else） 

4) 提供了可以替换继承关系的办法： 策略模式将算法封装在独立的Strategy类中使得 你可以独立于其Context改变它，使它易于切换、易于理解、易于扩展 

5) 需要注意的是：每添加一个策略就要增加一个类，当策略过多是会导致类数目庞大 

## 25. 职责链模式 

### 25.1. 基本介绍 

1) 职责链模式（Chain of Responsibility Pattern）, 又叫 责任链模式，为请求创建了一个接收者 对象的链(简单示意图)。这种模式对请求的 发送者和接收者进行解耦。 

2) 职责链模式通常每个接收者都包含对另一个接 收者的引用。如果一个对象不能处理该请求， 那么它会把相同的请求传给下一个接收者，依 此类推。 

3) 这种类型的设计模式属于行为型模式 

### 25.2. 职责链模式的原理类图 

对原理类图的说明-即(职责链模式的角色及职责) 

 职责链模式（Chain Of Responsibility）， 使多个对象都有机会处理请求，从而避 免请求的发送者和接收者之间的耦合关 系。将这个对象连成一条链，并沿着这 条链传递该请求，直到有一个对象处理 它为止. 

<img src="https://upload-images.jianshu.io/upload_images/3128142-aeecb884652472d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" style="zoom:50%;" />

1) Handler : 抽象的处理者, 定义了一个处理请求的接口, 同时含义另外Handler 

2) ConcreteHandlerA , B 是具体的处理者, 处理它自己负责的请求， 可以访问它的后继者(即下一个处 理者), 如果可以处理当前请求，则处理，否则就将该请求交个 后继者去处理，从而形成一个职责链 

3) Request ， 含义很多属性，表示一个请求  

### 25.3. 职责链模式解决OA系统采购审批 

1) 应用实例要求 编写程序完成学校OA系统的采购审批项目：需求 

• 采购员采购教学器材 

• 如果金额 小于等于5000, 由教学主任审批 

• 如果金额 小于等于10000, 由院长审批 

• 如果金额 小于等于30000, 由副校长审批 

• 如果金额 超过30000以上，有校长审批 

2) 思路分析和图解(类图) 

<img src="https://upload-images.jianshu.io/upload_images/3128142-a7429c0026fa35ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" style="zoom:50%;" />

3) 代码实现 

```java
public abstract class Approver {

    Approver approver;  //下一个处理者
    String name; // 名字

    public Approver(String name) {
        this.name = name;
    }

    //下一个处理者
    public void setApprover(Approver approver) {
        this.approver = approver;
    }

    //处理审批请求的方法，得到一个请求, 处理是子类完成，因此该方法做成抽象
    public abstract void processRequest(PurchaseRequest purchaseRequest);
}
```

```java
public class Client {

    public static void main(String[] args) {
        //创建一个请求
        PurchaseRequest purchaseRequest = new PurchaseRequest(1, 31000, 1);
        //创建相关的审批人
        DepartmentApprover departmentApprover = new DepartmentApprover("张主任");
        CollegeApprover collegeApprover = new CollegeApprover("李院长");
        ViceSchoolMasterApprover viceSchoolMasterApprover = new ViceSchoolMasterApprover("王副校");
        SchoolMasterApprover schoolMasterApprover = new SchoolMasterApprover("佟校长");
        //需要将各个审批级别的下一个设置好 (处理人构成环形: )
        departmentApprover.setApprover(collegeApprover);
        collegeApprover.setApprover(viceSchoolMasterApprover);
        viceSchoolMasterApprover.setApprover(schoolMasterApprover);
        schoolMasterApprover.setApprover(departmentApprover);
        departmentApprover.processRequest(purchaseRequest);
        viceSchoolMasterApprover.processRequest(purchaseRequest);
    }
}
```

```java
public class CollegeApprover extends Approver {

    public CollegeApprover(String name) {
        super(name);
    }

    @Override public void processRequest(PurchaseRequest purchaseRequest) {
        // TODO Auto-generated method stub
        if (purchaseRequest.getPrice() < 5000 && purchaseRequest.getPrice() <= 10000) {
            System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
        } else {
            approver.processRequest(purchaseRequest);
        }
    }
}
```

```java
public class DepartmentApprover extends Approver {

    public DepartmentApprover(String name) {
        super(name);
    }

    @Override public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() <= 5000) {
            System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
        } else {
            approver.processRequest(purchaseRequest);
        }
    }
}
```

```java
//请求类
public class PurchaseRequest {

    private int type = 0; //请求类型
    private float price = 0.0f; //请求金额
    private int id = 0;

    //构造器
    public PurchaseRequest(int type, float price, int id) {
        this.type = type;
        this.price = price;
        this.id = id;
    }

    public int getType() {
        return type;
    }

    public float getPrice() {
        return price;
    }

    public int getId() {
        return id;
    }
}
```

```java
public class SchoolMasterApprover extends Approver {

    public SchoolMasterApprover(String name) {
        super(name);
    }

    @Override public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() > 30000) {
            System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
        } else {
            approver.processRequest(purchaseRequest);
        }
    }
}
```

```java
public class ViceSchoolMasterApprover extends Approver {

    public ViceSchoolMasterApprover(String name) {
        super(name);
    }

    @Override public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() < 10000 && purchaseRequest.getPrice() <= 30000) {
            System.out.println(" 请求编号 id= " + purchaseRequest.getId() + " 被 " + this.name + " 处理");
        } else {
            approver.processRequest(purchaseRequest);
        }
    }
}
```

### 25.4. 职责链模式在SpringMVC框架应用的源码分析 

1) SpringMVC-HandlerExecutionChain 类就使用到职责链模式 

2) SpringMVC请求流程简图 

3) 代码分析+Debug源码+说明 

![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8zMTI4MTQyLTkwZDc1MTU3ZTVmM2UyZmUucG5n?x-oss-process=image/format,png)


```java
public class DispatchServlet {

    public static List<HandlerAdapter> handlerAdapters = new ArrayList<HandlerAdapter>();

    public DispatchServlet() {
        handlerAdapters.add(new AnnotationHandlerAdapter());
        handlerAdapters.add(new HttpHandlerAdapter());
        handlerAdapters.add(new SimpleHandlerAdapter());
    }

    public static void main(String[] args) {
        new DispatchServlet().doDispatch(); // http...
    }

    public void doDispatch() {

        // 此处模拟SpringMVC从request取handler的对象，
        // 适配器可以获取到希望的Controller
        HttpController controller = new HttpController();
        // AnnotationController controller = new AnnotationController();
        //SimpleController controller = new SimpleController();
        // 得到对应适配器
        HandlerAdapter adapter = getHandler(controller);
        // 通过适配器执行对应的controller对应方法
        adapter.handle(controller);
    }

    public HandlerAdapter getHandler(Controller controller) {
        //遍历：根据得到的controller(handler), 返回对应适配器
        for (HandlerAdapter adapter : handlerAdapters) {
            if (adapter.supports(controller)) {
                return adapter;
            }
        }
        return null;
    }
}
```

对源码总结： 

- springmvc 请求的流程图中，执行了 拦截器相关方法 interceptor.preHandler 等等 
-  在处理SpringMvc请求时，使用到职责链模式还使用到适配器模式 
- HandlerExecutionChain 主要负责的是请求拦截器的执行和请求处理,但是他本身不 处理请求，只是将请求分配给链上注册处理器执行，这是职责链实现方式,减少职责 链本身与处理逻辑之间的耦合,规范了处理流程 
- HandlerExecutionChain 维护了 HandlerInterceptor 的集合， 可以向其中注册相应 的拦截器. 

### 25.5. 职责链模式的注意事项和细节 

1) 将请求和处理分开，实现解耦，提高系统的灵活性 

2) 简化了对象，使对象不需要知道链的结构 

3) 性能会受到影响，特别是在链比较长的时候，因此需控制链中最大节点数量，一般 通过在Handler中设置一个最大节点数量，在setNext()方法中判断是否已经超过阀值， 超过则不允许该链建立，避免出现超长链无意识地破坏系统性能 

4) 调试不方便。采用了类似递归的方式，调试时逻辑可能比较复杂 

5) 最佳应用场景：有多个对象可以处理同一个请求时，比如：多级请求、请假/加薪 等审批流程、Java Web中Tomcat对Encoding的处理、拦截器 

## 26. 设计模式在框架或项目中源码分析的说明 

1) 为了让同学们可以更加深入的理解某个设计模式，我们会找该模式在spring, springMVC, MyBatis 或者 JDK 源码 中的应用 

2) 有一点需要说明：设计模式是程序员在编程中，有意或者是无意使用到的(也不是 所有的程序员都学习过设计模式)，并且同一种设计模式实现方式也不是100%的一 样，设计模式主要是提高程序的扩展性，可读性，可维护性、规范性。 

3) 所以在我们讲某个设计模式在源码框架中使用时，和我们的标准的设计模式写法可 能会有些出入，比如组合模式 Component 可以是抽象类，接口，也可以是一个实 现类， 我们讲源码时(JDK HashMap源码)，Component 就可能不一样 

 4) 对于框架源码，源码中部分使用了A设计模式，还部分使用了B设计模式，也是有 可能的，也就是说设计模式是可以结合使用的 

5) .因为设计模式主要是一种编程思想，既然是思想，具体实现方式，就不可能100% 的一样(当然，程序的设计结构基本是一样的) 

6) 所以，提醒大家我们学习设计模式时，（包括看源码分析），要抓住本质，就是使 用这个设计模式到底带了了什么好处？ 是 扩展性提高了，还是更加规范了，这样 我们才能领会设计模式的精妙之处。 

