#react是声明式
	声明式：只关注结果，不注重结果
	
	命令式：关注每一步的结果和过程。

#react特点
	* 高效：最小化重绘重排，先渲染虚拟dom，在与真实dom进行对比，最后只渲染真实dom中改变的部分。
	* 跨平台
	* 声明式
	* 组件化
	* 单向数据量：数据只能沿着 M--V--C 流动

#MVC架构
	M---module
	V---view
	C---control
#JSX

  1). 全称: JavaScript XML

  2). react定义的一种类似于XML的JS扩展语法: XML+JS

  3). 作用: 用来创建react虚拟DOM(元素)对象

    * var ele = <h1>Hello JSX!</h1>;
    * 注意1: 它不是字符串, 也不是HTML/XML标签
    * 注意2: 它最终产生的就是一个JS对象

  4). 标签名任意: HTML标签或其它标签

  5). 标签属性任意: HTML标签属性或其它

  6). 基本语法规则

    * 遇到 <开头的代码, 以标签的语法解析: html同名标签转换为html同名元素, 其它标签需要特别解析
    * 遇到以 { 开头的代码，以JS的语法解析: 标签中的js代码必须用{}包含

  7). babel.js的作用

    * 浏览器的js引擎是不能直接解析JSX语法代码的, 需要babel转译为纯JS的代码才能运行
    * 只要用了JSX，都要加上type="text/babel", 声明需要babel来处理

#两个创建虚拟dom对象的方式

* `var element = React.createElement('h1', {id:'myTitle'}, 'hello');`

* `let element = <h2>hello react</h2>`

#渲染虚拟dom

	  1). 语法: ReactDOM.render(virtualDOM, containerDOM) :
	  2). 作用: 将虚拟DOM元素渲染到真实容器DOM中显示
	  3). 参数说明
	    * 参数一: 纯js或jsx创建的虚拟dom对象
	    * 参数二: 用来包含虚拟DOM元素的真实dom元素对象(一般是一个div)

#什么是组件？

定义：需要交互的设计成组件

	  1). 定义组件
	    //方式1: 工厂(无状态)函数(定义简单的组件推荐使用)
	    function MyComponent1() {
	      return <h1>自定义组件标题11111</h1>
	    }
	    //方式2: ES6类语法(定义复杂的组件推荐使用)
	    class MyComponent3 extends React.Component {
	      render () {
	        return <h1>自定义组件标题33333</h1>
	      }
	    }

  	 2). 渲染组件标签
      ReactDOM.render(<MyComponent/>, document.getElementById('example'));


#创建虚拟dom与组件注意点

	* 渲染规则为将容器中的所有元素清除再渲染
	* 创建的虚拟dom标签必须有结束标签
	* 创建的组件虚拟dom标签必须开头字母大写
	* 如果有多个虚拟dom标签的话，必须用一个容器将他们包起来
	* style=‘’的时候，用两个大括号 style = {{}}

#组件的三大属性：props/state/refs

###1、state
>
 	1). 组件被称为"状态机", 通过更新组件的状态值来更新对应的页面显示(重新渲染) 
    2). 初始化状态:
      constructor (props) {
        super(props);
        this.state = {
          stateProp1 : value1,
          stateProp2 : value2
        };
      }
    3). 读取某个状态值
      this.state.statePropertyName
    4). 更新状态---->组件界面更新
      this.setState({
        stateProp1 : value1,
        stateProp2 : value2
      })
###2、props

	  1. 每个组件对象都会有props(properties的简写)属性
	  2. 组件标签的所有属性都保存在props中
	  3. 内部读取某个属性值: this.props.propertyName
	  4. 作用: 通过标签属性从组件外向组件内传递数据(只读)
	  5. 使用PropTypes对props进行类型检查
	    Person.propTypes = {
	      name: PropTypes.string.isRequired,
	      age: PropTypes.number.isRequired
	    }
	  6. 扩展属性: 将对象的所有属性通过props传递
	    <Person {...person}/>
	  7. 默认属性值
	    Person.defaultProps = {
	      name: 'Mary'
	    };
	  8. 组件类的构造函数
	    constructor (props) {
	      super(props);
	      console.log(props); // 查看所有属性
	    }

