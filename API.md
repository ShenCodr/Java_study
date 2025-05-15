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

//runtime.availableProcessors();返回Java虚拟机可用的处理及数量

//runtime.freeMemory(): 返回 Java 虚拟机中的可用内存量（以字节为单位）。

//runtime.totalMemory(): 返回 Java 虚拟机中的内存总量（以字节为单位）。

//runtime.maxMemory(): 返回 Java 虚拟机试图使用的最大内存量（以字节为单位）。

Runtime runtime = Runtime.getRuntime();
long freeMemory = runtime.freeMemory();
long totalMemory = runtime.totalMemory();
long maxMemory = runtime.maxMemory();
System.out.println("Available processors:"+runtime.availableProcessors());
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

## Objects
需导入java.lang.Objects

```Java
Objects.isNull(object obj)//判断对象是否为null
String s1="hello";
java.util.objects.isNull(s1);//true

Objects.equals(object a,object b)
String s1="Hello";
String s2="ShenCodr";
String s3="Hello";
java.util.objects.equals(s1,s2);//false
java.util.objects.equals(s1,s3);//true

```

## BigInteger
理论上，只要内存允许，BigInteger 可以表示任意大小的整数。

**不可变性 (Immutability)：** BigInteger 对象一旦创建，其值就不能被改变。所有对其进行运算的方法（如 add, multiply）都会返回一个新的 BigInteger 对象，而不是修改原始对象。

创建：

```java
BigInteger b1 = new BigInteger("123456789012345678901234567890");
BigInteger b2 = new BigInteger("-98765432109876543210");
BigInteger b3 = BigInteger.valueOf(100);//int转BigInteger
```

常见用法：
```Java
算术运算：

add(BigInteger val): 加法
subtract(BigInteger val): 减法
multiply(BigInteger val): 乘法
divide(BigInteger val): 除法 (结果为整数，舍弃小数)
remainder(BigInteger val): 取余
mod(BigInteger m): 取模 (结果始终为非负数，如果 m 为负数会抛异常)
pow(int exponent): 幂运算
negate(): 取反
abs(): 绝对值
divideAndRemainder(BigInteger val): 返回一个包含商和余数的 BigInteger 数组。


比较运算：

compareTo(BigInteger val): 比较大小。返回 -1 (小于), 0 (等于), 1 (大于)。
equals(Object x): 判断是否相等 (值必须完全相同)。
min(BigInteger val): 返回较小值。
max(BigInteger val): 返回较大值。
位运算：
and(BigInteger val)
or(BigInteger val)
xor(BigInteger val)
not()
shiftLeft(int n)
shiftRight(int n)
testBit(int n): 测试指定位是否为1。
setBit(int n): 设置指定位为1。
clearBit(int n): 清除指定位为0。
flipBit(int n): 翻转指定位。


转换：

intValue(), longValue(), floatValue(), doubleValue(): 转换为基本类型，可能会有精度损失或溢出。
toString(): 转换为十进制字符串。
toString(int radix): 转换为指定进制的字符串。
其他：
gcd(BigInteger val): 最大公约数。
isProbablePrime(int certainty): 以一定的概率判断是否为素数。
nextProbablePrime(): 返回大于当前值的下一个可能的素数。
```
**示例：**
```Java
BigInteger a = new BigInteger("12345678901234567890");
BigInteger b = new BigInteger("9876543210987654321");

BigInteger sum = a.add(b);
BigInteger product = a.multiply(b);
BigInteger quotient = a.divide(b);
BigInteger remainder = a.remainder(b);

System.out.println("a = " + a);
System.out.println("b = " + b);
System.out.println("a + b = " + sum);
System.out.println("a * b = " + product);
System.out.println("a / b = " + quotient); // 整数除法
System.out.println("a % b = " + remainder);

System.out.println("Is a > b? " + (a.compareTo(b) > 0)); // true
```

## BigDecimal

核心特性：
不可变性 (Immutability)： 与 BigInteger 类似，BigDecimal 对象也是不可变的。
任意精度： 可以表示具有任意小数位数的十进制数。
精确的十进制算术： 所有运算都基于十进制，避免二进制浮点数的舍入误差。
标度 (Scale)： 表示小数点后的位数。
舍入模式 (RoundingMode)： 在进行除法或调整标度时，可以指定如何处理多余的小数位

**创建：**
```java
通过字符串 (推荐)： 这是最精确和推荐的方式，因为它直接反映了你想要的十进制值。
BigDecimal bd1 = new BigDecimal("123.456");
BigDecimal bd2 = new BigDecimal("0.1");

通过 BigInteger 和标度：
BigInteger unscaledVal = new BigInteger("12345");
int scale = 2;
BigDecimal bd8 = new BigDecimal(unscaledVal, scale); // 结果是 123.45
```

