---
title:  Java学习之路--多态与对象转型
tags: [编程,学习,Java]
categories: [Java]
date: 2020-1-29

---
```java
/**
 * 测试多态与对象转型
 * @author 葛宇
 * 多态三要素：继承、重写、父类引用指向子类对象
 * 对象转型关键点：子类转向父类是自动转型、父类转向子类是强制转型、直系可转型、兄弟系互转可通过编译但是运行报错、编译阶段按表面类型、运行阶段按本质类型
 */
package 面向对象核心;

public class TestPolyf {
	public static void main(String[] args) {
		Animal a = new Animal();
		Animal d_ = new Dog();		//d_本质还是Dog类对象变量（自动转型）,但编译环节暂定为Animal类
		Animal c_ = new Cat();		//c_本质还是Cat类对象变量（自动转型）,但编译环节暂定为Animal类	
		Dog d = new Dog();
		Cat c = new Cat();
		
		d_.x = 1;		//不报错,编译环节暂定d_是Animal类，拥有Animal类的成员变量
	  //d_.y = 1;		//报错,编译环节暂定d_是Animal类，无法拥有Dog类的成员变量
		
		Dog _d = (Dog)d_;		//使用强制类型转换，将Animal类的d_转为Dog类
		_d.y = 1;				//转型后可以使用Dog类的成员变量
		
		Dog _d_ = (Dog)c_;		//编译通过，JVM运行时报错
		_d_.y = 1;				//编译通过，JVM运行时报错
	
	  //当传入子类对象的引用d,c,d_,c_发生多态：
		animalCry(a);
		animalCry(d_);		//在运行环节JVM会自动识别出d_本质是Dog类，从而完成多态
		animalCry(c_);		//在运行环节JVM会自动识别出c_本质是Cat类，从而完成多态
		animalCry(d);
		animalCry(c);
	}
	
	static void animalCry(Animal a/*这里一定要是父类引用*/) {
	  //多态机制实现了代码的重用与精简	
		a.shout();
	}
}

class Animal{
	int x;
	public void shout() {
		System.out.println("Animal shout!");
	}
}

class Dog extends Animal{	//继承
	int y;
	public void shout() {
		System.out.println("Dog shout!");	//重写
	}
}

class Cat extends Animal{	//继承
	int z;
	public void shout() {
		System.out.println("Cat shout!");	//重写
	}
}

```

---

**多态的注意事项**

```java
class father{
    int x = 1;

    void show(){
        System.out.println(x);
    }
}

class son extends father{
    int x = 2;
    
    @Override
    void show(){
        System.out.println(x);
    }

    void special(){}
}

class test{
    public static void main(String[] args) {
        father o = new son();
        
        o.show();
        System.out.println(o.x);
        // o.special(); ERROR

        son s = (son)o;
        
        s.show();
        System.out.println(s.x);
        s.special();
    }
}
```

**说明：**

- 对于方法的调用，编译时以左边引用类型有的为准，运行时以具体绑定的对象类型为准
- 对于变量的调用，编译时以左边引用类型有的为准，运行时也以左边的引用类型为准
- 可以通过强转的方式将指向子类对象的父类引用转回子类引用，转回后的引用支持访问子类特有的方法
