---
title:  Java学习之路--内部类相关
tags: [编程,学习,Java]
categories: [Java]
date: 2022-11-18

---

**静态内部类：**

```java
/**
    目标：静态内部类的研究（了解语法即可）

    什么是静态内部类？
        有static修饰，属于外部类本身，会加载一次。

    静态内部类中的成分研究：
        类有的成分它都有，静态内部类属于外部类本身，只会加载一次
        所以它的特点与外部类是完全一样的，只是位置在别人里面而已。

        外部类=宿主
        内部类=寄生

    静态内部类的访问格式：
        外部类名称.内部类名称

    静态内部类创建对象的格式：
        外部类名称.内部类名称 对象名称 = new 外部类名称.内部类构造器;

    静态内部类的访问拓展：
        静态内部类中是否可以直接访问外部类的静态成员?可以的，外部类的静态成员只有一份，可以被共享！
        静态内部类中是否可以直接访问外部类的实例成员?不可以的,外部类的是成员必须用外部类对象访问！！
    小结：
         静态内部类属于外部类本身，只会加载一次
         所以它的特点与外部类是完全一样的，只是位置在别人里面而已。

 */
public class InnerClass {
    public static void main(String[] args) {
        // 外部类名称.内部类名称 对象名称 = new 外部类名称.内部类构造器
        Outter.Inner in = new Outter.Inner();
        in.setName("张三");
        in.setAge(12);
        System.out.println(in.getName());
        System.out.println(in.getAge());
        in.show();
    }
}

class Outter{
    public static int age1 = 12;
    private double salary;

    // 静态内部类：有static修饰，属于外部类本身，只会加载一次
    public static class Inner{
        private String name;
        private int age;
        public static String schoolName = "黑马";

        public void show() {
            System.out.println(name+"-->"+age+"岁~");
            System.out.println(age1);
            //System.out.println(salary);
        }

        public Inner() {
        }

        public Inner(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

    }
}
```

**一般内部类：**

```java
/**
    目标：内部类_实例内部类（成员内部类）(了解语法为主)

    什么是实例内部类：
        无static修饰的内部类，属于外部类的每个对象的，跟着对象一起加载的。

    实例内部类的成分特点：
        实例内部类中不能定义静态成员，其他都可以定义。
        可以定义常量。

    实例内部类的访问格式：
        外部类名称.内部类名称。

    创建对象的格式：
        外部类名称.内部类名称 对象名称 = new 外部类构造器.new 内部构造器;

    拓展：
        实例内部类中是否可以直接访问外部类的静态成员？可以的，外部类的静态成员可以被共享访问！
        实例内部类中是否可以访问外部类的实例成员？可以的，实例内部类属于外部类对象，可以直接访问当前外部类对象的实例成员！
    小结：
        实例内部类属于外部类对象，需要用外部类对象一起加载，
        实例内部类可以访问外部类的全部成员！
 */
public class InnerClass {
    public static void main(String[] args) {
        // 实例内部类属于外部类对象。实例内部类的宿主是外部类对象！！
        Outter.Inner in = new Outter().new Inner();
        in.show();
    }
}
// 外部类
class Outter{
    public static int age = 1;
    private double salary;

    // 实例内部类：无static修饰，属于外部类的对象
    public class Inner{
        private String name ;

        public static final String schoolName = "黑马";
        // 不能在实例内部类中定义静态成员！！！
//      public static String schoolName = "黑马";
//      public static void test(){
//
//      }

        // 实例方法
        public void show(){
            System.out.println(name+"名称！");
            System.out.println(age);
            System.out.println(salary);
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```

**匿名内部类：**

```java
/**
    目标：匿名内部类的概述、

    什么是匿名内部类？
        就是一个没有名字的局部内部类。
        匿名内部类目的是为了：简化代码，也是开发中常用的形式。

    匿名内部类的格式：
        new 类名|抽象类|接口(形参){
            方法重写。
        }
     匿名内部类的特点：
         1.匿名内部类是一个没有名字的内部类。
         2.匿名内部类一旦写出来，就会立即创建一个匿名内部类的对象返回。
         3.匿名内部类的对象的类型相当于是当前new的那个的类型的子类类型。
    小结：
         1.匿名内部类是一个没有名字的内部类。
         2.匿名内部类一旦写出来，就会立即创建一个匿名内部类的对象返回。
         3.匿名内部类的对象的类型相当于是当前new的那个的类型的子类类型。

 */
public class Anonymity {
    public static void main(String[] args) {
        Animal a = new Animal(){
            @Override
            public void run() {
                System.out.println("猫跑的贼溜~~");
            }
        };
        a.run();
        a.go();

        Animal a1 = new Animal() {
            @Override
            public void run() {
                System.out.println("狗跑的贼快~~~");
            }
        };
        a1.run();
        a1.go();


    }
}
abstract class Animal{
    public abstract void run();

    public void go(){
        System.out.println("开始go~~~");
    }
}
```