常用方法：
```java
算术运算：

add(BigDecimal augend): 加法
subtract(BigDecimal subtrahend): 减法
multiply(BigDecimal multiplicand): 乘法
divide(BigDecimal divisor): 除法。如果结果是无限循环小数且未指定舍入模式，会抛出 ArithmeticException。
divide(BigDecimal divisor, int scale, RoundingMode roundingMode): 指定标度和舍入模式的除法。非常重要！
divide(BigDecimal divisor, RoundingMode roundingMode): 指定舍入模式的除法，标度由 this.scale() 决定。
pow(int n): 幂运算。
negate(): 取反。
abs(): 绝对值。


比较运算：

compareTo(BigDecimal val): 比较数值大小，忽略标度。返回 -1, 0, 1。这是进行数值比较的首选方法。
equals(Object x): 判断是否相等。注意：equals() 会同时比较值和标度 (scale)。 例如 new BigDecimal("2.0") 和 new BigDecimal("2.00") 的 equals() 返回 false，因为它们的标度不同，但 compareTo() 返回 0。
标度 (Scale) 和精度 (Precision)：
scale(): 返回此 BigDecimal 的标度。
precision(): 返回此 BigDecimal 的精度 (有效数字的总位数)。
setScale(int newScale): 设置新的标度，如果需要舍入，可能抛出 ArithmeticException。
setScale(int newScale, RoundingMode roundingMode): 设置新的标度，并指定舍入模式。常用！
舍入 (Rounding)：
RoundingMode 是一个枚举，定义了多种舍入策略：
UP: 向远离零的方向舍入。
DOWN: 向零的方向舍入 (截断)。
CEILING: 向正无穷方向舍入。
FLOOR: 向负无穷方向舍入。
HALF_UP: 四舍五入 (经典)。
HALF_DOWN: 五舍六入。
HALF_EVEN: 银行家舍入法 (向最接近的偶数舍入)。
UNNECESSARY: 断言请求的操作具有精确结果，因此不需要舍入。如果需要舍入，则抛出 ArithmeticException。

转换：


intValue(), longValue(), floatValue(), doubleValue(): 转换为基本类型，可能精度损失或溢出。
toString(): 转换为字符串，可能使用科学计数法 (如果指数很大)。
toPlainString(): 转换为不带指数的字符串表示。
```
**示例：**
```java
BigDecimal num1 = new BigDecimal("0.1");
BigDecimal num2 = new BigDecimal("0.2");
BigDecimal sum = num1.add(num2);
System.out.println("0.1 + 0.2 (BigDecimal) = " + sum); // 输出 0.3

BigDecimal price = new BigDecimal("19.99");
BigDecimal quantity = new BigDecimal("3");
BigDecimal total = price.multiply(quantity);
System.out.println("Total price: " + total); // 输出 59.97

BigDecimal dividend = new BigDecimal("10");
BigDecimal divisor = new BigDecimal("3");

// BigDecimal quotient = dividend.divide(divisor); // 会抛 ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.

BigDecimal quotientRounded = dividend.divide(divisor, 2, RoundingMode.HALF_UP);
System.out.println("10 / 3 (rounded to 2 decimal places): " + quotientRounded); // 输出 3.33

BigDecimal val1 = new BigDecimal("2.0");
BigDecimal val2 = new BigDecimal("2.00");
System.out.println("val1.equals(val2): " + val1.equals(val2));         // false (scale不同)
System.out.println("val1.compareTo(val2) == 0: " + (val1.compareTo(val2) == 0)); // true (数值相同)

BigDecimal amount = new BigDecimal("123.4567");
// 保留两位小数，四舍五入
BigDecimal roundedAmount = amount.setScale(2, RoundingMode.HALF_UP);
System.out.println("Rounded amount: " + roundedAmount); // 输出 123.46
System.out.println("Plain string: " + new BigDecimal("1.23E+10").toPlainString()); // 12300000000
toBigInteger(): 转换为 BigInteger (小数部分被丢弃)。
```


## time（了解即可）

