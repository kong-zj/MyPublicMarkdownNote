# 生命周期

## 引入

### 组件的 挂载（mount）与 卸载（unmount）

当组件第一次被渲染到DOM中的时候，称为 **挂载（mount）**
当组件从DOM中移除的时候，称为 **卸载（unmount）**

要将React元素渲染到根DOM节点中，我们通过把它传递给 **ReactDOM.render()** 的方法来将其渲染到页面上：
```jsx
// ReactDOM.render(HTML代码, 一个HTML元素) 
ReactDOM.render(VDOM, document.getElementById('example'));
```

要将React元素卸载掉，我们通过把它传递给 **ReactDOM.unmountComponentAtNode()** 的方法来将其卸载：
```jsx
// ReactDOM.unmountComponentAtNode(一个HTML元素)
ReactDOM.unmountComponentAtNode(document.getElementById('example'));
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>引出生命周期</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Life extends React.Component{
            death = ()=>{
                // 卸载组件
                ReactDOM.unmountComponentAtNode(document.getElementById('example'))
            }
            render() {
                return (
                    <div>
                        <h2>React学不会怎么办？</h2>
                        <button onClick={this.death}>不活了</button>
                    </div>
                )
            }
        }
        // 挂载组件
        ReactDOM.render(<Life/>, document.getElementById('example'))
    </script>

</body>
</html>
```

在html页面中点击按钮，就可以卸载组件

### 循环定时器（setInterval）

继续上面的例子，想进一步改成，每点一次按钮，文字变得越来越透明

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>引出生命周期</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Life extends React.Component{
            state = {opacity:1}
            death = ()=>{
                // 卸载组件
                ReactDOM.unmountComponentAtNode(document.getElementById('example'))
            }
            render() {
                console.log('render');
                // 循环定时器
                setInterval(()=>{
                    let {opacity} = this.state;
                    opacity -= 0.1;
                    if(opacity<=0){
                        opacity = 1
                    }
                    this.setState({
                        opacity:opacity
                    })
                },200);
                return (
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>React学不会怎么办？</h2>
                        <button onClick={this.death}>不活了</button>
                    </div>
                )
            }
        }
        // 挂载组件
        ReactDOM.render(<Life/>, document.getElementById('example'))
    </script>

</body>
</html>
```

引发了无线循环的递归，而且递归越来越快
![](resources/2023-12-14-21-42-13.png)

render() 函数在第一次挂载时调用，然后开了一个循环定时器，每隔200毫秒执行一次，修改了状态，然后又会调用 render() 函数，又开启了一个循环定时器，就会有越来越多的循环定时器
**render() 里面就不能写 setState**，说明循环定时器 setInterval 放的位置不对

那就把循环定时器放到一个函数中，按钮的onClick事件触发这个函数

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>引出生命周期</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Life extends React.Component{
            state = {opacity:1}
            death = ()=>{
                // 卸载组件
                ReactDOM.unmountComponentAtNode(document.getElementById('example'))
            }
            action = ()=>{
                // 循环定时器
                setInterval(()=>{
                    let {opacity} = this.state;
                    opacity -= 0.1;
                    if(opacity<=0){
                        opacity = 1
                    }
                    this.setState({
                        opacity:opacity
                    })
                },200);
            }
            render() {
                console.log('render');
                return (
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>React学不会怎么办？</h2>
                        <button onClick={this.death}>不活了</button>
                        <button onClick={this.action}>开始变化</button>
                    </div>
                )
            }
        }
        // 挂载组件
        ReactDOM.render(<Life/>, document.getElementById('example'))
    </script>

</body>
</html>
```

现在，点击一次按钮触发透明度变化，且透明度变化的频率就不会越来越快了
![](resources/2023-12-14-21-56-41.png)

但还是有问题，想要的是，打开html页面，透明度就可以自己变化，不需要手动点击按钮
我们现在要的是一个时间点，即**组件挂载到页面后的那个时刻**，启用循环定时器

### 引入 componentDidMount

组件的 componentDidMount 函数，在组件挂载到页面后的那个时刻，被React调用

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>引出生命周期</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Life extends React.Component{
            state = {opacity:1}
            death = ()=>{
                ReactDOM.unmountComponentAtNode(document.getElementById('example'))
            }
            // 组件挂载完毕调用
            componentDidMount() {
                // 循环定时器
                setInterval(()=>{
                    let {opacity} = this.state;
                    opacity -= 0.1;
                    if(opacity<=0){
                        opacity = 1
                    }
                    this.setState({
                        opacity:opacity
                    })
                },200);
            }
            // 初始化渲染、state更新后调用
            render() {
                console.log('render');
                return (
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>React学不会怎么办？</h2>
                        <button onClick={this.death}>不活了</button>
                    </div>
                )
            }
        }
        ReactDOM.render(<Life/>, document.getElementById('example'))
    </script>

