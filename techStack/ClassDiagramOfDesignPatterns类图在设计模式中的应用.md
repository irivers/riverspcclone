# 设计模式中的类图

> 初衷就是将23种设计模式，以类图的形式展现出来。第一版只有类图，没有其他@20181005
>
> 看不懂的地方，就先硬着头皮趟过去，趟完了这一小节，再回过头来看一下@20181006

## Creational design patterns- 创造型模式

> The design patterns are all about class instantiation.This pattern can be further divided into class-creation patterns and object-creational patterns. While class-creation patterns use inheritance effectively in the instantiation process, object-creation patterns use delegation effectively go get the job done

创建型模式是处理对象创建的设计模式，试图根据实际情况使用合适的方式创建对象。

基本的对象创建方式可能会导致设计上的问题，或增加设计的复杂度。创建型模式通过以某种方式控制对象的创建来解决问题。

创建型模式由两个主导思想构成:

1. 将系统使用的具体类封装起来
2. 隐藏这些具体类的实例创建和结合的方式

创建型模型又分为对象创建模式和类创建模式：

- 对象创建型模式处理对象的创建：把对象创建的一部分推迟到另一个对象中
- 类创建型模式处理类的创建：将它对象的创建推迟到子类中



### 1. Abstract Factory-- 抽象工厂模式

> Creates an instance of serveral families classes
>
> 为一个产品族提供了统一的创建接口

#### 模式动机 Motivation

- 产品等级结构
  - 即产品的继承结构
  - 如一个抽象类是电视机，其子类有海尔电视机、海信电视机、TCL电视机，则抽象电视机与具体品牌电视机之间构成了一个产品等级结构，抽象电视机是父类，而具体的品牌电视机是其子类
- 产品族
  - 由同一个工厂生产的，位于不同产品等级结构中的一组产品
  - 如海尔电器工厂生产海尔电视机、海尔电冰箱，海尔电视机位于电视机产品等级结构中，海尔电冰箱位于电冰箱等级结构中
- 在工厂方法中，具体的工厂负责生产具体的产品，每一个具体工厂对应着一种具体产品，工厂方法具有唯一性。工厂方法模式针对的是一个产品等级结构。抽象工厂模式则需要面对多个产品等级结构
- 抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形态

#### 模式扩展

- 工厂模式的退化
  - 当抽象工厂模式中，每一个具体工厂类只创建一个产品对象，也就是指存在一个产品等级结构时，抽象工厂模式退化为工厂方法模式
  - 当工厂方法模式中抽象工厂与具体工厂合并，提供一个统一的工厂来创建产品对象，并将创建对象的工厂方法设计为静态方法时，工厂方法模式退化成简单工厂模式
- “开闭原则”的倾斜性
  - ”开闭原则“要求系统对扩展开放，对修改封闭，通过扩展达到增强其功能的目的
    - 增加产品族：只需增加一个新的具体工厂即可，对已有代码无须做任何修改。很好支持了”开闭原则“
    - 增加新的产品等级结构：需要修改所有的工厂角色，包括抽象工厂类，在所有的工厂类中都需要增加生成新产品的方法，不能很好地支持”开闭原则“

#### 模式应用

在很多软件系统中需要更换界面主题，要求界面中的按钮、文本框、背景色等一起发生改变时，可以使用抽象工厂模式进行设计

当一个产品族汇总的多个对象被设计成一起工作时，它能够保证客户端始终只使用同一个产品族中的对象。这对一些需要根据当前环境来决定其行为的软件系统来说，是一种非常使用的设计模式

#### Code

```java
abstract class CPU{}
class EmberCPU extends CPU{}
class EnginolaCPU extends CPU{}

abstract class MMU{}
class EmberMMU extends MMU{}
class EnginolaMMU extends MMU{}

enum Architecture{ ENGINOLA, EMBER}

abstract class AbstractFactory{
    private static final EmberToolkit EMBER_TOOLKIT = new EmberToolkit();
    private static final EnginolaToolkit ENGINOLA_TOOLKIT = new EnginolaToolkit();
    
    static AbstractFactory getFactory(Architecture architecture){
        AbstractFactory factory = null;
        switch(architecture){
            case ENGINOLA:
                factory = ENGINOLA_TOOLKIT;
                break;
            case EMBER:
                factory = EMBER_TOOLKIT;
                break;
        }
        return factory;
    }
    public abstract CPU createCPU();
    public abstract MMU createMMU();
}

// class EmberFactory
class EmberToolKit extends AbstractFactory{
    @Override
    public CPU createCPU(){return new EmberCPU();}
    
    @Override
    public MMU createMMU(){return new EmberMMU();}
}
// class EnginolaFactory
class EnginolaToolkit extends AbstractFactory{
    @Override
    public CPU createCPU(){return new EnginolaCPU();}
    
    @Override
    public MMU createMMU(){return new EnginolaMMU();}
}

public class Client{
    public static void main(String[] args){
        AbstractFactory factory = AbstractFactory.getFactory(Architecture.EMBER);
        CPU cpu = factory.createCPU();
    }
}
```

