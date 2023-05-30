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

依赖倒置解决的问题如下（方法中传入的参数为类，而不是接口）：




> 代码下载地址：<https://github.com/ni-shiliu/neil-design-mode> 
{: .prompt-info }  

> 参考：《Head First 设计模式》


