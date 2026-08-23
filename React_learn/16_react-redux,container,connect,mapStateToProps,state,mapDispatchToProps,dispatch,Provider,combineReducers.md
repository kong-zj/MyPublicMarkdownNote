# react-redux

前面所讲的redux只是单纯的一个状态管理库，和react没有关系
react推出 react-redux插件库，更舒服地使用redux

## 安装

`npm install react-redux`

## 模型图

![](resources/2024-10-18-22-00-57.png)

# 求和案例

继续使用上一章的求和案例

## 实现容器组件

在`src`目录下创建一个`containers`文件夹，里面创建一个`Count`文件夹，里面创建一个`index.jsx`文件

现在项目的目录结构如下
```sh
todolist/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    index.html
  src/
    App.js
    index.jsx
    components/
      Count/
        index.jsx
    containers/
      Count/
        index.jsx
    redux/
      store.js
      count_reducer.js
      count_action.js
      constant.js
```

### src/containers/Count/index.jsx

容器组件起到**桥梁**作用，一边是**UI组件**，一边是**redux**
容器组件不能亲自写，需要使用`react-redux`提供的`connect`函数

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入redux中的store
import store from "../../redux/store";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

// 使用connect()()，创建并暴露一个Count容器组件
export default connect()(CountUI);
```

## 渲染容器组件

### src/App.js

之前是渲染`Count的UI组件`，现在改为渲染`Count的容器组件`

```js
import React, { Component } from 'react';
import Count from './containers/Count';

export default class App extends Component {
  render() {
    return (
      <div>
        <Count/>
      </div>
    )
  }
}
```

效果如下
![](resources/2024-10-18-22-40-16.png)

因为上面的`src/containers/Count/index.jsx`文件中的`Count的容器组件`，只连接了**UI组件**，还没有连接**redux**，所以报错

容器组件不需要自己引入`store`，必须在上一层通过`props`的方式传递
`src/App.js`文件修改如下
```js
import React, { Component } from 'react';
import Count from './containers/Count';
import store from './redux/store';

export default class App extends Component {
  render() {
    return (
      <div>
        <Count store={store}/>
      </div>
    )
  }
}
```

这样就相当于`Count的容器组件`连接了**redux**
运行后不再报错

## 容器组件（父）给UI组件（子）传递信息

**容器组件**和**UI组件**是父子关系
![](resources/2024-10-18-22-55-02.png)

### src/containers/Count/index.jsx（父）

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

// a函数的返回的对象中的key就作为传递给UI组件props的key，value就作为传递给UI组件props的value
// 传递状态
function a() {
    return { count: 900 };
    // 相当于 <CountUI count={900}/>
}

// a函数的返回的对象中的key就作为传递给UI组件props的key，value就作为传递给UI组件props的value
// 传递操作状态的方法
function b() {
    return { jia: () => { console.log(1); } }
}

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(a, b)(CountUI);
```

### src/components/Count/index.jsx（子）

```jsx
import React, { Component } from 'react';

export default class Count extends Component {

  // 加法
  increment = () => {
    const { value } = this.selectNumber;
  }

  // 减法
  decrement = () => {
    const { value } = this.selectNumber;
  }

  // 奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
  }

  // 异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
  }

  render() {
    console.log('UI组件接收到的props是', this.props);
    return (
      <div>
        <h1>当前求和为：{this.props.count}</h1>
        <select ref={c => this.selectNumber = c}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>&nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    )
  }
}
```

效果如下
![](resources/2024-10-18-23-18-19.png)

## 实现求和功能

### src/containers/Count/index.jsx（父）

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

import store from "../../redux/store";

// 传递状态
function a() {
    return { count: store.getState() };
}

// 传递操作状态的方法
function b() {
    return {
        jia: (number) => {
            // 通知redux执行加法
            store.dispatch({ type: 'increment', data: number });
        }
    }
}

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(a, b)(CountUI);
```

### src/components/Count/index.jsx（子）

```jsx
import React, { Component } from 'react';

