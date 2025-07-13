
# Thread的实现方法

## 1.继承Thread实现多线程

```java
//文件1
public class MyThread extends Thread{
    @Override
    public void run(){
        for(int i=0;i<100;i++){
            System.out.println(getName() + "启动！");
        }
    }
}

//文件2
public class Main {
    public static void main(String[] args) {
         MyThread test1 = new MyThread();
         MyThread test2 = new MyThread();
         
         test1.setName("线程1");
         test2.setName("线程2");
         test1.start();
         test2.start();
    }
}
```
线程1，2会交替执行
充分利用cpu资源

## 2.实现Runable接口

**用的最多**

```java
//文件1
public class Main{
    public static void main(String[] args){
     /*
     * 多线程的第二种启动方式：
     *     1. 自定义一个类实现Runnable接口
     *     2. 重写里面的run方法
     *     3. 创建自己的类的对象
     *     4. 创建一个Thread类的对象，并开启线程
     * 
     */

    // 创建MyRun的对象
    // 表示多线程要执行的任务
    MyRun mr = new MyRun();

    // 创建线程对象
    Thread t1 = new Thread(mr);
    Thread t2 = new Thread(mr);

    // 给线程设置名字
    t1.setName("线程1");
    t2.setName("线程2");

    // 开启线程
    t1.start();
    t2.start(); 
    }    
}

//文件2
public class MyRun implements Runnable{
    
    @Override
    public void run(){
        for(int i=0;i<100;i++){
        System.out.println(Thread.currentThread().getName() + "启动～");
	    }
    }
}
```

## 3. 利用Callable接口和Future接口方式实现

**用的第二多**

```java
//文件1
public class MyCallable implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        // 求1~100之间的和
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            sum = sum + i;
        }
        return sum;
    }
}

//文件2
public class Main {
    public static void main(String[] args)｛
/*
 * 多线程的第三种实现方式：
 * 特点：可以获取到多线程运行的结果
 *  1. 创建一个类MyCallable实现Callable接口
 *  2. 重写call（是有返回值的，表示多线程运行的结果）
 *  3. 创建MyCallable的对象（表示多线程要执行的任务）
 *  4. 创建FutureTask的对象（作用管理多线程运行的结果）
 *  5. 创建Thread类的对象，并启动（表示线程）
 */

 // 创建MyCallable的对象（表示多线程要执行的任务）
MyCallable mc = new MyCallable();
// 创建FutureTask的对象（作用管理多线程运行的结果）
FutureTask<Integer> ft = new FutureTask<>(mc);
// 创建线程的对象
Thread t1 = new Thread(ft);
// 启动线程
t1.start();

// 获取多线程运行的结果
Integer result = ft.get();
System.out.println(result);
	｝
｝
```

# Thread的主要成员方法

1. String getName() 和 void setName(String name)
 * getName(): 返回线程的名称。
 * setName(String name): 设置线程的名称。
 示例：
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // 在run方法内部，我们想知道当前是哪个线程在运行
        // 这时就需要先获取当前线程对象，再获取其名称
        System.out.println("执行当前任务的线程是: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();

        // 创建Thread对象，并传入Runnable实例
        Thread thread1 = new Thread(myRunnable);
        thread1.setName("我的自定义线程-1"); // 使用setName方法设置名称
        thread1.start();

        // 也可以在创建时就指定名称
        Thread thread2 = new Thread(myRunnable, "我的自定义线程-2");
        thread2.start();

        // 输出主线程的名称
        System.out.println("主线程的名称是: " + Thread.currentThread().getName());
    }
}
```
2. static Thread currentThread()
 * 说明：
   这是一个静态方法，它返回对当前正在执行的线程对象的引用。这个方法非常重要，尤其是在使用 Runnable 接口时。因为 Runnable 的实现类自身并不是一个线程，所以当你想在 run() 方法内部控制或获取当前线程的信息（比如它的名字、优先级等）时，就必须通过 Thread.currentThread() 来获得这个线程对象。
示例：
参见上面 getName() 的例子，在 run() 方法中正是通过 Thread.currentThread() 来获取线程对象的。
3. static void sleep(long time)
 * 说明：
   这是一个静态方法，它会让当前正在执行的线程暂停指定的毫秒数（time）。线程在休眠时会释放 CPU 执行权，但它不会释放对象锁。这是一个受查异常方法，**调用它必须捕获 InterruptedException 异常**，因为其他线程可能会在当前线程休眠时中断它。
示例：
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " -> " + i);
            try {
                // 让当前线程休眠1秒 (1000毫秒)
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                System.out.println("线程被中断了！");
                // 恢复中断状态，或者直接返回
                Thread.currentThread().interrupt();
                return;
            }
        }
    }
}
```
4. setPriority(int newPriority) 和 int getPriority()
 * setPriority(int newPriority): 设置线程的优先级。
 * getPriority(): 获取线程的优先级。
