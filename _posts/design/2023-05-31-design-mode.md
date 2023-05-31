---
title: 七大原则
date: 2023-05-31 08:08:08 +/-TTTT
categories: [设计模式, 七大原则]
tags: [TAG]     # TAG names should always be lowercase
author: Neil
---

> **懂了设计模式，你就懂了面向对象分析和设计（OOA/D）的精要**  

## 目的

编写软件过程中，我们会面临着来自耦合性，内聚性以及可维护性，可扩展性，重用性，灵活性等多方面的挑战，设计模式是为了让程序(软件)，具有更好：  
1. 代码重用性 (即：相同功能的代码，不用多次编写)
2. 可读性 (即：编程规范性, 便于其他程序员的阅读和理解)
3. 可扩展性 (即：当需要增加新的功能时，非常的方便，称为可维护)
4. 可靠性 (即：当我们增加新的功能后，对原来的功能没有影响)
5. 使程序呈现高内聚，低耦合的特性


## 单一职责原则

### 定义

对类来说的，即一个类应该只负责一项职责。

### 注意事项和细节

1. 降低类的复杂度，一个类只负责一项职责
2. 提高类的可读性，可维护性
3. 降低变更引起的风险
4. 通常情况下，我们应当遵守单一职责原则，只有逻辑足够简单，才可以在代码级违 反单一职责原则；只有类中方法数量足够少，可以在方法级别保持单一职责原则

## 接口隔离原则

### 定义

客户端不应该依赖它不需要的接口类，类之间的依赖关系应该建立在最小的接口上。

### 规范

1. 使用接口隔离原则前首先需要满足单一职责原则
2. 接口需要高内聚，也就是提高接口、类、模块的处理能力，少对外发布public的方法
3. 定制服务，就是单独为一个个体提供优良的服务，简单来说就是拆分接口，对特定接口进行定制

接口隔离解决的问题如下（实现类实现了接口中不需要的抽象方法）：
```java
//接口
interface Interface1 {
    void operation1();
    void operation2();
    void operation3();
}
 
class B implements Interface1 {
    public void operation1() {
        System.out.println("B 实现了 operation1");
    }
    public void operation2() {
        System.out.println("B 实现了 operation2");
    }
    public void operation3() {
        System.out.println("B 实现了 operation3");
    }
}
//问题所在：A类只用到了B类的 1,2 方法，但B类却要实现方法3，造成代码的冗余。
class A { //A 类通过接口Interface1 依赖(使用) B类，但是只会用到1,2方法
    public void depend1(Interface1 i) {
        i.operation1();
    }
    public void depend2(Interface1 i) {
        i.operation2();
    }
}
```
遵循接口隔离原则后（将抽象方法进行隔离，当需要时实现多个接口即可） ：
```java
// 接口1
interface Interface1 {
    void operation1();
    void operation2();
}

// 接口2
interface Interface2 {
    void operation3();
}

class B implements Interface1 {
    public void operation1() {
        System.out.println("B 实现了 operation1");
    }

    public void operation2() {
        System.out.println("B 实现了 operation2");
    }

}

class A { // A 类通过接口Interface1,Interface2 依赖(使用) B类，但是只会用到1,2,3方法
    public void depend1(Interface1 i) {
        i.operation1();
    }

    public void depend2(Interface2 i) {
        i.operation2();
    }
}
```

## 依赖倒置原则

### 定义

程序要依赖于抽象接口，不要依赖于具体实现。

### 原则

1. 高层模块不应该依赖底层模块，二者都应该依赖其抽象
2. 抽象不应该依赖细节（实现类），细节应该依赖抽象
3. 依赖倒置的中心思想是面向接口编程
4. 相对于细节的多样性，抽象的东西要稳定的多。以抽象为基础搭建的架构比以细节为基础的架构要稳的多。在 Java 中，抽象指的是接口和抽象类，细节就是具体的实现类
5. 使用接口或抽象类的目的是定制好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成

### 依赖关系传递的三种方式

1. 接口传递
2. 构造方法传递
3. setter方式传递

### 注意事项和细节

1. 底层模块尽量都要有抽象类或接口，或者两者都有，程序稳定性更好
2. 变量的声明类型尽量是抽象类或接口, 这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化
3. 继承时遵循里氏替换原则

## 里氏替换原则

### 定义

所有引用基类的地方必须能透明地使用其子类的对象，子类可以扩展父类的功能，但不能改变父类原有的功能

