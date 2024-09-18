# React 组件

## 模块与组件简介

### 模块（js模块）

理解：向外提供特定功能的js程序，一般就是一个js文件
为什么要拆成模块（只拆js）：随着业务逻辑增加，代码越来越复杂
作用：复用js，简化js的编写，提高js运行效率

#### 模块化

当应用的js都是以模块来编写的，这个应用就是一个模块化应用

### 组件

理解：用来实现局部功能效果的代码和资源的集合（html、css、js、img、video、font等等）
为什么要拆成组件（都拆）：页面的功能更复杂
作用：复用编码，简化项目编码，提高运行效率

#### 组件化

当应用是以多组件的方式实现，这个应用就是一个组件化应用

## 函数式组件（Function Component）

### 示例

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>React component</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        // 创建函数式组件
        function MyComponent() {
            console.log(this);  // 输出为undefined，因为在函数组件中写的是jsx语法, jsx语法要经过babel.js编译成js语法，而babel开启了严格模式
            return <h2>我是用函数定义的组件（适用于[简单组件]的定义）</h2>
        }
        // 渲染组件到页面
        ReactDOM.render(<MyComponent/>,document.getElementById('example'))
        // 执行ReactDOM.render(<MyComponent/>,document.getElementById('example')) 之后：
        // 1.React解析组件标签，找到了MyComponent组件
        // 2.发现组件是使用函数定义的，随后调用该函数
        // 3.将返回的虚拟DOM转为真实DOM，随后呈现在页面中
    </script>

</body>
</html>
```

注意，原生 HTML 元素名以小写字母开头，而自定义的 React 函数名以**大写**字母开头，比如 MyComponent 不能写成 myComponent，函数必须要有**返回值**，返回值就是VDOM

只要在 ReactDOM.render() 函数的参数中写好**组件标签** \<MyComponent/\>，React发现这个组件是使用函数定义的，会由React调用这个**函数**

效果如下
![](resources/2023-11-15-20-50-01.png)

console.log(this) 的输出为
![](resources/2023-11-15-22-10-49.png)
这里的 this 不是 Window 而是 undefined，是因为 jsx 代码要经过 babel 的翻译，babel 开启了**严格模式**（禁止自定义函数中的 this 指向 Window）

[babel在线编译器](https://babeljs.io/repl)

![](resources/2023-11-18-19-53-22.png)
jsx 其实就是 原始js 的**语法糖**

## 类式组件（Class Component）

### 原生js的类的复习

类的构造器方法和一般方法：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>类的基本知识</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/javascript">
        class Person {
            constructor(name,age){
                // 构造器中的 this 是谁？类的实例对象
                this.name = name
                this.age = age
            }
            // 一般方法
            speak(){
                // speak()方法放在了哪里？放在类的原型对象上，供实例使用
                // 通过Person实例调用speak()时，speak()中的this就是Person实例
                console.log(`我叫${this.name}，我的年龄是${this.age}`)
            }
        }
        const p1 = new Person('tom',18)
        const p2 = new Person('jerry',19)
        console.log(p1)
        console.log(p2)
        p1.speak()
        p2.speak()
        // call()可以更改函数中的 this 指向
        p1.speak.call({a:1,b:2})
    </script>

</body>
</html>
```

![](resources/2023-11-18-20-13-31.png)

类的继承：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>类的基本知识</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <script type="text/javascript">
        class Person {
            constructor(name,age){
                this.name = name
                this.age = age
            }
            speak(){
                console.log(`我叫${this.name}，我的年龄是${this.age}`)
            }
        }

        class Student extends Person {
            constructor(name,age,grade){
                // 要在构造器的最开始调用 super()
                super(name,age)
                this.grade = grade
            }
            // 重写从父类继承过来的方法
            speak(){
                console.log(`我叫${this.name}，我的年龄是${this.age}，我读的是${this.grade}年级`)
            }
            study(){
                console.log('我在努力学习')
            }
        }
        const s1 = new Student('zhang',15,'高一')
        console.log(s1)
        // 在原型链上查找这个方法
        s1.speak()
        s1.study()
    </script>

</body>
</html>
```

![](resources/2023-11-18-20-23-47.png)

总结：
1. 类中的构造器不是必须写的，要对实例进行一些初始化的操作时，如添加指定属性时，才写
2. 如果A类继承了B类，且A类中写了构造器，那么A类构造器中的super()必须要调用
3. 类中所定义的方法，都是放在了类的原型对象上，供实例去使用

### 示例

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>React component</title>
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>

    <div id="example"></div>
    <script type="text/babel">
        // 创建类式组件
        class MyComponent extends React.Component {
            render() {
                // render是放在哪里的？MyComponent的原型对象上，供实例使用，可是实例在哪里？
                // render中的this是谁？MyComponent的实例对象, 也称为 MyComponent组件实例对象
                console.log('render()中的this:',this);
                return <h2>我是用类定义的组件（适用于[复杂组件]的定义）</h2>
            }
        }
        // 渲染组件到页面
        ReactDOM.render(<MyComponent/>,document.getElementById('example'))
        // 执行ReactDOM.render(<MyComponent/>,document.getElementById('test')) 之后
        // 1.React解析组件标签，找到了MyComponent组件
        // 2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render()方法
        // 3.将render()返回的虚拟DOM转为真实DOM，随后呈现在页面中
    </script>

</body>
</html>
```

注意，自定义的 React 类名以**大写**字母开头，必须**继承** React.Component 这个父类，必须写 render() **方法**，且这个方法要有**返回值**，返回值就是VDOM

只要在 ReactDOM.render() 函数的参数中写好**组件标签** \<MyComponent/\>，React发现这个组件是使用**类**定义的，会由React创建出这个类的**实例**，并调用该实例的render()**方法**

效果如下
![](resources/2023-11-15-21-44-18.png)

console.log('render()中的this:',this) 的输出为
![](resources/2023-11-15-22-09-47.png)

## 复杂组件 和 简单组件

有 state 的组件是复杂组件，无 state 的组件是简单组件

> 类比：
> 人--状态--影响--行为
> 组件--状态--驱动--页面
> 我们的**数据**放在放在组件的 **state** 里边，状态改变驱动 **虚拟DOM** 改变，从而驱动页面上的 **真实DOM** 改变

- **函数式组件**也叫做**无状态组件**，因为函数式组件中的 this 指向 **undefined**
- **类式组件**也叫做**有状态组件**，因为定义类组件需要继承 React.Component 父类组件，类式组件中必须要有 render() 函数，render() 函数必须要有返回值，返回值就是VDOM。但是在类式组件的 render() 中的 this 指向 **类式组件创建的实例对象**。在这个实例对象中，有 **state 属性、props 属性、refs 属性**

### 新版React中的 hooks

通过 hooks，也能让函数式组件拥有 state 属性、props 属性、refs 属性 这三大属性

