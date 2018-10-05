# UML类图含义-Class Diagram

> 从关系含义、对应代码表示、UML类图三个维度进行说明
>
> @RiversYung 20181005

## Overview

The **Unified Modeling Language(UML)** is a general-purpose, developmental, modeling language in the field of software engineering, that is intended to provide a standard way to visualize the design of a system.



A class diagram in the Unified Modeling Language(UML) is a type of static structure diagram that describes the structure of a system by showing the system's classes, their attributes, operations(or methods), and the relationships among objects. 



In the diagram, classess are represented with boxes that contain three compartments:

- The top compartment contains the name of the class. It is printed in bold and centered, and the first letter is Capitalized
- The middle compartment contains the attributes of the class. They are left-aligned and the first letter is lowercase
- The bottom compartment contains the operations the class can execute.They are also left-aligned and the first letter is lowercase

### Visibility

To specify the visibility of a class member(i.e. any attribute or method), these notations must be placed before the member's name:

- `+`Public
- `-`Private
- `#`Protected
- `~`Package



## Relationships

### OverView

#### Class Relationship

In a system a class may be related to differenet classes,following are the different relationship.

- 依赖关系：Dependency (uses a )
- 关联关系：Association (knows a) 
  - Aggregation and Composition are subsets of association meaning they are specific cases of association
- 合成关系：Composition (has a )
  - Implies a relationship where the child **cannot exist independent **of the parent
  - Example:House (parent) and Room (child). Rooms don's exist separate to the House
- 聚合关系：Aggregation (has a )
  - Implies a relationship where the child **can exist independently** of the parent
  - Examples: Class (parent) and Student (child).Delete the Class and the Students still exist
- 继承关系：Inheritance (is a )

![img](https://4.bp.blogspot.com/-IY7kuMMUb3g/WddN2Yd0bWI/AAAAAAAAAPE/J0UQ5wZ4XQgSxjorGY4ZR25Pmrv1galGgCLcBGAs/s1600/Uml_classes_en.svg.png)




### 1. 依赖关系 Dependency 

#### Code

```java
public class PaymentSystem{
    public int getTransactionID(){}
}
public class Order{
    public void processPayment(PaymentSystem ps){
        
    }
}

```

#### Diagrams 

![dependency](http://www.plantuml.com/plantuml/png/LOv12i8m44NtSufPLeGUGRfmhs0lC2GV2IIJaXc58DxTLdJHxV_lmPlCfVcZP9gJP_0P2pH2GwTBYsWyZYU-IYzGltLp52OAMSpu-x_eoC-Q8Y-j1fZzq66d7EZzbtRx9YTrliFd9ceIF5KTDdnQAzQg3m00)

#### Defination

A dependency is generally shown as a dashed arrow pointing from the client(dependent) at the tail to supplier(provider) at the arrowhead.

Dependency indecates a "use" relationship between two classes. In a class diagram, a dependency relationship is rendered as a dashed directed line.

If a class A "uses" class B, then one or more of the following statements generally hold true:

1. Class B is used as the type of a local variable in one or more methods of class A.
2. Class B is used as the type of parameter for one or more methods of class A.
3. Class B is used as the return type for one or more methods of class A.
4. One or more methods of class A invoke one or more methods of class B.



### 2. 关联关系 Association

#### Different Multiplicity in a relation

|             |                                               |
| ----------- | --------------------------------------------- |
| 0..1        | No instances, or one instance (optional, may) |
| 1           | Exactly one instance                          |
| 0..*  or  * | Zero or more instances                        |
| 1..*        | One or more instances (at least one)          |



#### Code

1. Association Example

    > The unidirectional relationship shows that the source object can invoke methods of the destination class

    ```java
    public class Customer{
        private String name;
        private String address;
        private String contactNumber;
    }
    
    public class Car{
        private String modelNumber;
        private Customer owner;
    }
    ```



2. Bidirectional association

   > In the bidirectional(*function in two directions*) association each of the class in this relationship refers to each other by calling each others method.

   ```java
   public class Customer{
       private String name;
       private String address;
       private String contactNumber;
       private Car car;
   }
   
   public class Car{
       private String modelNumber;
       private Customer owner;
   }
   ```

3. Multiplicity in association

   ```java
   public class Car{
       private String brand;
       public Car(){}
       public Car(String brand){
           this.brand = brand;
       }
       
       public String getBrand(){
           return brand;
       }
       public voidsetBrand(String brand){
           this.brand = brand;
       }
   }//END CAR
   
   public class Customer{
       private Car[] vehicles;
       ArrayList<Car> carList = new ArrayList<Car>();
       public Customer(){
           vehicles = new Car[2];
           vehicles[0] = new Car("Audi");
           vehicles[1] = new Car("Mercedes");
           
           carList.add(new Car("BMW"));
           carList.add(new Car("Chevy"))
       }
   }
   
   ```

#### Diagram

1. Association Example

![PlantUML diagram](http://www.plantuml.com/plantuml/png/JOv1giD0243tdaAozmI5af9zD_qNi3DI1cOKrD15wTqRMaZ-qeS-tnp9dkleyDUo2ruYh3JEPWeBEnBH6N4YUwhuXCiQQKCS0KhdY1syWF2MtlI1oaEEYMjrYJX0CKqkULt7NVm4xzt4_oN3glJV3j3nzfkntSoYTOl-0000)



2. Bidirectional association

![PlantUML diagram](http://www.plantuml.com/plantuml/png/NOyn2iCm34LtdK9azmL2AMcpTsaleDgY66mP98KE9NUlco61ZkyX_l-Qp4bzgGKUsGlZDQUi73qteO8NinOp_GXcKXn291tm548uOwVs5kuyB-QjiY90B6IsYmy45Aeytbspl3fHIifcXuXDdABVblfaFNJl6NiZh7iaRMcr9Ix_nzkCyLYnmiII-bDV)

3. Multiplicity in association

![PlantUML diagram](http://www.plantuml.com/plantuml/png/HO-nIi0m48RtUueZKwjaS0rIMhkwwIOEPnfg84tWxbeGyTrDhDOfkV--_uCRHObrtn8yHqq19v7Y8sai6MPYD3S6hRK3cZk3EE-YPGkC03wHo1LyWiKZl4UVWhZQUtb5GFJ4Zr7KJSpqqNxtTJKWt5wzheUloqK_cZUclBWdvPZNHbA3plSCsxG6VMXSr_-JyfVzOkueSQdwCUIb7lWD)

#### Defination

One object is aware of another; it contains a pointer of reference to another object





## References

1. UML图绘制网站`http://www.plantuml.com/plantuml`
2. `https://www1.in.tum.de/lehrstuhl_1/files/teaching/ss07/SE/SE2007_Lecture16.pdf`

## Further reading























