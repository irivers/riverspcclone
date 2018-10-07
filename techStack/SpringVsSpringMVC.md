# Spring vs SpringMVC

> 写这个的初衷，就是想捋清一下二者的关系，真正从一些基本概念上，而不是应用上理解二者
> @RiversYung 20181007

## 概念

### 1. Spring
Spring是Spring的核心框架，提供了IOC(控制反转或依赖注入)、AOP(面向切面编程)、约定大于配置(JavaBean的生命周期管理)
### 2. SpringMVC
SpringMVC是一个WebMVC
Spring MVC is a complete HTTP oriented MVC framework managed by the Spring Framework and based in Servlets. The most popular elements in it are classes annotated with `@Controller` where you implements methods you can access using different HTTP requests.It has an equivalent `@RestController` to implements REST based APIs

## Spring Core Tutorial
> Spring Framework is based on two design principles Dependency Injection and Aspect Oriented Programing
> Talk is cheap show me the code

### 1. Dependency Injection

We hava an application where we consume `EmailService` to send emails.`EmailService` class holds the logic to send email message to the recipient email address.

```java
public class EmailService{
    public void sendEmail(String message, String receiver){
        //logic to send email
        System.out.println("Email sent to " + receiver + " with message="+message);
    }
}
```

Application code will be like below

```java
public class MyApplication{
    private EmailService email = new EmailService();
    public void processMessage(String message, String rec){
        //do some msg validation, manipulation logic etc
        this.email.sendEmail(msg, rec)
    }
}
```

Our client code that will use `MyApplication`  class to send email message will be like below

```java
public class MyLegacyTest{
    public static void main(String[] args){
        MyApplication app = new MyApplication();
        app.proecssMessage("Hi Rivers", "RiversYung@gmail.com")
    }
}
```

At first look, there seems nothing wrong with above implementation.But above logic has certain limitations.

- 初始化对象繁琐：`MyApplication`  class is responsible to initialize the email service and then use it.This leads to **hard-coded** （硬编码是将数据直接嵌入到程序或其他可执行对象的源代码中的软件开发实践，与从外部获取数据或在运行时生成数据不同。硬编码数据通常只能通过编辑源代码和重新编译可执行文件修改）dependency. If we want to switch to some other advanced email service in future, it will require code changes in MyApplication class. This makes our application hard to extend and if email service is used in multiple classes the that would be even more harder.
- "增加新信息种类麻烦"：If we want to extend our application to provide additional messageing feature,such as SMS or Facebook message then we would  need to write another application for that. This will involve code changes in applicaiton classes and the client classes too.
- "直接初始化类，测试不便"：Tesing the application will be very difficult since our application  is  directly creating the email service instance.There is no way we can mock these objects in our test classes.

One can argue that we can remove the email service instance creation from `MyApplication` class by having a constructor that requires email service as argument

```java
public class MyApplicaiton{
    private EmailService email = null;
    
    public MyApplication(EmailService svc){
        this.email = svc;
    }
    public void processMessages(String msg, String rec){
        //do some msg validation, manipulation logic etc
        this.email.sendEmail(msg,rec);
    }
}
```

In this case, we are asking client applications or test classes to initializing the email service that is not a good design decision.

We can apply java dependency injection pattern to solve all the problems with above implementation.Dependency Injection in java requires at least following:

1. **Service components** should be designed with base class or interface. It's better to prefer interfaces or abstract classes that would define contract for the services
2. **Consumer classes** should be written in terms of service interface
3. **Injection classes** that will initialize the service and then the consumer classes

#### 1.1 Java Dependency Injection - Service Componets

We can hava  `MessageServie` that will declare the contract for service implementations

```java
public interface MessageService{
    void sendMessage(String msg, String rec);
}
```

Now let's say we have Email and SMS services that implement above interfaces

```java
public class EmailServiceImpl implements MessageService{
    @Override
    public void sendMessage(String msg, String rec){
        //logic to send email
        System.out.println("Email sent to " + rec + " with Message=" + msg);
    }
}

public class SMSServierImpl implements MessageService{
    @Override
    public void sendMessage(String msg, String rec){
        //logic to send SMS
        System.out.println("SMS sent to " + rec + " with Message=" + msg);
    }
} 
```



#### 1.2 Java Dependency Injection - Service Consumer

We are not required to hava base interfaces for consumer classes but I will hava a `Consumer`  interface declaring contract for consumer classes

```java
public interface Consumer{
    void processMessage(String msg, String rec);
}

public class MyDIApplication implements Consumer{
    private MessageService service;
    public MyDIApplication(MessageService svc){
        this.service = svc;
    }
    @Override
    public void processMessages(String msg, String rec){
        //do some msg validation, manipulation logic etc
        this.service.sendMessage(msg, rec);
    }
}
```

Notice that our application class is just using the service. It does not initialize the service that leads to better "separation of concerns". Also use of service interface allows us to easily test the application by mocking the MessageService and bind the services at runtime rather than compile time

#### 1.3 Java Dependency Injection - Injection Classes

Let's hava an interface `MessageServiceInjection`  with method  declaration that returns the  `consumer`  class.

```java
public interface MessageServiceInjector{
    public Consumer getConsumer();
}
```

For every service, we will hava to create injector classes like below

```java
public class EmailServiceInjector implements MessageServiceInjector{
    @Override
    public Consumer getConsumer(){
        return new MyDIApplication(new EmailServiceImpl());
    }
}

public class SMSServiceInjector implements MessageServiceInjector{
    @Override
    public Consumer getConsumer(){
        return new MyDIApplication(new SMSServiceImpl());
    }
}
```

Now let's see how our client applicaiton will use the application with a simple progran

```JAVA
public class MyMessageDITest{
    public static void main(String[] args){
        String msg = "Hi Rivers";
        String email = "RiversYung@gmail.com";
        String phone = "88888888";
        MessageServiceInjector injecotr = null;
        Consumer app = null;
        
        // Send email
        injector = new EmailServiceInjector();
        app = injector.getConsumer();
        app.processMessages(msg, email);
        // Send SMS
        injector = new SMSServiceInjector();
        app = injector.getConsumer();
        app.processMessage(msg, phone);
    }
}
```

As you can see that our application classes are responsible only for using the service.Service classes are created in injectors. Also If we hava to further extend our application to allow facebook messaging, we will hava to write Service classes and injector classes only.

#### 1.4 Java Dependency Injection - JUnit Test Case with Mock Injector and Service

#### 1.5 Benefits of Java Dependency Injection

#### 1.6 Disadvantages of Java Dependency Injection







### 2. Spring Dependency Injection

### 3. Spring AOP

### 4. Spring IoC Container and Spring Bean

### 5. Spring Bean Autowiring

### 6. Spring Bean Life Cycle





## References
- Spring WebSite `http://spring.io/`
- Spring Tutorial `https://www.journaldev.com/2888/spring-tutorial-spring-core-tutorial`