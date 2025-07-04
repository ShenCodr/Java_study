# 字节输出流

## OutPutStream 

```java
try (FileOutputStream fos = new FileOutputStream("output.txt")) {
    String text = "Hello, Java!";

    // 将字符串转换为字节数组
    byte[] bytes = text.getBytes();

    // 将整个字节数组写入文件
    fos.write(bytes);

    System.out.println("数据成功写入到 output.txt 文件！");

    fos.close();
} catch (IOException e) {
    // 处理可能发生的IO异常
    System.err.println("写入文件时发生错误: " + e.getMessage());
}
```

**注意，每次写入时都会先清空文件里的数据再写入**

这里介绍一下FIleOutputStream

```java
FileOutputStream(String name, boolean append)
name: 文件路径。
append: 一个布尔值。
如果为 true：追加模式。如果文件已存在，则在文件末尾写入数据。如果文件不存在，则创建新文件。
如果为 false（或者不写这个参数，使用另一个构造方法）：覆盖模式。如果文件已存在，则清空文件内容再写入。
```

**所以只需要后面加一个true就可以完成续写**



下面介绍如何换行

对于windows来说，使用**\r\n**就可以了



## BufferedOutputStream

1. 你创建一个 BufferedOutputStream，它内部会自动创建一个**字节数组**作为缓冲区（默认大小为 8192 字节，即 8KB）。
2. 当你调用 bos.write(data) 时，数据并**不是**立刻被写入到硬盘，而是先被快速地写入到内存的这个缓冲区数组里。
3. 只有当以下情况发生时，缓冲区的数据才会被一次性地“刷（flush）”到硬盘上的文件里：
   - 
   - 缓冲区满了。
   - 你手动调用了 bos.flush() 方法。
   - 你调用了 bos.close() 方法（close 会在关闭前自动调用 flush）。

```java
// 1. 创建流对象 (使用 try-with-resources)
        try (
                // 使用缓冲流包装基础流
                BufferedInputStream bis = new BufferedInputStream(new FileInputStream("source_image.jpg"));
             BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copied_image.jpg"))
        ) {
            byte[] buffer = new byte[8192]; // 缓冲区大小可以自定义，但通常使用默认值或8K的倍数
            int len;
            // 经典读写循环
            while ((len = bis.read(buffer)) != -1) {
                // 从缓冲输入流读取，写入到缓冲输出流
                bos.write(buffer, 0, len);
            }

            // try-with-resources 会自动关闭所有流
            // bos.close() 会自动调用 fos.close()
            // bis.close() 会自动调用 fis.close()

        } catch (IOException e) {
            e.printStackTrace();
        }
```





# 字节输入流

## InPutStream 

1. int read() 
   - 
   - **作用**：从输入流中读取**一个字节**的数据。
   - **返回值**：返回的是一个 int 类型（0 到 255）。如果读到了流的末尾，没有更多数据了，它会返回 **-1**。
   - **为什么返回 int 而不是 byte？** 因为 byte 的范围是 -128 到 127，无法表示文件结束的信号 -1。所以用一个更大的 int 类型来返回读取到的字节数据（值在 0-255），并用 -1 这个特殊值来表示“读完了”。
2.  int read(byte[] b) 
   - **作用**：尝试从输入流中读取数据，并存入到你提供的字节数组 b 中。
   - **返回值**：返回**实际读取到的字节数**。这个数字可能是 0 到 b.length 之间的任意整数。如果还没开始读就到达了流的末尾，则返回 **-1**。
   - **重点**：这个方法**不保证**一定能填满你的字节数组！比如文件只剩下 100 个字节，但你的数组大小是 1024，那么它只会读取 100 个字节，并返回 100。
3. int read(byte[] b, int off, int len) 
   - **作用**：从输入流中读取 len 个字节，并从数组 b 的 off 索引位置开始存储。
   - **返回值**：同样是**实际读取到的字节数**，或者在到达流末尾时返回 **-1**。

```java
 try (FileInputStream fis = new FileInputStream("output.txt")) {
            
            // 创建一个缓冲区（字节数组），一次读取多个字节效率更高
            byte[] buffer = new byte[1024]; // 数组大小通常是1024的倍数
            
            int len; // 用来存储每次实际读取到的字节数
            
            System.out.println("开始读取 output.txt 文件内容：");
            
            // 这是读取文件的经典循环！！！
            // 1. fis.read(buffer) 尝试读取数据到 buffer
            // 2. 将读取的字节数赋值给 len
            // 3. 判断 len 是否等于 -1，如果不等于，说明还没读完
            while ((len = fis.read(buffer)) != -1) {
                // 将 buffer 中从 0 到 len 的有效字节转换为字符串并打印
                // 注意：必须使用 new String(byte[], offset, length) 这个构造方法！
                // 因为最后一次读取可能填不满整个 buffer
                String str = new String(buffer, 0, len);
                System.out.print(str);
            }
            System.out.println("\n文件读取完毕！");
            
        } catch (IOException e) {
            System.err.println("读取文件时发生错误: " + e.getMessage());
        }
```

解释：

while ((len = fis.read(buffer)) != -1) 这行代码是 Java IO 操作的**核心模式**，你必须深刻理解它：

1. 调用 fis.read(buffer)，程序会“阻塞”（暂停），直到读取到一些数据或到达文件末尾。
2. read 方法返回实际读到的字节数，这个值被赋给 len。
3. 然后 while 判断 len 是否为 -1。
   - 如果不是 -1，表示成功读取了 len 个字节，循环体内的代码执行。
   - 如果是 -1，表示文件已读完，循环结束。

new String(buffer, 0, len)：这步非常关键！因为 read 方法可能没有填满整个 buffer 数组，所以我们只把 buffer 中从索引0开始，长度为 len 的这部分**有效数据**转换成字符串。

## BufferedInputStream

- **工作原理**：
  1. 
  2. 当你创建 BufferedInputStream 时，它同样会创建一个内部的缓冲区数组。
  3. 你第一次调用 bis.read() 时，它并**不是**只从硬盘读取1个字节，而是一次性从硬盘文件中读取**一大块数据（比如8KB）**填充到它的内部缓冲区。
  4. 之后你再调用 bis.read()，它会直接从**内存**的缓冲区里把数据给你，速度极快。
  5. 直到缓冲区里的数据被你全部读完，下一次 read() 调用才会再次触发一次对硬盘的批量读取。

代码在`BufferedOutputStream`部分

## 文件拷贝

```java
FileInputStream fis = new FileInputStream("C:\\Users\\沈超\\Videos\\WeChat_20250704105208.mp4");
        FileOutputStream fos = new FileOutputStream("Copy.mp4");

        //创建一个缓冲区（字节数组），一次读取多个字节效率更高
        byte[] buffer = new byte[1024];
        int len; // 用来存储每次实际读取到的字节数
        while((len = fis.read(buffer)) != -1) { // 将读取的字节数赋值给 len
            // 将 buffer 中从 0 到 len 的有效字节写入到 fos
            fos.write(buffer, 0, len);
        }
            fos.close();
            fis.close();
```

多插嘴一句，在utf-8中，一个字母占一个字节，一个汉字占三个字节