说明：
Java 线程的优先级范围是 1 到 10，默认是 5。Thread 类提供了三个静态常量：
 * Thread.MIN_PRIORITY (值为 1)
 * Thread.NORM_PRIORITY (值为 5)
 * Thread.MAX_PRIORITY (值为 10)
设置较高的优先级只是向操作系统的线程调度器建议该线程应该被优先调度，高优先级的线程**不一定先于**低优先级的线程执行。
示例：
```java
// ... MyRunnable类的定义同上 ...

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable(), "高优先级线程");
        // 设置优先级为最高
        thread.setPriority(Thread.MAX_PRIORITY);
        thread.start();

        System.out.println("线程的优先级是: " + thread.getPriority());
    }
}
```
5. final void setDaemon(boolean on)
 * 说明：
   将线程设置为守护线程（Daemon Thread）。守护线程是一种在后台提供服务的线程，比如垃圾回收（GC）线程。它的一个关键特性是：当所有非守护线程（即用户线程）都结束时，JVM 会自动退出，而不会等待守护线程执行完毕。
示例：
```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread daemonThread = new Thread(() -> {
            int i = 0;
            while (true) { // 守护线程通常是无限循环
                System.out.println("我是守护线程..." + (i++));
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // 将其设置为守护线程
        daemonThread.setDaemon(true);
        daemonThread.start();

        // 主线程是用户线程
        System.out.println("主线程开始执行...");
        Thread.sleep(2000); // 主线程休眠2秒
        System.out.println("主线程执行结束，JVM即将退出。");
        // 主线程结束后，守护线程也会立刻终止，即使它还在while(true)循环中
    }
}
```
6. public static void join()
 * 说明：
   这是一个非常重要的方法，用于线程间的协调。当你在线程 A 中调用线程 B 的 join() 方法时，线程 A 会进入等待状态，直到线程 B 执行完毕。这可以确保某个任务必须在另一个任务完成后才能继续执行。它也有一个带参数的重载版本 join(long millis)，表示最多等待指定的毫秒数。
示例：
```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("主线程开始运行...");

        Thread workerThread = new Thread(() -> {
            System.out.println("工作线程开始处理任务...");
            try {
                Thread.sleep(3000); // 模拟耗时任务
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("工作线程任务处理完毕！");
        });

        workerThread.start();

        // 主线程等待工作线程结束
        System.out.println("主线程等待工作线程结束...");
        workerThread.join(); // 如果注释掉这行，下面这句会立刻打印

        System.out.println("工作线程已结束，主线程继续运行。");
    }
}
```


# 线程同步

为了解决这个问题，我们需要引入
synchronized 关键字就是 Java 中实现这把“锁”的最直接方式。

### 1. 同步代码块 (Synchronized Block)
同步代码块提供了更灵活的锁定方式。你可以精确地控制需要同步的代码范围，而不是锁定整个方法。
语法：
```java
synchronized (lockObject) {
    // 这部分代码是线程安全的
    // ...
}
```

 * lockObject：被称为锁对象或监视器对象。它可以是任何 Java 对象。
 * 工作原理：当一个线程执行到 synchronized 块时，它会尝试获取 lockObject 的锁。
   * 如果成功获取，它就执行花括号 {...} 里的代码。执行完毕后（无论是正常结束还是因异常退出），它会自动释放这把锁。
   * 如果获取失败（因为锁被其他线程持有），该线程就会进入阻塞状态，直到持有锁的线程释放锁，它才能再次尝试获取。

