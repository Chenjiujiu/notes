## React

- 安装脚手架

 ```
 npx create-react-app app-demo01
 ```

- 渲染根节点,react负责逻辑控制，react-dom负责渲染

```javascript
import React from 'react'
import ReactDOM from 'react-dom';

ReactDom.render(<App/>, document.getElementById("root"))
```

- 组件

```javascript
/*函数组件*/
function App1(props) {
  return
</div>
  {
    props.name
  }
</div>
}
/*类组件*/
class App extends Comment {
  constructor(props) {
    super(props);
    this.state = {};
  }
  formatName(user) {
    return user.name;
  }
  render() {
    return (
      <div>{formatName({name: 'damao'})}</div>
    );
  }
}
```

- 状态更新

```javascript
  this.setState(obj, callback);
this.setState(fn, callback);
/*参数是对象或者函数，第二个参数是回调函数*/
```

- 属性

```javascript
<Comp name="" fnName={fn} style={{...}}></Comp>
``` 

- 事件

```javascript
  <input onChange={this.onChange}></input>
<input onChange={() => this.onChange(p)}></input>
```

- 条件渲染，和循环

```javascript
class App extends Comment {
  constructor(props) {
    super(props);
    this.state = {};
  }
  render() {
    const a = this.state.data = 1 ? (<h1>this.state.data</h1>) : 'null';
    return (
      <div>
        <div>{a}</div>
        <ul>
          {this.state.arr.map(item => <li key={item.id}>item.name</li>)}
        </ul>
      </div>
    );
  }
}
/*在render里面进行条件判断，最后return出去*/
/*使用数组map操作进行循环渲染*/
```

- 生命周期

```javascript
创建时
更新时
卸载时
//constructor
//getDerivedStateFromProps
shouldComponentUpdate
//render
getSnapshotBeforeUpdate
//React 更新DOM和refs
componentDidMount
componentDidUpdate
componentWillUnmount
```

## Antd

- 引入

```javascript
import React, {Component} from 'react'
import Button from 'antd/lib/button'
import 'antd/dist/antd.css'

class AntdTest extends Comment {
  render() {
    return (
      <Button type='primary'>按钮</Button>
    );
  }
}
```

- 按需加载
```javascript
npm install antd --save
npm install react-app-rewired custom-cra --D
npm install babel-plugin-import --D
//config-overrides.js
const { fixBabelImports,override } = require("customize-cra");
module.exports = override(
  fixBabelImports("import",{
    "libraryName":"antd",
    "style":"css",
  })
);
//package.json
	"start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
```

- 样式覆盖

## 纯组件

- 纯组件(浅比较)

```javascript
 class Com extends React.PureComponent {
}
//可以在生命周期函数 shouldComponentUpdate(nextProps)中判断时候更新组件 return true； 会更新
```

- memo 让函数组件也有Pure组件功能

```javascript
const Com = React.memo(function (props) {
  return (<div>...</div>)
})
```

## 高阶组件HOC (Higher-Order Components)

- 高阶组件是一个函数，返回另一个加强后的组件

```javascript
const withComp = Comp => {
  const name = "高阶组件-函数";
  return props => <Comp {...props} name={name}/>;
};
const withComp2 = Comp => {
  const name2 = "高阶组件-类";
  return class extends React.Component {
    //可以添加生命周期钩子
    componentDidMount() {
      console.log("组件挂载");
    }
    render() {
      return <Comp {...this.props} name2={name2}/>
    }
  }
}
const NewComp = witnComp(Comp);
//可以链式调用
const NewComp2 = withComp2(witnComp(Comp));
<NewComp2/>
```

- 装饰器 ES7  (只能用于class)
	addDecoratorsLegacy   
	@babel/plugin-proposal-decorators   
	customize-cra

```javascript
const {override, addDecoratorsLegacy,} = require("customize-cra");
module.exports = override(addDecoratorsLegacy());
```

```javascript
@装饰器1
@装饰器2
@装饰器3 class Comp extends React.Component {
.....
}

<Comp/>
```

## 组件复合 Composition 等同于插槽slot