1. 继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是在设定规范和契约，虽然它不强制要求所有子类都必须遵循这种契约，但是如果子类对这些已经实现的方法任意修改，就会对这个继承体系造成破坏
2. 继承在给程序设计带来便利的同时，也带来了弊端。比如使用继承会给程序带来侵入性，程序的可移植性降低，增加对象间的耦合性，如果一个类被其他的类所继承，则当这个类需要修改时，必须虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障
3. 问题：在编程中如何正确的使用继承，答案是：遵循里氏替换原则

```java
class A {
    public int func1(int num1, int num2) {
        return num1 - num2;
    }
}
 
// 增加了一个新功能：完成两个数相加,然后和9求和
class B extends A {
    //这里，重写了A类的方法
    public int func1(int a, int b) {
        return a + b;
    }
 
    public int func2(int a, int b) {
        return func1(a, b) + 9;
    }
}

```
遵循里氏替换原则后（提取一个公共的类，将A类与B类进行组合） ：
```java
class Base {
}
 
class A extends Base {
    public int func1(int num1, int num2) {
        return num1 - num2;
    }
}
 
// 增加了一个新功能：完成两个数相加,然后和9求和
class B extends Base {
    //如果B需要使用A类的方法,使用组合关系
    private A a = new A();
 
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

> 通用方法：原来的父类和子类都继承一个更通俗的基类，原有的继承关系去掉，采用依赖，聚合，组合等关系代替。
{: .prompt-tip }


## 开闭原则

### 定义

一个软件实体如类、模块和函数应该对扩展开放，对修改关闭

```java
//这是一个用于绘图的类 [使用方]
class GraphicEditor {
    //接收Shape对象，然后根据type，来绘制不同的图形
    public void drawShape(Shape s) {
        //**问题所在：此类属于使用方，但当我们需要扩展新的图形时，却要修改使用方，就不符合OCP原则
        if (s.m_type == 1) {
            drawRectangle(s);
        } else if (s.m_type == 2) {
            drawCircle(s);
        }
    }

    //绘制矩形
    public void drawRectangle(Shape r) {
        System.out.println(" 绘制矩形 ");
    }

    //绘制圆形
    public void drawCircle(Shape r) {
        System.out.println(" 绘制圆形 ");
    }
}

//Shape类，基类
class Shape {
    int m_type;
}

class Rectangle extends Shape {
    Rectangle() {
        super.m_type = 1;
    }
}

class Circle extends Shape {
    Circle() {
        super.m_type = 2;
    }
}
```

遵循开闭替换原则（将公共方法提取到抽象类，在实现类中实现需要调用的方法，使用类中直接调用公共方法即可） ：

```java
class GraphicEditor {
    public void drawShape(Shape s) {
        s.draw();
    }
}
 
abstract class Shape {
    public abstract void draw();
}

class Rectangle extends Shape {
    @Override
    public void draw() {
        System.out.println("绘制矩形");
    }
}
 
class Circle extends Shape {
    @Override
    public void draw() {
        System.out.println("绘制圆形");
    }
}
```

## 迪米特法则

### 定义

有时候也叫做**最少知识原则**，一个软件实体应尽可能少地与其他实体发生相互作用。

### 规则

1. 一个对象应该对其他对象保持最少的了解
2. 类与类关系越密切，耦合度越大。
3. 迪米特法则(Demeter Principle)又叫最少知道原则，即一个类对自己依赖的类知道的越少越好。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的public 方法，不对外泄露任何信息。
4. 迪米特法则还有个更简单的定义：只与直接的朋友通信
5. 直接的朋友：每个对象都会与其他对象有耦合关系，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式很多，依赖，关联，组合，聚合等。其中，我们称出现成员变量，方法参数，方法返回值中的类为直接的朋友，而出现在局部变量中的类不是直接的朋友。也就是说，陌生的类最好不要以局部变量的形式出现在类的内部。

### 注意事项和细节

迪米特法则的核心是降低类之间的耦合。
但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低类间(对象间)耦合关系，并不是要求完全没有依赖关系。

## 合成复用原则

### 定义

尽量使用合成/聚合的方式，而不是使用继承。

## 设计原则核心思想
1. 找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起。
2. 针对接口编程，而不是针对实现编程。
3. 为了交互对象之间的松耦合设计而努力

> 代码下载地址：<https://github.com/ni-shiliu/neil-design-mode> 
{: .prompt-info }  

> 参考：《Head First 设计模式》
> and <https://developer.aliyun.com/article/940284>