- 官方图例代码

  整体看下来，这个所谓的官方要表达的图实际是不怎么样的，留在此处，仅做纪念处理

  ```java
  //有两种不同的product、每一种product又有两种platform来进行生产
  interface AbstractProductOne{}
  class ProductOnePlatformOne implements AbstractProductOne{}
  class ProductOnePlatformTwo implements AbstractProductOne{}
  
  interface AbstractProductTwo{}
  class ProductTwoPlatformOne implements AbstractProductTwo{}
  class ProductTwoPlatformTwo implements AbstractProductTwo{}
  
  interface AbstractPlatform{}
  class PlatformOne implements AbstractPlatform{}
  class PlatfromTwo implements AbstractPlatform{
      public AbstractProductOne makeProductOne(){
          return new ProductOnePlatformTwo();
      }
      public AbstractProductTwo makeProductTwo(){
          return new ProductTwoPlatformTwo();
      }
  }
  
  //下面的Class1写的不对，这应该是Client在做的事情，图示中的class是用来整合这些资源的
  public class Class1{
      public static void main(String[] args){
          AbstractPlatform platform = AbstractPlatform.getPlatform();
              platform.makeProductOne();
      }
  }
  ```

- 更好的例子

  - AbstractFactory：抽象工厂
  - ConcreteFactory：具体工厂
  - AbstractProduct：抽象产品
  - Product：具体产品

#### Diagram



