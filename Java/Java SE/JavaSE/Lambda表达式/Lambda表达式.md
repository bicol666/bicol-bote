
# Lambda式表达式总结

## 一、简介

--/ 目的

为了简化大量使用匿名内部类的情况，jdk8提供了lambda表达式；



--/ 使用前提
必须要是函数型接口；



--/ 函数型接口

只包含一个抽象方法的接口，就称为函数式接口；

ps：我们可以给接口加上注解：@FunctionalInterface来检查当前接口是否满足函数型接口；

---/ 示例

```java
@FunctionalInterface
public interface Runnable {
    void run();
}
```



## 二、语法
--/ 格式
```java
( 形参1 , 形参2 , …… )->{
    方法体……
}
```
--/ 说明
- ` ( ) `： 对应要重写的抽象方法的参数列表；
- ` { }` ： 对应要重写的抽象方法的方法体；
- 当要重写的抽象方法有形参的时候，形参的数据类型可以省略不写，即`( )`中的`数据类型`可以省略不写；
- 当要重写的抽象方法只有`一个形参`的时候，`( )`可以省略不写；
- 当要重写的抽象方法的方法体中只有一条语句时：`{ }`可以省略不写；
- 当要重写的抽象方法的方法体中只有一条语句，并且这条语句是return语句的时：关键字`return`和`{ }`可以省略不写；



## 三、示例

--/ Test.java

```java
public class Test {

    public static void main(String[] args) {

        //匿名内部类形式
        Runnable run01 = new Runnable() {
            @Override
            public void run(String name) {

            }
        };

        //1、原来的匿名内部类可以换成Lambda表达式的形式
        Runnable run02 = (String name) -> {
            System.out.println(name + "会跑");
        };

        //2、当（）内有形参的时候：数据类型可以省略不写；
        Runnable run03 = (name) -> {
            System.out.println(name + "会跑");
        };

        //3、当{ }内只有一条语句的时候：{ }可以省略不写；
        Runnable run04 = (name) -> System.out.println(name + "会跑");

        //4、当（）内只有一个参数的时候：（）可以省略不写；
        Runnable run05 = name -> System.out.println(name + "会跑");

        //5、当抽象方法有返回值，且{ }内只有一条语句，这条语句是return语句时：
        //  { } 和 关键字return可以省略不写；
        Nullable nullable = a -> true;
    }


    @FunctionalInterface
    interface Runnable {
        void run(String name);
    }

    @FunctionalInterface
    interface Nullable {
        boolean isNull(Object a);
    }
}
```
