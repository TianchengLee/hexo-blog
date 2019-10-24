---
title: React资料整理
date:  2019-01-18 14:50:04
tags:
---

## React

[TOC]

### 前置概念

> **库**:**小而精**,从功能上来说的话,就是专精某一项功能(jquery,axios)
>
> **框架**:**大而全**,从功能上来说就是面面俱到,在使用框架的过程中提供了用户所需要的完整的解决方案。(vue,bootstrap)
>
> **组件化**: 基于**UI界面**对代码进行封装重用
>
> **模块化**: 基于**业务逻辑**对代码进行封装重用

### React概念

#### 虚拟DOM

> DOM：浏览器中为了表现页面结构使用JS对象对元素进行模拟。
>
> 虚拟DOM: 框架中为了表示DOM元素使用JS对象对DOM进行模拟。
>
> **虚拟DOM的本质**: 就是JS的对象,用于模拟页面DOM的嵌套关系。
>
> **虚拟DOM的目的**: 实现页面的高效更新。
>
> **虚拟DOM实现高效更新的过程**:
>
> 1. 页面在第一次渲染时,内存中存在对象A(虚拟DOM)
> 2. 当页面发生变化时,在内存中生成一个新的对象B(虚拟DOM)
> 3. 对对象A和对象B进行对比,找出有差异的地方，存储到内存中C(虚拟DOM)
> 4. 在页面进行渲染时,只渲染C即可实现高效更新。

```:arrow_double_down:
// tree属性的对比 相当于TreeDIff
// 属性名称的对比,相当于ComponentDiff
// 属性值的对比, 相当于ElementDiff
const obj1 = {
	tree:'A'
	
    name:'zs',

}

const obj2 = {
	tree:'B'
	
	name:'ls'
   
}
```

#### Diff算法

> **TreeDIff**:最顶层的对比,判断连个虚拟DOM树类型是否一样,如果不一样，则直接渲染新虚拟DOM,如果一样，再实现ComponentDiff
>
> **ComponentDiff**:如果虚拟DOM类型一样,再对比组件类型是否一样,如果不一样,直接记录当前组件，接着对比下一个组件；如果一样，再实现ElementDiff
>
> **ElementDiff**：如果组件类型一样，再对比组件中的元素是否一样，如果不一样,记录下来,找到所有记录的内容(虚拟DOM)，最后渲染这个虚拟DOM

#### JSX语法

#### 单向数据流

### webpack4.0

#### 基本环境配置

```js
// 1. 创建项目文件夹
mkdir webpack-study
// 2. 使用npm初始化package.json
npm init
// 3. 进入项目文件夹创建对应的子文件夹和文件
mkdir src dist
touch webpack.config.js
// 4. 进入src目录创建index.html/index.js 
touch index.html index.js
// 5. 使用npm命令安装webpack webpack-cli
cnpm i webpack webpack-cli -D
// 6. 在webpack配置文件中书写如下内容
module.exports = {
  mode:"development"
}
// 7. 执行webpack命令进行打包(一定要确定是在当前项目根目录下面运行命令)
webpack
```

#### webpack-dev-server

> 是一个小型的Node服务器,用于托管当前项目中所有代码,同时监听文件的变化,自动进行打包编译

```js
// 1. 安装webpack-dev-server
npm i webpack-dev-server -D

// 2. 在package.json文件中加入以下代码
"scripts": {
    "dev":"webpack-dev-server --open --hot --port 8888"
}

// 3. 更改index.html中main.js导入的路径
<script src="/main.js"></script>

// 4. 在项目根目录运行命令启动项目
npm run dev
```

#### html-webpack-plugin

> 根据指定的模板文件生成一个内存中index.html文件存在于项目的根目录。同时可以在生成的这个文件中自动添加打包好的main.js文件。

