# JavaScript 快速入门

[廖雪峰的JavaScript教程](https://www.liaoxuefeng.com/wiki/1022910821149312)

## js代码的位置

### 直接嵌在网页中

把JavaScript代码放到 .html 文件的 \<head\> 中：
```html
<html>
<head>
  <script>
    alert('Hello, world');
  </script>
</head>
<body>
  ...
</body>
</html>
```
由 \<script\>...\</script\> 包含的代码就是JavaScript代码，它将直接被浏览器执行

### 放到一个单独的 .js 文件

把JavaScript代码放到一个单独的 .js 文件，然后在HTML中通过 \<script src="..."\>\</script\>引入这个文件：
```html
<html>
<head>
  <script src="static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
```
这样，static/js/abc.js 就会被浏览器执行

把JavaScript代码放入一个单独的 .js 文件中更利于维护代码，并且多个页面可以各自引用同一份 .js 文件

#### \<script\>标签的type属性

有些时候你会看到\<script\>标签还设置了一个type属性：
```html
<script type="text/javascript">
  ...
</script>
```
但这是没有必要的，因为默认的type就是JavaScript，所以不必显式地把type指定为JavaScript

## 调试

使用 Google Chrome浏览器 的 F12 开发者工具


console.log()
---
图片
待完成

https://www.liaoxuefeng.com/wiki/1022910821149312/1023020895584256



## 基本语法

JavaScript严格区分大小写

JavaScript并不强制要求在每个语句的结尾加 ;
但我们不会省略 ;

一个完整的赋值语句：
```js
var x = 1;
```

语句块是一组语句的集合，例如，下面的代码先做了一个判断，如果判断成立，将执行 {...} 中的所有语句：
```js
if (2 > 1) {
    x = 1;
    y = 2;
    z = 3;
}
```

以 // 开头直到行末的字符被视为行注释，注释是给开发人员看到，JavaScript引擎会自动忽略：
```js
// 这是注释
```

另一种块注释是用 /\*...\*/ 把多行字符包裹起来，把一大块视为一个注释：
```js
/* 从这里开始是块注释
仍然是注释
仍然是注释
注释结束 */
```

## 数据类型和变量

### number

JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型：
```js
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
0xff00; // 十六进制用 0x 前缀和0-9，a-f表示
```

Number可以直接做四则运算：
```js
1 - 2; // -1
(1 + 2) * 5 / 2; // 7.5
2 / 0; // Infinity
0 / 0; // NaN
10 % 3; // 1
10.5 % 3; // 1.5
```

要注意，JavaScript的Number不区分整数和浮点数，也就是说，12.00 === 12。（在大多数其他语言中，整数和浮点数不能直接比较）并且，JavaScript的整数最大范围不是 ±2^63，而是 ±2^53，因此，超过 2^53 的整数就可能无法精确表示

### string

JavaScript的字符串就是用 '' 或 "" 括起来的字符表示

如果字符串内部既包含 ' 又包含 " 怎么办？可以用转义字符 \ 来标识，比如：
```js
'I\'m \"OK\"!';
```

ASCII字符可以以 \x## 形式的十六进制表示：
```js
'\x41'; // 完全等同于 'A'
```

还可以用 \u#### 表示一个Unicode字符：
```js
'\u4e2d\u6587'; // 完全等同于 '中文'
```

#### 多行字符串

由于多行字符串用 \n 写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号 \`...\` 表示：
```js
`这是一个
多行
字符串`;
```

#### 模板字符串

要把多个字符串连接起来，可以用 + 号连接：
```js
let name = '小明';
let age = 20;
let message = '你好, ' + name + ', 你今年' + age + '岁了!';
alert(message);
```

如果有很多变量需要连接，用 + 号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：
```js
let name = '小明';
let age = 20;
let message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```

#### 操作字符串

获取字符串的长度用 .length，获取字符串某个指定位置的字符，使用类似Array的下标操作，索引号从0开始：
```js
var s = 'Hello, world!';
s.length; // 13
s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
```

需要特别注意的是，**字符串是不可变的**，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果：
```js
var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```

#### 常用方法

- toUpperCase()
- toLowerCase()
- indexOf()
- substring()

### boolean

一个布尔值只有 true、false 两种值

可以直接用 true、false 表示布尔值，也可以通过布尔运算计算出来：
```js
true; // 这是一个true值
false; // 这是一个false值
2 > 1; // 这是一个true值
2 >= 3; // 这是一个false值
```

#### 布尔运算符

- && ：与运算
- || ：或运算
- ! ：非运算

#### 比较运算符

- \>
- \<
- \>=
- ==
- ===

JavaScript允许对任意数据类型做比较：
```js
false == 0; // true
false === 0; // false
```

##### == 和 ===

JavaScript在设计时，有两种比较运算符：
1. == ，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果
2. === ，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较

由于JavaScript这个设计缺陷，不要使用 == 比较，始终坚持使用 === 比较

一个例外是 NaN 这个特殊的Number与所有其他值都不相等，包括它自己：
```js
NaN === NaN; // false
```

唯一能判断 NaN 的方法是通过 isNaN() 函数：
```js
isNaN(NaN); // true
```

还要注意浮点数的相等比较：
```js
1 / 3 === (1 - 2 / 3); // false
```

这不是JavaScript的设计缺陷。浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值：
```js
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
```

### bigint

要精确表示比 2^53 还大的整数，可以使用内置的BigInt类型，它的表示方法是在整数后加一个 n，例如 9223372036854775808n，也可以使用 BigInt() 把Number和字符串转换成BigInt：
```js
var bi1 = 9223372036854775807n;
var bi2 = BigInt(12345);
var bi3 = BigInt("0x7fffffffffffffff");
console.log(bi1 === bi2); // false
console.log(bi1 === bi3); // true
console.log(bi1 + bi2);
```

使用BigInt可以正常进行加减乘除等运算，结果仍然是一个BigInt，但不能把一个BigInt和一个Number放在一起运算：
```js
console.log(1234567n + 3456789n); // OK
console.log(1234567n / 789n); // 1564, 除法运算结果仍然是BigInt
console.log(1234567n % 789n); // 571, 求余
console.log(1234567n + 3456789); // Uncaught TypeError: Cannot mix BigInt and other types
```

### null 和 undefined




### 数组





### 对象


