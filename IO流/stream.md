# stream

Java8引入，极大地简化了对集合（以及其他数据源）的处理和操作，并支持函数式编程风格。

可以把它想象成一条**数据处理流水线**，数据在这条流水线上经过一系列中间操作，最终产生一个结果或副作用。



**核心特性：**

1. **元素序列 (Sequence of elements)**：Stream提供了一种对数据源中元素进行操作的方式，这些元素会按顺序（或并行地）被处理。
2. **数据源 (Source)**：Stream从数据源获取数据，例如 Collection、Array、I/O channel 或生成器函数。
3. **聚合操作 (Aggregate operations)**：Stream支持类似SQL语句中的 FILTER、MAP、REDUCE、FIND、MATCH、SORT 等操作，这些操作可以被链式调用，形成一个流水线。
4. **流水线 (Pipelining)**：许多Stream操作本身会返回一个新的Stream。这允许操作被链接起来，形成一个更长的流水线。这使得代码更具可读性，并且可以进行某些优化（如延迟计算和短路）。
5. **内部迭代 (Internal iteration)**：与集合的外部迭代（如 for-each 循环或显式使用 Iterator）不同，Stream API使用内部迭代。你只需要声明要做什么，而不是如何去做。
6. **惰性求值 (Lazy evaluation)**：很多中间操作（如 filter, map）是惰性的，它们不会立即执行。只有当一个**终端操作**被调用时，整个流水线才会开始处理数据。这允许进行优化，例如，如果只需要找到第一个满足条件的元素，那么在找到后就不会处理剩余的元素。
7. **一次性消费 (Consumable)**：一个Stream通常只能被消费（即遍历）一次。一旦一个终端操作被调用，该Stream就被认为是“已消费”的，不能再被重用。如果需要再次操作相同的数据源，需要从源头重新创建一个新的Stream。
8. **可以并行化 (Parallelizable)**：Stream API可以轻松地在多核处理器上并行执行操作，只需将 stream() 替换为 parallelStream()，或者在现有流上调用 parallel() 方法。



## 创建

**1.从集合创建**:

```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream(); // 创建顺序流
```

**2.从数组创建**:

```java
String[] array = {"x", "y", "z"};
Stream<String> streamFromArray = Arrays.stream(array);
Stream<String> streamFromValues = Stream.of("x", "y", "z"); // 也可以直接用值创建
Stream<Integer> streamFromInts = Stream.of(1, 2, 3);
```

**3.基本类型流**:
Java 8 为 int, long, double 提供了专门的流：IntStream, LongStream, DoubleStream。这些流避免了自动装箱/拆箱的开销，效率更高。

```java
IntStream intStream = IntStream.range(1, 5); // 1, 2, 3, 4 (不包含5)
LongStream longStream = LongStream.rangeClosed(1, 5); // 1, 2, 3, 4, 5 (包含5)
DoubleStream doubleStream = new Random().doubles(3); // 生成3个随机double
```



## 常见的中间操作



**1.filter(Predicate<T> predicate)**:
接收一个 Predicate 函数式接口（一个返回 boolean 的函数），用于筛选出满足条件的元素。

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");
names.stream()
     .filter(name -> name.startsWith("A") || name.startsWith("E"))
     .forEach(System.out::println); // Alice, Eve
```

**2.map(Function<T, R> mapper)**:
接收一个 Function 函数式接口，将流中的每个元素 T 转换为另一个类型的元素 R（或同类型但值不同）。

```java
List<String> words = Arrays.asList("hello", "world");
words.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println); // HELLO, WORLD

List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.stream()
       .map(n -> n * n)
       .forEach(System.out::print); // 1 4 9 16 25
```

**3.distinct()**:
返回一个由流中不重复元素组成的流（基于元素的 equals() 方法）。

```java
List<Integer> numsWithDuplicates = Arrays.asList(1, 2, 2, 3, 4, 4, 5);
numsWithDuplicates.stream()
                  .distinct()
                  .forEach(System.out::print); // 12345
```

**4.sorted() / sorted(Comparator<T> comparator)**:
对流中的元素进行排序。无参版本使用自然顺序，带参版本使用指定的比较器。

```java
List<String> unsortedNames = Arrays.asList("Eve", "Charlie", "Alice", "Bob");
unsortedNames.stream()
             .sorted()
             .forEach(System.out::println); // Alice, Bob, Charlie, Eve

unsortedNames.stream()
             .sorted(Comparator.reverseOrder())
             .forEach(System.out::println); // Eve, Charlie, Bob, Alice

