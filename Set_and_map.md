# Set
**Set 的核心特性：**
无重复元素： 这是 Set 最主要的特点。元素的唯一性通常通过元素的 equals() 和 hashCode() 方法来判断（对于 HashSet 和 LinkedHashSet），或者通过 Comparable 或 Comparator 来判断（对于 TreeSet）。
通常无序： 大多数 Set 实现不保证元素的顺序，但某些特定实现（如 LinkedHashSet 和 TreeSet）可以。

### 1.HashSet<E>
```Java
import java.util.HashSet;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        Set<String> names = new HashSet<>();

        // 添加元素
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");
        System.out.println("Added Alice: " + names.add("Alice")); // false, Alice已存在
        names.add("David");
        names.add(null); // 可以添加一个 null
        names.add(null); // 再次添加 null，返回 false

        System.out.println("Set: " + names); // 输出顺序不确定，例如：[null, Alice, David, Bob, Charlie]

        // 检查是否包含元素
        System.out.println("Contains Bob? " + names.contains("Bob")); // true
        System.out.println("Contains Eve? " + names.contains("Eve")); // false

        // 移除元素
        names.remove("David");
        System.out.println("After removing David: " + names);

        // 遍历 (顺序不保证)
        System.out.println("Iterating over HashSet:");
        for (String name : names) {
            System.out.println(name);
        }
    }
}
```

### 2.LinkedHashSet<E>
与HashSet类似，只不过变为链表链接，稍微多了一些开销

**有序（插入顺序）**： 元素按照它们被添加到集合中的顺序进行迭代


### 3. TreeSet<E>
**特点：**

**底层数据结构：** 基于红黑树（一种自平衡的二叉搜索树，实际上内部使用了 TreeMap）。

**有序（排序顺序）：** 元素总是处于排序状态。排序方式取决于：

**自然排序：** 元素类实现了 java.lang.Comparable 接口，并重写了 compareTo() 方法。
**自定义排序：** 在创建 TreeSet 时提供一个 java.util.Comparator 对象。

**性能：** 对于 add, remove, contains 操作，时间复杂度为**O(log n)。**

不允许 null 元素（默认情况）： 如果使用自然排序，添加 null 会抛出 NullPointerException，因为 null 无法与非 null 对象比较。如果使用 Comparator，则取决于 Comparator 是否能处理 null。

依赖 Comparable 或 Comparator： 元素的唯一性是通过 compareTo() (或 compare()) 方法的返回值为0来判断的，而不是 equals()。如果 compareTo() 返回0，则认为元素是相同的。

**示例（自然排序）：**
```Java
import java.util.TreeSet;
import java.util.Set;

public class TreeSetNaturalOrderExample {
    public static void main(String[] args) {
        Set<Integer> numbers = new TreeSet<>(); // Integer 实现了 Comparable
        numbers.add(50);
        numbers.add(20);
        numbers.add(80);
        numbers.add(20); // 不会重复添加
        // numbers.add(null); // 会抛出 NullPointerException

        System.out.println("TreeSet (natural order): " + numbers); // 输出: [20, 50, 80] (升序)

        Set<String> sortedStrings = new TreeSet<>();
        sortedStrings.add("Banana");
        sortedStrings.add("Apple");
        sortedStrings.add("Cherry");
        System.out.println("TreeSet (strings): " + sortedStrings); // 输出: [Apple, Banana, Cherry] (字典序)
    }
}
```

**示例（自定义排序）：**
```Java
import java.util.TreeSet;
import java.util.Set;
import java.util.Comparator;

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }

    // 为了 HashSet/LinkedHashSet 的正确行为，通常也需要 equals 和 hashCode
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && java.util.Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return java.util.Objects.hash(name, age);
    }
}

public class TreeSetComparatorExample {
    public static void main(String[] args) {
        // 按年龄升序排序，如果年龄相同，按名字字典序升序
        Comparator<Person> ageThenNameComparator = Comparator
                .comparingInt((Person p) -> p.age)
                .thenComparing(p -> p.name);

        Set<Person> people = new TreeSet<>(ageThenNameComparator);
        people.add(new Person("Alice", 30));
        people.add(new Person("Bob", 25));
        people.add(new Person("Charlie", 30));
        people.add(new Person("David", 25));

        System.out.println("TreeSet (custom comparator):");
        for (Person person : people) {
            System.out.println(person);
        }
        // 输出 (按年龄升序，年龄相同按名字升序):
        // Person{name='Bob', age=25}
        // Person{name='David', age=25}
        // Person{name='Alice', age=30}
        // Person{name='Charlie', age=30}
    }
}
```

