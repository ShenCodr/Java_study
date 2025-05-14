## Math
**和c++的函数一样，只不过调用方法为**

**`Math.函数名（）`**

```JAVA
Math.random(): 返回一个带正号的 double 值，该值大于等于 0.0 且小于 1.0。

Math.round(float a) / Math.round(double a): 返回最接近参数的 int (对于 float) 或 long (对于 double)。这是标准的四舍五入

Math.ceil(double a): 返回大于或等于参数的最小整数（向上取整）。
Math.floor(double a): 返回小于或等于参数的最大整数（向下取整）。

常量有Math. PI和Math.E

```


## Runtime
需导入包：java.lang.Runtime
**用途：**
Runtime 类允许应用程序与其运行环境进行交互。例如，可以获取可用处理器数量、内存信息，或者执行外部进程。

**特点：**

Runtime 类采用单例模式。通过静态方法 Runtime.getRuntime() 来获取唯一的 Runtime 对象实例。

```JAVA

System. exit(0)//退出虚拟机实际上就是
Runtime.getRuntime().exit(0)

//runtime.freeMemory(): 返回 Java 虚拟机中的可用内存量（以字节为单位）。

//runtime.totalMemory(): 返回 Java 虚拟机中的内存总量（以字节为单位）。

//runtime.maxMemory(): 返回 Java 虚拟机试图使用的最大内存量（以字节为单位）。

Runtime runtime = Runtime.getRuntime();
long freeMemory = runtime.freeMemory();
long totalMemory = runtime.totalMemory();
long maxMemory = runtime.maxMemory();
System.out.println("Free Memory: " + freeMemory / (1024 * 1024) + " MB");
System.out.println("Total Memory: " + totalMemory / (1024 * 1024) + " MB");
System.out.println("Max Memory: " + maxMemory / (1024 * 1024) + " MB");


Runtime.getRuntime().exec(string command);: 在单独的进程中执行指定的字符串命令。（在命令行中运行command）
例如Runtime。getRuntime().exec("文件地址")
运行shutdown命令
-s 默认一分钟以后关机
-s -t 指定一段时间后关机，单位（秒）
-a 取消关机
-r 关机并重启
Runtime。getRuntime().exec("shutdown -s -t 3600");3600秒后关机

```

