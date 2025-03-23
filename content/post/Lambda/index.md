---
date : '2025-03-23T21:36:15+08:00'
draft : false
title : 'Lambda和Stream流'
categories:
    - Java
tags:
    - Lambda
---

## Lambda表达式

Lambda表达式是Java 8引入的一项重要功能，它允许我们以更简洁和灵活的方式编写代码。

- 使用Lambda函数替代某些匿名内部类对象，从而让程序代码更简洁，可读性更好
- Lambda并不能简化全部的匿名内部类，Lambda**只能简化函数式接口的匿名内部类**

**Lambda**表达式的基本结构是：

```java
f -> { }
```

箭头（->）前表示参数变量有多个变量或没有参数的时候，必须使用小括号；箭头（->）后的执行语句只有一条时，可以不加大括号包裹

```java
student1 -> {}

(student1, student2) -> {}

() -> {}

s -> System.out.println(s);
```



**函数式接口**：有且仅有一个抽象方法的接口

```java
@FunctionalInterface
public interface Play{
    void playing();
}
```

**Lambda表达式的省略规则**

用于进一步简化Lambda表达式的写法

- 参数类型全部可以省略不写
- 如果只有一个参数，参数类型省略的同时“（）”也可以省略，但多个参数不能省略
- 如果lambda表达式中只有一行代码，大括号可以不写，同时要省略分号”“；”，如果这行代码是return语句，也必须去掉return

```java
// 遍历水果集合，打印水果的名字
for (int i = 0; i < fruits.size(); i++) {
  Fruit f = fruits.get(i);
  System.out.println(f.getName());
}

// 等价于
fruits.forEach(f -> {
    System.out.println(f.getname());
})
```

> `f`变量的类型是根据上下文**自动**识别的

**引用外部变量**

Lambda表达式`{}`内的执行语句，除了能引用参数变量以外，还可以引用外部的变量

```java
List<Fruit> fruits = Array.asList(...);
String message = "水果名称：";

fruits.forEach(f -> {
    System.out.println(message + f.getname());
});

// message = ""; 加上这句会报错
```

> 注意：从Lambda表达式引用的本地变量必须是最终变量或实际上的最终变量（引用的局部变量不允许被修改、参数不能与局部变量同名）

**方法引用**

- 类名::静态方法

- 如果某个Lambda表达式里只是调用**一个静态方法**，并且“ -> ”**前后参数的类型一致**，就可以使用静态方法引用

```java
Arrays.sort(students,(o1,o2) -> Student.compareByAge(o1,o2));
// 等价于
Arrays.sort(students,Student::compareByAge);
```

**实例方法引用**

- 对象名::实例方法
- 如果某个Lambda表达式里只是通过对象名称调用**一个实例方法**，并且“ -> ”**前后参数的形式一致**，就可以使用实例方法引用

**特定类型方法引用**

- 特定类的名称::方法
- 如果某个Lambda表达式里只是调用**一个特定类型的实例方法**，并且前面参数列表中的第一个参数是作为方法的主调，后面的所有参数都是作为该实例方法的入参的，则此时就可以使用特定类型的方法引用

```java
Arrays.sort(names,(o1,o2) -> o1.compareToIgnoreCase(o2));
// 等价于
Arrays.sort(names,String::compareToIgnoreCase);
```

**构造器引用**

- 类名::new
- 如果某个Lambda表达式里只是在创建对象，并且“ -> ”前后参数情况一致，就可以使用构造器引用

**父类方法**

```java
public class Lambda extends LambdaExample {

public void print(List<Fruit> fruits) {
    fruits.forEach(super::print);
    }
    
}
```