# Map

**Map 的核心特性：**

**键值对存储：** 数据以 (key, value) 的形式存储。

**键唯一：** Map 中不能有重复的键。如果尝试用一个已存在的键 put 一个新的值，旧的值会被覆盖。

**值可重复：** 不同的键可以映射到相同的值。

### 1. HashMap<K, V>
**特点：**

**底层数据结构：** 基于哈希表（数组 + 链表/红黑树）。当链表长度超过一定阈值（默认为8）且数组长度达到一定大小（默认为64）时，链表会转换为红黑树以提高查询效率。

**无序：** 不保证键值对的迭代顺序。

**高效：** 对于 put, get, remove 操作，平均时间复杂度为 O(1)。

允许 null 键和 null 值： 最多允许一个 null 键，可以有多个 null 值。

**依赖键的 hashCode() 和 equals()：** 与 HashSet 类似，键的唯一性和查找依赖这两个方法。如果使用自定义对象作为键，务必正确重写。

**示例:**
```Java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class HashMapExample {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();

        // 添加键值对
        scores.put("Alice", 90);
        scores.put("Bob", 85);
        scores.put("Charlie", 95);
        scores.put("Alice", 92); // "Alice" 的值被更新为 92
        scores.put(null, 70);    // null 键
        scores.put("David", null); // null 值

        System.out.println("HashMap: " + scores); // 输出顺序不确定

        // 获取值
        System.out.println("Bob's score: " + scores.get("Bob")); // 85
        System.out.println("Eve's score: " + scores.get("Eve")); // null

        // 检查键/值是否存在
        System.out.println("Contains key 'Charlie'? " + scores.containsKey("Charlie")); // true
        System.out.println("Contains value 90? " + scores.containsValue(90));       // false (Alice's score is 92)

        // 移除
        scores.remove("Bob");
        System.out.println("After removing Bob: " + scores);

        // 遍历方法1: 遍历 keySet
        System.out.println("\nIterating using keySet():");
        for (String key : scores.keySet()) {
            System.out.println("Key: " + key + ", Value: " + scores.get(key));
        }

        // 遍历方法2: 遍历 entrySet (推荐，效率更高，因为不需要再次get)
        System.out.println("\nIterating using entrySet():");
        for (Map.Entry<String, Integer> entry : scores.entrySet()) {
            System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
        }

        // 遍历方法3: 遍历 values (如果只需要值)
        System.out.println("\nIterating using values():");
        for (Integer value : scores.values()) {
            System.out.println("Value: " + value);
        }

        // Java 8+ forEach
        System.out.println("\nIterating using forEach (Java 8+):");
        scores.forEach((key, value) -> System.out.println("Key: " + key + ", Value: " + value));
    }
}
```

### 2. LinkedHashMap<K, V>
**特点：**

**底层数据结构：** 基于哈希表和双向链表。它继承自 HashMap，并维护了一个双向链表来记录键值对的插入顺序或访问顺序。

**有序（插入顺序或访问顺序）：**

默认情况下，迭代顺序是插入顺序。

可以配置为访问顺序（通过构造函数 LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder)，将 accessOrder 设为 true）。访问顺序对于实现 LRU (Least Recently Used) 缓存很有用。

高效： 性能与 HashMap 基本相同。

允许 null 键和 null 值： 与 HashMap 相同。

依赖键的 hashCode() 和 equals()： 与 HashMap 相同。

