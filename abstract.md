### 抽象方法没有方法体，非抽象方法有默认实现

抽象方法：只声明不实现，用 abstract 标记，形如：
```Java
abstract class 父类 {
    public abstract void doSomething();  // 没有方法体
}
```
子类 必须 重写（实现）这个方法，否则子类也要声明为 abstract 

### 非抽象方法：有完整的方法体，提供了默认行为：
```Java
class 父类 {
    public void doSomething() {
        System.out.println("默认行为");
    }
}
```
子类可以选择重写，也可以直接继承默认实现

### 抽象类对比普通父类

**1.共享代码和状态：**

想象一下，我们有很多相关的类（比如各种动物：狗、猫、鸟），它们有一些共同的属性（比如名字、年龄）和共同的行为（比如睡觉、吃饭）。
如果我们不用抽象类，直接让它们都从一个普通的基类继承，那个普通的基类可以包含这些共同的属性和方法的实现。这也没问题。
但是，如果这个基类本身代表的是一个很抽象的概念（比如“动物”），如果我们不希望直接创建“一个动物”的对象，只想创建具体的“狗”、“猫”对象。把基类设为抽象类，就可以防止这种情况发生啦

**2.强制子类实现特定行为：**

在动物的例子中，所有的动物都会叫，但是狗有狗的叫声，猫有猫的叫声。在“动物”这个层次，没法给一个统一的“叫”的方法实现。
如果用普通类，我们可能只能在基类里写一个空的 makeSound() 方法，或者一个打印“未知声音”的方法。这样子类可能忘了去重写它，或者用了那个无意义的默认实现。
但如果把 makeSound() 声明成抽象方法放在抽象类里，Java编译器就会强制所有继承这个抽象类的非抽象子类必须实现 makeSound() 方法。


**例：**
```Java
// 这是一个抽象类：小动物
abstract class Animal {
    String name;

    // 构造器
    public Animal(String name) {
        this.name = name;
    }

    // 具体方法：所有动物都会吃东西
    public void eat() {
        System.out.println(name + " 正在吃东西。");
    }

    // 抽象方法：不同的动物叫声不一样，这里只声明，不实现
    public abstract void makeSound(); // 没有方法体哦！
}

// Dog 类继承 Animal
class Dog extends Animal {
    // 子类构造器调用父类构造器
    public Dog(String name) {
        super(name);
    }

    // 子类必须实现抽象方法 makeSound()
    @Override
    public void makeSound() {
        System.out.println(name + " 汪汪汪！");
    }
}

// Cat 类继承 Animal
class Cat extends Animal {
    // 子类构造器调用父类构造器
    public Cat(String name) {
        super(name);
    }

    // 子类必须实现抽象方法 makeSound()
    @Override
    public void makeSound() {
        System.out.println(name + " 喵喵喵！");
    }
}

// 测试一下
public class AnimalDemo {
    public static void main(String[] args) {
        // 不能直接创建 Animal 对象： Animal myAnimal = new Animal("某个动物"); // 错误！

        Dog myDog = new Dog("阿黄");
        myDog.eat(); // 调用父类的具体方法
        myDog.makeSound(); // 调用子类实现的抽象方法

        Cat myCat = new Cat("小花");
        myCat.eat(); // 调用父类的具体方法
        myCat.makeSound(); // 调用子类实现的抽象方法

        // 可以使用父类（抽象类）引用指向子类对象
        Animal anotherAnimal = new Dog("大黑");
        anotherAnimal.makeSound(); // 汪汪汪！ (多态的体现)
    }
}

```
