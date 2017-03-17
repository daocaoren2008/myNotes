##1. 什么是react
React 是一个用于构建用户界面的JavaScript库
##2. 安装react
需要先安装bower
```
npm install bower -g
```
然后通过bower安装react
```
$ bower install react babel --save
```
##3. 直接在浏览器中使用React
```
<script src="./lib/react.js"></script>
<script src="./lib/react-dom.js"></script>
<script src="./lib/browser.js"></script>
<script type="text/babel" src="index.js"></script>
```
- react.js 是 React 的核心库
- react-dom.js 是提供与DOM相关的功能,会在window下增加ReactDOM属性
- browser.js 的作用是将JSX语法转为JavaScript语法
**script中的type属性为text/babel,因为React独有的JSX语法,跟JavaScript不兼容**
##4. JSX 语法
是一种JS和HTML混合的语法,将组件的结构、数据甚至样式都聚合在一起定义组件,会编译成普通的Javascript。
- 遇到HTML标签(以 < 开头)，就用HTML规则解析
- 遇到代码块(以 { 开头)，就用JavaScript规则解析
- 使用样式时可以让style等于一个样式对象
- 使用样式类时只能使用className=类名,因为class是Javascript关键字
```
var persons = ['刘德华', '范冰冰', '郭跃'];
var style = {color:'red'};
ReactDOM.render(
  <div>
  {
    persons.map(function (person) {
      return <div style={style}>Hello, {person}!</div>
    })
  }
  </div>,
  document.getElementById('app')
);
```
##5. ReactDOM.render
> **ReactDOM.render** 是 React的**最基本方法**,用于将标签模板转为HTML语言，并插入指定的DOM节点
- 下面代码将一个h1标题，插入app元素内部
```
    ReactDOM.render(
    <h1>珠峰培训</h1>,
        document.getElementById('app')
    );
```
##6.组件
React允许将代码封装成组件，然后像插入普通HTML标签一样，在网页中插入这个组件。

我们可以将一个复杂的页面分割成若干个独立组件,每个组件包含自己的逻辑和样式 再将这些独立组件组合完成一个复杂的页面。 这样既减少了逻辑复杂度，又实现了代码的重用
###6.1组件的特点
- 可组合：一个组件可以和其他的组件一起使用或者可以直接嵌套在另一个组件内部
- 可重用：每个组件都是具有独立功能的，它可以被使用在多个场景中
- 可维护：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护

###6.2定义组件
- 组件类的**第一个字**母必须大写
- 组件类能且只能包含**一个顶层标签**
- 组件其实是一个类
- 组件里有一个render方法，必须返回一个虚拟DOM元素
> 如果react渲染的时候发现是首字母大写，会把它当做一个组件，**实例化此组件**，并调用他的render方法得到一个react**虚拟DOM元素**。
```
var Message = React.createClass({
    render: function() {
        return <h1>Hello</h1>;
    }
});
ReactDOM.render(
    <Message/>,
    document.getElementById('app')
);
```
###6.3 组件的属性
- 每个组件可以有自己的属性,一般用来存放组件初始后**不变**的数据,比如人的性别，姓名等
- 属性一般用作组件的**数据源**，一般由父组件传入,比如你的名字一般是由你父母取的
- 属性可以通过this.props中取出
- propTypes可以用来定义传入组件属性的名称和类型
- getDefaultProps函数可以用来定义组件的属性默认值，可以被在标签中传入的属性值覆盖
```
var Person = React.createClass({
    //类似于约定了一个接口文档,用于这是验证传递给组件的属性，
    propTypes: {
        //定义name的属性类型为字符串，必须传入
        name: React.PropTypes.string.isRequired,
        gender: React.PropTypes.string.isRequired,
        age:React.PropTypes.number.isRequired
    },
    getDefaultProps:function(){
        return {name:'无名氏'}
    },
    render: function() {
        //属性可以通过属性对象this.props中取出
        return (<h1> {this.props.name}
                     {this.props.gender}
                     {this.props.age}
                </h1>);
    }
});

var props = {
    gender:'男',
    age:18
}

ReactDOM.render(
    <Person {...props} />,//属性可以在使用组件时传入，es6语法传入对象多个属性
    document.getElementById('app')
);
```
###6.4 this.props.children
**this.props**对象的属性与组件实例的属性一一对应,但**this.props.children**属性表示组件的所有子节点 **React.Children.map**是一个工具方法，用于实现对数组元素的映射
- 当组件的子节点只有1个时this.props.children是一个**对象**。当子节点超过一个时this.props.children是一个**数组**。所以使用React.Children.map可处理这种类型不一致的问题，只把它当成数组就行了。
```
var Person = React.createClass({
    render: function() {
      return (
            <ol>
                {
                    React.Children.map(this.props.children,
                      function (child) {
                        return <li>{child}</li>;
                    })
                }
            </ol>
      );
    }
});

ReactDOM.render(
    <Person>
        <span>大毛</span>
        <span>二毛</span>
        <span>三毛</span>
    </Person>,
    document.getElementById('app')
);
```
###6.5 state状态
- 组件的状态就像人的心情，会经常变化，而且只能由自己来改变
- 组件一开始有一个初始状态,然后用户互动,导致状态变化，从而触发界面重新渲染
- getInitialState用来定义初始状态
- 可以给按钮绑定事件，当事件发生的时候调用对应的方法改变组件的状态
```
var Bind = React.createClass({
    //1.获取初始状态对象  this.state= this.getInitialState();
    //状态只能内部初始化，只能组件内部改变
    getInitialState(){
     return {val:''}
    },
    //在事件处理函数中，有一个事件对象参数。
    //事件源 值
    handleChange(event){
     //设置状态对象.必须 调用setSetState方法，因为只调用了此方法才会出发render方法的重新渲染
      this.setState({val:event.target.value});
    },
    render(){
        return (
            <div>
                <p>{this.state.val}</p>
                //此处事件不同于dom事件，这里遵守的是驼峰命名法
                <input type="text" value={this.state.val} onChange={this.handleChange}/>
            </div>
        )
    }
});
ReactDOM.render(<Bind/>,app);
```
###6.6 表单元素双向数据绑定
注意: 如果给表单元素设置了value属性，则必须指定**onChange**事件处理函数，否则 此字段会变成只读状态
```
var Sum = React.createClass({
    //this.refs是一个对象，属性是虚拟DOM元素的引用名，值是此虚拟DOM元素对应的真实DOM对象
    changeA(event){
      var aVal = event.target.value;
      var bVal = this.refs.inputB.value;
      console.log(aVal,bVal);
      this.refs.result.value = (isNaN(aVal)?0:Number(aVal))+(isNaN(bVal)?0:number(bVal));
    },
    changeB(event){
        var bVal = event.target.value;
        var aVal = this.refs.inputA.value;
        console.log(aVal, bVal);
        this.refs.result.value = (isNaN(aVal) ? 0 : Number(aVal) ) + (isNaN(bVal) ? 0 : Number(bVal))
    },
    render(){
        return  (
            <div>
            //元素设置ref属性，以便其他元素使用本元素的属性值。
                <input ref="inputA" type="text" onChange={this.changeA}/>+
                <input ref="inputB" type="text" onChange={this.changeB}/>=
                <input ref="result" type="text"/>
            </div>
        )
    }
});
ReactDOM.render(<Sum></Sum>,app);
```
##7. 复合组件
多个简单的组件嵌套，可构成一个复杂的复合组件，从而完成复杂的交互逻辑
```
var Panel = React.createClass({
    render(){
        return (
            <div className="panel panel-default">
                <PanelHead content={this.props.head}/>
                <PanelBody>{this.props.body}</PanelBody>
                <PanelFooter content={this.props.footer}/>
            </div>
        )
    }
});

var PanelHead = React.createClass({
    render(){
        return <div className="panel-heading">{this.props.content}</div>;
    }
});
var PanelBody = React.createClass({
    render(){
        //{this.props.children}与父组件中标签的子元素相对应
        return <div className="panel-body">{this.props.children}</div>
    }
});
var PanelFooter = React.createClass({
    render(){
        return <div className="panel-footer">{this.props.content}</div>
    }
});
```
##8. 组件的生命周期

```
/**
 * 组件有生命周期，在生命周期不同的阶段会调用不同的函数
 */
var Life = React.createClass({
    //获取默认属性
   getDefaultProps(){
       console.log('1. 获取默认属性 getDefaultProps');
     return {name:'计数器'};
   },
    //获取初始化的状态
    getInitialState(){
       console.log('2. 获得初始状态 getInitialState');
       return {num:0};
    },
    //组件将要被挂载
    componentWillMount(){
        console.log('3. 组件将要被加载到页面中去，componentWillMount');
    },
    handleClick(){
        this.setState({num:this.state.num+1});
    },
    render(){
        console.log('4.挂载组件');
        return (
             <div>
                <p>{this.state.num}</p>
                 <button onClick={this.handleClick}>+</button>
             </div>
        )
    },
    //background: 'rgba(255, 0, 0, 1)'
    componentDidMount(){
        console.log('5.挂载组件完成之后，在这里操作dom');
    }

});
ReactDOM.render(<Life></Life>,app);
```
React中可以指定在组件的生命周期的不同阶段执行的函数

- 渲染前
    - getDefaultProps 在组件类创建的时候调用一次,则此处返回的对象中的相应属性将会合并到this.props
    - getInitialState 在组件挂载之前调用一次。返回值将会作为this.state的初始值。
    - componentWillMount 在首次渲染之前触发
- 渲染
  - render 当调用的时候，会检测this.props和this.state，返回一个组件
- 渲染后
  - componentDidMount 在初始化渲染执行之后立刻调用一次
  - shouldComponentUpdate 在接收到新的props或者state，将要渲染之前调用,返回false则不更新组件
  - componentWillUpdate 做一些更新之前的准备工作
  - componentDidUpdate 更新之后触发
  - componentWillReceiveProps 在组件接收到新的props的时候调用
- 移除
  - componentWillUnmount 在组件从DOM中移除的时候立刻被调用
  - componentDidUnmount 组件移除之后调用
##9. DOM操作
给组件加上ref="xxx"后，可在父组件中通过this.refs.xxx获取该DOM元素  