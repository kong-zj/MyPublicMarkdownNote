# JSON

- JSON 指的是 **J**ava**S**cript **O**bject **N**otation（JavaScript 对象标记法）
- JSON 使用 JavaScript 语法，但是 JSON 格式是**纯文本**的
- JSON 是轻量级的**数据交换格式**
- 我们能够把任何 **JavaScript对象 转换为 JSON**，然后将 JSON 发送到服务器；我们也能把从服务器接收到的任何 **JSON 转换为 JavaScript对象**（以这样的方式，我们能够把**数据**作为 **JavaScript对象** 来处理，无需复杂的解析和转译）
- JSON 独立于语言
- JSON 是 自描述的 且 易于理解

## JSON文本 与 JavaScript对象 的互相转换

### JavaScript对象 转换为 JSON文本

把数据存储在 JavaScript对象 中，可以把该对象转换为 JSON，之后可将其发送到服务器

使用 `JSON.stringify()` 方法
```js
var myObj = { name:"Bill Gates",  age:62, city:"Seattle" };
var myJSON =  JSON.stringify(myObj);
console.log(myJSON);
```

#### `JSON.stringify()` 方法

[官方文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

语法：
```js
JSON.stringify(value[, replacer [, space]])
```

参数：
- **value**：必需，要转换的 JavaScript值（通常为对象或数组）
- **replacer**：可选，如果该参数是一个**函数**，则在序列化过程中，被序列化的值的**每个属性都会经过该函数**的转换和处理；如果该参数是一个**数组**，则只有**包含在这个数组中的属性名**才会被序列化到最终的 JSON字符串 中；如果该参数为 **null** 或者**未提供**，则对象**所有的属性**都会被序列化
- **space**：可选，用来控制结果字符串里面的间距。如果是一个**数字**，则在字符串化时每一级别会比上一级别缩进多这个数字值的空格（最多 10 个空格）；如果是一个**字符串**，则每一级别会比上一级别多缩进该字符串（或该字符串的前 10 个字符）

返回值：
一个表示给定值的 JSON 字符串

给 **replacer参数** 传入一个**函数**（function），将string类型的数据丢弃，示例：
```js
var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};
function replacer(key, value) {
    if (typeof value === "string") {
        return undefined;
    }
    return value;
}
var jsonString = JSON.stringify(foo, replacer);     // 结果: {"week":45,"month":7}
```

给 **replacer参数** 传入一个**数组**（array），只保留"week"和"month"属性值，示例：
```js
var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};
var jsonString = JSON.stringify(foo, ['week', 'month']);     // 结果: {"week":45,"month":7}
```

给 **space参数** 传入 `"\t"`，美化 JSON字符串 的格式，示例：
```js
var jsonString = JSON.stringify({ uno: 1, dos: 2 }, null, "\t");
// 结果:
// {
// 	"uno": 1,
// 	"dos": 2
// }
```

### JSON文本 转换为 JavaScript对象

以 JSON 格式接收到数据，能够将其转换为 JavaScript对象

使用 `JSON.parse()` 方法
```js
var myJSON = '{ "name":"Bill Gates", "age":62, "city":"Seattle" }';
var myObj =  JSON.parse(myJSON);
console.log(myObj);
```

#### `JSON.parse()` 方法

[官方文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

语法：
```js
JSON.parse(text)
JSON.parse(text, reviver)
```

参数：
- **text**：必需，一个有效的 JSON 字符串
- **reviver**：可选，一个转换结果的**函数**，将为对象的每个成员调用此函数

返回值：
与给定的 JSON文本 相对应的 Object、Array、string、number、boolean 或者 null 值

给 **reviver参数** 传入一个**函数**，将指定的字符串转换为日期，示例：
```js
var text =  '{ "name":"Bill Gates", "birth":"1955-10-28", "city":"Seattle"}';
var obj = JSON.parse(text, function (key, value) {
    if  (key == "birth") {
        return new Date(value);
    } else {
        return value;
    }
});
```

## JSON 语法规则

JSON 语法衍生于 JavaScript 对象标记法语法：
- 数据为 **键/值**对
- 数据由**逗号**分隔
- **花括号**保存对象
- **方括号**保存数组

### JSON文本实例 和对应 JavaScript对象实例

JSON文本实例：
```js
var myJSON = `{
    "employees":[
        {"firstName":"Bill", "lastName":"Gates"}, 
        {"firstName":"Steve", "lastName":"Jobs"},
        {"firstName":"Alan", "lastName":"Turing"}
    ]
}`;
```
上面实例中，反引号包裹的部分，可称为 **JSON对象**

对应 JavaScript对象实例：
```js
var myObj = {
    employees:[
    	{firstName:"Bill", lastName:"Gates"},
    	{firstName:"Steve", lastName:"Jobs"},
    	{firstName:"Alan", lastName:"Turing"}
	]
};
```

### 键/值对

#### 键

JSON 数据的书写方式是 键/值对，类似 JavaScript 对象属性
不同的是：
- **JSON格式数据**中的 键/值对 中的**键**需要**双引号**
- **JavaScript对象属性**中的 键/值对 中的键**不**需要双引号

#### 值

在 JSON 中，值必须是以下数据类型之一：
- 字符串
- 数字
- 对象（JSON对象）
- 数组
- 布尔
- null

在 JavaScript 中，以上所列均可为值，外加其他有效的 JavaScript 表达式，包括：
- 函数
- 日期
- undefined

## JSON 文件

JSON 文件的文件类型是 `.json`
JSON 文本的 MIME 类型是 `application/json`

[常见 MIME 类型列表](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)
















---






还剩：
https://www.w3school.com.cn/js/js_best_practices.asp

函数
Async
HTML DOM
Browser DOM
Web API
AJAX
jQuery
图形




学到：
https://www.w3school.com.cn/js/js_object_iterables.asp

https://www.bilibili.com/video/BV14a411T7dC/?vd_source=fbc0e3c28b30361c540f78c17d710823