```Java
LocalDate:
表示一个不带时间和时区的日期（年-月-日）。例如：2023-10-27。
用于表示生日、纪念日等只关心日期的场景。
LocalTime:
表示一个不带日期和时区的时间（时-分-秒-纳秒）。例如：10:15:30.123456789。
用于表示营业时间、事件发生时间等只关心时间的场景。
LocalDateTime:
表示一个不带时区的日期和时间。它是 LocalDate 和 LocalTime 的组合。例如：2023-10-27T10:15:30。
用于表示特定事件的本地发生时间，不考虑时区差异。
Instant:
表示时间线上的一个瞬时点，精确到纳秒。它通常代表自 1970-01-01T00:00:00Z (UTC) 以来的秒数和纳秒数。
与 java.util.Date 的概念最接近，但不可变且精度更高。非常适合用于记录事件时间戳、进行时间比较等。
ZonedDateTime:
表示一个带有时区的日期和时间。它将 LocalDateTime 与一个 ZoneId (时区ID) 结合起来。
用于处理需要明确时区的场景，例如跨国会议安排、国际航班时间等。
ZoneId 和 ZoneOffset:
ZoneId: 表示一个时区标识符，例如 "Asia/Shanghai" 或 "Europe/Paris"。它包含了处理夏令时等复杂规则。
ZoneOffset: 表示格林威治/UTC 时间的固定偏移量，例如 "+08:00"。
Duration:
表示以秒和纳秒为单位的时间量（时间跨度）。用于计算两个 Instant、LocalTime 或 LocalDateTime 之间的时间差。
例如："3小时20分钟"。
Period:
表示以年、月、日为单位的日期量（日期跨度）。用于计算两个 LocalDate 之间的时间差。
例如："2年3个月5天"。
DateTimeFormatter:
用于格式化日期时间对象为字符串，以及将字符串解析为日期时间对象。
它是线程安全的，可以安全地在多线程环境中使用。
提供了预定义的格式器，也支持自定义模式。
TemporalAdjusters:
一个工具类，提供了一系列有用的静态方法来调整日期时间对象，例如获取"当月的第一天"、"下一个星期二"等。
```
**示例：**
```Java
import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.ChronoUnit;
import java.time.temporal.TemporalAdjusters;
import java.util.Date; // 仅用于与旧API转换示例

public class JavaTimeExamples {

    public static void main(String[] args) {
        // --- 1. 创建日期和时间对象 ---
        System.out.println("--- 创建对象 ---");
        LocalDate today = LocalDate.now(); // 当前日期，例如：2023-10-27
        System.out.println("LocalDate.now(): " + today);

        LocalTime currentTime = LocalTime.now(); // 当前时间，例如：14:30:55.123456789
        System.out.println("LocalTime.now(): " + currentTime);

        LocalDateTime currentDateTime = LocalDateTime.now(); // 当前日期和时间
        System.out.println("LocalDateTime.now(): " + currentDateTime);

        Instant timestamp = Instant.now(); // 当前UTC时间戳
        System.out.println("Instant.now(): " + timestamp);


        // --- 2. 获取日期/时间的各个部分 ---
        System.out.println("\n--- 获取部分 ---");
        System.out.println("Year: " + today.getYear()); // 2023
        System.out.println("Month (enum): " + today.getMonth()); // OCTOBER
        System.out.println("Month (value): " + today.getMonthValue()); // 10
        System.out.println("Day of month: " + today.getDayOfMonth()); // 27
        System.out.println("Day of week: " + today.getDayOfWeek()); // FRIDAY
        System.out.println("Day of year: " + today.getDayOfYear()); // 300 (例如)

        System.out.println("Hour: " + currentTime.getHour());
        System.out.println("Minute: " + currentTime.getMinute());
        System.out.println("Second: " + currentTime.getSecond());
        System.out.println("Nano: " + currentTime.getNano());

        // --- 3. 日期/时间计算 (不可变性，返回新对象) ---
        System.out.println("\n--- 日期计算 ---");
        LocalDate tomorrow = today.plusDays(1);
        System.out.println("Tomorrow: " + tomorrow);
        LocalDate lastWeek = today.minusWeeks(1);
        System.out.println("Last week: " + lastWeek);
        LocalDateTime twoHoursLater = currentDateTime.plusHours(2);
        System.out.println("Two hours later: " + twoHoursLater);

        // 使用 withXxx() 修改字段 (返回新对象)
        LocalDate firstDayOfMonth = today.withDayOfMonth(1);
        System.out.println("First day of this month: " + firstDayOfMonth);
        LocalTime atNoon = currentTime.withHour(12).withMinute(0).withSecond(0).withNano(0);
        System.out.println("This time at noon: " + atNoon);

        // --- 4. 格式化 (Date/Time -> String) 和解析 (String -> Date/Time) ---
        System.out.println("\n--- 格式化与解析 ---");
        DateTimeFormatter formatter1 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String formattedDateTime = currentDateTime.format(formatter1);
        System.out.println("Formatted (yyyy-MM-dd HH:mm:ss): " + formattedDateTime);

        DateTimeFormatter formatter2 = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        String formattedDate = today.format(formatter2);
        System.out.println("Formatted (dd/MM/yyyy): " + formattedDate);

        // 使用预定义的格式器
        DateTimeFormatter isoDateFormatter = DateTimeFormatter.ISO_DATE;
        System.out.println("Formatted (ISO_DATE): " + today.format(isoDateFormatter));

        DateTimeFormatter localizedFormatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.FULL);
        System.out.println("Formatted (Localized FULL): " + today.format(localizedFormatter)); // 依赖区域设置

        // 解析
        String dateStr = "2023-11-15";
        LocalDate parsedDate = LocalDate.parse(dateStr); // 默认使用 ISO_LOCAL_DATE 格式
        System.out.println("Parsed (default ISO): " + parsedDate);

        String dateTimeStr = "2024/05/20 10:30";
        DateTimeFormatter customParser = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm");
        LocalDateTime parsedDateTime = LocalDateTime.parse(dateTimeStr, customParser);
        System.out.println("Parsed (custom yyyy/MM/dd HH:mm): " + parsedDateTime);

        // --- 5. 时区处理 (ZonedDateTime, ZoneId, ZoneOffset) ---
        System.out.println("\n--- 时区处理 ---");
        ZoneId shanghaiZone = ZoneId.of("Asia/Shanghai");
        ZoneId newYorkZone = ZoneId.of("America/New_York");
        ZoneId systemDefaultZone = ZoneId.systemDefault();

        System.out.println("System default zone: " + systemDefaultZone);

        ZonedDateTime shanghaiTime = ZonedDateTime.now(shanghaiZone);
        System.out.println("Current time in Shanghai: " + shanghaiTime);

        ZonedDateTime newYorkTime = ZonedDateTime.now(newYorkZone);
        System.out.println("Current time in New York: " + newYorkTime);

        // 将本地时间转换为特定时区的时间
        LocalDateTime localEventTime = LocalDateTime.of(2024, 1, 1, 9, 0); // 假设是北京时间
        ZonedDateTime beijingEventTime = localEventTime.atZone(shanghaiZone);
        System.out.println("Event in Beijing: " + beijingEventTime);

        // 转换到另一个时区，保持同一瞬间
        ZonedDateTime newYorkEventTime = beijingEventTime.withZoneSameInstant(newYorkZone);
        System.out.println("Same event in New York: " + newYorkEventTime);

        // Instant 与 ZonedDateTime 转换
        Instant eventInstant = beijingEventTime.toInstant();
        System.out.println("Event as Instant (UTC): " + eventInstant);
        ZonedDateTime fromInstant = eventInstant.atZone(ZoneId.of("Europe/London"));
        System.out.println("Instant converted to London time: " + fromInstant);

        // ZoneOffset
        ZoneOffset offsetPlus8 = ZoneOffset.of("+08:00");
        OffsetDateTime offsetDateTime = OffsetDateTime.now(offsetPlus8);
        System.out.println("Current time with +08:00 offset: " + offsetDateTime);

        // --- 6. 计算时间/日期差 (Duration, Period, ChronoUnit) ---
        System.out.println("\n--- 时间差计算 ---");
        LocalTime time1 = LocalTime.of(10, 0);
        LocalTime time2 = LocalTime.of(12, 30, 15);
        Duration duration = Duration.between(time1, time2);
        System.out.println("Duration between " + time1 + " and " + time2 + ": " + duration); // PT2H30M15S
        System.out.println("Duration in minutes: " + duration.toMinutes()); // 150
        System.out.println("Duration in seconds: " + duration.getSeconds()); // 9015

        LocalDate date1 = LocalDate.of(2023, 1, 1);
        LocalDate date2 = LocalDate.of(2024, 3, 15);
        Period period = Period.between(date1, date2);
        System.out.println("Period between " + date1 + " and " + date2 + ": " + period); // P1Y2M14D
        System.out.println("Period in months: " + period.toTotalMonths()); // 14
        System.out.println("Period years: " + period.getYears() + ", months: " + period.getMonths() + ", days: " + period.getDays());

        // ChronoUnit (更通用的单位计算)
        long daysBetween = ChronoUnit.DAYS.between(date1, date2);
        System.out.println("Days between (ChronoUnit): " + daysBetween);
        long hoursBetween = ChronoUnit.HOURS.between(
                LocalDateTime.of(date1, LocalTime.MIDNIGHT),
                LocalDateTime.of(date2, LocalTime.NOON)
        );
        System.out.println("Hours between (ChronoUnit): " + hoursBetween);

        
```





