```js
// 1. 安装html-webpack-plugin
npm i html-webpack-plugin -D
// 2. 在webpack.config.js中加入插件配置
const path  = require('path')
// 导入html-webpack-plugin
const HtmlWebpackPlugin  = require('html-webpack-plugin')
// 创建HtmlWebpackPlugin实例对象
const htmlWebpackPlugin = new HtmlWebpackPlugin({
  template: path.join(__dirname,'./src/index.html'), // 模板文件的绝对路径
  filename:'index.html'
})
// 3. 在module.exports导出对象中添加如下代码
plugins:[
   htmlWebpackPlugin
]
// 4. 去掉index.html中导入的main.js标签
// 5. 重新执行命令运行项目
npm run dev

```

### JSX语法

> 在JS中书写符合XML规范的语法
>
> JSX解析过程:遇到`<`以`HTML`规则解析,遇到`{}`以`JS`规则解析

#### 配置babel-loader解析jsx语法

```javascript
// 1. 安装相关的包-直接装以下包
yarn add babel-loader --dev
yarn add @babel/core --dev
yarn add @babel/plugin-transform-runtime --dev
yarn add @babel/plugin-proposal-class-properties --dev
yarn add @babel/preset-env --dev
yarn add @babel/runtime --dev
yarn add @babel/preset-react --dev

// 2. 在根目录下添加.babelrc文件，并书写以下代码
{
  "plugins": [
    "@babel/plugin-transform-runtime",
    "@babel/plugin-proposal-class-properties"
  ],
  "presets": [
    "@babel/preset-env",
     "@babel/react"
  ]
}

// 3. 在webpack.config.js中配置中书写以下代码
module:{
    rules:[
        {test:/\.js|jsx$/,use:'babel-loader',exclude:/node_modules/}
    ]
}
```



#### 集成react渲染虚拟DOM

```javascript
// 1. 安装react、react-dom
// yarn add react react-dom / cnpm react react-dom -S

// 2. 导入react、react-dom
import React from 'react'
import ReactDOM from 'react-dom'

// 3. 使用react提供的API创建虚拟DOM
// const mydiv = React.createElement('div',{className:'box'},'这是虚拟DOM对象')
const mydiv = <div className="box">
  这是虚拟DOM对象
  {list.map((v,i) => <p key={i}>{v}</p>)}
</div>

// 4. 使用react-dom提供的API渲染虚拟DOM
ReactDOM.render(<Hello username={obj.name} age={obj.age} />,document.getElementById('app'))

```



### 组件

#### 无状态组件

> 无状态组件：通过构造函数方式创建的组件,组件内部没有生命周期函数和私有数据。
>
> 无状态组件是一个**纯函数**
>
> **纯函数**：函数内部对接收到数据不做任何处理,这样的函数称作纯函数

- 创建方式

```jsx
// 以构造函数方式创建
function Hello(){
    return (
        <div>
            <h3>构造函数方式创建的组件</h3>	
        </div>
    )
}
```

- 使用

```jsx
// 以标签的方式使用
ReactDOM.render(<Hello />,document.getElementById('app'))
```

- 传值

```jsx
// 形参props用于接收传递的属性
function Hello(props){
    return (
        <div>
            <h3>构造函数方式创建的组件</h3>
            <p>{props.name}</p>
            <p>{props.age}</p>
        </div>
    )
}

const obj = {
  name:'zs',
  age:18
}

//ReactDOM.render(<Hello username={obj.name} age={obj.age} />,document.getElementById('app'))

// 属性扩散(使用es6的展开运算符进行传值)
ReactDOM.render(<Hello {...obj} />,document.getElementById('app'))
```



##### 配置webpack省略文件后缀和项目根目录

```js
// 在webpack.config.js中添加如下配置
resolve:{
    extensions:['.js','.jsx','.json'], // 省略.jsx后缀名
    alias:{
      "@":path.join(__dirname,'./src') // 配置src目录的绝对路径为@
    }
}
```



#### 有状态组件

> 有状态组件：通过class关键字创建的组件,组件内部可以定义生命周期函数和私有数据。

