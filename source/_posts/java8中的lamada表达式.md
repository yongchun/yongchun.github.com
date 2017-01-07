---
title: java8中的lamada表达式
date: 2017-01-07 16:23:37
tags:
---

### 概述
'lamada表达式'是一段可以传递的代码，它可以被执行一次或多次，至于为什么要使用lamda表达式，这里就不在多做介绍了。简而言之就是java是一门面向对象的语言，在java中你不可能将一个代码片段到处传递，但是为了使得java可以支持这一特性，java8就引入了lamada表达式。

### lamada表达式语法
lamada的语法比较简单，但是其有很多简写的变种，刚开始接触时，可能会造成困惑。lamada语法如下：
(参数1类型 参数1, 参数2类型 参数2, 参数3类型 参数3, ...) -> { // do something }
举例如下：
```java
(String x, String y) -> x.compareToIgnoreCase(y)
```
下面说一说lamada语法的变种:
##### 参数这一边：
###### 如果参数类型可以推导出来，则参数类型可以省略:
```java
(x, y) -> x.compareToIgnoreCase(y)
```
###### 如果没有参数：
```java
() -> { // do something }
```
###### 如果只有一个参数，且参数类型可以推导出来，小括号也可以省略：
```java
 List<String> stringList = Lists.newArrayList("test","spring");

 String[] words = stringList.toArray(new String[0]);

 Stream.of(words).forEach(p -> { System.out.println(p) });
```
###### 参数也可以添加注解或者被final修饰
```java
(@NotNull final String x, String y) -> x.compareToIgnoreCase(y)
```
##### {}这边
###### 如果{}里的逻辑可以使用一个;号表示，大括号也可以省略。
```java
 Stream.of(words).forEach(p -> System.out.println(p));
```


### 备注
lamada表达式其实还有很多其他的变种，使用时候需要多积累，如：
```java
List<String> stringList = Lists.newArrayList("test","spring");

String[] words = stringList.toArray(new String[0]);

Stream.of(words).forEach(System.out::println);
```
