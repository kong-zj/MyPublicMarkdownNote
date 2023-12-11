# refs 与 事件处理

组件内的标签可以定义 **ref 属性**来**标识**自己（类似与原生里的 id）

## 引入

需求：
1. 点击按钮，提示第一个输入框中的值
2. 当第二个输入框失去焦点时，提示这个输入框的值

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>refs</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Demo extends React.Component{
            // 展示左侧输入框的数据
            showData = ()=>{
                const input1 = document.getElementById('input1');
                alert(input1.value);
            }
            render(){
                return (
                    <div>
                        <input id="input1" type="text" placeholder="点击按钮提示数据" />
                        <button onClick={this.showData}>点我提示左侧的数据</button>
                        <input type="text" placeholder="失去焦点提示数据" />
                    </div>
                )
            }   
        }
        ReactDOM.render(<Demo />, document.getElementById('example'));
    </script>

</body>
</html>
```

用 id 属性可以实现，但没有必要，react 里面封装了类似的东西：**ref 属性**

## 字符串形式的 ref（过时）

### 使用 ref 属性来标识标签

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>refs</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Demo extends React.Component{
            // 展示左侧输入框的数据
            showData = ()=>{
                console.log(this);
            }
            render(){
                return (
                    <div>
                        <input ref="input1" type="text" placeholder="点击按钮提示数据" />
                        <button ref="button1" onClick={this.showData}>点我提示左侧的数据</button>
                        <input ref="input2" type="text" placeholder="失去焦点提示数据" />
                    </div>
                )
            }   
        }
        ReactDOM.render(<Demo />, document.getElementById('example'));
    </script>

</body>
</html>
```

输出的组件实例对象如下
![](resources/2023-12-11-19-11-30.png)
只要敢**用 ref 属性来给标签打标识**，React 都会将其**收集到组件的 refs 属性里**（注意 ref 与 refs 的区别）
refs 属性包含多个 key:value 组合，其中 **key** 是我们定义的**字符串**，**value** 就是当前**标签对应的 DOM 元素**

### 从 this 中获取 refs

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>refs</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Demo extends React.Component{
            // 展示左侧输入框的数据
            showData = ()=>{
                const {input1} = this.refs
                alert(input1.value)
            }
            // 展示右侧输入框的数据
            showData2 = ()=>{
                const {input2} = this.refs
                alert(input2.value)
            }
            render(){
                return (
                    <div>
                        <input ref="input1" type="text" placeholder="点击按钮提示数据" />
                        <button onClick={this.showData}>点我提示左侧的数据</button>
                        <input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据" />
                    </div>
                )
            }   
        }
        ReactDOM.render(<Demo />, document.getElementById('example'));
    </script>

</body>
</html>
```

实现了前面所提的需求

## 回调形式的 ref

因为字符串形式的 ref 的效率低，所以 React 建议使用回调形式的 ref

回调形式的 ref 接收一个回调函数，这个**回调函数在组件挂载或者卸载的时候被调用**。当 ref 用于一个HTML元素的时候，ref 指定的回调函数在调用的时候会接收一个参数，该参数就是指定的 DOM 元素

当组件**挂载**的时候 React 会给 ref 回调函数传入当前的 DOM，在组件**卸载**的时候会传入 null

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>refs</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Demo extends React.Component{
            // 展示左侧输入框的数据
            showData = ()=>{
                const {input1} = this
                alert(input1.value)
            }
            // 展示右侧输入框的数据
            showData2 = ()=>{
                const {input2} = this
                alert(input2.value)
            }
            render(){
                return (
                    <div>
                        <input ref={(currentNode) => {this.input1 = currentNode}} type="text" placeholder="点击按钮提示数据" />
                        <button onClick={this.showData}>点我提示左侧的数据</button>
                        <input ref={c => this.input2 = c} onBlur={this.showData2} type="text" placeholder="失去焦点提示数据" />
                    </div>
                )
            }   
        }
        ReactDOM.render(<Demo />, document.getElementById('example'));
    </script>

</body>
</html>
```

**ref={(currentNode) => {this.input1 = currentNode}}** 的意思是：
把这个 ref 所在的 **DOM 节点**，起了名字 input1，然后**挂载到组件实例自身上**，然后就可以通过组件实例，来访问到这个 DOM 节点，进行操作了
**ref={(currentNode) => {this.input1 = currentNode}}** 可以简写为 **ref={c => this.input1 = c}**

效果和之前相同

### 回调 ref 中的回调执行次数问题

[关于回调 refs 的说明-官网](https://www.bookstack.cn/read/react-17.0-zh/bea7c60ba4ba6103.md)

> 如果 ref 回调函数是以**内联函数**的方式定义的，在**更新**过程中它会被执行**两次**，第一次传入参数 null，然后第二次会传入参数 DOM 元素。这是因为在**每次渲染时会创建一个新的函数实例**，所以 React **清空旧的 ref 并且设置新的**。**通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题**，但是大多数情况下它是无关紧要的

回到之前天气炎热凉爽的例子

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>refs</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Weather extends React.Component{
            state = {isHot:false}
            render(){
                const {isHot,wind} = this.state
                return(
                    <div>
                        <h1 ref={(currentNode) => {this.h1 = currentNode; console.log('@',currentNode)}}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
                        <button onClick={this.changeWeather}>点我切换天气</button>
                    </div>
                ) 
            }
            changeWeather = ()=>{
                const {isHot} = this.state
                this.setState({isHot:!isHot})
            }
        }
        ReactDOM.render(<Weather/>,document.getElementById('example'))
    </script>

</body>
</html>
```

效果如下
![](resources/2023-12-11-21-37-12.png)

#### 将 ref 的回调函数定义成 class 的绑定函数

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>refs</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Weather extends React.Component{
            state = {isHot:false}
            render(){
                const {isHot,wind} = this.state
                return(
                    <div>
                        {/*<h1 ref={(currentNode) => {this.h1 = currentNode; console.log('@',currentNode)}}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>*/}
                        <h1 ref={this.h1ref}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
                        <button onClick={this.changeWeather}>点我切换天气</button>
                    </div>
                ) 
            }
            h1ref = (currentNode) => {
                this.h1 = currentNode;
                console.log('@',currentNode);
            }
            changeWeather = ()=>{
                const {isHot} = this.state
                this.setState({isHot:!isHot})
            }
        }
        ReactDOM.render(<Weather/>,document.getElementById('example'))
    </script>

</body>
</html>
```

效果如下
![](resources/2023-12-11-21-59-43.png)

无论怎么更新 DOM 元素，都**不会频繁调用** 已经放在实例自身的 h1ref，因为 React 知道之前**调用过**实例自身的 h1ref，它**不再是一个新的函数**

但是，使用内联函数，还是定义成 class 的绑定函数，在实际使用中的差别忽略不计

## 














---



P30


