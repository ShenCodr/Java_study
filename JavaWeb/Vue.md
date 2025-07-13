# Vue

### 1.vue-入门框架

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- 声明文档类型为HTML5 -->
    <!DOCTYPE html>
    <!-- 设置语言为英语 -->
    <html lang="en">
    <head>
        <!-- 定义文档的字符编码为UTF-8，支持多语言显示 -->
        <meta charset="UTF-8">
        <!-- 设置视口以适配移动设备，确保页面在不同设备上响应式显示 -->
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <!-- 定义网页标题 -->
        <title>Vue3入门框架</title>
    </head>
<body>
    <!-- Vue应用的挂载点，Vue会将数据绑定到这个div中 -->
    <div id="app">
        <!-- 使用Vue的模板语法，绑定数据中的message变量 -->
        <h1>{{message}}</h1>
    </div>

    <script type="module">
        import { createApp } from "https://unpkg.com/vue@3/dist/vue.esm-browser.js";

        /* 创建Vue应用实例
           - data() 方法定义了应用的数据
           - message 是一个响应式变量，绑定到HTML中的 {{message}} 模板语法
        */
        createApp({
            data() {
                return {
                    message: 'Hello Vue!' // 定义数据变量，初始值为 "Hello Vue!"
                }
            }
        }).mount('#app'); // 挂载Vue应用到id为app的DOM元素上
    </script>
</body>
</html>
```



### 2.Vue基础语法

**不想说了，自己去[‌‌⁠﻿‍‌﻿‬‌‍﻿⁠⁠⁠‍⁠﻿‬‬‬﻿‍‌⁠⁠‌﻿﻿﻿‌﻿‍这里](https://heuqqdmbyk.feishu.cn/wiki/FxTdw2K9mieDgAkhSqucg59Cn8f)看吧**