>
>props和states区别

	props从组件外向组件内传递数据,组件内部的数据只读不可修改
	
	states组件数据内部的传递，只有组件内部的数据使用，其他的地方不使用

###3、refs

      1). 组件内的标签都可以定义ref属性来标识自己
      2). 在组件中可以通过this.refs.refName来得到对应的真实DOM对象
      3). 作用: 用于操作指定的ref属性的dom元素对象(表单标签居多)
        * <input ref='username'>
        * this.refs.username //返回input对象
    2. 事件处理
      1). 通过onXxx属性指定组件的事件处理函数(注意大小写)
        * React使用的是自定义(合成)事件, 而不是使用的DOM事件
        * React中的事件是通过委托方式处理的(委托给组件最外层的元素)
      2). 通过event.target得到发生事件的DOM元素对象
        <input onFocus={this.handleClick}/>
        handleFocus(event) {
          event.target  //返回input对象
        }
    3. 强烈注意
      1). 组件内置的方法中的this为组件对象
      2). 在组件中自定义的方法中的this为null
        * 强制绑定this
        * 箭头函数(ES6模块化编码时才能使用)
    4. 问题: 如何给一个函数强制指定内部的this?
    	call/apply  其中call绑定的是立即执行函数

# **功能界面的组件化编码流程(无比重要)**

	1)拆分组件: 拆分界面,抽取组件
	2)实现静态组件: 使用组件实现静态页面效果
	3)实现动态组件
	a.动态显示初始化数据
	b.交互功能(从绑定事件监听开始)

#组件生命周期

1、理解：

	1)组件对象从创建到死亡它会经历特定的生命周期阶段
	2)React组件对象包含一系列的勾子函数(生命周期回调函数), 在生命周期特定时刻回调
	3)我们在定义组件时, 可以重写特定的生命周期回调函数, 做特定的工作

组件声明周期图示：

----------

![组件声明周期图示](https://i.imgur.com/lOviJqY.png)



----------

2、生命周期详述

	1)组件的三个生命周期状态:
	    * Mount：插入真实 DOM
	    * Update：被重新渲染
	    * Unmount：被移出真实 DOM
	2)React 为每个状态都提供了勾子(hook)函数
	    * componentWillMount()
	    * componentDidMount()
	    * componentWillUpdate()
	    * componentDidUpdate()
	    * componentWillUnmount()
	3)生命周期流程:
	a.第一次初始化渲染显示: ReactDOM.render()
	      * constructor(): 创建对象初始化state
	      * componentWillMount() : 将要插入回调
	      * render() : 用于插入虚拟DOM回调
	      * componentDidMount() : 已经插入回调
	b.每次更新state: this.setSate()
	      * componentWillUpdate() : 将要更新回调
	      * render() : 更新(重新渲染)
	      * componentDidUpdate() : 已经更新回调
	c.移除组件: ReactDOM.unmountComponentAtNode(containerDom)
	      * componentWillUnmount() : 组件将要被移除回调
	      
3、重要的勾子

	1)render(): 初始化渲染或更新渲染调用
	2)componentDidMount(): 开启监听, 发送ajax请求
	3)componentWillUnmount(): 做一些收尾工作, 如: 清理定时器
	4)componentWillReceiveProps(): 后面需要时讲

	componentWillReceiveProps ：当props更新的时候会被调用这个方法。
	
	在componentWillReceiveProps方法里使用this.setState()，能够在调用render()之前更新状态，来对prop转换做出反应。 旧的props可以通过this.props访问。 在这个函数中调用this.setState()不会触发额外的渲染。
	
	新的属性的设置 this.setState   获取旧的属性 this.props