```javascript
//Dialog组件做为容器，不关心内容和逻辑
function Dialog(props) {
  return <div style={{border: "2px solid red"}}>{props.children}</div>;
}
//Welcome组件通过复合提供内容
function Welcome(props) {
  return (
    <Dialog {...props}>
      <h1>标题</h1>
      <p>内容</p>
    </Dialog>
  );
}

//具名传递使用属性值传递
function Dialog(props) {
  return (
    <div style={{border: "2px solid red"}}>
      {props.children}
      <div class="footer">{props.footer}</div>
    </div>
  );
}
function Welcome(props) {
  return (
		<Dialog {...props}>
      <h1>标题</h1>
      <p>内容</p>
    </Dialog>
  );
}

<Welcome />
<Welcome footer={<button>页脚按钮</button>}/>
```
- 节点过滤，修改
```javascript
function Filter(props) {
  return (
    <div>{
      React.Children.map(props.children, child => {
      	child.type = 节点类型(div / p / ul / li);
      	// 过滤
      	if (child.type !== props.type){
      	  return;
      	}
      	return child;
    	})
    }
    </div>
  )
}
<Filter type="h1">
	<h1>这是h1</h1>
	<p>这是p</p>
</Filter>
```
- 给子组件添加属性
```javascript
function RadioGroup(props) {
	return (<div>
			{React.Children.map(props.children,child=>{
        //修改！Vdom不可更改，需要先克隆；
        return React.cloneElement(child, {name: props.name})
			})}
		</div>)
}
//解构出child 与属性，child是子组件里面的文本
function Radio({child,...rest}){
  return (
    <label><input type="radio" {...rest}>{child}</label>
	)
}
<RadioGroup name="mvvm">

	<Radio value="vue">vue</Radio>
	<Radio value="react">react</Radio>
	<Radio value="angular">angular</Radio>
</RadioGroup>
```

## 跨层级通信 Context

- 函数话组件Hook API  
在不编写class的情况下使用state以及其他特性
```javascript
import React,{useState} from 'react'
function HookTest(props) {
	//解构状态与更改状态函数，State里面设置初始值,可以死=是对象
	//useState()函数返回一个数组，数组有两个元素一个状态，一个更改函数
  const [count,setCount] = useState(0);
  return <div><button onclick={() => setCount(count++)}>{count}</button></div>
}
```
- 副作用钩子 Effect Hook  
跟componentDidMount、componentDidUpdate、和componentWillUnmount 具有相同的用途，合成成一个API
```javascript
import React,{useState,useEffect} from 'react'
// 副作用钩子会在每次渲染时都执行
useEffect(()=>{
  console.log('被渲染了count次');
})
//如果副作用钩子只需要执行一次,第二个参数为依赖数组，为空时只执行一次
useEffect(()=>{
  console.log('被渲染了count次');
},[])
```
- 自定义钩子  
	自定义钩子就是一个函数，名称使用use开头，函数内部可以使用其他钩子  
```javascript
function useAge(){
	const [age,setAge]=useState(0);
	setTimeout(()=>{
		setAge(20);
	},2000)
	return age;
};
//后面使用
const age =useAge();
{age?age:'loading...'}
```
## 组件跨层级通信 Context
- 相关api  
```javascript
React.createContext//创建上下文
Context.Provider//提供者
Class.ContextType//静态
Context.Consumer//消费者

const MyContent = React.createContext();
const {Provider,consumer}=MyCOntent;
//使用1 使用Consumer
<Provider value={{foo:'bar'}}>//使用提供者包含，用来提供值
	<Consumer>//使用消费者来拿到值
    <Child>{value.foo}</Child>	
		{value=><Child2 {...value}/>}
		<Chiled3></Chiled3>
		
	</Consumer>
</Provider>
//使用2 使用useContex
import React,{useContext} from 'react';
const MyContent = React.createContext();

function Chiled3(props) {
	const ctx = useContext(MyContent);
	return <div>{ctx.foo}</div>
}
//使用3 class
class Child4 extends Component{
  static contextType = MyContent;
  render(){
    renturn <div>{this.context.foo}</div>
	}
}
```
## redux
```javascript
import {createStore} from "redux";
const countNum = (state = 0,action)=> {
  switch (action.type) {
    case "add":
      return state+1;
    case "minus":
      return state-1;
    default:
      return state;
  }
}
const store = createStore(countNum);
export default store;
//使用
import store from '../store'
class Test {
  return (
    <div>
		<p>{store.getState()}</p>
		<button onClick={()=>{store.dispatch({type:"add"})}}>-</button>
		<button onClick={()=>{store.dispatch({type:"minus"})}}>+</button>
		</div>
	)
}
//状态更新后
store.subscribe(()=>{ReactDOM.render(<App />,document.getElementById('root'));})
```