```jsx
class Home extends React.Component {
    constructor(){
        super()
        // 定义私有数据
        this.state = {}
    }
      render(){
        return <div>
          <h3>这是class关键字创建的组件</h3>
        </div>
      }
}
```



### class/extends关键字

> 实例成员:
>
> 1. 实例属性：通过实例对象进行访问的属性叫做实例属性
> 2. 实例方法：通过实例对象调用的方法叫做实例方法
>
> 静态成员:
>
> 1. 静态属性：通过构造函数或者类名进行访问的属性叫做静态属性
> 2. 静态方法：通过构造函数或者类名调用的方法叫做静态方法
>
> `extends`实现继承:
>
> 1. 在子类内部如果书写了`constructor,``constructor`内部必须调用`super`函数
> 2. 在子类里面如果需要传参,需要在`constructor`定义形参,在`super`上传入形参
>
> 注意:
>
> 1.  调用子类的`super`其实就是在调用父类的`constructor`
>
> 2.  `super`函数内部会创建父类的`this`，同时会将父类的`this`指向子类的实例

#### 构造函数方式创建对象

```js
function Person(name,age){
    this.name = name
    this.age = age
}

// 静态属性(直接挂载到构造函数上面)
Person.count = "60亿"

// 静态方法
Person.run = function(){
    conso.log('run')
}

// 实例方法
Person.prototype.say = function(){
    console.log('hello')
}

const p = new Person('李白',1000)
console.log(Person.count)

---继承---
    
function Chinese(){
    
}

// 原型继承(让子类的原型指向父类的一个实例)
Chinese.prototype = new Person()

const c = new Chinese()


```

#### Class类方式创建对象

> `class`类的花括号不是一个对象，内部只允许写函数以及使用`static`关键字定义静态属性或者静态方法

```js
class Person {
    // 构造器
    constructor(name,age){
        this.name = name
        this.age = age
    }
    
    // 静态属性(通过static关键字创建静态属性)
    static count = "60亿"

	// 静态方法
    static run(){
		console.log('run')
    }

	// 实例方法
    say(){
		console.log('hello')
    }
}

const p = new Person('李白',1000)
p.say()
console.log(Person.count)
Person.run()


class Chinese extends Person{
    
}
const c = new Chinese('韩信',2000)
c.say()

Chinese.run()
```



### CSS-IN-JS 样式

#### style行内样式

> 注意:
>
> 1. 行内样式`style`属性后面跟的是一个js对象,所以需要时`{}`进行包裹
> 2. 组件的样式属性都以键值对的方式在样式对象中进行描述
> 3. 对于带中划线的样式属性,需要改为驼峰命名或者把属性名用引号引起了

```jsx
render(){
    return <div style={{color:'red',backgroundColor:'pink','font-size':'16px'}}>
        绝云气,负青天
    </div>
}
```

#### css文件书写样式

```js
// 1. 安装解析css文件的加载器
yarn add style-loader css-loader --dev

// 2. 在webpack.config.js中添加解析后缀为.css文件的加载器解析规则
{
  test:/\.css$/,
  use:['style-loader','css-loader']
  // css-loader 解析后缀为.css的文件,读取出css文件中的样式代码
  // style-loader 将样式代码插入到html中成为内联样式
}
// 3. 书写css文件,并在组件中导入使用
import cssobj from './css/index.css'

```

##### 给css文件启用模块化

> 注意:
>
> 1. **模块化只能作用在类名选择器和id选择器上面,对于标签选择器不生效。**
> 2. **如果id选择器和类名选择器同名,会安装权重使用id选择器的样式**
> 3. 在启用模块化后,css文件中的所有样式默认都是会被模块化处理,可以使用`:global(选择器{})`这种方式将某些样式成为具有全局作用域的样式。*`:local(选择器{})`也可以单独指定某个选择器具有单独作用域*

