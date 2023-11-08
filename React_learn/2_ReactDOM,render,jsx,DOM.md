# React 元素渲染

我们使用 直接在 HTML 文件中编写 React 的方法学习，初始的 HTML 文件为：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <script type="text/babel"> /* 此处一定要写babel */
        
    </script>

</body>
</html>
```

## 元素

元素是构成 React 应用的最小单位，它用于描述屏幕上输出的内容
```jsx
const VDOM = <h1>Hello,React</h1>
```

## 将元素渲染到 DOM 中

首先我们在一个 HTML 页面中添加一个 id="example" 的 \<div\>：
```html
<div id="example"></div>
```
在此 div 中的所有内容都将由 React DOM 来管理，所以我们将其称为 根 DOM 节点

要将React元素渲染到根DOM节点中，我们通过把它们都传递给 **ReactDOM.render()** 的方法来将其渲染到页面上：
```jsx
// ReactDOM.render(HTML代码, 一个HTML元素) 
ReactDOM.render(VDOM, document.getElementById('example'));
```

完整的 HTML 文件内容为：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel"> /* 此处一定要写babel */
        // 创建虚拟DOM
        const VDOM = <h1>Hello,React</h1> /* 此处一定不要写引号，因为不是字符串 */
        // 渲染虚拟DOM到页面
        ReactDOM.render(VDOM, document.getElementById('example'));
    </script>

</body>
</html>
```

效果如下
![](resources/2023-11-08-21-57-24.png)

## 更新元素渲染

React 元素都是不可变的。当元素被创建之后，你是无法改变其内容或属性的
目前更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法

完整的 HTML 文件内容为：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel"> /* 此处一定要写babel */
        // 创建虚拟DOM
        const VDOM = <h1>Hello,React</h1> /* 此处一定不要写引号，因为不是字符串 */
        const VDOM2 = <h1>Bye,React</h1>
        // 渲染虚拟DOM到页面
        ReactDOM.render(VDOM, document.getElementById('example'));
        ReactDOM.render(VDOM2, document.getElementById('example'));
    </script>

</body>
</html>
```
注意这里的 ReactDOM.render() 是替换的动作，而不是追加的动作
页面中只会显示后渲染的 Bye,React

# React JSX

- JSX 的全称为 JavaScript XML
- JSX 允许我们在 React 中编写 HTML
- JSX 使在 React 中编写和添加 HTML 变得更加容易

## jsx 相较于 js 的优势

jsx 允许我们在 JavaScript 中编写 HTML 元素并将它们放置在 DOM 中，而无需任何 createElement() 或 appendChild() 方法（如果多个标签嵌套，jsx的写法与js相比更简洁）

### 使用 jsx 创建虚拟DOM

和上面的代码类似

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel"> /* 此处写babel */
        const VDOM = (
            <h1 id="title">
                <span>Hello,React</span>
            </h1>
        )
        ReactDOM.render(VDOM, document.getElementById('example'));
    </script>

</body>
</html>
```

效果如下
![](resources/2023-11-08-22-21-58.png)

### 使用 js 创建虚拟DOM

React.createElement(标签名, 标签属性, 标签内容)
React.createElement(component,props,…children)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <!-- 不需要babel，因为不需要将 jsx 转为 js -->
</head>
<body>

    <div id="example"></div>
    <script type="text/javascript"> /* 此处写javascript */
        // const VDOM = (
        //     <h1 id="title">
        //         <span>Hello,React</span>
        //     </h1>
        // )
        const VDOM = React.createElement('h1', {id:'title'}, 
                        React.createElement('span', {}, 'Hello,React')
                     )
        ReactDOM.render(VDOM, document.getElementById('example'));
    </script>

</body>
</html>
```
这种方式实现起来更复杂

效果同上

### 总结

jsx 创建虚拟DOM的写法：
```jsx
        const VDOM = (
            <h1 id="title">
                <span>Hello,React</span>
            </h1>
        )
```
是 js 创建虚拟DOM的写法：
```js
        const VDOM = React.createElement('h1', {id:'title'}, 
                        React.createElement('span', {}, 'Hello,React')
                     )
```
的**语法糖**

## jsx 语法






# 虚拟DOM 与 真实DOM



















---
p4




