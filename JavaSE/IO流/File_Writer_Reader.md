# 字符流

- **单位**：**字符（Character）**。它内部自带一个“解码器/编码器”。
- **作用**：它像一个智能的桥梁，在你的程序（处理字符）和数据源（存储字节）之间进行自动转换。
  - **读取时（Reader）**：它会从数据源读取字节，然后根据你指定的（或默认的）编码，把字节智能地转换成完整的字符，再交给你。它绝不会把一个汉字“劈”开。
  - **写入时（Writer）**：你把字符串（由字符组成）交给它，它会根据编码，正确地转换成字节序列，再写入到目的地。

### **专门用于处理纯文本数据**（.txt, .java, .html, .xml等）



和字节流的` InputStream/OutputStream `对应，字符流的两大抽象父类是：

- `java.io.Reader`：所有字符**输入**流的父类。
- `java.io.Writer`：所有字符**输出**流的父类。



## 输出流

### 1.writer

- `void write(int c):` 写入一个字符。
- `void write(char[] cbuf): `写入一个字符数组。
- `void write(String str):` **这是字符流比字节流方便得多的地方，可以直接写入一个字符串！**
- `void flush():` 刷新缓冲区。
- `void close():` 关闭流。

```java
 try(FileWriter writer = new FileWriter("output.txt",true)){
            writer.write("林花谢了春红，太匆匆\n无奈朝来寒雨晚来风\n胭脂泪，相留醉，几时重\n自是人生长恨水长东\n\n");
        }catch(IOException e) {
            e.printStackTrace();
    }
```

### 2.**BufferedWriter** **&** **PrintWriter**

- `BufferedWriter: `和 `BufferedOutputStream` `一样，提供缓冲，极大提升性能。它有一个特有且好用的方法 newLine()，可以写入一个跨平台的换行符。
- `PrintWriter: `**处理文本输出的终极武器**。它在 `BufferedWriter` 的基础上，提供了更强大的` print() `和` println() `方法，可以打印各种数据类型，并且` println() `会自动换行。

```java
try (PrintWriter pw = new PrintWriter(new BufferedWriter(new FileWriter("pro_output.txt")))) {
            
            pw.println("使用 PrintWriter 写入的第一行。");
            pw.println("你看，println 会自动换行，多方便！");
            pw.print("如果用 print，");
            pw.print("就不会换行。");
            pw.println(); // 写入一个换行
            
            int age = 25;
            pw.printf("我的名字是 %s, 今年 %d 岁。", "小明", age);
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```



## 输入流

### 1.**FileReader**

- `int read():` 读取一个字符，返回其 Unicode 码值。到末尾返回 -1。
- `int read(char[] cbuf): `读取多个字符到字符数组，返回实际读取的字符数。到末尾返回 -1。

```java
 try (FileReader reader = new FileReader("char_output.txt")) {
            char[] buffer = new char[1024];
            int len;
            
            // 经典读取循环，注意 buffer 是 char[]
            while ((len = reader.read(buffer)) != -1) {
                // 直接用 new String(char[], offset, length)
                System.out.print(new String(buffer, 0, len));
            }
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```

### 2.**BufferedReader**

- `public String readLine() throws IOException:` **一次读取一行文本！** 它会自动处理各种换行符（`\r`, `\n`, `\r\n`）。当读到文件末尾时，返回` null`。

```java
 try (BufferedReader br = new BufferedReader(new FileReader("pro_output.txt"))) {
            
            String line;
            System.out.println("开始按行读取文件：");
            
            // 按行读取的经典循环！！！
            // 1. 调用 br.readLine() 读取一行
            // 2. 将结果赋值给 line
            // 3. 判断 line 是否为 null，不为 null 说明还没读完
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```