```js
// 1. 在配置解析规则时添加models参数启用模块化
{
  test:/\.css$/,
  use:['style-loader','css-loader?modules']
}
// 2. 在导入css文件时用对象进行接收
import cssobj from './css/index.css'

// 3. 使用样式时,类名和id选择器的名称都是导入的对象属性
<div className={cssobj.title} id={cssobj.box}></div>
```

##### 使用localIdendName参数处理生成的类名

```js
// 1. 在解析规则后面再加上localIdentName参数
{
  test:/\.css$/,
  use:['style-loader','css-loader?modules&localIdentName=[path][name]-[local]-[hash:5]']
}
// 2. 参数代表的意思
// [path]:样式表文件相对于根目录的相对路径
// [name]:样式表文件的名称
// [local]:样式表文件中选择器的名称
// [hash:5]:随机5位字符串,防止重复
```

##### 解析less

```js
// 1. 安装less less-loader
yarn add less less-loader --dev
// 2. 在webpack.config.js中添加解析规则
{
    test:/.less$/,
    use:['style-loader','css-loader','less-loader']
}
```

##### 解析scss

```js
// 1. 安装node-sass sass-loader
yarn add node-sass sass-loader --dev
// 2. 在webpack.config.js中添加解析规则
{
    test:/.scss$/,
    use:['style-loader','css-loader','sass-loader']
}
```

##### 解析字体图标或者图片

```js
// 1. 安装file-loader url-loader
yarn add file-loader url-loader --dev

// 2. 在webpack.config.js中添加解析规则
{
    test:/\.ttf|woff|svg|woff2|eot$/, // 解析字体图标
    // test:/\.png|jpg|bmp|gif$/,
    use:'url-loader?limit=5000'
    // 如果文件被打包成base64的字符串,只会使用url-loader
    // 如果文件被打包成为路径嵌入到js中,会使用url-loader和file-loader
}
```



### 事件绑定

> 注意: 
>
> 1. 绑定事件时必须使用箭头函数进行包裹业务逻辑函数,目的是为了传参方便
> 2. 定义业务逻辑处理函数必须使用箭头函数进行定义,目的是让函数内部this总是指向当前组件实例

```js
// 1. 绑定事件使用空的箭头函数包裹自己定义的一个实例方法
<button onClick={ ()=>{ this.btnClick(123) }}>按钮</button>
// 2. 定义实例方法的时候,使用箭头函数进行定义
btnClick = (arg)=>{
    console.log(this.state.msg)
}
```

### 单向数据流

#### setState()数据更新

> 在react中如果需要修改state中的数据,需要使用`this.setState({})`进行修改，只有这种方式对数据进行修改之后，可以触发视图的更新。
>
> 注意: `this.setState({})`方法是一个**异步方法**,如果需要在修改数据之后离开对数据进行使用，需要提供第二个参数-回调函数。

```js
btnClick = (arg)=>{
     // this.setState({
     //   msg:'hello'
     // })
    
    this.setState({
      msg:123
    },function(){
      console.log(this.state.msg)
    })
}
```

#### 受控表单

> React中的表单元素只要添加了value属性,就不能由用户在界面中对表单的值进行修改,必须通过JS代码对数据进行修改,这种表单就叫做**受控表单**
>
> 单向数据流里面如果希望实现类似于双向数据绑定额功能,其实主要就是需要程序员自己手动写代码去实现视图到模型的数据更新这一块功能。
>
> 实现思路:一般都是监听表单的值改变事件(onChange)，在这个事件中将视图中的数据赋值给模型中成员。

```jsx
render(){
    return <div>
    	<input value = {this.state.msg} onChange = { (e) => this.valueChange(e) }/>
    </div>
}

valueChange(e){
    // 1. 获取用户输入的文本框内容
    const value = e.taget.value
    // 2. 使用this.setState({})修改msg的数据
    this.setSatte({
        msg:value
    })
}
```

#### ref 获取DOM引用