export default class Count extends Component {

  // 加法
  increment = () => {
    const { value } = this.selectNumber;
    this.props.jia(value*1);
  }

  // 减法
  decrement = () => {
    const { value } = this.selectNumber;
  }

  // 奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
  }

  // 异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
  }

  render() {
    return (
      <div>
        <h1>当前求和为：{this.props.count}</h1>
        <select ref={c => this.selectNumber = c}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>&nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    )
  }
}
```

## 小精简（connect参数）

### src/containers/Count/index.jsx

可以不引入`store`，因为我们想要的`state`和`dispatch`，`connect`会自动帮我们传入

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

// import store from "../../redux/store";

// 传递状态
// redux自动调用 state = store.getState()，并把 state 作为参数传入
function a(state) {
    return { count: state };
}

// 传递操作状态的方法
// redux自动把 dispatch 作为参数传入
function b(dispatch) {
    return {
        jia: (number) => {
            // 通知redux执行加法
            dispatch({ type: 'increment', data: number });
        }
    }
}

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(a, b)(CountUI);
```

## 使用 action creators

### src/containers/Count/index.jsx（`mapStateToProps`、`mapDispatchToProps`）

- 不要自己写`action`，而是调用`action creators`返回一个`action`
- 函数名要见名知意，改为`mapStateToProps`、`mapDispatchToProps`

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

import {createIncrementAction} from "../../redux/count_action";

// 传递状态
// redux自动调用 state = store.getState()，并把 state 作为参数传入
function mapStateToProps(state) {
    return { count: state };
}

// 传递操作状态的方法
// redux自动把 dispatch 作为参数传入
function mapDispatchToProps(dispatch) {
    return {
        jia: (number) => {
            // 通知redux执行加法
            dispatch(createIncrementAction(number));
        }
    }
}

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(mapStateToProps, mapDispatchToProps)(CountUI);
```

## 完善求和功能

### src/containers/Count/index.jsx（父）

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

import {
    createIncrementAction, 
    createDecrementAction, 
    createIncrementAsyncAction
} from "../../redux/count_action";

// 传递状态
// redux自动调用 state = store.getState()，并把 state 作为参数传入
function mapStateToProps(state) {
    return { count: state };
}

// 传递操作状态的方法
// redux自动把 dispatch 作为参数传入
function mapDispatchToProps(dispatch) {
    return {
        jia: number => dispatch(createIncrementAction(number)),
        jian: number => dispatch(createDecrementAction(number)),
        jiaAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time))
    }
}

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(mapStateToProps, mapDispatchToProps)(CountUI);
```

### src/components/Count/index.jsx（子）

```jsx
import React, { Component } from 'react';

export default class Count extends Component {

  // 加法
  increment = () => {
    const { value } = this.selectNumber;
    this.props.jia(value * 1);
  }

  // 减法
  decrement = () => {
    const { value } = this.selectNumber;
    this.props.jian(value * 1);
  }

  // 奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
    if (this.props.count % 2 !== 0) {
      this.props.jia(value * 1);
    }
  }

  // 异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
    this.props.jiaAsync(value * 1, 500);
  }

  render() {
    return (
      <div>
        <h1>当前求和为：{this.props.count}</h1>
        <select ref={c => this.selectNumber = c}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>&nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    )
  }
}
```

## 总结

![](resources/2024-10-21-20-58-39.png)

## 优化

### 优化编码

#### src/containers/Count/index.jsx

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

import {
    createIncrementAction,
    createDecrementAction,
    createIncrementAsyncAction
} from "../../redux/count_action";

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(
    state => ({ count: state }),
    dispatch => ({
        jia: number => dispatch(createIncrementAction(number)),
        jian: number => dispatch(createDecrementAction(number)),
        jiaAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time)),
    })
)(CountUI);
```

### 简写 mapDispatchToProps（源代码API层级的优化）

`mapDispatchToProps`是一个函数，也可以写成一个对象

#### src/containers/Count/index.jsx

不用自己写`dispatch`，`connect()()`会自动帮我们处理

```jsx
// 引入Count的UI组件
import CountUI from "../../components/Count";
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

