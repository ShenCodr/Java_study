# 1.转换流

### InputStreamReader (字节输入流 -> 字符输入流)

- **继承关系**：InputStreamReader **extends** Reader。所以，它本质上是一个**字符输入流 (Reader)**。

- **作用**：它接收一个 InputStream (字节输入流) 作为数据源，并使用指定的字符集（编码）将其解码成字符。你可以把它看作一个内置了“**字节到字符解码器**”的 Reader。

  `public InputStreamReader(InputStream in, String charsetName)`

允许你明确指定用哪种字符集（如 "UTF-8", "GBK"）来解码字节流。这是它最有价值的地方。

```java
//假设我们有一个 test_gbk.txt 文件，它的内容是 "你好"，并且是用 GBK 编码保存的。
try (
            FileInputStream fis = new FileInputStream("test_gbk.txt");
            InputStreamReader isr = new InputStreamReader(fis, "GBK"); // 指定用 GBK 解码
            BufferedReader br = new BufferedReader(isr) // 再用 BufferedReader 包装，以便用 readLine()
        ) {
            String line = br.readLine();
            System.out.println("成功读取内容：" + line); // 输出：成功读取内容：你好
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```



### OutputStreamWriter (字节输出流 <- 字符输出流)

- **继承关系**：OutputStreamWriter **extends** Writer。所以，它本质上是一个**字符输出流 (Writer)**。

- **作用**：它接收一个 OutputStream (字节输出流) 作为数据目的地，并将你写入的字符（字符串）使用指定的字符集编码成字节，再写入到字节流中。你可以把它看作一个内置了“**字符到字节编码器**”的 Writer。

  `public OutputStreamWriter(OutputStream out, String charsetName)`

  允许你明确指定用哪种字符集将字符串编码成字节序列。



```java
// 我们想把字符串 "Hello, 世界" 以 UTF-8 编码写入文件
        try (
            FileOutputStream fos = new FileOutputStream("output_utf8.txt");
            OutputStreamWriter osw = new OutputStreamWriter(fos, "UTF-8") // 指定用 UTF-8 编码
        ) {
            osw.write("Hello, 世界");
            osw.flush(); // 字符流写入后最好 flush 一下
            
            System.out.println("以 UTF-8 编码写入文件成功！");
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```



# 2.序列化流

### 序列化流

将对象写入文件当中（乱码）

```
// 创建一个 Student 对象，参数为姓名和学号
        Student stu = new Student("sc", 123);
        // 创建 ObjectOutputStream，用于将对象写入文件
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.txt"));
        // 将 stu 对象序列化写入文件
        oos.writeObject(stu);
        // 关闭输出流，释放资源
        oos.close();
        
```

注意`Student`类必须用`Serializable`接口

```java
public class Student implements Serializable {......
                                             //如果不想把当前属性序列化到本地文件当中
    										//就加一个transient，例如：private transient String address
                                             }
```

### 反序列化

将文件中的对象写回

```java
// 创建 ObjectInputStream，从 student.txt 文件读取对象
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.txt"));
            // 读取对象（需要确保 student.txt 中存储的是序列化对象）
            Object o = ois.readObject();
            // 打印读取到的对象
            System.out.println(o);
            // 关闭输入流，释放资源
            ois.close();
```

**注意**：转换时不能修改`student`类

或者指定序列号`private static final long serialVersionUID = 1L; // 序列化版本号`在Student类中



# 3.打印流

1. **方便性**：提供了丰富的 print() / println() / printf() 方法，可以直接“打印”各种类型的数据，无需手动转换成字符串或字节数组。
2. **永不抛出 IOException**：这是它们一个非常独特的设计。打印流在内部会捕获所有 IOException，并设置一个内部的错误状态。你可以通过调用 checkError() 方法来检查是否曾发生过错误。这使得在一些简单的输出场景（如打印日志）下，代码可以写得更简洁，不用到处都是 try-catch。
3. **自动刷新 (Auto-flushing)**：PrintWriter 可以在构造时设置为自动刷新模式。在这种模式下，每当调用 println(), printf(), 或 format() 方法时，它会自动调用 flush()，确保数据立刻被输出。

