### 内部类主要分为四种类型：
**1.成员内部类 (Member Inner Class)**

**2.静态嵌套类 (Static Nested Class)**

**3.局部内部类 (Local Inner Class)**

**4.匿名内部类 (Anonymous Inner Class)**

### 这里最常用的是**匿名内部类**（重点讲）

匿名内部类是没有名字的局部内部类。它通常用于快速实现一个接口或者继承一个类，并且只使用一次。

特点：

没有类名，在定义的同时就创建了实例。

通常用于实现只有一个方法的接口（函数式接口），或者简单地重写父类的方法。

语法结构：**new 父类构造器(参数) / 接口() { // 类体 }**

和局部内部类一样，可以访问外部类的所有成员，以及所在作用域中 final 或事实上的 final 局部变量。

不能定义构造器（因为它没有名字），但可以通过代码块来执行类似的初始化。

不能定义静态成员（除了 static final 常量）。

**它要么继承一个类，要么实现一个接口：** 这是它存在的唯一目的。你不能创建一个既不继承也不实现任何东西的匿名内部类。
**不需要写（接口）implement，（继承）extends**

**例1（抽象类）：只能在方法体内部实现，不能独立实现**
```java
abstract class MessagePrinter {
    private String prefix;

    public MessagePrinter(String prefix) {
        this.prefix = prefix;
    }

    abstract void print(String message);

    public String getPrefix(){
        return prefix;
    }
}

public class AnonymousExtendDemo {
    public void displayMessage(String msg) {
        // 重点看这里：
        MessagePrinter printer = new MessagePrinter("LOG:") { // MessagePrinter 是一个抽象类
            // 这里你必须实现 MessagePrinter 抽象类中的 print 方法
            @Override
            void print(String message) {
                // 可以访问外部方法的final或effectively final变量
                // 也可以访问父类的成员 (通过 super 或直接访问 protected/public)
                System.out.println(super.getPrefix() + " " + message + " (Printed by anonymous class)");
            }
        };
        // 编译器理解为：
        // 1. 创建一个匿名类，这个类 extends MessagePrinter
        // 2. 在这个匿名类中，实现了抽象方法 print
        // 3. 调用父类 MessagePrinter 的构造器 new MessagePrinter("LOG:")
        // 4. 创建这个匿名类的一个实例，并赋值给 printer

        printer.print(msg);
    }

    public static void main(String[] args) {
        AnonymousExtendDemo demo = new AnonymousExtendDemo();
        demo.displayMessage("Hello World");
    }
}
```
**例2：（接口）**
```java
// 1. 定义一个接口
interface Greeting {
    void sayHello();
}

// 2. 使用匿名内部类来实现接口的类
public class AnonymousGreetingDemo {

    public void performGreeting(String occasion) {
        // 假设 occasion 是一个场景描述，比如 "Morning" 或 "Evening"
        // 我们想根据这个场景，临时创建一个 Greeting 对象来打招呼

        System.out.println("Occasion: " + occasion);

        // 3. 使用匿名内部类实现 Greeting 接口
        // 注意语法：new 接口名() { ...类体... }
        Greeting temporaryGreeter = new Greeting() {
            // 这里是匿名内部类的类体
            // 我们必须实现 Greeting 接口中的所有抽象方法

            @Override
            public void sayHello() {
                // 这个匿名内部类的 sayHello 实现可以很简单，
                // 也可以利用其外部作用域的变量 (如果它们是 final 或 effectively final)
                if ("Morning".equalsIgnoreCase(occasion)) {
                    System.out.println("Good morning!");
                } else if ("Evening".equalsIgnoreCase(occasion)) {
                    System.out.println("Good evening!");
                } else {
                    System.out.println("Hello there!");
                }
            }

            // 匿名内部类也可以有自己的（非接口定义的）方法和字段，但通常较少使用，
            // 因为外部很难直接调用这些非接口方法。
            // public void extraMethod() { System.out.println("Extra from anonymous"); }
        };

        // 4. 使用这个匿名内部类创建的对象
        temporaryGreeter.sayHello();
        // temporaryGreeter.extraMethod(); // 这行会编译错误，因为 temporaryGreeter 的类型是 Greeting，Greeting 接口没有 extraMethod
    }

    public static void main(String[] args) {
        AnonymousGreetingDemo demo = new AnonymousGreetingDemo();

        demo.performGreeting("Morning");
        System.out.println("---");
        demo.performGreeting("Evening");
        System.out.println("---");
        demo.performGreeting("Anytime");


        // 另一种常见的匿名内部类用法：直接作为参数传递
        System.out.println("\n--- Greeting as a direct argument ---");
        greetSomeone(new Greeting() {
            private String name = "Guest"; // 匿名内部类可以有自己的字段
            @Override
            public void sayHello() {
                System.out.println("Hello, " + name + "! This is a direct greeting.");
            }
        });
    }

    // 一个接收 Greeting 类型参数的方法
    public static void greetSomeone(Greeting greeter) {
        greeter.sayHello();
    }
}
```

## 简单说一下其他几种

### 成员内部类
这是最普通的内部类形式，它定义在外部类的成员位置（和成员变量、成员方法同级）。

特点：

可以访问外部类的所有成员（包括私有成员）。

实例化成员内部类之前，必须先实例化外部类。也就是说，成员内部类的实例总是依附于一个外部类的实例。

成员内部类中不能定义静态成员（除非是 static final 的常量）。

**例：**
```java
class Outer {
    private String outerMessage = "Hello from Outer!";
    private int outerNumber = 10;

    // 成员内部类
    class Inner {
        private String innerMessage = "Hello from Inner!";

        public void display() {
            // 访问外部类的私有成员
            System.out.println("Outer message: " + outerMessage);
            System.out.println("Outer number: " + outerNumber);
            System.out.println("Inner message: " + innerMessage);
            System.out.println("Accessing Outer's 'this': " + Outer.this.outerNumber); // 显式访问外部类实例
        }
    }

    public void createInner() {
        Inner inner = new Inner(); // 外部类内部可以直接创建
        inner.display();
    }

    public static void main(String[] args) {
        // 1. 创建外部类实例
        Outer outer = new Outer();
        outer.createInner(); // 通过外部类方法创建和使用内部类

        System.out.println("---");

        // 2. 在外部类之外创建成员内部类实例 (需要外部类实例)
        Outer.Inner innerInstance = outer.new Inner();
        innerInstance.display();
    }
}

// 输出:
// Outer message: Hello from Outer!
// Outer number: 10
// Inner message: Hello from Inner!
// Accessing Outer's 'this': 10
// ---
// Outer message: Hello from Outer!
// Outer number: 10
// Inner message: Hello from Inner!
// Accessing Outer's 'this': 10
```
### 2. 静态内部类 (Static Nested Class)
静态内部类使用 static 关键字修饰，它与外部类的实例无关。

特点：

不持有外部类的引用：它不能直接访问外部类的非静态成员（实例变量和实例方法）。

可以像普通类一样拥有静态成员和非静态成员。

可以直接通过 OuterClass.StaticNestedClass 的方式访问和实例化，不需要外部类的实例。
```JAVA
class School {
    private static String schoolName = "My Awesome School";
    private String headmaster = "Mr. Smith";

    // 静态内部类
    static class Department {
        private String departmentName;

        public Department(String name) {
            this.departmentName = name;
        }

        public void displayInfo() {
            // 可以访问外部类的静态成员
            System.out.println("School: " + schoolName);
            // 不能直接访问外部类的非静态成员，下面这行会报错
            // System.out.println("Headmaster: " + headmaster);
            System.out.println("Department: " + departmentName);
        }

        public static void staticMethodInNested() {
            System.out.println("Static method in static nested class.");
        }
    }

    public void showHeadmaster() {
        System.out.println("Headmaster: " + headmaster);
    }

    public static void main(String[] args) {
        // 1. 创建静态内部类实例 (不需要外部类实例)
        School.Department mathDept = new School.Department("Mathematics");
        mathDept.displayInfo();

        School.Department.staticMethodInNested(); // 调用静态内部类的静态方法

        System.out.println("---");
        // 如果需要访问外部类的非静态成员，需要一个外部类实例
        School mySchool = new School();
        mySchool.showHeadmaster();
        // mathDept 不能直接访问 mySchool.headmaster
    }
}

// 输出:
// School: My Awesome School
// Department: Mathematics
// Static method in static nested class.
// ---
// Headmaster: Mr. Smith
```
### 3. 局部内部类 (Local Inner Class)
用的较少，不做具体介绍