> 如果在开发中需要操作DOM,可以改DOM标签添加一个ref属性,属性值是字符串,然后通过`this.refs.xxx`可以直接获取当前这个DOM对象的引用。

```jsx
render(){
    return <div>
    	<input ref = 'inp' value = {this.state.msg} onChange = { () => this.valueChange() }/>
    </div>
}

valueChange(e){
    // 1. 获取用户输入的文本框内容
    // 1.1 使用this.refs.inp获取到DOM引用
    const target = this.refs.inp
    // 1.2 再通过DOM对象的value属性获取到文本框的值
    const value = taget.value
    // 2. 使用this.setState({})修改msg的数据
    this.setSatte({
        msg:value
    })
}
```



### 生命周期

#### 属性校验

> 属性校验是在组件内部对传进来的外界属性的类型进行校验,主要是为了增加程序的健壮性，当传递数据的类型不符合要求时,可以在控制台做出警告提示。

```jsx
// 1. 安装prop-types模块并导入
yarn add prop-types
import Types from 'prop-types'

// 2. 在组件内部定义静态属性propTypes
class Com extends React.Componnet{
 
    // 2.1 静态属性 propTypes 设置需要校验的属性
    static propTypes = {
        // count 这个属性值只接受数值类型
        count:Types.number,
        msg:Types.string.isRequired
    }

	// 2.2 静态属性 defaultProps 设置需要校验的属性的默认值
    static defaultProps = {
		count:0,
        msg:'hello'
    }

	// 构造函数(不属于生命周期函数)
    constructor(){
		super()
        this.state = {
            
        }
    }
}
```



#### 生命周期函数

```jsx
class Com extends React.Componnet{
	// 构造函数(不属于生命周期函数)
    constructor(){
		super()
        this.state = {
            
        }
    }
    
    /* 组件的创建阶段(3个函数) */ 
    // 执行时机: 组件在内存中被创建出来，然后所有的属性和实例方法都已经挂载完毕
    // 特点(只执行1次): 可以访问当前组件的属性和方法,通常用于数据初始化和发送异步请求(类似vue的created)
    componentWillMount(){
        
    },
    // 执行时机：当前组件需要被渲染到真实页面时执行,此时仅仅只是在内存中建立好了虚拟DOM对象
    // 特点(1次以上): 用于创建当前组件的DOM结构
    render(){
        return (
            <div>
                jsx虚拟DOM
            </div>)
    },
    // 执行时机: 数据全部挂载完毕,同时所有的虚拟DOM都已经被渲染成真实DOM
    // 特点(只执行1次)：可以在这个函数中操作真实DOM(类似vue中的mounted)
    componentDidMount(){

    },
    /* 组件的运行阶段(4个函数) */ 
    // 执行时机: 组件所接收的外界数据props发生变化的时候触发
    // 特点: 在这个函数中可以通过形参nextProps获取到最新的props数据
    componentWillReceiveProps(nextProps){
		// this.props里面是旧值
        // nextProps里面是最新的值
    },
    // 执行时机: state或props中的数据发生变化时会触发该函数
    // 特点: 根据该函数内部return值决定组件是否需要被重新更新(渲染)
    shouldComponentUpdate(nextProps,nextState){
        // this.state.xxx 旧值
        // nextSatte 新值
        // this.props.xxx 旧值
        // nextProps 新值
        // return true会执行后续生命周期函数,会重新渲染组件
        // return false不会执行后续生命周期,组件不会重新渲染
		return true
    },
    // 执行时机: 当shouldComponentUpdate内部return true时会执行该函数
    // 特点: 当前组件的状态(props,state)已经是最新值,但是DOM中绑定的数据还是旧值，即DOM还没有被更新(类似vue中的beforeUpdate)
    componentWillUpdate(){
		// this.state.xxx 新值
        // this.props.xxx 新值
        // 真实DOM中的数据还是旧的
    },
        
	// 注意: componentWillUpdate 执行完毕之后会立刻重新执行 render 函数,然后将最新数据渲染到虚拟DOM中
        
	// 执行时机: 虚拟DOM被渲染到真实DOM中会执行该函数
   	// 特点: 组件的状态和当前DOM界面都是最新的,已经完成了同步(类似vue中的updated)
    componentDidUpdate(){
		// this.state.xxx 新值
        // this.props.xxx 新值
        // 真实DOM中的数据还是新的
    },
        
    /* 组件销毁阶段(1个函数) */
    // 执行时机: 组件即将被销毁,但是还没有被销毁时执行
    // 特点: 还可以继续访问组件的属性和方法,在这个函数中可以做一些数据的清理工作,防止内存泄露
    componentWillUnmount(){

    }
}
```