List<Person> people = Arrays.asList(new Person("Alice", 30), new Person("Bob", 25));
people.stream()
      .sorted(Comparator.comparingInt(Person::getAge))//getAge需要自己实现
      .forEach(p -> System.out.println(p.getName())); // Bob, Alice
```

 **5.limit **

返回一个不超过`maxSize`个元素的新流。若原始流元素不足，则保留所有元素。

```java
Stream.of(1, 2, 3, 4, 5, 6)
    .limit(4)  // 取前4个元素
    .forEach(System.out::print);  // 输出：1234
```

**6.skip**

 跳过前`n`个元素，返回剩余元素的流。若流中元素不足`n`个，返回空流。

```java
Stream.of(1, 2, 3, 4, 5)
    .skip(2)  // 跳过前2个元素
    .forEach(System.out::print);  // 输出：345
```



## 常用的终端操作

**1.forEach(Consumer<T> action) / forEachOrdered(Consumer<T> action)**:
对流中的每个元素执行提供的 action。forEach 不保证顺序（尤其在并行流中），而 forEachOrdered 会按流中元素的遭遇顺序执行。

```java
List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");
fruits.stream().forEach(System.out::println);
```

**2.toArray() / toArray(IntFunction<A[]> generator)**:
将流中的元素收集到一个数组中。

```java
String[] fruitArray = fruits.stream().toArray(String[]::new);
// String[]::new 是一个构造函数引用，等价于 size -> new String[size]
```

**3.reduce(T identity, BinaryOperator<T> accumulator)**:
将流中的元素组合起来，生成一个单一的值。identity是初始值，accumulator 是一个二元操作，用于合并两个值。

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream().reduce(0, (a, b) -> a + b); // 15,每个相加

// 无初始值的reduce，返回Optional
Optional<Integer> product = numbers.stream().reduce((a, b) -> a * b);
product.ifPresent(System.out::println); // 120，每个相乘
```

**4.collect(Collector<T, A, R> collector)**:

非常通用的终端操作，可以将流中的元素收集到各种数据结构中（如 List, Set, Map）或进行其他复杂的汇总操作。java.util.stream.Collectors 类提供了许多预定义的收集器。

- **Collectors.toList() / Collectors.toSet()**:

  ```java
  List<String> upperFruits = fruits.stream().map(String::toUpperCase).collect(Collectors.toList());
  Set<String> uniqueFruits = fruits.stream().collect(Collectors.toSet());//fruits为同类型集合
  ```

- **Collectors.toMap(keyMapper, valueMapper)**:

```java
Map<String, Integer> nameLengthMap = fruits.stream()
    .collect(Collectors.toMap(
        Function.identity(), // 键是水果名本身
        String::length,      // 值是水果名的长度
        (oldValue, newValue) -> oldValue // 处理重复键的合并函数 (可选)
    ));
// {"Apple"=5, "Banana"=6, "Cherry"=6}
```

- **Collectors.toCollection(Supplier<C> collectionFactory)**: 收集到指定类型的集合。

```java
TreeSet<String> sortedFruitSet = fruits.stream()
        .collect(Collectors.toCollection(TreeSet::new));
System.out.println("Sorted Fruit Set (TreeSet): " + sortedFruitSet);
// Output: Sorted Fruit Set (TreeSet): [Apple, Banana, Blueberry, Cherry]
```

- **Collectors.joining()/joining(CharSequence delimiter) **: 将Stream中的String元素连接成一个字符串。（用delimiter隔开）

  ```java
  //fruits为集合，通常为list
  String joinedFruits = fruits.stream().distinct().collect(Collectors.joining());
  System.out.println("Joined Fruits (no delimiter): " + joinedFruits); // AppleBananaCherryBlueberry (去重后)
  String joinedFruits = fruits.stream().distinct().collect(Collectors.joining(","));
  // Apple, Banana, Cherry, Blueberry
  ```

**5.计算 (Aggregating)**

- **Collectors.counting()**: 计算Stream中元素的数量 (返回 Long)。

  ```java
  long countPeople = people.stream().collect(Collectors.counting());
  System.out.println("Number of people: " + countPeople); // 5
  // 等价于: long countPeople = people.stream().count();
  ```

- **Collectors.averagingInt(ToIntFunction mapper) / averagingLong / averagingDouble**:

```java
double averageAge = people.stream().collect(Collectors.averagingInt(Person::getAge));
System.out.println("Average age: " + averageAge); // 26.4
```