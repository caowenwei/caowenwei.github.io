---
title: 纯函数和引用透明概念
tags: 函数式编程 java
categories: 函数式编程
published: true
---

表面上看，函数式编程相对于面向对象编程是一个巨大的编程范式迁移，似乎有着很陡峭的学习曲线，但其实函数式编程的出发点只有一个：**只使用纯函数来编写程序**。从这一个简单的起点，演化出了一系列复杂概念和结果。今天来讲一下**纯函数(Pure Function)**和与它相关的**引用透明(Referential Transparency)**概念。

# 纯函数

纯函数是没有副作用的函数，并且纯函数的输出只取决于输入。就像数学中定义的函数$y=f(x)$一样，是一个单纯的从自变量$x$到应变量$y$的映射，我们只关心对于每一个$x$，这个映射都会产生唯一的结果$y$，我们并不关心完成映射以后，$x$是否改变，或者其它任何事物是否因为这个映射而改变。这些改变就是副作用。在数学函数的定义中，副作用毫无意义，但在编程的函数中，副作用就很常见了。严格来说，以下这些常规操作都涉及到函数副作用：
- 变量重新赋值
- 改变变量本身数据结构
- 改变对象的属性
- 抛出异常或者错误退出
- 输出到终端或者读取用户输入
- 读写文件
- 在屏幕上作图

什么？连变量重新赋值都不允许？那还怎么写类似像这样的，天天在写的循环？
~~~java
for (int i=0; i<100; i++){
  System.out.println(i);
}
~~~
事实上，完全可以，具体例子以后再展开讲。

# 引用透明

纯函数具有很多良好性质，其中最重要的一项就是引用透明。简单的说，任意函数，或者任意代码段，如果它可以被它的计算结果直接替代，仍然不影响任何调用它的程序，那么这个函数或代码段就拥有引用透明的性质。函数式编程的代码里，经常可以看到大量的函数套函数的情况，但是只要记住，只要是
纯函数，就是引用透明的，就可以直接用它的结果替代它，我们就可以想理解数学公式一样，一层层替代函数符号，极为自然地去抽丝剥茧、逐层理解。
~~~java
public class A{
    public static int add(int a, int b) {
        return a + b;
    }
}

public class B{
    private static int value = 0;
    public static int add(int nextValue) {
        this.value += nextValue;
        return this.value;
    }
}
~~~
比较以上两个最简单的代码段，第一个add是纯函数，使用起来简单明了，第二个add存在副作用，结果依赖于输入以外的变量，使用者必须理解类里value的变化情况，明显困难得多。
