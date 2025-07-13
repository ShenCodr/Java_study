**Java中的String貌似都是常量，不方便修改，所以我使用StringBuilder**

**但是StringBuilder不支持从键盘输入，所以采用以下方法**

```Java
String str=scanner.nextLine();
StringBuilder sb=new StringBuilder(str);
```

**以下是StringBuilder的一些用法**

```Java
//append(各种类型):可以把各种东西（字符串、整数、字符、布尔值等等）加到末尾。
StringBuilder sb = new StringBuilder("你好");
sb.append("ShenCodr");     // 现在是 "你好，ShenCodr"

//insert(int offset, 各种类型): 可以在指定的位置（offset）插入东西
StringBuilder sb = new StringBuilder("你好！");
sb.insert(2, "最"); // 在第2个位置（从0开始数）插入 "最" ,结果："你最好！"

//setCharAt(int index, char ch)修改索引为i的字符为ch
StringBuilder sb = new StringBuilder("我好帅");
sb.setCharAt(2, '棒'); 
System.out.println("修改后：" + sb); // 输出：我好棒

//delete(int start, int end): 可以删除从 start 位置开始（包含）到 end 位置结束（不包含）的一段字符。
StringBuilder sb = new StringBuilder("你们最最好了！");
sb.delete(2, 4); // 删除索引2和3的字符 ("最最")
System.out.println(sb); // 输出 "你们好了！"

//replace(int start, int end, String str): 可以把从 start 到 end（不包含）的字符替换成新的字符串 str
StringBuilder sb = new StringBuilder("你们坏蛋！");
sb.replace(2, 4, "最棒"); // 把 "坏蛋" 替换成 "最棒"
System.out.println(sb); // 输出 "你们最棒！"
```
