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

### JSON文本 转换为 JavaScript对象

以 JSON 格式接收到数据，能够将其转换为 JavaScript对象

使用 `JSON.parse()` 方法
```js
var myJSON = '{ "name":"Bill Gates", "age":62, "city":"Seattle" }';
var myObj =  JSON.parse(myJSON);
console.log(myObj);
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

















---






学到
https://www.w3school.com.cn/js/js_json_syntax.asp