import {
    createIncrementAction,
    createDecrementAction,
    createIncrementAsyncAction
} from "../../redux/count_action";

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(
    state => ({ count: state }),
    // mapDispatchToProps 的一般写法
    // dispatch => ({
    //     jia: number => dispatch(createIncrementAction(number)),
    //     jian: number => dispatch(createDecrementAction(number)),
    //     jiaAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time)),
    // })

    // mapDispatchToProps 的简写：对象
    {
        jia: createIncrementAction,
        jian: createDecrementAction,
        jiaAsync: createIncrementAsyncAction,
    }
)(CountUI);
```

#### 总结

![](resources/2024-10-21-21-25-33.png)

### 使用 Provider 组件

#### src/App.js

如果有很多容器组件，要给每个容器组件都传递 `store`，太麻烦了

```js
import React, { Component } from 'react';
import Count from './containers/Count';
import store from './redux/store';

export default class App extends Component {
  render() {
    return (
      <div>
        <Count store={store}/>
        <Demo store={store}/>
        <Demo store={store}/>
        <Demo store={store}/>
        <Demo store={store}/>
        <Demo store={store}/>
        <Demo store={store}/>
      </div>
    )
  }
}
```

删掉`store={store}`，不要自己一遍一遍传

```js
import React, { Component } from 'react';
import Count from './containers/Count';
import store from './redux/store';

export default class App extends Component {
  render() {
    return (
      <div>
        <Count/>
      </div>
    )
  }
}
```

#### src/index.jsx

在最外层使用 `Provider` 组件，给容器组件传递 `store`

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from './App';
import store from './redux/store';
import { Provider } from "react-redux";

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);

// 不需要自己检测redux中状态的改变了
```

### 整合UI组件和容器组件

删掉`components`目录及下面的文件
保留`containers`目录

#### src/containers/Count/index.jsx

合二为一

```jsx
import React, { Component } from 'react';
// 引入connect用于连接UI组件与redux
import { connect } from "react-redux";

import {
    createIncrementAction,
    createDecrementAction,
    createIncrementAsyncAction
} from "../../redux/count_action";

// 定义UI组件
class Count extends Component {

  // 加法
  increment = () => {
    const { value } = this.selectNumber;
    this.props.jia(value * 1);
  }

  // 减法
  decrement = () => {
    const { value } = this.selectNumber;
    this.props.jian(value * 1);
  }

  // 奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
    if (this.props.count % 2 !== 0) {
      this.props.jia(value * 1);
    }
  }

  // 异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
    this.props.jiaAsync(value * 1, 500);
  }

  render() {
    return (
      <div>
        <h1>当前求和为：{this.props.count}</h1>
        <select ref={c => this.selectNumber = c}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>&nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    )
  }
}

// 使用connect()()，创建并暴露一个Count容器组件
export default connect(
    state => ({ count: state }),
    // mapDispatchToProps 的简写：对象
    {
        jia: createIncrementAction,
        jian: createDecrementAction,
        jiaAsync: createIncrementAsyncAction,
    }
)(Count);
```

### 总结

![](resources/2024-10-21-21-59-30.png)

# 数据共享

上面的例子里，只有一个组件
下面实现多个组件（Count组件、Person组件）共享状态
继续使用上面的求和案例

## 调整目录结构

在`redux`目录下创建`reducers`文件夹，将`count_reducer.js`文件放入其中，重命名为`count.js`
在`redux`目录下创建`actions`文件夹，将`count_action.js`文件放入其中，重命名为`count.js`
并更改引入文件的路径

现在项目的目录结构如下
```sh
todolist/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    index.html
  src/
    App.js
    index.jsx
    containers/
      Count/
        index.jsx
    redux/
      reducers/
        count.js
      actions/
        count.js
      store.js
      constant.js
```