### promise的封装

```js
// 1. 定义一个函数,函数内部return一个promise实例
function fetch(){
    return new Promise(function(resolve,reject){
      // 2. 在promise实例内部执行真正的异步操作(网络请求、定时器)
      $.ajax({
        url:'http://www.lovegf.cn:8899',
        success:function(data){
          // 2.1 异步操作成功时,调用resolve函数将数据返回
          resolve(data)
        },
        fail:function(err){
          // 2.2 异步操作失败时,调用reject函数将错误返回
          reject(err)
        }
      })
    })
}
// 3. 调用封装好的函数时使用.then接收数据
fetch().then(function(data){
	consloe.log(data)
},function(err){
	consloe.log(data)
}).catch(function(){

})

// 4. 使用async和await 简化promise的调用
// 4.1. 定义一个使用async修饰的函数
async fetch_async(){
    // 4.2. 在这个函数内部使用await修饰能够返回promise实例的函数的调用
    const res = await fetch()
    console.log(res)
}
```

### React路由

> `HashRouter` 用于指定当前项目中使用哪一种路由模式
>
> `Route` 用于配置路由规则,同时当做路由占位符
>
> `Link` 用于设置路由跳转链接，使用to属性指定跳转路径
>
> ---
>
> 注意:
>
> 1. `HashRouter`用于指定路由模式,在项目中只使用一次,而且其内部只能有一个根元素
> 2. `Route`用于配置路由规则,同时也是路由占位符
> 3. `exact`属性可以设置路由规则被精确匹配

#### 基本使用

```jsx
// 1. 安装react-router-dom
yarn add react-router-dom

// 2. 在项目根组件中导入路由组件
import { HashRouter, Route, Link } from 'react-router-dom'

import Hello from '@/components/Hello.jsx'
import CommentList from '@/components/CommentList.jsx'

// 3. 使用Route创建路由规则
render(){
    return <HashRouter>
        <div>
        	<Link to='/home'>首页</Link>
            <Link to='/list'>列表</Link>
            
            <Route path='/home' component={Home}></Route>
            <Route path='/list' component={List}></Route>
        </div>
    </HashRouter>
}
```



#### 路由传参

```jsx
// 方式一:params传参
// 1. 设置跳转路径(12345是实参)
<Link to='/list/12345'>列表</Link> 或者
this.props.hitory.push('/list/12345')
// 2. 设置路由规则(:id是形参)
<Route path='/list/:id/:type/:tag' component={List}></Route>
// 3. 在路由对应的组件中获取参数
console.log(this.props.match.params.id) // 12345
    
// 方式二:query传参
// 特点:不需在再路由规则中添加形参;在地址栏中看不到传递的参数
// 1. 设置跳转路径
<Link to={{pathname:'/list',query:{id:12345,type:'movie'}}}>列表</Link> 或者
this.props.hitory.push({pathname:'/list',query:{id:12345,type:'movie'}})
// 2. 设置路由规则
<Route path='/list' component={List}></Route>
// 3. 在路由对应的组件中获取参数
console.log(this.props.location.query.type) // movie
    
// 方式三:state传参
// 特点:不需要在路由规则中添加形参;在地址栏中看不到传递的参数；可以将参数缓存,刷新页面参数也会存在
// 1. 设置跳转路径
<Link to={{pathname:'/list',state:{id:12345,type:'movie'}}}>列表</Link> 或者
this.props.hitory.push({pathname:'/list',state:{id:12345,type:'movie'}})
// 2. 设置路由规则
<Route path='/list' component={List}></Route>
// 3. 在路由对应的组件中获取参数
console.log(this.props.location.state.type) // movie

// 方式四:search传参
<Link to={{pathname:'/list?id=12345&type=movie'}}>列表</Link> 或者
this.props.hitory.push({pathname:'/list?id=12345&type=movie')
// 2. 设置路由规则
<Route path='/list' component={List}></Route>
// 3. 在路由对应的组件中获取参数
console.log(this.props.location.search) // ?id=12345&type=movie
// id=12345&type=movie
// [id=12345,type=movie]
// {id:12345,type:'movie'}

```