</body>
</html>
```

现在，只要打开html页面，透明度就可以自己变化

但是还有个小问题，点击按钮，想要卸载组件，会报错（组件都没了，就不能更新状态了）
![](resources/2023-12-14-22-24-33.png)

在卸载组件之前，**清除定时器**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>引出生命周期</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Life extends React.Component{
            state = {opacity:1}
            death = ()=>{
                // 清除定时器
                clearInterval(this.timer)
                ReactDOM.unmountComponentAtNode(document.getElementById('example'))
            }
            // 组件挂载完毕调用
            componentDidMount() {
                // 循环定时器
                this.timer = setInterval(()=>{
                    let {opacity} = this.state;
                    opacity -= 0.1;
                    if(opacity<=0){
                        opacity = 1
                    }
                    this.setState({
                        opacity:opacity
                    })
                },200);
            }
            // 初始化渲染、state更新后调用
            render() {
                console.log('render');
                return (
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>React学不会怎么办？</h2>
                        <button onClick={this.death}>不活了</button>
                    </div>
                )
            }
        }
        ReactDOM.render(<Life/>, document.getElementById('example'))
    </script>

</body>
</html>
```

这样就不会报错了

但是有更好的方法，我们现在要的是一个时间点，即**组件将要被卸载之前的那个时刻**，清除定时器

### 引入 componentWillUnmount

组件的 componentWillUnmount 函数，在组件将要被卸载之前的那个时刻，被React调用

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>引出生命周期</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        class Life extends React.Component{
            state = {opacity:1}
            death = ()=>{
                ReactDOM.unmountComponentAtNode(document.getElementById('example'))
            }
            // 组件挂载完毕调用
            componentDidMount() {
                // 循环定时器
                this.timer = setInterval(()=>{
                    let {opacity} = this.state;
                    opacity -= 0.1;
                    if(opacity<=0){
                        opacity = 1
                    }
                    this.setState({
                        opacity:opacity
                    })
                },200);
            }
            // 组件将要被卸载时调用
            componentWillUnmount() {
                // 清除定时器
                clearInterval(this.timer)
            }
            // 初始化渲染、state更新后调用
            render() {
                console.log('render');
                return (
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>React学不会怎么办？</h2>
                        <button onClick={this.death}>不活了</button>
                    </div>
                )
            }
        }
        ReactDOM.render(<Life/>, document.getElementById('example'))
    </script>

</body>
</html>
```

效果和之前相同

### 收获

组件的 挂载（mount）与 卸载（unmount）过程中，React会在合适的时间点去做一些事（例如componentDidMount、componentWillUnmount，这些函数称为 **生命周期回调函数** 或 **生命周期钩子函数**）

## 简介

1. 组件从创建到死亡它会经历一些特定的阶段
2. React组件中包含一系列的钩子函数（生命周期回调函数），会在特定时刻调用
3. 我们在定义组件时，会在特定的生命周期回调函数中，做特定的工作
4. render()函数，被调用1+n次，1次是页面初次渲染，n次是页面更新的次数

## 生命周期流程（旧）

![](resources/2023-12-14-22-55-30.png)

### 挂载时的流程

如图中红线所示
![](resources/2023-12-14-23-34-20.png)

#### constructor -> componentWillMount -> render -> componentDidMount -> componentWillUnmount

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>生命周期（旧）</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        // 创建组件
        class Count extends React.Component{
            // 构造器
            constructor(props){
                console.log('Count-constructor');
                super(props);
                // 初始化状态
                this.state = {
                    count: 0
                }
            }
            // 点我+1 按钮的回调
            add = ()=>{
                const {count} = this.state
                this.setState({
                    count: count+1
                })
            }
            // 卸载组件 按钮的回调
            death = ()=>{
                ReactDOM.unmountComponentAtNode(document.getElementById('example'));
            }
            // 组件将要挂载的钩子
            componentWillMount(){
                console.log('Count-componentWillMount');
            }
            // 组件挂载完毕的钩子
            componentDidMount(){
                console.log('Count-componentDidMount');
            }
            // 组件将要卸载的钩子
            componentWillUnmount(){
                console.log('Count-componentWillUnmount');
            }
            render(){
                console.log('Count-render');
                const {count} = this.state;
                return(
                    <div>
                        <h2>当前求和为：{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                        <button onClick={this.death}>卸载组件</button>
                    </div>
                )
            }
        }
        // 渲染组件
        ReactDOM.render(<Count/>,document.getElementById('example'))
    </script>

</body>
</html>
```