## 新增Person组件

在`containers`目录下创建`Person`文件夹，在其中创建`index.jsx`文件

现在项目的目录结构如下
```sh
todolist/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    index.html
  src/
    App.js
    index.jsx
    containers/
      Count/
        index.jsx
      Person/
        index.jsx
    redux/
      reducers/
        count.js
      actions/
        count.js
      store.js
      constant.js
```

### src/containers/Person/index.jsx

```jsx
import React, { Component } from 'react';

export default class Person extends Component {

    addPerson = () => {
        const name = this.nameNode.value;
        const age = this.ageNode.value;
        console.log(name, age);
    }
  render() {
    return (
      <div>
        <h2>Person组件</h2>
        <input ref={c => this.nameNode = c} type="text" placeholder='输入姓名'/>
        <input ref={c => this.ageNode = c} type="text" placeholder='输入年龄'/>
        <button onClick={this.addPerson}>添加</button>
        <ul>
            <li>姓名---年龄</li>
            <li>姓名---年龄</li>
            <li>姓名---年龄</li>
        </ul>
      </div>
    )
  }
}
```

### src/App.js

```js
import React, { Component } from 'react';
import Count from './containers/Count';
import Person from './containers/Person';

export default class App extends Component {
  render() {
    return (
      <div>
        <Count/>
        <hr/>
        <Person/>
      </div>
    )
  }
}
```

效果如下
![](resources/2024-10-23-22-26-40.png)

## 给Person组件准备一套redux

要准备好 **action**、**reducer**、**常量**

### src/redux/constant.js

```js
/*
    该模块是用于定义action对象中type类型的常量值，目的：便于管理的同时避免程序员写错字符串
*/

export const INCREMENT = 'increment';
export const DECREMENT = 'decrement';
export const ADD_PERSON = 'add_person';
```

### src/redux/actions/person.js

```js
import { ADD_PERSON } from "../constant";

// 创建增加一个Person的action动作对象
export const createAddPersonAction = (PersonObj) => ({ type: ADD_PERSON, data: PersonObj });
```

### src/redux/reducers/person.js

```js
import { ADD_PERSON } from "../constant";

// 初始化Person的列表
const initState = [{ id: 1, name: 'tom', age: 18 }];

export default function personReducer(preState = initState, action) {
    const { type, data } = action;
    switch (type) {
        // 添加一个Person
        case ADD_PERSON:
            // 新增的放到最前面
            return [data, ...preState];
        default:
            return preState;
    }
}
```

现在项目的目录结构如下
```sh
todolist/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    index.html
  src/
    App.js
    index.jsx
    containers/
      Count/
        index.jsx
      Person/
        index.jsx
    redux/
      reducers/
        count.js
        person.js
      actions/
        count.js
        person.js
      store.js
      constant.js
```

## 完成数据共享

### src/redux/store.js

#### 管理一个状态

redux如果只管理**一个状态**时，直接存储这个状态就行

```js
/*
    该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/ 

// 引入createStore，专门用于创建redux中最核心的store对象
import { createStore, applyMiddleware } from 'redux';
// 引入为Count组件服务的reducer
import countReducer from './reducers/count';
// 引入redux-thunk，用于支持异步action
import {thunk} from 'redux-thunk';
// 暴露store
// 为了处理异步，使用applyMiddleware并把thunk传入使用
export default createStore(countReducer, applyMiddleware(thunk));
```

#### 管理多个状态

redux要管理**多个状态**时，必须用**对象**去存储所有的状态

要**合并多个reducer到一个字典中**，需要用到**combineReducers**，它是一个函数，接收一个对象作为参数，对象的属性就是各个reducer

```js

```

### src/containers/Count/index.jsx

```jsx

```

### src/containers/Person/index.jsx

```jsx

```
















--- 

P111 15min

https://www.bilibili.com/video/BV1wy4y1D7JT?p=104



[代码](https://github.com/xzlaptt/React)
