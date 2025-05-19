# Lambda
它的存在是为了解决当匿名内部类很多时，用lambda表达式代替匿名内部类，让代码更简洁、更易读。

Lambda表达式的基本语法是：
`(parameters) -> expression`

或者 (如果方法体是代码块):

`(parameters) -> { statements; }`

***解释：***

`()`：用于包裹参数列表。

如果只有一个参数，且类型可以推断，可以省略括号：`param -> expression`

如果没有参数，括号不能省略：`() -> expression`

->：Lambda操作符，读作 **"goes to"** 或 **"becomes"**。

`expression`：单个表达式，`Lambda`会自动返回这个表达式的值。

`{ statements; }`：代码块，可以包含多条语句。如果方法有返回值，需要显式使用`return`语句。

**示例：**
```Java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class LambdaSort {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("peter", "anna", "mike", "xenia");

        // 升序
        Collections.sort(names, (String a, String b) -> a.compareTo(b));
        System.out.println(names); // [anna, mike, peter, xenia]

        // 编译器可以推断参数类型，所以可以省略
        Collections.sort(names, (a, b) -> b.compareTo(a)); // 降序
        System.out.println(names); // [xenia, peter, mike, anna]

        // 如果Lambda体只有一行，且是返回语句，可以省略return和花括号
        // (a, b) -> { return b.compareTo(a); }  等价于 (a, b) -> b.compareTo(a)
    }
}
```
还有很多用法，后续再更新


