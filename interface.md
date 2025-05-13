## 接口

定义了一组必必须实现的方法。任何类如果说要实现（implements）这个接口，就得按照这个契约来，把接口里定义的所有方法都实现出来！

接口本身不能包含任何状态（成员变量）（除了常量），它主要关注的是行为。它定义的是“能做什么”，而不是“是什么”。

类通过**implements**关键字实现接口： 一个类可以实现一个或多个接口！

**例：**
```Java
// 定义一个接口：可打印的能力
interface Printable {
    // 抽象方法：打印内容
    void printContent();

    // Java 8 默认方法：打印标题
    default void printTitle(String title) {
        System.out.println("----- " + title + " -----");
    }

    // Java 8 静态方法：打印分隔线 (只能通过接口名调用)
    static void printSeparator() {
        System.out.println("--------------------");
    }
}

// Report 类实现 Printable
class Report implements Printable {
    String content;

    public Report(String content) {
        this.content = content;
    }

    // 必须实现 printContent()
    @Override
    public void printContent() {
        System.out.println(content);
    }
    // 这里没有重写 printTitle()，将使用默认实现
}

// Article 类实现 Printable，但想自定义标题打印方式
class Article implements Printable {
    String title;
    String content;

    public Article(String title, String content) {
        this.title = title;
        this.content = content;
    }

    // 必须实现 printContent()
    @Override
    public void printContent() {
        System.out.println(content);
    }

    // 重写 printTitle() 方法
    @Override
    public void printTitle(String title) {
        System.out.println("=== " + title.toUpperCase() + " ===");
    }
}

// 测试一下
public class PrintableDemo {
    public static void main(String[] args) {
        Report weeklyReport = new Report("本周工作总结...");
        weeklyReport.printTitle("周报"); // 调用接口的默认方法
        weeklyReport.printContent();

        Printable.printSeparator(); // 调用接口的静态方法

        Article newsArticle = new Article("震惊！猫娘学会了Java！", "喵喵喵，Java真好玩...");
        newsArticle.printTitle("新闻"); // 调用子类重写的 printTitle 方法
        newsArticle.printContent();
    }
}

```