效果如下
![](resources/2023-12-14-23-30-57.png)

### setState()的流程

如图中圈2所示
![](resources/2023-12-14-23-30-22.png)

#### shouldComponentUpdate 简介

执行setState()之后，React调用 shouldComponentUpdate 这个钩子，shouldComponentUpdate 是一个**阀门**，控制是否走这个流程，如果 shouldComponentUpdate 返回 true，则继续走流程；如果返回 false，则不继续走流程

如果自己不写 shouldComponentUpdate 这个钩子，它的返回值永远是 true

##### 返回值为 true

shouldComponentUpdate 这个钩子的返回值为 **true** 时：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>生命周期（旧）</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        // 创建组件
        class Count extends React.Component{
            // 构造器
            constructor(props){
                console.log('Count-constructor');
                super(props);
                // 初始化状态
                this.state = {
                    count: 0
                }
            }
            // 点我+1 按钮的回调
            add = ()=>{
                const {count} = this.state
                this.setState({
                    count: count+1
                })
            }
            // 卸载组件 按钮的回调
            death = ()=>{
                ReactDOM.unmountComponentAtNode(document.getElementById('example'));
            }
            // 组件将要挂载的钩子
            componentWillMount(){
                console.log('Count-componentWillMount');
            }
            // 组件挂载完毕的钩子
            componentDidMount(){
                console.log('Count-componentDidMount');
            }
            // 组件将要卸载的钩子
            componentWillUnmount(){
                console.log('Count-componentWillUnmount');
            }
            // 控制组件更新的阀门
            shouldComponentUpdate(){
                console.log('Count-shouldComponentUpdate');
                return true;
            }
            render(){
                console.log('Count-render');
                const {count} = this.state;
                return(
                    <div>
                        <h2>当前求和为：{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                        <button onClick={this.death}>卸载组件</button>
                    </div>
                )
            }
        }
        // 渲染组件
        ReactDOM.render(<Count/>,document.getElementById('example'))
    </script>

</body>
</html>
```

效果如下
![](resources/2023-12-14-23-50-20.png)

##### 返回值为 false

shouldComponentUpdate 这个钩子的返回值为 **false** 时：
```jsx
// 其他部分与上面的例子中相同
            // 控制组件更新的阀门
            shouldComponentUpdate(){
                console.log('Count-shouldComponentUpdate');
                return false;
            }
```

效果如下
![](resources/2023-12-14-23-54-57.png)

#### shouldComponentUpdate -> omponentWillUpdate -> render -> componentDidUpdate
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>生命周期（旧）</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        // 创建组件
        class Count extends React.Component{
            // 构造器
            constructor(props){
                console.log('Count-constructor');
                super(props);
                // 初始化状态
                this.state = {
                    count: 0
                }
            }
            // 点我+1 按钮的回调
            add = ()=>{
                const {count} = this.state
                this.setState({
                    count: count+1
                })
            }
            // 卸载组件 按钮的回调
            death = ()=>{
                ReactDOM.unmountComponentAtNode(document.getElementById('example'));
            }
            // 组件将要挂载的钩子
            componentWillMount(){
                console.log('Count-componentWillMount');
            }
            // 组件挂载完毕的钩子
            componentDidMount(){
                console.log('Count-componentDidMount');
            }
            // 组件将要卸载的钩子
            componentWillUnmount(){
                console.log('Count-componentWillUnmount');
            }
            // 控制组件更新的阀门
            shouldComponentUpdate(){
                console.log('Count-shouldComponentUpdate');
                return true;
            }
            // 组件将要更新的钩子
            componentWillUpdate(){
                console.log('Count-componentWillUpdate');
            }
            // 组件更新完毕的钩子
            componentDidUpdate(){
                console.log('Count-componentDidUpdate');
            }
            render(){
                console.log('Count-render');
                const {count} = this.state;
                return(
                    <div>
                        <h2>当前求和为：{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                        <button onClick={this.death}>卸载组件</button>
                    </div>
                )
            }
        }
        // 渲染组件
        ReactDOM.render(<Count/>,document.getElementById('example'))
    </script>

</body>
</html>
```

效果如下
![](resources/2023-12-15-00-23-09.png)

### forceUpdate()的流程

如图中圈3所示
![](resources/2023-12-14-23-30-22.png)







### 父组件render的流程

如图中圈1所示
![](resources/2023-12-14-23-30-22.png)














---



P40


