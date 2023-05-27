---
title: 单利模式（Factory）
date: 2023-05-27 18:08:08 +/-TTTT
categories: [目录, 设计模式]
tags: [TAG]     # TAG names should always be lowercase
author: Neil
---

> **天无二日，国无二主**

## 定义

确保一个类只有一个实例，并提供一个全局的访问点。

## 使用场景

只需要有一个对象：线程池、数据库连接池、缓存、日志对象、处理偏好设置和注册表对象、任务管理器等

## 特点

- 优点：
 - 节省内存空间
 - 避免频繁创建销毁对象，减轻GC工作，提高性能
 - 为整个系统提提供了一个全局访问点
- 缺点：
 - 不适用于频繁变化的对象
 - 扩展困难

## 实现

### 饿汉式

```java
public class HungrySingleton {
    private static HungrySingleton hungrySingleton = new HungrySingleton();

    public HungrySingleton() {
    }

    public static HungrySingleton getInstance() {
        return hungrySingleton;
    }
}
```
> 线程安全

### 懒汉式（Double-check Locking）

```java
public class LazySingleton {

    private static volatile LazySingleton lazySingleton = null;
    public LazySingleton() {
    }

    public static LazySingleton getInstance() {
        if (lazySingleton == null) {
            synchronized (LazySingleton.class) {
                if (lazySingleton == null) {
                    lazySingleton = new LazySingleton();
                }
            }
        }
        return lazySingleton;
    }
}
```

保证线程安全的原理：
`双重判空`：**第一次判空**，如果实例已存在，则直接返回实例，提升性能；**第二次判空**：当多个线程一起达到锁位置时，待第一个线程创建对象完成释放锁，其他线程竞争到锁后，判空直接返回实例  
`synchronized`：保证需要执行创建对象的线程只能有一个  
`volatile`：**可见性**、**禁止指令重排序**
- 可见性：**volatile**修饰的变量的修改对其他任何线程都是可见的，**两次判空**依赖该特性
- 禁止指令重排序：`new`关键字创建对象不是原子的，需要三步
> 1.在堆内存开辟内存空间
> 2.调用构造方法，初始化对象
> 3.饮用变量指向内存空间



> 代码下载地址：<https://github.com/ni-shiliu/neil-design-mode> 
{: .prompt-info }  

> 参考文献：《Head First 设计模式》


