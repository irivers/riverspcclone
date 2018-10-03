# javap编译相关

### 命令功能

- 根据class字节码文件，反射析出当前类对应的code区（汇编有指令）、本地变量表、异常表和代码行偏移量映射表、常量池等信息

  - 一些信息（本地变量表、指令和代码行偏移量映射表、常量池中方法的参数名称等等），需要在使用javac编译成class文件时，指定参数才能输出

  - `javac -g xx.java`才会生成相应的信息

    ```
    Classfile /Users/riversyung/Hit/JavaDev/sublime/HelloJava.class
      Last modified Oct 2, 2018; size 254 bytes
      MD5 checksum 67177b5c7040a5e4ecfad9834966d9cd
      Compiled from "HelloJava.java"
    public class HelloJava
      minor version: 0
      major version: 54
      flags: (0x0021) ACC_PUBLIC, ACC_SUPER
      this_class: #4                          // HelloJava
      super_class: #5                         // java/lang/Object
      interfaces: 0, fields: 2, methods: 1, attributes: 1
    Constant pool:
       #1 = Methodref          #5.#15         // java/lang/Object."<init>":()V
       #2 = Fieldref           #4.#16         // HelloJava.a:I
       #3 = Fieldref           #4.#17         // HelloJava.c:I
       #4 = Class              #18            // HelloJava
       #5 = Class              #19            // java/lang/Object
       #6 = Utf8               a
       #7 = Utf8               I
       #8 = Utf8               c
       #9 = Utf8               <init>
      #10 = Utf8               ()V
      #11 = Utf8               Code
      #12 = Utf8               LineNumberTable
      #13 = Utf8               SourceFile
      #14 = Utf8               HelloJava.java
      #15 = NameAndType        #9:#10         // "<init>":()V
      #16 = NameAndType        #6:#7          // a:I
      #17 = NameAndType        #8:#7          // c:I
      #18 = Utf8               HelloJava
      #19 = Utf8               java/lang/Object
    {
      int a;
        descriptor: I
        flags: (0x0000)
    
      int c;
        descriptor: I
        flags: (0x0000)
    
      public HelloJava();
        descriptor: ()V
        flags: (0x0001) ACC_PUBLIC
        Code:
          stack=3, locals=1, args_size=1
             0: aload_0
             1: invokespecial #1                  // Method java/lang/Object."<init>":()V
             4: aload_0
             5: aload_0
             6: getfield      #2                  // Field a:I
             9: iconst_1
            10: iadd
            11: putfield      #3                  // Field c:I
            14: return
          LineNumberTable:
            line 1: 0
            line 3: 4
    }
    SourceFile: "HelloJava.java"
    ```

    ```
    Classfile /Users/riversyung/Hit/JavaDev/sublime/HelloJava.class
      Last modified Oct 2, 2018; size 314 bytes
      MD5 checksum 94d76d0e004956a161075f9e55c8e459
      Compiled from "HelloJava.java"
    public class HelloJava
      minor version: 0
      major version: 54
      flags: (0x0021) ACC_PUBLIC, ACC_SUPER
      this_class: #4                          // HelloJava
      super_class: #5                         // java/lang/Object
      interfaces: 0, fields: 2, methods: 1, attributes: 1
    Constant pool:
       #1 = Methodref          #5.#18         // java/lang/Object."<init>":()V
       #2 = Fieldref           #4.#19         // HelloJava.a:I
       #3 = Fieldref           #4.#20         // HelloJava.c:I
       #4 = Class              #21            // HelloJava
       #5 = Class              #22            // java/lang/Object
       #6 = Utf8               a
       #7 = Utf8               I
       #8 = Utf8               c
       #9 = Utf8               <init>
      #10 = Utf8               ()V
      #11 = Utf8               Code
      #12 = Utf8               LineNumberTable
      #13 = Utf8               LocalVariableTable
      #14 = Utf8               this
      #15 = Utf8               LHelloJava;
      #16 = Utf8               SourceFile
      #17 = Utf8               HelloJava.java
      #18 = NameAndType        #9:#10         // "<init>":()V
      #19 = NameAndType        #6:#7          // a:I
      #20 = NameAndType        #8:#7          // c:I
      #21 = Utf8               HelloJava
      #22 = Utf8               java/lang/Object
    {
      int a;
        descriptor: I
        flags: (0x0000)
    
      int c;
        descriptor: I
        flags: (0x0000)
    
      public HelloJava();
        descriptor: ()V
        flags: (0x0001) ACC_PUBLIC
        Code:
          stack=3, locals=1, args_size=1
             0: aload_0
             1: invokespecial #1                  // Method java/lang/Object."<init>":()V
             4: aload_0
             5: aload_0
             6: getfield      #2                  // Field a:I
             9: iconst_1
            10: iadd
            11: putfield      #3                  // Field c:I
            14: return
          LineNumberTable:
            line 1: 0
            line 3: 4
          LocalVariableTable:
            Start  Length  Slot  Name   Signature
                0      15     0  this   LHelloJava;
    }
    SourceFile: "HelloJava.java"
    ```
