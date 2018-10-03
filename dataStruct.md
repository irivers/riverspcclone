# dataStruct

## 栈和队列

### 栈

- 数据结构定义

  ```C++
  #define MAXSIZE 100
  typedef struct{
      Elemtype data[MAXSIZE];
      int top;
  }SqStack; //SequenceStack顺序栈
  //初始时，top==-1; 
  //进栈操作：data[++top]
  //出栈操作：data[top--]
  //栈满条件：top == MAXSIZE-1
  //栈长度：top+1;
  
  //这里有一种设计模式非常好，出栈就是元素直接出去，额外还有一个读栈顶元素的操作，使得操作细分得更好
  //顺序栈
  struct stack{
      int[] data;//数据域
      int top;//栈顶
      int bottom// 栈底
      //约定初始状态时bottom=top=0.但是实际看下来，要bottom也没有什么意义
  }
  //栈的逻辑操作
  //+属性判断：栈的大小，栈是否满，栈是否为空
  int stackSize(Stack stack){
      return stack.top-stack.bottom;
  }
  boolean isEmpty(Stack stack){
      if(stack.top == 0)
          return true;
      else 
          return false;
  }
  boolean isFull(Stack stack){
      if(stack.top == MAXSIZE-1)
          return true;
      else
          return false;
  }
  //入栈操作
  void push(Stack stack, int value){
      if(!isFull(stack)){
          stack.data[++stack.top] = value;
      }
  }
  //出栈操作
  int pop(Stack stack){
      if(!isEmpty(stack)){
          return stack.data[stack.top--];
      }else{
          return -1;//这里应该设置一个特殊值
      }
  }
  //设计的是，忘了传递栈进去，确实是失误
  ```

- 共享栈

  ```C++
  两端向中间靠拢
  // 初始状态
  top0==0, top1=MAXSIZE-1
  //两端分别入栈操作
  top0[top0++] = val;  top1[--MAXSIZE]；
  //两端分别出栈操作
  top0[--top0];    top1[MAXSIZE++]
  //栈满条件
  top1 - top0 == 1
  
  //这样设计，初始条件实际是不合理的，造成思维上的浪费。-1，MAXSIZE是最合适的
  //初始状态
  top0 == -1;  top1 == MAXSIZE;
  //两端入栈操作
  stack0[++top0] = val;  stack1[--MAXSIZE] = val;
  //两端出栈操作
  stack0[top0--];    stack1[MAXSIZE++];
  //栈满条件
  top1 - top0 = 1;
  //栈空条件,初始状态即可
  top0 == -1. top1 == MAXSIZE
  ```

### 队列

- 数据结构定义

  ```C++
  #define MaxSize 50
  typedef struct{
      Elemtype data[MaxSize];
      int front,rear;
  }SqQueue;
  //这里队尾、队头很难界定：实际上尾部就是进来的地方，头部就是出去的地方。有点像排队打饭，进队列就是站在队尾进行排队，出队列就是排在队头，可以打到饭了
  // 非循环队列的情况下
  //初始状态：Q.front == Q.rear == 0;
  //进队操作：Q.data[Q.rear++] = val;
  //出队操作：Q.data[Q.front++];
  //队空判断：Q.front == Q.rear
  //队满判断：Q.rear == MaxSize -1;
  //
  ```