![img](http://www.plantuml.com/plantuml/png/RL5XIyCm4Fr-lo8VEwNx0KN6vfWoN5RH-ab4iYxKO9f0cWfI_UzUjsZFj1z2kFVUlToxB1ild6zh3Ulx9tRrbQPiKn-amueWfbB6Qj63m3dLOHKGekj1M1qQxqJfR_1Ozqbw2clrk_9Z1VK8eh7F1OfMhdnJNfQ-THA0chBvV34ac6PEuz5t0ajpJlCq7woraiQpD5smQ4O04G1MQj4q30qx-8Pp4QQZIF0DODpDUTnDaA4xW21DM__Rq8HdF747k7b-lNbuBV9yTxSjChgwdjpVRlVvRZNmqJ-nIlwQ9VJnMHOlvvALJWelCMqoonXUVn3d9mYl8HsPOvoP484v_qg7bhLG9ddqJfdg-GLTFwmwecufpR6jzGy0)





- 官方图例

  ![Scheme of Abstract Factory](https://sourcemaking.com/files/v2/content/patterns/Abstract_Factory.png)

- 更好例子对应Diagram

  ![../_images/AbatractFactory.jpg](https://design-patterns.readthedocs.io/zh_CN/latest/_images/AbatractFactory.jpg)

### 2. Factory Method--工厂方法模式

> Creates an instance of several derived classes
>
> 定义一个接口用于创建对象，但是让子类决定初始化哪个类。工厂方法把一个类的初始化下放到子类

#### 模式应用

JDBC中工厂使用工厂方法:在使用JDBC进行数据库开发时，如果数据库由MySQL改成Oracle时，则只需要修改一下数据库驱动名称即可，其他的不用修改

#### Code

```java
public interface Reader{
    void read();
}
//JPG图片
public class JpgReader implements Reader{
    @Override
    public void read(){
        //method
    }
}
public class GifReader implements Reader{}

//定义一个抽象的工厂接口ReaderFactory
public interface ReaderFactory{
    Reader getReader();
}
//每一个对象都有其工厂
public class JpgReaderFactory implements ReaderFactory{
    @Override
    public Reader getReader(){
        return new JpgReader();
    }
}
public class GifReaderFactory implements ReaderFactory{
    @Override
    public Reader getReader(){
        return new GifReader();
    }
}
//客户端使用的时候
//读取JPG时
ReaderFactory factory = new JpgReaderFactory();
Reader reader = factory.getReader();
reader.read();
//读取GIF时
ReaderFactory factory = new GifReaderFactory();
```



- 比较好的例子

  工厂方法模式包含如下角色：

  - Product：抽象产品
  - ContreteProduct：具体产品
  - Factory：抽象工厂
  - ConcreteFactory：具体工厂



#### Diagram

![img](http://www.plantuml.com/plantuml/png/POx13e8m44Jl-nLxX9Zq0z0OJffuz0ygB8G4MhCi6eF-kngghUkjdPati-KebcKQUZYIhObnSpS63-Ts-Vwe-wu9Qf1tjXBFDyK4LMUIXfW1JQ4nssG-0j5Ext1U24zUn0_e6zHj1JB9n0uTNQEPvMiDwfqK36O0eND2tDYAS15dU01KlHt7k80ph91VpomlyEAxEcQ-PVyHrHslUKTR4lvSWKNmrNIAg4LbbUJgQUYV)



- 较好例子Diagram

  ![../_images/FactoryMethod.jpg](https://design-patterns.readthedocs.io/zh_CN/latest/_images/FactoryMethod.jpg)




### 3. Builder--生成器模式（建造者模式）

> Separates object construction from its representation
>
> 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示



#### 模式动机 Motivation

一些复杂的对象，拥有多个组成部分，对于大多数用户而言，无须知道这些部件的装配细节，也几乎不会使用单独某个部件。

复杂对象相当于一辆有待建造的汽车，而对象的属性相当于汽车的部件，建造产品的过程就相当于组合部件的过程。由于组合部件的过程很复杂，因此，这些部件的组合过程往往被”外部化“到一个称作建造者的对象里，建造者返还给客户端的是一个已经建造完毕的完整产品对象，而用户无须关心该对象所包含的属性以及它们的组装方式，这就是建造者模式的模式动机 Motivation

#### 模式分析

抽象建造者中定义了产品的创建方法和返回方法

指挥官类Director，该类的作用有两个：

1. 隔离了客户预生产的过程
2. 负责产品的生成过程

指挥官针对抽象建造者编程，客户端只需要知道具体建造者的类型，即可通过指挥官类调用建造者相关方法，返回一个完整的产品对象

#### 实例





#### 模式结构

- Builder： 抽象建造者
- ConcreteBuilder：具体建造者
- Director：指挥者
- Product：产品角色

#### Code

```java
/* "Product" */
class Pizza{
    private String dough;	//生面团
    private String sauce;	//调味汁
    private String topping;	//(食物顶部)配料
    
    //Setter Method;
}

/* "Abstract Builder" */
abstract class PizzaBuilder{
    protected Pizza pizza;
    public Pizza getPizza(){ return pizza;}
    public void createNewPizzaProduct(){
        pizza = new Pizza();
    }
    public abstract void buildDough();
    public abstract void buildSauce();
    public abstract void buildTopping();
}

/* "ConcreteBuilder" */
class HawaiianPizzaBuilder extends PizzaBuilder{
    public void builderDough(){ 
        pizza.setDough("cross");}
    public void builderSauce(){
        pizza.setSauce("mild");}
    public void buildTopping(){
        
        pizza.setTopping("ham+pineapple");}
}

/* "ConcreteBuilder" */
class SpicyPizzaBuilder extends PizzaBuilder{
    public void builderDough(){ 
        pizza.setDough("pan baked");}
    public void builderSauce(){
        pizza.setSauce("hot");}
    public void buildTopping(){
        pizza.setTopping("pepperoni+salami");}
}

/* "Director" */
class Waiter{
    private PizzaBuilder pizzaBuilder;
    
    public void setPizzaBuilder(PizzaBuilder pb){
        pizzaBuilder = pb;
    }
    
    public Pizza getPizza(){
        return pizzaBuilder.getPizza()p;
    }
    public void constructPizza(){
        pizzaBuilder.createNewPizzaProduct();
        pizzaBuilder.buildDough();
        pizzaBuilder.buildSauce();
        pizzaBuilder.buildTopping();
    }
}

/* "A customer ordering a pizza." */
public class PizzaBuilderDemo{
    public static void main(String[] args){
        Waiter waiter = new Waiter();
        PizzaBuilder hawaiianPizzaBuilder = new HawaiianPizzaBuilder();
        PizzaBuilder spicyPizzaBuilder = new SpicyPizzaBuilder();
        
        waiter.setPizzaBuilder(hawaiianPizzaBuilder);
        waiter.constructPizza();
        
        Pizza pizza = waiter.getPizza();
    }
}
```



#### Diagram

![img](http://www.plantuml.com/plantuml/png/jLB1JWCX4Btp5NDiOzeFG6CQxS6JQL8JBrvObbQIBMnWiB7LVvVTiHIIvU31qy9xyzuyPcVbKJWCXuvjzEaa7eBkMkNWxcknW2Tn55eBapCJPTjUoy-YfYnQBkzX1DYhq1W16qLblR6eeB68zW1s1rJ7OQsTHEk8xjGE8raeELmiIeV9w1mUhP5EeQgew2L_aupL77fdso2nI9gUqMUEo-WcK3shuwZSa6usltqpHLapVDSsxDyht5O4gIhSY-rxRVQHPKnsmdkAUMOBQ0TB3bjqiI3U_M2JtP6a2Ra1hv1o43Bdy65rImI5c20k2Khgcp7HU7H28gHSAVkP0OsA4noTzWh7usn-nIe3JDrJX-i8gpHDiSgcELxpK-ofzJf7UKF7iMbI9JWrA-5Q5rh7_sr1ApeiHb-iVTwL_cYEpFBBhsVjRrdZqGL9J1vWfKmR3jqF)







### 4. Object Pool*-- 对象池模式

> Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
>
> 通过回收利用对象避免获取和释放资源所需的昂贵成本



### 5. Prototype -- 原型模式

> A fully initialized instance to be copied or cloned
>
> 用原型实例指定创建对象的种类，并且通过拷贝这些原型，创建新的对象



#### Code

Abstract Factory might store a set of Prototypes from which to clone and return product objects

```java
/* "Prototype" */
interface Person{
    Person clone();
}

/* "ConcretePrototype" */
class Tom implements Person{
    private final String NAME = "Tom";
    
    @Override
    public Person clone(){
        return new Tom();
    }
    
    @Override
    public String toString(){
        return NAME;
    }
}

/* "ConcretePrototype" */
class Dick implements Person{
    private final String NAME = "Dick";
    
    @Override
    public Person clone(){
        return new Dick();
    }
    
    @Override
    public String toString(){
        return NAME;
    }
}

/* "Client" */
class Factory{
    private static final Map<Stirng,Person> prototypes = new HashMap<>();
    
    static {
        prototypes.put("tom", new Tom());
        prototypes.put("dick", new Dick());
    }
    public static Person getPrototype(String type){
        return prototypes.get(type).clone();
    }
}

public class PrototypeFactory{
    public static void main(String[] args){
        if(args.length > 0){
            for(String type : args){
                Person prototype = Factory.getPrototype(type);
                if(prototype != null){
                    System.out.println(prototype);
                }
            }
        }
    }
}

```



#### Diagram

![img](http://www.plantuml.com/plantuml/png/fP1D2i8m48NtSufPLaek44GggBjAGIyGubYARQOauw9Kxsv3MbHmvovvFnzvAO8OB_Uk1QZ81tQuVYY5P-w-xhl6tW0EnWhx0PNQO781e752_ceipT88ETgM7MKhlQIU0BOr8KJk20gFstAlyII-SVMFU8x2oOnYEhqPyIr_G-OfaTDx5fQXfw2nDFrKh4cgHSnIytDBGUuo_TWB)



### 6. Singleton—单例模式

> A class of which only a single instance can exist
>
> 确保一个类只有一个实例，并提供对该实例的全局访问

#### 模式动机 Motivation

对于系统中的某些类来说，只有一个实例很重要。如何保证一个类只有一个实例并且这个实例易于被访问呢？定义一个全局变量可以确保对象随时都可以被访问，但不能防止我们实例化多个对象

一个更好的解决办法是让类自身负责保存它的唯一实例。这个类可以保证没有其他实例被创建，并且它可以提供一个访问该实例的方法。这就是单例模式的动机

#### 模式定义

单例模式要点有三个:

1. 某个类只能有一个实例
2. 它必须自行创建这个实例
3. 它必须自行向整个系统提供这个实例

#### 模式应用

一个具有自动编号主键的表可以有多个用户同时使用，但数据库中只能有一个地方分配下一个主键编号，否则会出现主键重复，因此该主键编号生成器必须具备唯一性，可以通过单例模式来实现

#### Code

```java
public class Singleton{
    private Singleton(){}
    
    private static class SingletonHolder{
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance(){
        return SisngltonHolder.INSTANCE;
    }
}
```



#### Diagram

![img](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuGhEp4lFIIt9prEmqTNDLu1pkRYISnABYn42rLow2fv-mI6EViwkLaZgT15i3KqkRONqr1BFFA3nUScf6fh82hbgkHnIyrA0bW80)



## Structual design patterns—结构型模式

> These design patterns are all about Class's object communication. Behavioral patterns are those patterns that are most specifically concerned with communication between objects.

结构型模式的重点在于如何通过灵活的体系组织不同的对象，并在此基础上完成更为复杂的类型（或者类型系统），而参与组合的各类型之间始终保持尽量松散的结构关系

### 7. Adapter—适配器模式

> Match interface of different classes
>
> 将某个类的接口转换成客户端期望的另一个接口表示。适配器模式可以消除由于接口不匹配所造成的类兼容性问题

#### 模式定义

将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器（Wrapper）。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式

#### 模式结构

适配器模式包含如下角色：

- Target：目标抽象类
- Adapter：适配器类
- Adaptee：适配者类
- Client：客户类

#### Code

```java
/* The OLD */
class SquarePeg{
    private double width;
    
    public SquarePeg(double width){ this.width = width;}
    public double getWidth(){return width;}
    public void setWidth(double width){this.width = width;}
}
/* The NEW*/
class RoundHole{
    private final int radius;
    
    public RoundHole(int redius){
        this.radius = radius;
        System.out.println("RoundHole:max SquarePeg is " + radius*Math.sqrt(2));
    }
    public int getRadius(){return redius;}
}

//Design a "wrapper" class that can "impedance match" the old to the new
class SquarePegAdapter{
    private final SquarePeg squarePeg;
    
    public SquarePegAdapter(double w){
        squarePeg = new SquarePeg(w);
    }
    //Identify the desired interface
    public void makeFit(RoundHole roundHole){
        double amount = squarePeg.getWidth() - roundHole.getRadius()*Math.sqrt(2);
        System.out.println("reducing SquarePeg " + squarePeg.getWidth() + "by" +((amount < 0)? ): amount + "amount");
        if(amount > 0){
            squarePeg.setWidth(squarePeg.getWidth() - amount);
            System.out.println(" Width is now "+ squarePeg.getWidth());
        }
    }
}
public class AdapterDemoSquarePeg{
    public static void main(String[] args){
        RoundHole roundHole = new RoundHole(5);
        SquarePegAdapter squarePegAdapter;
        for(int i = 6; i < 10; i++){
            squarePegAdapter = new SquarePegAdapter((double)i);
            squarePegAdapter.makeFit(roundHole);
        }
    }
}

```



- 一个很好的使用前后对比的例子

  ```java
  /* Before */
  class Line{
      public void draw(int x1, int y1, int x2, int y2){
          //...
      }
  }
  class Rectangle{
      public void draw(int x ,int y, int width, int height){
          //...
      }
  }
  
  public class AdapterDemo{
      public static void main(String[] args){
          Objectp[] shapes = { new Line(), new Rectangle()};
          int x1 = 10, y1 = 20; x2,y2,width, height;
          for(Object shape : shapes){
              if(shape.getClass().getSimpleName().equals("Line")){
                  ((Line)shape).draw(x1, y1, x2, y2);
              }else if(shape.getClass().getSimpleName().equals("Rectangle")){
                  ((Rectangle)shape).draw(x2, y2, width, height);
              }
          }
      }
  }
  ```

  ```java
  /* After */
  interface Shape{
      void draw(int x, int y, int z, int j);
  }
  
  class Line{
      public void draw(int x1, int x2, int y1, int y2){
          //...
      }
  }
  class Rectangle{
      public void draw(int x , int y, int width, int height){
          //...
      }
  }
  
  class LineAdapter implements Shape{
      private Line adaptee;
      
      public LineAdapter(Line line){
          this.adaptee = line;
      }
      @Override
      public void draw(int x1, int y1, int x2, int y2){
          adaptee.draw(x1, y1, x2, y2);
      }
  }
  class RectangleAdapter implements Shape{
      private Rectangle adaptee;
      
      public RectangleAdapter(Rectangle rectangle){
          this.adaptee = rectangle;
      }
      @Override
      public void draw(int x1, int y1, int x2, int y2){
          int x = Math.min(x1, x2);
          int y = Math.min(y1, y2);
          int width = Math.abs(x2 - x1);
          int height = Math.abs(y2 - y1);
          adaptee.draw(x, y, width, height);
      }
  }
  
  public class AdapterDemo{
      public static void main(String[] args){
          Shape[] shapes = {new RectangleAdapter(new Ractangle()),
                            new LineAdapter(new Line())};
          int x1 = 10, y1 = 20;
          int x2 = 30, y2 = 60;
          for(Shape shape: shapes){
              shape.draw(x1, y1, x2, y2);
          }
      }
  }
  ```


#### Diagram

- 这个图是一个蛮特殊的情形，能实现适配器就可以，也算是一种实现方式了

![img](http://www.plantuml.com/plantuml/png/PP0n3y8W58HtViKRQHh-036cBiRHrC4PvQKIswBmqC7utwLAIsD7lhlS7Q_of1o5hcM7Ur0EzzXijBeHEX1Y3RKDlTxP5fVoIWYO831sRfNtcRoOWAVHT96WRJZ5X3AB_8UAo1gaus3ZnSpe4nnzyc6DPkzKabSD9Kvf4xm4qnChiyJZE-NNNqSiY2Y7_7zeoF8ZarbcvS8wcz-fAsuDyQct00bfROMzZfVz0000)



### 8. Bridge--桥接模式

> Separates an object's interface from its implements
>
> 将一个抽象与实现解耦，以便两者可以独立地变化

#### 模式动机 Motivation

设想如果要绘制矩形、圆形、椭圆、正方形，我们至少需要4个形状类，但是如果绘制的图形需要具有不同的颜色，如红色、绿色、蓝色等，此时至少有如下两种方案：

- 为每一种形状都提供一套各种颜色的版本
- 根据实际需要对形状和颜色进行组合

对于有两个变化维度(即两个变化原因)的系统，采用方案二来进行设计系统中类的个数更少，且系统扩展更为方便。设计方案二即为桥接模式的应用。桥接模式将继承关系转换为关联关系，从而降低了类与类之间的耦合，减少了代码编写量

#### 模式定义

桥接模式（Bridge Pattern）：将抽象部门与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体（Handle and Body）模式或接口（Interface）模式

#### 优缺点

优点：

- 分离抽象接口及其实现部分
- 桥接模式有时类似于多继承方案，但是多继承方案违背了类的单一职责原则（即一个类只有一个变化的原因），复用性比较差，而且多继承结构中类的个数非常庞大，桥接模式是比多继承方案更好的解决方法
- 桥接模式提高了系统的可扩展性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统
- 实现细节对客户透明，可以对用户隐藏实现细节

缺点：

- 桥接模式的引入会增加系统的理解和设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程
- 桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围具有一定的局限性

#### 模式结构

桥接模式包含以下角色：

- Abstraction：抽象类
- RefinedAbstraction: 扩充抽象类
- Implementor：实现类接口
- ConcreteImplementor：具体实现类

#### Code

```java
class Node{
    public int value;
    public Node prev, next;
    
    public Node(int value){
        this.value = value;
    }
}

class StackArray{
    private int[] items;
    private int size = -1;
    
    public StackArray(){
        this.items = new int[12];
    }
    public StackArray(int cells){
        this.items = new int[cells];
    }
    public void push(int i){
        if(!isFull){
            items[++size] = i;
        }
    }
    public boolean isEmpty(){
        return size == -1;
    }
    public boolean ifFull(){
        return size == items.length - 1;
    }
    public int pop(){
        if(isEmpty()){
            return -1;
        }
        return items[size--];
    }
}

class StackList{
    private Node last;
    public void push(int i){
        if(last == null){
            last = new Node(i);
        }else{
            last.next = new Node(i);
            last.next.prev = last;
            last = last.next;
        }
    }
    public boolean isFull(){
        return last == null;
    }
    public boolean isEmpty(){
        return false;
    }
    public int top(){
        if(siEmpty()){
            return -1;
        }
        return last.value;
    }
    public int pop(){
        if(isEmpty()){
            return -1;
        }
        int ret = last.value;
        last = last.prev;
        return ret;
    }
}

class StackFIFO extends StackArray{
    private StackArray stackArray = new StackArray();
    public int pop(){
        while(! isEmpty()){
            statckArray.push(super.pop());
        }
        int ret = stackArray.pop();
        while(!stackArray.isEmpty()){
            push(stackArray.pop());
        }
        return ret;
    }
}
//把例子敲完，看一下这里的桥接模式究竟是如何工作的
class StackHanoi extends StackArray{
    private int totalRejected = 0;
    public int reportRejected(){
        return totalRejected;
    }
    public void push(int in){
        if(!isEmpty()  &&  in > top){
            totalRejected ++;
        }else{
            super.push(i);
        }
    }
}

public class BridgeDisc{
    public static void main(Stirng[] args){
        StackArray[] stacks ={ nrew StackArray(), new StackFIFO(), new StackHanoi()};
        StackList stackList = new Stacklist();
        for(int i = 1, num; i < 15; i++){
            stacks[0].push(i);
            stackList.push(i);
            stacks[1].push(i);
        }
        Random random = new Random();
        for(int i = i, num; i < 15; i++){
            stacks[2].push(random.nextInt(20));
        }
        while(!stacks[0].isEmpty()){stacks[0].pop();}
        while(!stackList.isEmpty()){stackList.pop()}
    }
}
// 这个例子怎么个原理没有看明白，但是👇有一个非常好的例子，应该学会
```

- 非常棒的例子，跨平台视频播放器

  ```java
  public interface VideoFile{
      public void decode(String osType, String fileName);
  }
  
  public class AVIFile implements VideoFile{
      public void decode(String osType, Stirng fileName){
          //...
      }
  }
  public class WMVFile implements VideoFile{
      public void decode(String osType, String filenName){}
  }
  
  public abstract class OperatingSystemVersion{
      protected VideoFile videoFile;
      public void setVideo(VideoFile videoFile){
          this.videoFile = videoFile;
      }
      public abstract void play(String fileName);
  }
  
  public class LinuxVersion extends OperatingSystemVersion{
      public void play(String fileName){
          //...
      }
  }
  public class WindowsVersion extends OperatingSystemVersion{
      pubicl void play(String fileName){
          //...
      }
  }
  
  public class Client{
      public static void main(String[] args){
          VideoFile videoFile;
          OperatingSystemVersion osType1 = new LinuxVersion();
          videoFile = new AVIFile();
          osType1.setVideo(videoFile);
          osTYpe1.play("AVI");
          
          OperatingSystemVersion osType2 = new WindowsVersion();
          videoFile = new AVIFile();
          osType2.setVideo(videoFile);
          osType2.play("AVI");
      }
  }
  ```


#### Diagram

- 跨平台视频播放器的例子

![img](https://img-blog.csdn.net/20151110160157860)

![img](http://www.plantuml.com/plantuml/png/XP1FJyCm3CNl_XG-5fNk0qmSnWK92SHXKdVs6eCifSaKkuDfyDrPMJh-AAAdo4-_vxpdbHeb6teFhJYET-8PRzuN2xnjxyVXhxnvRB_afnqdCWalwuCQzoqdbHXEJDFWWuJXBIl_WXi9BhxgXFxygsaYbgq001ACqpDrZ1VB4Mfqt4N7rThIoNHzVgwMk8_Yu0EWywIQYytp6S-5vt6WhLgYpl1iBAS_0k9-ohpyZWyrAjiuLXTjsbtZblBgdQT3LIxsw-nZt1ARqNpuENZ5mGsz_mG0)



### 9. Composite—组合模式

> A tree structure of simple and composite objects
>
> 把多个对象组成树状结构来表示局部与整体，这样用户可以一样地对待单个对象和对象的组合

### 10. Decorator--修饰模式

> Add respinsibilities to object dynamically
>
> 向某个对象动态地添加更多的功能。修饰模式是除类继承外另一种扩展功能的方法

#### 模式动机 Motivation 

一般有两种方式可以实现给一个类或对象增加行为：

- 继承机制：使用继承机制是给类添加功能的一种有效途径，通过继承一个现有类可以使得子类在拥有自身方法的同时还拥有父类的方法。但是这种方法是静态的，用户不能控制增加行为的方式和时机
- 关联机制，即将一类的对象嵌入到另一个对象中，由另一个对象来决定是否调用嵌入对象的行为以便扩展自己的行为，我们称这个嵌入的对象为装饰器（Decorator）

装饰模式以对客户透明的方式动态地给一个对象附加上更多的责任

#### 模式定义





### 11. Facade--外观模式

> A single class that represents an entire subsystem
>
> 为子系统中一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用呢

### 12. Flyweight—享元模式

> A fine-grained instance used for efficient shaing
>
> 通过共享以便有效地支持大量小颗粒对象

### 13. Private Class Data*--私有类别资料

> Restricts accessor/mutator access
>
> 限制存取者/修改者的存取

### 14. Proxy--代理模式

> An object representing another object
>
> 为其他对象提供一个代理以控制对这个对象的访问



## Behavioral design patterns—行为型模式

> These design patterns are all about Class's objects communication. Behavioral patterns are those patterns that are most specifically concerned with communication between objects.

行为型模式用来识别对象之间的常用交流模式并加以实现，在进行这些交流活动时增强弹性

### 15. Chain of responsibility—责任链模式

> A way of passing a request between a chain as an object
>
> 为解除请求的发送者和接受者之间耦合，而使多个对象都有机会处理这个请求。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它

### 16. Command-- 命令模式

> Encapsulate a command request as an object
>
> 将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化

### 17. Interpreter—解释器模式

> A way to include language elements in a program
>
> 给定一个语言，定义它的文法的一种表示，并定义一个解释器，该解释器使用该表示来解释语言中的句子

### 18. Iterator—迭代器模式

> Sequentially access the elements of a collection
>
> 提供一种方法顺序访问一个聚合对象中各个元素，而又不需暴露该对象的内部表示

### 19. Mediator—中介者模式

> Defines simplified communication between classes
>
> 包装了一系列对象相互作用的方式，使得这些对象不必明显作用，从而使它们可以松散耦合。档某些对象之间的作用发生改变时，不会立即影响其他的一些对象之间的作用，保证这些作用可以彼此独立地变化

### 20. Memento--备忘录模式

> Capture and restore an object's internal state
>
> 一个用来存储另外一个对象内部状态的快照的对象
>
> 备忘录模式的用意是在不破坏封装的条件下，将一个对象的状态捕捉，并外部化，存储起来，从而可以在将来合适的时候把这个对象还原到存储起来的状态

### 21. Null Object*—空对象

> Designed to act as a default value of an object
>
> 通过提供默认对象来避免空引用

### 22. Observer—观察者模式

> A way of notifying change to a number of classes
>
> 在对象间定义了一个一对多的联系性，由此档一个对象改变了状态，所有其他相关的对象会被通知并且自动刷新

### 23. State-- 状态模式

> Alter an object's behavior when its state changes
>
> 让一个对象在其内部状态改变的时候，其行为也随之改变
>
> 状态模式需要对每一个系统可能取得的状态创立一个状态类的子类，当系统状态变化时，系统变改变所选的子类

### 24 Strategy--策略模式

> Encapsulates an algorithm inside a class
>
> 定义一个算法的系列，将其各个分装，并且使它们有交互性
>
> 策略模式使得算法在用户使用的时候能独立地改变

### 25. Template method--模板方法模式

> Defer the exact steps of an algorithm to a subclass
>
> 构建一个顶级逻辑框架，将逻辑的细节留给具体的子类去实现
>
> 模板方法模式准备一个抽象类，将部分逻辑以具体方法及具体构造子类的形式实现，然后声明一些抽象方法来迫使子类实现剩余的逻辑。不同的子类可以以不同的方式实现这些抽象方法，从而对剩余的逻辑有不同的实现

### 26. Visitor—访问者模式

> Defines a new operation to a class without change
>
> 封装一些施加于某种数据结构元素之上的操作。一旦这些操作需要修改，接收这操作的数据结构可以保持不变。
>
> 访问者模式适用于数据结构相对未定的系统，它把数据结构和作用于结构上的操作之间的耦合解脱开，使得操作结合可以相对自由地演化



## References

1. Design Pattern`https://sourcemaking.com/design_patterns`
2. Class Diagram Draw `http://www.plantuml.com/plantuml`
3. 抽象工厂模式`https://design-patterns.readthedocs.io/zh_CN/latest/creational_patterns/abstract_factory.html`