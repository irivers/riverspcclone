# 设计模式中的类图

> 初衷就是将23种设计模式，以类图的形式展现出来。第一版只有类图，没有其他

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



### 2. Factory Method--工厂方法模式

> Creates an instance of several derived classes
>
> 定义一个接口用于创建对象，但是让子类决定初始化哪个类。工厂方法把一个类的初始化下放到子类





### 3. Builder--生成器模式

> Separates object construction from its representation
>
> 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示





### 4. Object Pool*-- 对象池模式

> Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
>
> 通过回收利用对象避免获取和释放资源所需的昂贵成本



### 5. Prototype -- 原型模式

> A fully initialized instance to be copied or cloned
>
> 用原型实例指定创建对象的种类，并且通过拷贝这些原型，创建新的对象



### 6. Singleton—单例模式

> A class of which only a single instance can exist
>
> 确保一个类只有一个实例，并提供对该实例的全局访问



## Structual design patterns—结构型模式

> These design patterns are all about Class's object communication. Behavioral patterns are those patterns that are most specifically concerned with communication between objects.

结构型模式的重点在于如何通过灵活的体系组织不同的对象，并在此基础上完成更为复杂的类型（或者类型系统），而参与组合的各类型之间始终保持尽量松散的结构关系

### 7. Adapter—适配器模式

> Match interface of different classes
>
> 将某个类的接口转换成客户端期望的另一个接口表示。适配器模式可以消除由于接口不匹配所造成的类兼容性问题

### 8. Bridge--桥接模式

> Separates an object's interface from its implements
>
> 将一个抽象与实现解耦，以便两者可以独立地变化

### 9. Composite—组合模式

> A tree structure of simple and composite objects
>
> 把多个对象组成树状结构来表示局部与整体，这样用户可以一样地对待单个对象和对象的组合

### 10. Decorator--修饰模式

> Add respinsibilities to object dynamically
>
> 向某个对象动态地添加更多的功能。修饰模式是除类继承外另一种扩展功能的方法

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