在这个例子中，多个线程（窗口）需要共享票数。因此，我们可以让这多个线程操作同一个 Runnable 实例，这个实例里包含了票数。锁对象就可以是这个共享的 Runnable 实例本身（this）。
```java
// 实现Runnable接口，代表售票任务
class TicketSeller implements Runnable {
    // 共享的票源，不需要static，因为我们会让所有线程共享同一个TicketSeller实例
    private int ticketCount = 100;
    
    // 创建一个锁对象，也可以直接用 this
    private final Object lock = new Object();

    @Override
    public void run() {
        while (true) {
            // 使用同步代码块
            // 所有线程都会争夺 lock 对象的锁
            synchronized (lock) { 
                if (ticketCount > 0) {
                    // 模拟出票的延迟
                    try {
                        Thread.sleep(10); 
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    
                    String windowName = Thread.currentThread().getName();
                    System.out.println(windowName + " 卖出了第 " + ticketCount + " 号票");
                    ticketCount--;
                } else {
                    System.out.println("票已售罄，" + Thread.currentThread().getName() + " 关闭。");
                    break; // 票卖完了，退出循环
                }
            } // 锁在这里被释放
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // 1. 创建一个共享的售票任务实例
        TicketSeller seller = new TicketSeller();

        // 2. 创建三个线程（三个窗口），都使用这同一个任务实例
        Thread window1 = new Thread(seller, "窗口1");
        Thread window2 = new Thread(seller, "窗口2");
        Thread window3 = new Thread(seller, "窗口3");

        // 3. 启动线程
        window1.start();
        window2.start();
        window3.start();
    }
}
```

分析：
在上面的代码中，ticketCount > 0 的判断和 ticketCount-- 的自减操作被包裹在 synchronized(lock) 块中。这保证了任何一个窗口（线程）在检查和修改票数时，其他窗口都不能进入这个代码块，从而避免了竞态条件。所有三个线程共享同一个 seller 对象，因此它们也共享同一个 lock 对象，同步得以生效。
### 2. 同步方法 (Synchronized Method)

同步方法是一种更简洁的同步方式，它直接在方法声明上使用 synchronized 关键字。
语法：
```java
public synchronized void myMethod() {
    // 整个方法体都是线程安全的
    // ...
}
```

可以先写同步代码块，然后再选中Synchronized中的代码按**Ctrl+Alt+m**自动生成同步方法
 * 工作原理：它本质上是一个语法糖，其锁定的对象是固定的。
   * 对于非静态（实例）同步方法，锁对象就是该方法所属的实例本身，即 this。
   * 对于静态同步方法，锁对象是该方法所属的类的 Class 对象（例如 TicketSeller.class）。
一个非静态同步方法等价于：
```java
public void myMethod() {
    synchronized(this) {
        // ...
    }
}
```

使用同步方法实现售票
我们可以将售票的逻辑封装在一个独立的同步方法中。
```java
class TicketSellerWithMethod implements Runnable {
    private int ticketCount = 100;

    @Override
    public void run() {
        while (ticketCount > 0) {
            sellOneTicket();
        }
    }

    // 将售票逻辑定义为一个同步方法
    // 这个方法的锁是 this，即 TicketSellerWithMethod 的实例
    private synchronized void sellOneTicket() {
        if (ticketCount > 0) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            
            String windowName = Thread.currentThread().getName();
            System.out.println(windowName + " 卖出了第 " + ticketCount + " 号票");
            ticketCount--;
        }
        // 当方法执行完毕时，锁会自动释放
    }
}


public class Main {
    public static void main(String[] args) {
        // 创建一个共享的任务实例
        TicketSellerWithMethod seller = new TicketSellerWithMethod();

        // 创建三个线程，共享同一个任务
        Thread window1 = new Thread(seller, "窗口1");
        Thread window2 = new Thread(seller, "窗口2");
        Thread window3 = new Thread(seller, "窗口3");

        window1.start();
        window2.start();
        window3.start();
    }
}
```
分析：
在这个版本中，run() 方法本身没有同步。但是它调用的 sellOneTicket() 方法是同步的。因为三个线程共享同一个 seller 对象，所以当任何一个线程调用 sellOneTicket() 时，它都会锁定这个 seller 对象。其他线程此时若想调用 sellOneTicket()，就必须等待，从而实现了和同步代码块版本完全相同的安全保证。