#### 路由重定向

```jsx
// 1. 导入路由重定向组件
import { HashRouter, Route, Link, Redirect } from 'react-router-dom'

// 2. 使用Route组件上的render函数实现重定向
render(){
    return <HashRouter>
        <div>
        	<Link to='/home'>首页</Link>
            <Link to='/list'>列表</Link>
            
          	{/* 访问根路径 / 时 会跳转到 /home*/}  
            <Route path='/' render={() => <Redirect to='/home'/>} exact></Route>
            
            <Route path='/home' component={Home} exact></Route>
            <Route path='/list' component={List} exact></Route>
        </div>
    </HashRouter>
}
```

####  路由嵌套

> React路由嵌套实现的是每个组件中都可以定义路由规则
>
> A组件对应的路由是 '/home' `<Route path='/home' component={A}></Route>`
>
> A组件内部如果又定义了路由规则  `<Route path='/newslist' component={B}></Route>`
>
> 路由规则 `/newslist` 是`/home`的子路由,同时B组件也是A组件的子组件。



---

```js
作业一:
使用antd-movie搭建移动端项目(http://vue.lovegf.cn) 不需要用嵌套路由
作业二:
使用antd搭建pc端后台管理系统(http://oa.lovegf.cn) 必须要用到嵌套路由
```



### fetch

> `fetch`是window对象下的一个新的API,和`XMLHttpRequest`作用一样,是专门用来发送异步请求。
>
> fetch特点:
>
> 1. 是基于promise封装的,语法简洁
> 2. `fetch`支持同构(跨平台)，可以在移动设备上运行

#### GET请求

```js
fetch('http://www.lovegf.cn:8899/api/getlunbo').then(res=>res.json())
    .then(data=>{
    console.log(data)
})
```

#### POST请求

```js
// application/x-www-form-urlencoded // name=zs&age=18
// application/json // {name:'zs',age:18}

fetch('http://www.lovegf.cn:8899/api/addproduct',{
    method:"POST",
    body:{
        content:'zs'
    }
}).then(res=>res.json())
    .then(data=>{
    console.log(data)
})
```



### create-react-app

```js
// 1. 全局安装create-react-app
yarn global add create-react-app 或者 cnpm i create-react-app -g

// 2. 初始化项目模板
create-react-app react_project

// 3. 运行项目
yarn start
```



---

### jsx有状态组件代码片段

> 将以下代码复制到javascriptreact.json文件中

```json
{
	"jsx": {
		"prefix": "jsx",
		"body": [
			"import React from 'react'\r",
			"\r",
			"export default class ${1:ComName} extends React.Component {\r",
			"  constructor(){\r",
			"    super()\r",
			"    this.state = {\r",
			"       $3\r",
			"    }\r",
			"  }\r",
			"  render(){\r",
			"    return <div>\r",
			"        绝云气,负青天$4\r",
			"    </div>\r",
			"  }\r",
			"}"
		],
		"description": "创建react有状态组件"
	}
}
```