**示例:**
```Java
import java.util.LinkedHashMap;
import java.util.Map;

public class LinkedHashMapExample {
    public static void main(String[] args) {
        Map<String, Integer> lruCache = new LinkedHashMap<>(16, 0.75f, true); // accessOrder = true for LRU

        // 插入顺序
        Map<Integer, String> orderedMap = new LinkedHashMap<>();
        orderedMap.put(3, "Three");
        orderedMap.put(1, "One");
        orderedMap.put(2, "Two");
        orderedMap.put(1, "Uno"); // 更新key 1的值

        System.out.println("LinkedHashMap (insertion order): " + orderedMap);
        // 输出: {3=Three, 1=Uno, 2=Two} (按插入顺序，1的值被更新了但位置不变)

        // 访问顺序示例
        lruCache.put("A", 1);
        lruCache.put("B", 2);
        lruCache.put("C", 3);
        System.out.println("LRU Cache initial: " + lruCache); // {A=1, B=2, C=3}

        lruCache.get("A"); // 访问 A
        System.out.println("LRU Cache after accessing A: " + lruCache); // {B=2, C=3, A=1} (A移到末尾)

        lruCache.put("D", 4); // 添加 D
        System.out.println("LRU Cache after adding D: " + lruCache); // {B=2, C=3, A=1, D=4}
    }
}
```

### 3. TreeMap<K, V>

**特点：**
底层数据结构： 基于红黑树。

有序（按键排序）： 键值对总是根据键的自然顺序或构造时提供的 Comparator 进行排序。

性能： 对于 put, get, remove 操作，时间复杂度为 **O(log n)**。

键不允许 null (默认情况)： 如果使用自然排序，键不能为 null。如果使用 Comparator，取决于其是否能处理 null 键。值可以为 null。

依赖键的 Comparable 或 Comparator： 与 TreeSet 类似，键的排序和唯一性依赖于此。

**示例:**
```Java
import java.util.TreeMap;
import java.util.Map;
import java.util.Comparator;

public class TreeMapExample {
    public static void main(String[] args) {
        // 自然排序 (按键的字典序)
        Map<String, String> sortedMap = new TreeMap<>();
        sortedMap.put("C_Language", ".c");
        sortedMap.put("Java", ".java");
        sortedMap.put("Python", ".py");
        sortedMap.put("A_Assembly", ".asm"); // 为了演示排序

        System.out.println("TreeMap (natural key order): " + sortedMap);
        // 输出: {A_Assembly=.asm, C_Language=.c, Java=.java, Python=.py}

        // 自定义排序 (按键的长度排序)
        Map<String, Integer> lengthSortedMap = new TreeMap<>(Comparator.comparingInt(String::length));
        lengthSortedMap.put("One", 1);
        lengthSortedMap.put("Three", 3);
        lengthSortedMap.put("Two", 2);
        lengthSortedMap.put("Four", 4); // "Four" 和 "Five" 长度一样， TreeMap 会保留先添加的，或根据更细致的比较器
        lengthSortedMap.put("Five", 5); // 如果只按长度比较，"Four"和"Five"长度相同，后一个会覆盖前一个，如果比较器认为它们相等

        System.out.println("TreeMap (custom key order - by length): " + lengthSortedMap);
        // 输出 (取决于Comparator对于等长字符串的处理，这里只按长度，所以后面的同长度key会覆盖前面的)
        // 如果要确保同长度不覆盖，Comparator需要更细致，比如 thenComparing
        // 例如：{One=1, Two=2, Five=5, Three=3} (One, Two, Three/Five/Four) - "Four" 和 "Five" 长度相同，
        // Comparator.comparingInt(String::length) 只比较长度，如果长度相同，则视为键相同，后面的会覆盖。
        // 为了避免覆盖，可以：
        Map<String, Integer> lengthSortedMapSafe = new TreeMap<>(
            Comparator.comparingInt(String::length).thenComparing(Comparator.naturalOrder())
        );
        lengthSortedMapSafe.put("One", 1);
        lengthSortedMapSafe.put("Three", 3);
        lengthSortedMapSafe.put("Two", 2);
        lengthSortedMapSafe.put("Four", 4);
        lengthSortedMapSafe.put("Five", 5);
        System.out.println("TreeMap (custom key order - by length then natural): " + lengthSortedMapSafe);
        // {One=1, Two=2, Five=5, Four=4, Three=3} (One(3), Two(3), Five(4), Four(4), Three(5))

    }
}
```


