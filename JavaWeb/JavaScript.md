# JavaScript



### **1.引用**

**a. 内部脚本** 直接将代码放在 `<script>` 标签内。

```html
<!DOCTYPE html>
<html>
<head>
    <title>内部脚本</title>
</head>
<body>
    <script>
        // 在这里编写 JavaScript 代码
        console.log("你好，世界！"); 
        alert("欢迎来到 JavaScript 的世界！");
    </script>
</body>
</html>
```



**b. 外部脚本** 将 JavaScript 代码保存在一个单独的 `.js` 文件中，然后通过 `<script>` 标签的 `src` 属性引入。这是最常用、最推荐的方式，因为它能让代码结构更清晰。

```html
<!DOCTYPE html>
<html>
<head>
    <title>外部脚本</title>
</head>
<body>
    <script src="a.js"></script>
</body>
</html>
```

```javascript
// a.js 文件
console.log("这是从外部文件加载的 JavaScript！");
```





### 2.变量

变量是用来存储数据的容器。在 JavaScript 中，我们使用 `let`、`const` 和 `var` 关键字来声明变量。

- `let`：声明一个块级作用域的局部变量。值可以被修改。
- `const`：声明一个块级作用域的只读常量。一旦赋值，就不能再被修改。

#### a. 原始类型

1. **String (字符串)**：文本数据，用单引号 `' '` 或双引号 `" "` 包裹。

   ```js
   let greeting = "你好";
   let school = '清华大学';
   ```

2. **Number (数字)**：整数或浮点数.

   ```js
   let myAge = 18;
   let price = 99.9;
   ```

3. **Boolean (布尔值)**：只有两个值 `true` 和 `false`，常用于逻辑判断。

   ```js
   let isLoggedIn = true;
   let hasPermission = false;
   ```

4. **Null (空值)**：表示一个“空”或者“无”的值。它是一个被赋给变量的值，用来表示“没有值”。

   ```js
   let user = null; // 用户对象为空
   ```

5. **Undefined (未定义)**：表示一个变量已被声明，但尚未被赋值。

   ```javascript
   let country;
   console.log(country); // 输出 undefined
   ```

6. **BigInt**：用于表示大于 `2^53 - 1` 的整数。

   ```javascript
   const bigNumber = 9007199254740991n;
   ```



### 3. 函数 

#### a. 函数声明 (Function Declaration)

```js
function greet(name) {
  return "你好, " + name + "!";
}
let greetingMessage = greet("小明");
console.log(greetingMessage); // 输出: 你好, 小明!
```



#### b. 函数表达式 

将一个匿名函数赋值给一个变量。

```js
const add = function(a, b) {
  return a + b;
};
let sum = add(5, 3);
console.log(sum); // 输出: 8
```



#### c. 箭头函数 

ES6 引入的更简洁的函数语法。

```js
// 完整写法
const multiply = (a, b) => {
  return a * b;
};

// 如果函数体只有一行 return 语句，可以省略 {} 和 return
const subtract = (a, b) => a - b;

// 如果只有一个参数，可以省略 ()
const square = x => x * x;

console.log(multiply(4, 5)); // 20
console.log(subtract(10, 4)); // 6
console.log(square(9)); // 81
```



### 4.自定义对象

```js
let car = {
  // 属性 (Properties)
  brand: "Tesla",
  model: "Model 3",
  color: "白色",
  batteryLevel: 85,
  isCharging: false,

  // 方法 (Methods)
  start: function() {//尽量不使用箭头函数
    console.log("车辆已启动！");
  },
  charge: function() {
    this.isCharging = true;
    console.log("开始充电...");
  },
  showStatus: function() {
    // `this` 关键字指向该对象本身 (car)
    console.log(`品牌: ${this.brand}, 型号: ${this.model}, 电量: ${this.batteryLevel}%`);
  }
};

car.showStatus(); // 输出 "品牌: Tesla, 型号: Model 3, 电量: 85%"
car.charge();     // 输出 "开始充电..."
console.log(car.isCharging); // 输出 true
```



# JSON

**一个 JSON 字符串的例子：**

```json
{
  "name": "王五",
  "age": 29,
  "isStudent": false,
  "courses": ["数学", "英语"],
  "address": {
    "city": "深圳",
    "street": "科技南路"
  },
  "spouse": null
}
```



### `JSON.stringify()`: 从 JavaScript 对象到 JSON 字符串

```javascript
// 这是一个 JavaScript 对象
const user = {
  name: "王五",
  age: 29,
  isStudent: false,
  courses: ["数学", "英语"],
  // 方法和 undefined 将在转换时被忽略
  sayHello: function() { console.log("Hello"); },
  girlfriend: undefined 
};

// 使用 JSON.stringify() 将其转换为 JSON 字符串
const jsonString = JSON.stringify(user);

console.log(jsonString);
// 输出结果 (一个很长的字符串):
// {"name":"王五","age":29,"isStudent":false,"courses":["数学","英语"]}

// 注意：sayHello 方法和 girlfriend 属性都消失了
```



### `JSON.parse()`: 从 JSON 字符串到 JavaScript 对象

```javascript
// 这是一个从服务器接收到的 JSON 字符串
const receivedJsonString = '{"id":101,"productName":"耳机","price":399,"inStock":true}';

// 使用 JSON.parse() 将其转换为 JavaScript 对象
const productObject = JSON.parse(receivedJsonString);

console.log(productObject);
// 输出一个真正的 JavaScript 对象:
// { id: 101, productName: '耳机', price: 399, inStock: true }

// 现在可以像操作普通 JS 对象一样操作它了
console.log(productObject.productName); // 输出 "耳机"
console.log(productObject.price);       // 输出 399
```





# DOM



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>JS-DOM</title>
  </head>
  <body>
    <h1 id="title1">Welcome to Day 2</h1>
    <h1 id="title2">Welcome to Day 2</h1>
    <script>
      //1. 修改第一个h1标签中的文本内容
      //1.1 获取DOM对象
      let h1 = document.querySelector('#title1');
      h1.innerHTML = '修改后的文本内容';

      let hs = document.querySelectorAll("h1");//hs是一个数组，里面存放了所有的h1标签

      //1.2 调用DOM对象中属性或方法
      hs[1].innerHTML = "修改后的文本内容";
    </script>
  </body>
</html>

```

