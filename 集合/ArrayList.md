**类似于C++的vector**

**定义：**
```Java
//T：Integer,Double,Boolean,String,Character（没有int，double等）
ArrayList<T> list=new ArrayList();//括号中可指定大小
```

**用法：**
```Java
//自带元素的初始化
ArrayList<Integer> list=new ArrayList<>(Arrays.asList(1,2,3,4,5,6,7,8,9));

//add(E element): 把一个元素加到列表的末尾
ArrayList<String> names = new ArrayList<>();
names.add("你们");
names.add("最棒"); // 现在列表是 ["你们", "最棒"]

//add(int index, E element): 在指定的位置 index 插入一个元素，后面的元素会自动往后挪
names.add(1, "是"); // 在索引1的位置插入 "是"， 现在列表是 ["你们", "是", "最棒"]

//get(int index): 获取指定位置 index 上的元素
String who = names.get(0); // who 的值是 "你们"

//set(int index, E element): 替换指定位置 index 上的元素。把旧玩具换成新玩具！
names.set(2, "最厉害"); // 把索引2的 "最棒" 换成 "最厉害"，现在列表是 ["你们", "是", "最厉害"]

//remove(int index): 删除指定位置 index 上的元素。后面的元素会往前补位。
names.remove(1); // 删除索引1的 "是"
// 现在列表是 ["你们", "最厉害"]

//size(): 获取列表里现在有多少个元素
//isEmpty(): 检查列表是不是空的
//clear(): 清空列表里所有的元素
```

**自定义排序方法：**
```Java
list.sort(list,new Comparator<Integer>() {//Comparator名字不能改
            @Override
            public int compare(Integer o1, Integer o2) {//compare名字不能改
                return o1-o2;//compare 方法应该返回一个负数、零或正数，分别表示小于、等于或大于的关系
            }
        });
```
