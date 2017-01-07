---
title: java 函数式接口
date: 2017-01-07 15:19:10
tags:
---

### java函数式接口由来
java函数式接口是伴随着java8引入的lamada表达式而来的，在java里对象是第一位的，而不支持函数类型，自然也不支持函数作为参数传递。
```java
Arrays.sort(words, (String x, String y) -> x.compareToIgnoreCase(y));

Arrays.sort(words, (x, y) -> x.compareToIgnoreCase(y));

Arrays.sort(words, String::compareToIgnoreCase);
```
上面这段对一个字符串数组进行排序的代码，看起来像是函数式编程语言里的把函数作为参数传递一样，那么java是如何做到这一步的呢？
其实在java里，会将这一段lamada表达式转换为java里的一个接口对应的实现类，而这个接口就是函数式接口。如：java.lang.Comparable接口。


### java函数式接口长什么样子
```java
package java.util.function;

import java.util.Objects;

/**
 * Represents an operation that accepts a single input argument and returns no
 * result. Unlike most other functional interfaces, {@code Consumer} is expected
 * to operate via side-effects.
 *
 * <p>This is a <a href="package-summary.html">functional interface</a>
 * whose functional method is {@link #accept(Object)}.
 *
 * @param <T> the type of the input to the operation
 *
 * @since 1.8
 */
@FunctionalInterface
public interface Consumer<T> {

    /**
     * Performs this operation on the given argument.
     *
     * @param t the input argument
     */
    void accept(T t);

    /**
     * Returns a composed {@code Consumer} that performs, in sequence, this
     * operation followed by the {@code after} operation. If performing either
     * operation throws an exception, it is relayed to the caller of the
     * composed operation.  If performing this operation throws an exception,
     * the {@code after} operation will not be performed.
     *
     * @param after the operation to perform after this operation
     * @return a composed {@code Consumer} that performs in sequence this
     * operation followed by the {@code after} operation
     * @throws NullPointerException if {@code after} is null
     */
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```
这是java8提供的一个函数式接口Consumer<T> 用来处理一个T类型的值,那么究竟什么才是一个函数式接口呢？
从概念上来讲，所有只含有一个抽象方法的接口都是函数式接口，所以java8之前的java.lang.Comparable接口也是一个函数式接口。可以在任意函数式接口上标注@FunctionalInterface注解，这样做有两个好处。首先，编译器会检查标注该注解的实体，检查它是否是只包含一个抽象方法的接口。另外，在javadoc页面也会包含一条声明，说明这个接口是一个函数式接口。该注解并不要求强制使用。

3.lamada表达式和函数式接口的关系
在java中任何一个lamada表达式都可以被转化为一个函数式接口，事实上，函数式接口的转换是你在java中使用lambda表达式能做的唯一一件事。所以一段lamada表达式可以赋值给一个函数式接口的变量。如：
```java
BiFunction<String,String,Integer> comp = (first,second) -> Integer.compare(first.length(),second.length());
```
其中接口BiFunction<T,U,R>描述了T和U类型的方法参数以及返回类型R。

但是当一个lambda表达式被转换为一个函数式接口的实例时，需注意处理检查期异常。如果lambda表达式中可能会抛出一个检查期异常，那么该异常需要在目标接口的抽象方法中进行声明。例如，以下表达式会产生一个错误：
```java
//错误：Thread.sleep可以抛出一个检查期的InterruptedException
Runnable sleeper = () -> {
            System.out.println("test");
            Thread.sleep(5000);
        };
```
由于Runnable.run不能抛出任何异常，所以这个赋值是不合法的。有两种方法可以修正该问题。一种是在lambda表达式中捕获异常，另一种是将lambda表达式赋值给一个其抽象方法可以抛出异常的接口。例如，Callable接口的call方法可以抛出任何异常。修改为以下的形式：
```java
Runnable sleeper = () -> {
            System.out.println("test");
            try {
                Thread.sleep(5000);
            } catch (Exception e) {
                //Ignore
            }
        };
```
或者
```java
  Callable sleeper = () -> {
            System.out.println("test");
            Thread.sleep(5000);
            return null;
        };
```

### java8提供的函数式接口
java8提供的函数式接口位于java.util.function包下，可以参考:[函数式接口](http://colobu.com/2014/10/28/secrets-of-java-8-functional-interface/)