### 字节打印流 (PrintStream)

```java
public PrintStream(OutputStream out): 包装一个字节输出流。
public PrintStream(String fileName): 直接关联到一个文件名。
public PrintStream(OutputStream out, boolean autoFlush): 包装一个字节流，并可以设置是否自动刷新。
```

实例：

```java
try (PrintStream ps = new PrintStream("print_stream_output.txt")) {
            
            ps.print("Hello, ");
            ps.println("PrintStream!"); // println 会换行
            
            int age = 30;
            double salary = 12345.67;
            
            ps.println("这是一个整数: " + age); // 直接拼接和打印
            
            // 使用 printf 实现格式化输出，和 C 语言非常像
            // %s 代表字符串, %d 代表十进制整数, %.2f 代表保留两位小数的浮点数
            ps.printf("姓名: %s, 年龄: %d, 薪水: %.2f", "李四", age, salary);
            
            // 检查是否有错误发生 (通常不会)
            if (ps.checkError()) {
                System.out.println("打印过程中发生了错误。");
            }
            
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
```



### 字符打印流 (PrintWriter)

- **设计目的**：它是在 PrintStream 之后出现的，可以说是 PrintStream 的字符流版本。它修复了 PrintStream 依赖平台默认编码的问题，并继承了所有方便的 print/println 方法。**在处理文本输出时，官方更推荐使用 PrintWriter**。

```java
public PrintWriter(Writer out, boolean autoFlush): 最常用！ 包装一个字符流，并设置自动刷新。
public PrintWriter(OutputStream out): 也可以直接包装一个字节流。内部会自动用 OutputStreamWriter 转换。
public PrintWriter(String fileName): 直接关联文件名。
public PrintWriter(String fileName, String csn): 直接关联文件名，并指定字符编码。
```

实例：

```java
// 使用 try-with-resources，并开启自动刷新
        // 组合: PrintWriter(包装)->BufferedWriter(缓冲)->FileWriter(目的地)
        try (PrintWriter pw = new PrintWriter(new BufferedWriter(new FileWriter("print_writer_output.txt")), true)) {
            
            pw.println("你好，这里是 PrintWriter。");
            pw.println(true); // 打印布尔值
            pw.println(3.14159); // 打印 double
            
            // 自动刷新开启后，每次 println 都会立刻写入文件，无需手动 flush
            
            pw.printf("圆周率约等于 %.4f", Math.PI);
            
            // 检查错误状态
            if (pw.checkError()) {
                System.out.println("写入时发生错误。");
            }

        } catch (IOException e) {
            // 虽然 PrintWriter 不抛出 IOException, 但它的构造器和底层的流会抛出
            e.printStackTrace();
        }
```

#### PrintStream vs. PrintWriter 对比

| 特性         | PrintStream (字节打印流)                      | PrintWriter (字符打印流)                                |
| ------------ | --------------------------------------------- | ------------------------------------------------------- |
| **流类型**   | **字节流** (OutputStream)                     | **字符流** (Writer)                                     |
| **主要构造** | 包装 OutputStream 或文件路径                  | 包装 Writer, OutputStream 或文件路径                    |
| **处理数据** | 可以处理字节和字符数据                        | 主要处理字符数据                                        |
| **字符编码** | 使用平台**默认编码**，不易控制                | **可以指定编码**（通过包装 OutputStreamWriter），更灵活 |
| **自动刷新** | 可以设置，但只对 byte[] 的 println 等方法有效 | 可以设置，对 println, printf, format 有效               |
| **异常处理** | 内部捕获 IOException，通过 checkError() 检查  | 内部捕获 IOException，通过 checkError() 检查            |
| **官方推荐** | 用于 System.out 等底层输出和二进制兼容场景    | **处理文本输出时的首选**                                |