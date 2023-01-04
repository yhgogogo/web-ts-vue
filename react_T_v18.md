 

# React

## 一、React的简介

### 1、介绍

1. React 是一个用于构建用户界面的 JAVASCRIPT 库。

2. React主要用于构建UI，很多人认为 React 是 MVC 中的 V（视图）。

3. React 起源于 Facebook 的内部项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。

4. React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它

   

### 2、特点 

1）. **高效** −React通过对DOM的模拟，最大限度地减少与DOM的交互。

2）. **灵活** −React可以与已知的库或框架很好地配合。

3）. **JSX** − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。

4）. **组件** − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。

​       单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。



### 3、框架对比

与其他框架的共同点是，都采用虚拟dom，和数据驱动

|          | angularJs | reactJs | vueJs       |
| -------- | --------- | ------- | ----------- |
| 控制器   | √         | -       | -           |
| 过滤器   | √         | -       | √           |
| 指令     | √         | -       | √           |
| 模板语法 | √         | -       | √           |
| 服务     | √         | -       | -           |
| 组件     | -         | √       | √           |
| jsx      | -         | √       | 2.0之后加入 |



## 二、环境搭建

### 1、引入文件的方式（CDN)



1、React.js：

​       React的核心库，解析组件,识别jsx 

https://cdn.staticfile.org/react/16.4.0/umd/react.development.js

 

2、reactDom.js：

​      处理有dom相关的操作

https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js

 

3、Babel.js

​        Babel 可以将 ES6 代码转为 ES5 代码，这样我们就能在目前不支持 ES6 浏览器上执行 React 代码。Babel 内嵌了对 JSX 的支持。通过将 Babel 和 babel-sublime 包（package）一同使用可以让源码的语法渲染上升到一个全新的水平  

https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js



### 2、官方脚手架(模块化)

**安装** [create-react-app](https://create-react-app.dev/docs/getting-started)

```js
yarn global add create-react-app | npm install create-react-app	-g
```

**创建** react项目

```js
create-react-app 目录 | npx create-react-app 目录 | npm init react-app 目录
yarn eject   解构出所有的配置文件 可选
yarn start |  npm start 			开发
yarn build |  npm run build	 打包

//调试 需要安装给chrome浏览器一个插件 react-dev-tools
```

**环境解析**

- react: 核心包，解析组件,识别jsx 
- react-dom: 编译 -> 浏览器 
- react-scrpts: react的项目环境配置
- manifest.json 生成一个网页的桌面快捷方式时，会以这个文件中的内容作为图标和文字的显示内容
- registerServiceWorker.js支持离线访问，所以用起来和原生app的体验很接近,只有打包生成线上版本的react项目时，registerServiceWorker.js才会有效。服务器必须采用https协议
- 对Internet Explorer 9,10和11的支持需要polyfill。



### 第三方脚手架

yeomen/dva/umi



## 第一个React程序

```jsx
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>Hello React!</title>
<script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
<script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
<script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
</head>
<body>
    <div id="box">
    </div>
</body>
</html>
<script type="text/babel">
    
	ReactDOM.render(
	    <h1>Hello, world!</h1>, //JavaScript中竟然可以写HTML代码，这就是JSX
	    document.getElementById('box')
	);
    
/*
也可以这样写：
    const h = <h1>Hello, world!</h1>;
    ReactDOM.render(
        h,
        document.getElementById('box')
    );
*/
</script>
```



> 注意：script标签的type取值为：text/babel

> 总结：一个react的程序，就是把JSX通过ReactDOM.render()函数渲染到网页上。

> 另外，React的代码也可以单独写在一个文件里，扩展名为js，或者jsx。引入时，记住type=“text/babel”



## JSX

### 1、JSX的介绍

​        什么是JSX：JSX=javascript xml，就是Javascript和XML结合的一种格式。是 JavaScript 的语法扩展，**只要你把HTML代码写在JS里，那就是JSX**。

​        在实际开发中，JSX 在产品***\*打包阶段\****都已经编译成纯 JavaScript，不会带来任何副作用，反而会让代码更加直观并易于维护。官方定义是：类 XML 语法的 ECMAScript 扩展。



### 2、特点：

​    JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。

​    它是类型安全的，在编译过程中就能发现错误。

​    使用 JSX 编写模板更加简单快速。



###  3、JSX的语法

​       JSX就是把html代码写在javascript中，那么，写在javascript中有啥要求（**与原来在html中的区别**），这就是jsx语法要说的内容。

​	示例：

```js
var msg="哥们！"

const element = <h1>Hello, world {msg}</h1>     没有双引号，不是字符串

const List = () => (

​      <ul>

​        <li >list item</li>

​        <li>list item</li>

​        <li>list item</li>

​      </ul>  

  );   
```



**XML基本语法**

- 只能有一个根标签，养成外面加上圆括号的习惯。

- **标签要闭合（单边标签得有斜杠）**

  

**元素类型**

- 小写首字母对应 HTML的标签，组件名首字母大写。

- 注释使用 / * 内容 */  html标签内注释｛/\*  最外层有花括号\*/｝

  

**元素属性**

- 内联样式的style：

  **style属性不能直接按照html的写法写。应该用 JavaScript的写法（json对象）**。

  样式名以驼峰命名法表示, 如font-size需写成fontSize。默认像素单位是 px（px不用写）

  ```jsx
  	let _style = { backgroundColor:"red" };
  	
  	ReactDOM.render(
  	    <h1 style={_style}>Hello, world!</h1>, 
  	    document.getElementById('box')
  	);
  ```

  

- 外部样式的class：HTML中曾经的 class 属性改为 **className**（因为class是js的关键字），外部样式时使用

  ```
  	ReactDOM.render(
  	    <h1  className="bluebg">Hello, world!</h1>,
  	    document.getElementById('box')
  	);
  ```

  

- for 属性改为 **htmlFor**（因为for是js的关键字）。（for属性在html标签中是扩充单选框|复选框的选择范围）

- 事件名用驼峰命名法。HTML是**全小写**（onclick），JSX中是驼峰(onClick)

  

**javascript表达式**

- 使用**单花括号**。

  react里没有vue中的指令，在JSX的任何地方需要javascript的变量，表达式等等，只需要套上 **单花括号**就行。

  ```
  const element = <h1>Hello, {120+130}!</h1>
  const element = <h1>Hello, {getName("张三疯")}!</h1>
  <input type="text" value={val ? val : ""} />
  ```
  
  > 注意：单花括号里只能写表达式，不能写语句（如：if，for）

**总结：**

​        对比原生，JSX相当于把原生里的html和javascript代码混写在一起，

​        vue中有很多的指令，react中只需要使用**单花括号**就行。



## **ReactDOM.render()函数** 

​       ReactDOM.render 是 React 的最基本方法。用于将JSX写的模板（经过模板渲染（把javascript的结果替换单花括号里的内容））转为 HTML 语言，并渲染到指定的HTML标签里。

​	 ReactDOM.render( JSX写的html模板，dom容器对象);

   

   总结：一个react的程序，就是把JSX通过ReactDOM.render()函数渲染到网页上。

​              程序员完成的是JSX的编写。



## 条件渲染

```jsx
var sex='女';
if(sex=='男'){
	var sexJSX=<p>我是男的</p>;
}else{
	var sexJSX=<p>我是女的</p>;
}

ReactDOM.render(
	<ul>
		{sexJSX}
	</ul>,
	document.getElementById('box')
);
```

> 注意：if语句不要写在单花括号里。



## 列表渲染  

1）、渲染数组：

```jsx
//数组里存放jsx
var arr=[
    <li>张三疯</li>,
    <li>张四疯</li>,
    <li>张五疯</li>
]

const show = ()=> (
    <ul>{arr}</ul>
)
ReactDOM.render(show(),document.getElementById("box"));
```

2）使用Javascript的map（）或者普通for循环进行列表的渲染

```jsx
//普通for循环

let arr = ["铅笔","油笔","钢笔","毛笔"];
var arr2 =[];
for(let i in arr){
	arr2.push(<li>{arr[i]}</li>);
}

const show = ()=> (
	<ul>{arr2}</ul>
)

ReactDOM.render(show(),document.getElementById("box"));

//map
const goods = ['铅笔','钢笔'];
const goodsJSX = goods.map(function(val,index){
	return <li>{val}</li>
});				

ReactDOM.render(
    //以下相当于react里的模板，不能出现js的语句，可以有表达式
	<ul>
		{goodsJSX}
	</ul>,
	document.getElementById('box')
);
```



## 组件

react中定义组件有三种写法：**函数方式**，ES5的写法，**ES6（类）的写法**

### 函数方式的组件

 函数的**返回值是JSX**就行。即就是：如果一个函数的返回值是JSX，那么就可以当**标签的方式**使用。

```jsx
	// 定义组件，组件名首字母大写（大驼峰）
	function MyCom(){
		const msg="hello";
		return (
			<ul>
				<li>{msg}:三国演义</li>
				<li>{msg}:红楼梦</li>
			</ul>
		)
	}
	
	ReactDOM.render(
	    <MyCom/>, //使用组件
	    document.getElementById('box')
	);
```

### ES5的写法：

React.CreateClass()函数（React16后，已经被废弃了）

```jsx
var MyCom = React.createClass({  
  render:function(){ //vue中也有render函数，是完成模板的代码
    return (
      <div>
        <h1>Hello, world!</h1>
      </div>
    );
  }
});
```

### ES6类的写法：

定义一个类，**继承自 React.Component**，并且，在该类里，必须**有个render()函数**，render函数**返回一个JSX代码**。

换句话说：一个普通的ES6的类，继承自React.Component，有一个render()函数，并且render()函数返回一个JSX，那么就是组件。

```jsx
	// 定义组件
class MyCom extends  React.Component{
	constructor(props){ //props：是外部传入的数据，相当于vue中的props
		super(props);
		// state是内部的数据，相当于vue中的date
		this.state={
			name:"田哥"
		}
	}

	render(){
		const msg="hello";
		return (
			<ul>
				<li>{this.state.name}:三国演义</li>
				<li>{msg}:红楼梦</li>
			</ul>
		)
	}
}
```

### 多个组件

```jsx
	// 标题
	function MyTitle(){
		return (
			<h1>商品列表</h1>
		)
	}
	// 详情
	function MyDetail(){
		return (
			<ul>
				<li>铅笔</li>
				<li>钢笔</li>
			</ul>
		)
	}
	
	ReactDOM.render(
		<div>
			<MyTitle/>
			<hr/>
			<MyDetail/>
		</div>,
	    document.getElementById('box')
	);
```

## props

props 是组件对外的接口。接收外部传入的数据。是组件的属性（等同于html标签的属性）。
注意：Props对于使用它的组件内部来说，**是只读的**。**一旦赋值不能修改**。

### 外部传值：

```jsx
<组件名 属性名1=值1 属性名2=值2 .. />
```

> 属性值="静态值"  
>
> 属性值={js数据}

### 组件内部使用：

**1）、函数组件：**

```
{props.属性名}
```

示例：

```jsx
function MyPerson(props){
	return (
		<div>
			<p>{props.name}</p>
			<p>{props.sex}</p>
		</div>
	)
}

ReactDOM.render(
	<MyPerson name="张三疯" sex="男"/>,
	document.getElementById('box')
);
```

**2）、类组件：**

```
{this.props.属性名}
```

示例：

```jsx
class MyPerson extends React.Component{
	
	render(){
		return (
			<div>
				<p>{this.props.name}</p>
				<p>{this.props.sex}</p>
			</div>
		)
	}
}

ReactDOM.render(
	<MyPerson name="张三疯" sex="男"/>,
	document.getElementById('box')
);
```



**补充：**如果传递数据多的话，可以使用对象，但是必须使用扩展运算符（...） 

 扩展运算符： https://blog.csdn.net/jiang7701037/article/details/103192777 

```js
class MyPerson extends React.Component{
	// constructor(props){
	// 	super(props);		
	// }
	render(){
		return (
			<div>
				<p>{this.props.name}</p>
				<p>{this.props.sex}</p>
			</div>
		)
	}
}

let person={
	name:"张三疯",
	sex:"男"
}

ReactDOM.render(
	<MyPerson {...person}/>,
	document.getElementById('box')
);
```



### props的默认值



**1）、用 ||** 

```jsx
function MyPerson(props){
	let sex1 = props.sex || "女";
	return (
		<div>
			<p>性别：{sex1}</p>
		</div>
	)
}
ReactDOM.render(
	<MyPerson  />,
	document.getElementById('box')
);
```

**2）、defaultProps**

```jsx
格式：

//1）、函数式组件和类组件都可以：

组件名.defaultProps={
    属性名: 默认值
}

//2)、若为类组件，也可以在类的内部使用static修饰。

static defaultProps={
    属性名: 默认值
}

```



```jsx
示例：
function MyPerson(props){
	let sex1 = props.sex || "女";
	return (
		<div>
			<p>姓名：{props.name}</p>
			<p>性别：{sex1}</p>
		</div>
	)
}

MyPerson.defaultProps={
	name:"无名氏"
}

ReactDOM.render(
	<MyPerson />,
	document.getElementById('box')
);

```



### props的类型检查：

注意：react15.5后，React.propTypes已经移入到另外一个库里，请使用prop-types。

https://cdn.staticfile.org/prop-types/15.6.1/prop-types.js 

```jsx

//类型约定:
组件名.propTypes={	
    属性名1:PropTypes.类型名,
    属性名2:PropTypes.类型名
    //必传参数
	属性名: PropsTypes.类型名.isRequired
}

```

类型名的可以取值为：

PropTypes.array,
PropTypes.bool,
PropTypes.func,
PropTypes.number,
PropTypes.object,
PropTypes.string,
PropTypes.symbol,

如：

```jsx
function MyPerson(props){
	return (
		<div>
			<p>年龄：{props.age}</p>
		</div>
	)
}

MyPerson.propTypes={
    //注意大小写
	age:PropTypes.number.isRequired
}

ReactDOM.render(
	<MyPerson age={12} />,
	document.getElementById('box')
);
```



## state状态机

state 是状态，状态就是数据，state表示组件的内部数据。而props是外部传入组件的数据，**类组件可以直接使用**，但是函数式组件就得用hooks（useState）

### **定义并初始值**

```jsx
class App extends React.Component {
  constructor(props){   
      super(props);  
      
      this.state={
    	  //设置状态的初始值
       	  属性名:属性值
      }
  }
}
```

### **读取状态**

```

this.state.属性名
```

### **修改状态**

   **必须调用setState()函数**，不要直接赋值，而且，setState()函数**是异步的**。

**1、修改状态时，**必须要调用setState。因为，只要调用了setState函数，那就会调用了render()。如果直接赋值，不会把数据响应式地渲染到DOM上。（即：没有调render()函数）

```jsx
class Book extends React.Component{        

        constructor(props){
            super(props);
            this.state = {
                copyright:"版权归2011"
            }

            // 千万不能这么干，因为，数据修改后，不会响应式的呈现页面上（不会再次渲染DOM）
            // this.copyright="版权归2011";

            // 下面这句话，可以先不管
            this.changeVal = this.changeVal.bind(this);
        }

        changeVal(){
            // this.copyright = "哈哈哈";
            // console.log("this.copyright ",this.copyright);

            // 这不行，这个不会把数据响应式地渲染到DOM上。（即：没有调render()函数）
            // this.state.copyright = "哈哈哈";

            // 这 行，因为，只要调用了setState函数，那就会调用了render();
            this.setState({
                copyright:"哈哈哈"
            });
        }

        render=()=>{
            console.log("render");
            return (
                <ul>
                    <li>书名：{this.props.name}</li>
                    <li>书龄：{this.props.age}</li>
                    <li>{this.state.copyright}</li>
                    <li>{this.copyright}</li>
                    <input type="button" value="修改" onClick={this.changeVal} />
                </ul>
            )
        }
    }

    Book.propTypes = {
        "name":PropTypes.string.isRequired,
        "age":PropTypes.number
    }

    const jsx = <div><Book name="西游记" age={15} /></div>

    ReactDOM.render(jsx,document.getElementById("box"));
```

```js
//浅合并state
this.setState({
	属性名:属性值
})

示例代码：
//1、假设定义的状态是：
this.state = {
    person:{
        name:"魏鹏",
        sex:"男"
    }
}
//2、如果修改时，使用下面的方式，肯定不行。因为，是浅合并，不会进入到下一级的属性进行对比
this.setState({
    person:{
        name:"鹏鹏"
    }
})

```

>vue和react在响应式上的区别：
>
>1、vue：背后使用了数据劫持和观察者模式
>
>2、react，修改数据时，调用setState，setState内部调用了render。
>
>



**2、setState()函数是异步的**

```jsx
//改变状态前想做一些事情：
this.setState((prevState,prevProps)=>{
  //一般是用于在setState之前做一些操作,this.state==prevState==修改之前的state 
    
   return {
    sname:value
  }
}) 

//改变状态后想做一些事情（如：使用新值）：
this.setState({
  属性名:属性值
}, () => {
    
  //一般是用于在setState之后做一些操作。
  //this.state == 修改之后的state
    
})


//改变状态前后都想做一些事情：
this.setState((prevState)=>{
    // prevState:是旧值
    console.log("prevState",prevState)
    return {
        age:15
    }
},()=>{
    // this.state:就是新值。
    console.log(this.state.age);
});
```

```js
 class Book extends React.Component {

        constructor() {
            super();
            this.msg = "hi";
            this.state = {
                name: "三国演义",
                author: "李茂军02"
            }
            this.changeVal = this.changeVal.bind(this);
        }

        changeVal() {

            // setState函数是异步的
            // this.setState({
            //     name:"ddd"
            // });
			//console.log("this.state.name", this.state.name);//三国演义
            
            // 在修改状态之前做事情
            // this.setState((prevState)=>{
            //     console.log("prevState",prevState);  //三国演义 
            //     console.log("this.state",this.state);//三国演义
            //     return {
            //         name:"ddd"
            //     }
            // });

            // 在修改之后做一些事情

            this.setState({
                name: "ddd"
            }, () => {
                console.log("this.state.name", this.state.name);//ddd
                console.log(document.getElementById("pId").innerHTML);//ddd
            });

        }

        render() {
            console.log("render");
            return (
                <div>
                    <p>{this.msg}</p>
                    <p id="pId">书名：{this.state.name}</p>
                    <p>作者：{this.state.author}</p>
                    <p>价格：{this.props.price}</p>
                    <input type="button" value="修改" onClick={this.changeVal} />
                </div>
            )
        }
    }

    const h = (<div>
        <h1>hello 我是react</h1>
        <div>
            <Book author="李茂军" price="9.9" />
        </div>
    </div>);

    ReactDOM.render(
        h,
        document.getElementById("box")
    );

```

>注意：
>
>1、 直接修改state属性的值不会重新渲染组件 ，如：this.state.msg="hello"; //亲，千万不要这么干
>
>

**补充：**

给引用类型的某个属性赋值

```jsx
class Book extends React.Component{    

        constructor(props){
            super(props);
            this.state = {
                book:{
                    name:"做一个有钱的人",
                    age:22
                }
            }
            this.changeVal = this.changeVal.bind(this);
        }

        changeVal(){
            // 这是不行的
            // this.setState({
            //     "book.name":"做一个有意义的人"
            // });

            // 得这样做（浪费了空间）：就得这么写
            let b = {
                ...this.state.book,
                name:"做一个有意义的人"
            };
            this.setState({
                book:b
            });
            
            //或者这样做（节约了空间）：这样不推荐，因为，这样破坏了setState的异步
            let b = this.state.book;
            b.name = "大学";
            this.setState({
                book:b
            });

        }

        render=()=>{
            return (
                <ul>
                    <li>{this.state.book.age}</li>
                    <li>{this.state.book.name}</li>
                    <input type="button" value="修改" onClick={this.changeVal} />
                </ul>
            )
        }
    }
```



 示例代码：

1）、基本示例：

```jsx
class MyPerson extends React.Component{
	constructor(props){
		super(props);
		this.state={
			age:12
		}
		this.changeAge = this.changeAge.bind(this);
	}

	changeAge(){
		this.setState({
			age:this.state.age+1
		});
	}

	render(){
		return (
			<div>
				<input type="button" value="修改年龄" onClick={this.changeAge} />
				<p>年龄：{this.state.age}</p>
			</div>
		);
	}
}
```

2）、setState是异步的示例：

```jsx

class MyPerson extends React.Component{
	constructor(props){
		super(props);
		this.state={
			age:12,
			isAdult:"未成年"
		}
		this.changeAge = this.changeAge.bind(this);
	}
	
	/*
	这种写法达不到效果：
	changeAge(){
		this.setState({
			age:this.state.age+1
		});
		// setState是异步的,所以，以下的打印不是最新的值
		console.log(this.state.age);
		// setState是异步的,所以，以下的判断达不到效果
		if(this.state.age>=18){
			this.setState({
				isAdult:"已成年"
			});
		}
	}
	*/
	
	changeAge(){
		this.setState({
			age:this.state.age+1
		},()=>{
			console.log(this.state.age);
			if(this.state.age>=18){
				this.setState({
					isAdult:"已成年"
				});
			}
		});
	}

	render(){
		return (
			<div>
				<input type="button" value="修改年龄" onClick={this.changeAge} />
				<p>年龄：{this.state.age}</p>
				<p>年龄：{this.state.isAdult}</p>
			</div>
		);
	}
}
```



## 无状态组件

​        无状态组件就是组件内部没有（不需要）state，无状态组件也可以理解为**展示组件**，仅做展示用，可以根据外部传来的props来渲染模板的内容，内部没有数据。相当于木偶（没脑子）。你说显示啥，咱就显示啥。

```jsx
var MyFooter = (props) => (
    <div>{props.xxx}</div>
);
```

仅仅只是一个函数，就ok了。

## 有状态组件

​        有状态组件就是不但外部可以传入，内部也有state。

​        有状态组件也可以理解为**容器组件**，用来容纳展示组件，在容器组件里处理数据的逻辑，把结果在展示组件里呈现。容器组件也叫智能组件

​       创建有状态组件如下：

```jsx
class Home extends React.Component { //容器组件
    constructor(props) {
        super(props);
  		this.state={
  			Hots:[],
       		Goodslist:[]
		}
	}
	
    getHots(){
        发送axios请求获取数据
    }

    getGoodsList(){
       发送axios请求获取数据
    }
    render() {
        return (
		 <div>
     		<Hot hots={this.state.Hots} ></Hot> //展示组件
     		<Goodslist goodslist={this.state.Goodslist} ></Goodslist >//展示组件
		</div>
        )
    }
}
```

>注意：
>
> 做项目时，经常会把有状态组件和无状态组件进行结合使用。
>
>1）、 有状态组件：一般会使用类组件，处理数据的业务逻辑，包括和后端进行交互，获取数据。把数据传给无状态组件
>
>2）、 无状态组件：一般会使用函数式组件，只做展示用，没有自己的数据，会接收有状态组件传来的数据。



## 事件处理

### React事件的特点：

1、React 事件绑定属性的命名采用**驼峰式**写法，而不是小写。如：onClick。

2、如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而**不是一个字符串(DOM 元素的写法)**

3、在 React 中另一个不同是你不能使用return false 的方式阻止默认行为， 你必须明确的使用 preventDefault。

4、事件处理函数里的this是undefined（啊………），如果希望事件处理函数里的this是React组件对象本身，则需要用bind。



### 事件语法

#### **1、格式**

<JSX元素 onClick={this.实例方法|函数体}  />

示例：

```js
class Home extends React.Component{
    
fn01() {
  console.log("fn01");
}
    
render(){
  return (
   <div>
     <input type="button" value="测试01" onClick={this.fn01} />
     <input type="button" value="测试02" onClick={()=>{console.log("事件绑定箭头函数")}} />
    </div>
  )
}
}
```



#### **2、事件处理函数里的this**

事件处理函数里的this是undefined，如何让事件处理函数里的this是React组件**对象**本身

```jsx
一、使用bind
1）、构造器（构造函数）里：this.方法=this.方法.bind(this)  
2）、在事件绑定时写，onClick={this.方法.bind(this)}

二、使用箭头函数
3）、定义方法时，直接使用箭头函数: 方法=()=>{箭头函数定义方法}  
4）、事件绑定时，使用箭头函数：onClick={()=>this.方法()}
```

如：

```jsx
class MyPerson extends React.Component{
	constructor(props){
		super(props);
		this.state={
			age:12,
			isAdult:"未成年"
		}
		this.changeAge = this.changeAge.bind(this);
	}

	changeAge(){
		………………………………
	}
	//直接使用箭头函数
	changeAge=()=>{
		this指向会找上一级
	}

	render(){
		return (
			<div>
				<input type="button" value="修改年龄" onClick={this.changeAge} />
				<input type="button" value="修改年龄" onClick={()=>{this.changeAge()}} />
				<input type="button" value="修改年龄" onClick={this.changeAge} />
				<p>年龄：{this.state.age}</p>
				<p>年龄：{this.state.isAdult}</p>
			</div>
		);
	}
}
```



#### **3、事件对象**

event对象是经过react处理过的。

如何获取事件对象------直接声明即可。

  实例方法(ev)	ev 代理事件对象 ，ev.target 返回真实DOM 

事件对象的获取：

1）、直接声明（没有其它参数的情况下）

```jsx
changeAge1=(ev)=>{
    console.log(ev);
    console.log(ev.target);
}

<input type="button" value="修改年龄2" onClick={(ev)=>this.changeAge1(ev)} />
<input type="button" value="修改年龄2" onClick={this.changeAge1} />
```

2）、箭头函数里直接声明（有其它参数的情况下）

```jsx
changeAge2(ev,num){
    console.log(ev);
    console.log(ev.target);
    console.log(num);
}
//注意：给onClick绑定的函数，还是只有一个参数，这个参数就是事件对象，此处在绑定的函数里再调用另外一个函数进行传参
<input type="button" value="修改年龄2" onClick={(ev)=>this.changeAge2(ev,2)} />
```

> 注意：给事件属性绑定的函数，永远只会有一个参数，该参数就是事件对象。

#### **4、阻止浏览器的默认行为：**

​         只能用preventDefault，不能在事件函数里使用return false



## 组件的内容 ：children



组件的内容，使用 props.children属性获取

```js

function Button(props) {
  console.log("props",props);
  return (
    <>
      <div>Button_children:{props.children}</div>
    </>

  )
}

<Button >我是内容</Button>
```



## refs

​        获取DOM的。

​        表示对组件真正实例（也就是html标签，也就是DOM对象）的引用，其实就是ReactDOM.render()返回的组件实例；ref可以写在html官方标签里，也可以写在组件（自定义标签里），和vue的ref是同样的意思。

​       官方建议： 勿过度使用 Refs（尽量不要操作DOM），在对逻辑进行处理的时候尽量优先考虑state（数据驱动）

**用法**

1）. 赋值为 字符串(官方不推荐使用)

```jsx
  <input type="text" ref="username" />
 
   this.refs.username.value
```

2）. 赋值为 回调函数

​     当给 HTML 元素添加 ref 属性时，ref 回调接收了底层的 DOM 元素作为参数。

​     ref 回调会在componentDidMount 或 componentDidUpdate 这些生命周期回调之前执行。

```jsx
//ref的值赋成回调函数时，回调的参数就是当前dom元素。
// callback refs  回调

<jsx ref={el => this.定义一个实例属性 = el} //el就是dom对象
    
    
this.定义一个实例属性 //后期用作访问jsx元素
//当组件挂载时，将 DOM el元素传递给 ref 的回调
//当组件卸载时，则会传递 null。
//ref 回调会在 componentDidMount 和 componentDidUpdate 生命周期之前调用
    
    如：
  <input type="text" ref={(currDom) => this.input1  = currDom} />
  <input type="text" ref={(currDom) => this.input2 = currDom} />
```

3）. React.createRef() （React16.3提供）

​        使用此方法来创建ref。将其赋值给一个变量，通过ref挂载在dom节点或组件上，该ref的current属性将能拿到dom节点或组件的实例

```jsx
//1、在构造函数里
// 实例化了ref对象
this.firstRef = React.createRef() //发生在构造器

//2、挂载在ref属性上
<jsx ref={this.firstRef} />
    
//3、获取dom原始
this.firstRef.current //current是官方的属性
```

## 受控元素(组件)

 一个标签（组件）受react中控制，受数据，受函数，等等（其实，就是一个标签（组件）里用了react里的东西）

表单的value受控，受**数据**控制，

```jsx
<input type="text" value={this.state.数据名}  /> //model 控制 view。
<input type="text" onChange={this.方法} />   //view 控制 model
```



### 双向绑定

```jsx
class MyCom extends React.Component{
		constructor(){
			super();
			this.state={
				username:"李祥"
			}
		}

		changeVal(e){
			this.setState({
				username:e.target.value
			})
		}

		render(){
			return (
				<div>
					<input type="text" value={this.state.username} onChange={(e)=>this.changeVal(e)} />
				</div>
			)
		}
	}	
```



### **处理多个输入元素（双向绑定的封装）**

可以为每个元素添加一个 name 属性(通常和数据名一致)，处理函数根据 event.target.name 的值来选择要做什么

```jsx
class MyCom extends React.Component{
    constructor(props){
        super(props);
        this.state={
            userName:'',
            content:''
        }
    }

    changeVal(ev){
        this.setState({
            [ev.target.name]:ev.target.value
        });
    }

    render(){
        return (
            <div>
                <p>{this.state.userName}</p>
                <input name="userName" type="text" onChange={(ev)=>this.changeVal(ev)} />
                <p>{this.state.content}</p>
                <input name="content" type="text" onChange={(ev)=>this.changeVal(ev)} />
            </div>
        )
    }
}
```



## 非受控元素(组件)

​       要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 [使用 ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据

```jsx
<input type="text" value="哈哈"  ref="xx" />
```



**默认值**

​       表单元素上的 `value` 将会覆盖 DOM 节点中的值，在非受控组件中，你经常希望 React 能赋予组件一个初始值，但是不去控制后续的更新,指定一个 `defaultValue` 属性，而不是 `value`



###### 可选案例：

**增删改查**



## 生命周期及其钩子函数

react组件生命周期经历的阶段：初始化阶段 -----> 运行阶段（更新期）-----> 销毁阶段

### 初始化阶段 （挂载）:（在这个阶段完成了vue中数据挂载和模板渲染）

组件**实例被创建并插入 DOM 中**时，其生命周期钩子函数的调用顺序如下（粗体为使用比较多的）：

**1)、constructor**

​		构造函数里，可以做状态的初始化，接收props的传值

2)、componentWillMount： 在渲染前调用，相当于vue中的beforeMount

**3)、render**

 	 渲染函数，不要在这里修改数据。 vue中也有render函数。

**4)、componentDidMount**  相当于vue中的 mounted

​       渲染完毕，在第一次渲染后调用。之后组件已经生成了对应的DOM结构， 如果你想和其他JavaScript框架（swiper）一起使用，可以在这个方法中使用，包括调用setTimeout, setInterval或者发送AJAX请求等操作，相当于vue的mounted



### 运行中阶段（更新）（相当于vue中更新阶段）

当组件的 props 或 state 发生变化时会触发更新（**严谨的说，是只要调用了setState（）或者改变了props时**）。组件更新的钩子函数调用顺序如下：

1）、shouldComponentUpdate(nextProps, nextState)	是否更新？ 需要返回true或者false。如果是false，那么组件就不会继续更新了。

2）、componentWillUpdate，即将更新。相当于vue中的 beforeUpdate

3）、 componentWillReceiveProps(nextProps)： 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。nextProps 是props的新值，而 this.props是旧值。相当于vue中的 beforeUpdate

4）、render

​	   不要在这里修改数据

5）、componentDidUpdate。相当于vue中的 updated

在组件完成更新后立即调用。在初始化时不会被调用。 相当于vue中的updated

 

###  销毁阶段（卸载）

**componentWillUnmount()**

即将卸载，可以做一些组件相关的清理工作，例如取消计时器、网络请求等

示例：

```js

class MyPerson extends React.Component{
	constructor(props){
		super(props);
		console.log("====constructor===");
		this.state={
			age:12
		}
	}

	changeAge2(ev,num){
		this.setState({
			age:this.state.age+1
		});
	}

	componentWillMount(){
		console.log("====首次渲染前：componentWillMount===");
	}

	componentDidMount(){
		console.log("====首次渲染完毕：componentDidMount===");		
	}

	shouldComponentUpdate(){
		console.log("====希望更新组件吗？（state发生变化了）：shouldComponentUpdate===");		
		return true;
	}

	componentWillUpdate(){
		console.log("====组件更新前（state发生变化了）：componentWillUpdate===");				
	}

	componentWillReceiveProps(){
		console.log("====组件更新前（props发生变化了）：componentWillReceiveProps===");		
	}

	componentDidUpdate(){
	
	
		console.log("====组件更新后：componentDidUpdate===");		
	}

	render(){		
		console.log("====render===");
		return (
			<div>
				<input type="button" value="修改年龄2" onClick={(e)=>this.changeAge2(e,2)} />
				<p>年龄：{this.state.age}</p>
			</div>
		);
	}
}

ReactDOM.render(
	<MyPerson />,
	document.getElementById('box')
);
```



注意(面试题)：

>父更新则子更新，子更新父不更新



```js

class BookList extends React.Component{    
    constructor(props){
       super();
       console.log("BookList:constructor");
    }
    // 首次渲染前
    componentWillMount(){
        console.log("BookList：componentWillMount");
    }    
    // 首次渲染后
    componentDidMount(){
        console.log("BookList：componentDidMount");
    }

    change=()=>{
        this.setState({});
    }

    componentWillUpdate(){
        console.log("BookList：componentWillUpdate");
    }

    componentDidUpdate(){
        console.log("BookList：componentDidUpdate");
    }

    render(){
        console.log("BookList:render");        
        return (
            <div>
                <h1>书籍列表</h1>
                <Book></Book>
                <input type="button" value="测试" onClick={this.change} />
            </div>
        )        
    }
}

class Book extends React.Component{    
    constructor(props){
        super();
       console.log("Book:constructor");       
    }

        // 首次渲染前
    componentWillMount(){
        console.log("Book：componentWillMount");
    }
    
    // 首次渲染后
    componentDidMount(){
        console.log("Book：componentDidMount");
    }

    componentWillUpdate(){
        console.log("Book：componentWillUpdate");
    }

    componentDidUpdate(){
        console.log("Book：componentDidUpdate");
    }

    change=()=>{
        this.setState({});
    }

    render(){
        console.log("Book:render");    
        return (
            <div>
                <input type="button" value="子组件的测试" onClick={this.change} />
            </div>
        )        
    }
}

ReactDOM.render(
    <div>
        <BookList>
        </BookList>
    </div>,
    document.getElementById("box")
);
```



react17，不建议使用的钩子函数（**面试过程中有问到**）：

UNSAFE_componentWillMount 不建议用，可以会出现bug，不能初始化因为会受到React16.xFiber的协调算法，函数会执行多次，如果把异步请求放到该钩子函数中，异步请求可能也会执行多次。

UNSAFE_componentWillReceiveProps
UNSAFE_componentWillUpdate



### es5版(可以不用看了)

**实例化**

1. 取得默认属性(**getDefaultProps**) 外部传入的props
2. 初始状态(**getInitialState**)  state状态
3. 即将挂载 **componentWillMount**
4. 描画VDOM  **render**
5. 挂载完毕 **componentDidMount**



### 补充：vue组件更新和react组件更新的触发条件（更新时机）不一样

1、vue的更新触发条件：

​	 https://blog.csdn.net/jiang7701037/article/details/103077021 

2、react组件的更新触发条件：

​        只要改变props或者state，无论数据有没有显示在模板里，都会引发渲染。并且，只要是赋值，无论赋值前后的数据是不是一样都会引起组件的渲染（其实就是**只要调用setState**函数，传参为空对象就行），所以，在React里需要牵扯到组件更新的性能优化。



## 性能优化

​        react组件更新的时机：只要setState()被调用了，就会引起组件更新。不论**数据改前改后是否一样**，或者修改的**数据是否在页面上呈现**，都会进行更新组件。

​         但是vue中，**数据必须在模板使用**，并且**数据发生变化**才能引起组件的重新渲染。所以，在React里，如果要做性能优化，可以把这种无效的渲染阻止掉。

​    **1）、shouldComponentUpdate（）钩子函数**   

```js

shouldComponentUpdate(nextProps, nextState){
	console.log("this.state",this.state);//当前状态
	console.log("nextState",nextState);//新的状态
    return false;//不再渲染。 
}

```

我们可以在这个钩子函数里return false，来跳过组件更新

示例：

```js
class Books extends React.Component {

        constructor(props) {
            super();
            this.state = {
               msg:"hello react",
                d_str:"how are you!"   //约定：以 d_ 开头的数据不再模板上渲染
            }
        }

          // 比较两个对象的内容（属性不是以d_开头）是否一样。
        compareObj(obj1,obj2){
            // 1、两个对象的属性数量如果不一样，那么就肯定不相等
            if(Object.keys(obj1).length!=Object.keys(obj2).length){
                return false;
            }
            // 2、两个对象的属性数量一样
            for(let key in obj1){
                if(typeof obj1[key]=="object"){
                    if(typeof obj2[key]=="object"){
                        // 两个属性都是对象。
                        if(!key.startsWith("d_") && !this.compareObj(obj1[key],obj2[key])){
                            return false;
                        }
                    }else{
                        // obj1是对象，obj2不是对象
                        return false;
                    }
                }else{
                   if(typeof obj2[key]=="object"){                       
                        // obj1不是对象，obj2是对象
                        return false;
                   }else{
                       if(!key.startsWith("d_") && obj1[key]!=obj2[key]){
                            return false;
                       }
                   }
                }
            }
            return true;
        }

        shouldComponentUpdate(nextProps,nextState){
            console.log("shouldComponentUpdate:是否更新？");
            console.log("this.state",this.state);
            console.log("nextState",nextState);
            // if(数据没有变化 || 数据发生了变化，但是该数据并不在模板上使用){
            //     return false
            // }

            if(this.compareObj(this.state,nextState)){
                return false;
            }

            return true;
        }

       changeMsg=()=>{
            this.setState({
                msg:this.state.msg+"1"
                // msg:this.state.msg
            })
        }

        changeStr=()=>{
            this.setState({
                d_str:this.state.d_str+"1"
            })
            // this.setState({});
        }

        render(){
            console.log("render");            
            return (
                <div>
                    <h1>生命周期及其钩子函数</h1>
                    <p>{this.state.msg}</p>
                    <input type="button" value="修改msg" onClick={this.changeMsg} /><br/>
                    <input type="button" value="修改str" onClick={this.changeStr} /><br/>
                </div>
            )
        }
    }

```



**2）、PureComponent**

​        React15.3中新加了一个 PureComponent 类，只要把继承类从 Component 换成 PureComponent 即可

- PureComponent 默认提供了一个具有**浅比较的shouldComponentUpdate方法**。只是比较数据（第一层）是否有变化，并不会判断数据是否在页面上展示。

- 当props或者state改变时，PureComponent将对props和state进行浅比较。即：如果说状态是引用类型，只要地址发生变化，照样会重新渲染、

- 不能再重写shouldComponentUpdate

  

  > Component和PureComponent的区别：
  >
  > ​         当组件继承自 Component 时，只要setState()被调用了，就会引起组件更新。不论**数据改前改后的是否一样**，或者修改的**数据是否在页面上呈现**，都会进行更新组件。 
  >
  > ​         PureComponent只能保证**数据不变化的情况下不做再次的渲染**，而不关心数据是否在页面上渲染，而且比较数据变化时，只比较第一层。

  


```jsx
class MyCom extends React.PureComponent{
		constructor(){
			super();
			this.state={
				username:"张三疯",
				age:12,
				wife:{
					name:"宝宝的宝宝"
				}
			}
		}

		componentWillUpdate(){
			console.log("componentWillUpdate");
			
		}
		componentDidUpdate(){
			console.log("componentDidUpdate");			
		}

		fn(){
			//1、 不会引起组件的更新，因为，值没有变
			// this.setState({
			// 	username:this.state.username
			// });
			//2、 会引起组件的更新，因为，值变了
			// this.setState({
			// 	username:"dddd"
			// });
			//3、 会引起组件的更新，因为，值变了(即使age么有在页面上显示，也会引起更新)
			// this.setState({
			// 	age:this.state.age+1
			// })

			//4、会引起组件的更新，虽然，name的值没有变化，但是，wife的值变化了（wife是引用类型）
			let obj = this.state.wife;
			obj.name="宝宝的宝宝";			
			this.setState({
				wife:obj
			});
		}

		render(){
			console.log("render");
			return (
				<div>
					<input type="text" value={this.state.username}  /><br/>
					<input type="button" value="测试" onClick={()=>this.fn()} />
				</div>
			)
		}
	}	

```



## 脚手架

### facebook的官方脚手架

**1）、安装** [create-react-app](https://create-react-app.dev/docs/getting-started)  （CRA）

```js
npm install create-react-app -g | yarn global add create-react-app 
```

**2）、用脚手架创建** react项目

```
create-react-app  项目名称
如：create-react-app  reactapp01
```

> 注意：项目名称不能有大写字母。



**3）、 启动项目：**

```
npm start   |   yarn start
```

**4）、目录解析：**

**4.1）第一级目录**

![1596264176068](img\01脚手架目录01.png)

node_modules:是项目依赖的模块

src：是程序员写代码的地方，src是source的缩写，表示源代码

public: 静态资源。react脚手架中的静态图片资源和动态图片资源都放在此处

![image-20220117203739917](react_T_v18.assets/image-20220117203739917.png)



**4.2）展开目录：**

![1596264209566](img/02脚手架目录02.png)

- **Public：**

index.html：是html网页，是个容器。这个文件千万不敢删除，也不能改名。

只有Public目录下 的文件才会被index.html文件引用，这是静态资源，index.html不会引用src目录下的文件

manifest.json： 生成一个网页的桌面快捷方式时，会以这个文件中的内容作为图标和文字的显示内容

- **src：**

 src目录是源代码，webpack只会打包这个目录下的文件，所以，把需要打包的文件都放在这个目录下。

   Index.js：是主js文件，千万不敢删除，也不能改名

   Index.css：是index.js引入的css文件（也是模块，webpack会把css也打包成模块）  千万不敢删除，也不能改名

   App.js：是一个组件示例（模块），在 index.js里会引入这个组件。我们自己需要写组件时，只需要复制App.js文件即可。

  App.css：是App.js文件引入的css文件（也是模块，webpack会打包）。

  Logo.svg：是图片

​    registerServiceWorker.js：支持离线访问，所以用起来和原生app的体验很接近,只有打包生成线上版本的react项目时，registerServiceWorker.js才会有效。服务器必须采用https协议

​    

**5）、打包**

```js
npm run  build | yarn build
```

**6）、如果要解构出配置文件：**

```js
npm  run  eject  |  yarn eject   解构出所有的配置文件 可选
```

**7）、如果需要调试，安装react-dev-tools工具**

1. 先进入到https://github.com/facebook/react网址

2. 通过git clone https://github.com/facebook/react-devtools.git下载到本地（或者直接点击下载）

3. 下载之后进入到react-devtools目录下，用npm安装依赖

​       npm --registry https://registry.npm.taobao.org install

4. 然后在npm run build:extension:chrome

   

**环境配置**

```js
1、把配置解构
npm run eject | yarn eject

  
2、修改端口
//修改script/start.js
const DEFAULT_PORT = parseInt(process.env.PORT, 10) || 3001;

3、去除eslint 警告
//config/webpack.config.js
//注释关于eslint的导入和rules规则
```

**资源限制**

- 本地资源导入(import) 不可以导入src之外的包

- **图片声音等静态资源的路径：**

    **都放在public下，写路径时，不要写public（和vue脚手架不一样）。因为，react脚手架里不会对静态的src路径对应的图片打包。**

  

**在脚手架里做项目的步骤：**

1）、创建目录

​    在src目录下创建以下文件夹：

- assets ：静态资源文件夹

  ​    放置的是import或者require引入的资源。

  ​    **静态的图片路径不要放在此处，应该放在public下。**

- components：组件文件夹

  ​    /components/a组件/  a.js  和 a.css

- pages：页面文件夹

  
  
  

2）、图片文件夹

1．如果不希望图片被打包处理：

​       把图片放到public文件夹中，使用绝对路径（img src="/img/image.jpg" />）

2．如果希望图片被打包

​       使用require引用，require(‘图片的相对路径')，Require中只能使用字符串不能使用变量。如：

<img src={require("../../assets/img/banner04.jpg").default} />



##### vue脚手架和react脚手架不同

1、编译打包时，

​        图片路径问题：vue：静态路径会打包，动态不会；react都不会打包，如果希望react里的静态路径打包，那就使用用require引入图片。



**反向代理：**

 https://create-react-app.dev/docs/proxying-api-requests-in-development/ 

1、安装模块（http-proxy-middleware）：

```js
 npm install http-proxy-middleware --save-dev

 yarn add http-proxy-middleware -D
```

2、在项目源代码的根目录创建文件： src/setupProxy.js 

```js
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
  console.log("proxy");
  app.use(
    '/api',
    createProxyMiddleware({
      target: 'http://xmb8nf.natappfree.cc',
      changeOrigin: true,
      // 重写接口路由
      pathRewrite: {
        '^/api': '' 
      }
    })
  );
};
```

3、重启服务器

```js
yarn start
```



### 第三方脚手架

yeomen/dva/umi



## 组件传值

### **1）．父子组件通信方式** 

**（1） Props传递数据与Props传递方法** 

​      **父组件--->子组件传递数据，用props传递数据**

```jsx
1）、定义组件时，声明props形参：
class Person extends React.Component {
  constructor(props) {
     super(props); 
}
render() {
    return (
        <div>       
          姓名：<span>{this.props.name}</span><br/>
      </div>
    );
  }
}

2）、使用组件时，传入参数：
<Person name='张三疯'   >
```

  **子组件--->父组件传递数据，用props传递方法**

​       父组件利用props传递方法给子组件，子组件回调这个方法的同时，将数据传递进去，使得父组件的相关方法得到回调，这个时候就可以把数据从子组件传递给父组件了

```jsx
//1、父组件
class App extends React.Component {
    constructor(){
        super();
        this.state={
            t:"Home"
        }
    }

    changeData(str){
        console.log("子传给了父亲：",str);
    }

    render = () => (
        <div className="App">
           <Home title={this.state.t} fn={this.changeData}/>
           <My title="My"/>
        </div>
    )
};

//2、子组件
class Home extends React.Component {
    constructor(name){
        super();
        this.name=name;
        this.state={
            msg:"hello "
        }
    }

    render = () => (
        <div>
             <h1>我是{this.props.title}</h1>
             <p>{this.state.msg+this.props.title}</p>
             <input 
                type="button" 
                value="传给父亲" 
                onClick={()=>this.props.fn('HELLO dad')} />
        </div>
    )
}
```



**（2） ref 标记** 

组件间通信除了props外还有onRef方法，不过React官方文档建议不要过度依赖ref。

​       思路：当在子组件中调用onRef函数时，正是调用从父组件传递的函数。this.props.onRef（this）这里的参数指向子组件本身，父组件接收该引用作为第一个参数：onRef = {ref =>（this.child = ref）}然后它使用this.child保存引用。之后，可以在父组件内访问整个子组件实例，并且可以调用子组件函数。



```jsx
//子组件
class Person extends React.Component {
  constructor(props) {
    super(props);
    this.state={
        msg:"hello"
    }
  }
  //渲染完毕
  componentDidMount(){
      //子组件首次渲染完毕后，把子组件传给父组件（通过props的onRef）
      this.props.onRef(this);//把子组件传给父组件，注意,此处的this是子组件对象
  }
      
  render() {
    return (   <div> 我是子组件</div> );
  }
}

//父组件：
class Parent extends React.Component{
    constructor(props){
        super(props);
        this.child =null;//定义了一个属性，该属性表示子组件对象
    }
    
    testRef=(ref)=>{
        this.child = ref //给父组件增加属性child，child保存着子组件对象
        console.log(ref) // -> 获取整个Child元素
    }
    
    handleClick=()=>{
        alert(this.child.state.msg) // -> 通过this.child可以拿到child所有状态和方法
        this.child.setState({
          msg:"哈哈"
        })
    }
    
    render(){
        return (
            <div>
                <Person onRef={this.testRef}  ></Person>
            </div>
        )
    }
}
ReactDOM.render(<Parent />,$("box"));
```



### **2）．非父子组件通信方式** 

**（1）订阅发布**(pubsub模块)  相当于vue的事件总线

- 订阅:	token=pubsub.subscribe('消息名',回调函数('消息名',数据))，相当于事件总线的$on绑定事件
- 发布：  pubsub.publish('消息名',数据)，相当于事件总线的$emit触发事件
- 清除指定订阅：pubsub.unsubscribe(token|'消息名');
- 清除所有：pubsub.unsubscribeAll()

```jsx
<script src="https://unpkg.com/PubSub@3.6.0//dist/pubsub.min.js"></script> 
var pubsub = new PubSub();

class MyCom1 extends React.Component{
		constructor(){
			super();
		}	
		
		testf(){
			pubsub.publish('user_add', {
				firstName: 'John',
				lastName: 'Doe',
				email: 'johndoe@gmail.com'
			});
		}		
		
		render(){
			return (
				<div>
					<p>MyCom1</p>
					<input type="button" value="传递数据" onClick={()=>this.testf()} />					
				</div>
			)
		}
	}

	class MyCom2 extends React.Component{
		constructor(){
			super();
			this.state={
				msg:"hi"
			}
			//订阅
			pubsub.subscribe('user_add', function (data) {
				console.log('User added');
				console.log('user data:', data);
			});
		}

		render(){
			return (
				<div>
					<h1>MyCom2</h1>
				</div>
			)
		}
	}

	const jsx = <div><MyCom1/><hr/>
					<MyCom2/></div>

	ReactDOM.render(
		jsx,
	    document.getElementById('box')
	);
```



**（2）状态提升**

​        使用 react 经常会遇到几个组件需要共用状态（数据）的情况。这种情况下，我们最好将这部分共享的状态提升至他们最近的父组件当中进行管理。
即：把原本属于子组件的state，放在父组件里进行管理。

https://www.reactjscn.com/docs/lifting-state-up.html

```jsx
父组件：src/components/Parent.js

import React from "react";
import Son1 from "./Son1";
import Son2 from "./Son2";

export default class Parent extends React.Component {
    constructor(props){
        super(props);
        this.state = {val:'默认值'};
    }

    tempFn(val){
        console.log("tempFn");
        console.log("val",val);
        
        this.setState({
            val:val
        })
        
    }
    
    render = () => (
        <div  >
             父组件
             <hr/>
             <Son1 onMyClick={(val)=>this.tempFn(val)} />
             <Son2 val={this.state.val} />
        </div>
    )
}

Son1组件
src/components/Son1.js

export default class Son1 extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            name:"我是son1"
        };
    }

    chuan(){
        this.props.onMyClick(this.state.name);
    }
    
    render = () => (
        <div  >
             son1组件
             <input type="button" value="son1传给parent" onClick={()=>{this.chuan()}} />
             <hr/>
        </div>
    )
}

Son2组件
src/components/Son2.js

export default class Son2 extends React.Component {
    constructor(props){
        super(props);
        this.state = {};
    }
    
    render = () => (
        <div  >
            <p>son2组件</p>
            <p>val:{this.props.val}</p>
            <hr/>             
        </div>
    )
}
```



### **3）．context 状态树传参**

​      在平时使用react的过程中，数据都是自顶而下的传递方式，例如，如果在顶层组件（如：App）的state存储了theme主题相关的数据作为整个App的主题管理。那么在不借助任何第三方的状态管理框架的情况下，想要在子组件里获取theme数据，就必须的一层层传递下去，即使两者之间的组件根本不需要该数据，这样的传递方式很麻烦（代码复杂），并且浪费内存。

​        Context 旨在共享一个组件树，可被视为 “全局” 的数据，**达到越级传递**，场景：当前经过身份验证的用户，主题或首选语言，包括管理当前的 locale，theme，或者一些缓存数据。



**createContext()：**用于创建context对象（上下文），需要一个defaultValue的参数，并返回一个包含Provider（提供者），以及Consumer（消费者）的对象

**Provider：**提供者，提供数据。接收一个将要被往下层层传递的props，该值需在组件树最顶层设置。一个Provider可以关联到多个Consumers。这是一个顶层用于提供context的组件，包含一个**value**的props，value是实际的context数据。

**Consumer：**消费者，使用者，**接收一个函数作为子（DOM）节点**，函数接收当前 context 的值。这是一个底层用于获取context的组件，需要一个函数作为其子元素，该函数包含一个value的参数，这个参数就是上层所传递context value



创建一个context组件，并设定默认值。语法如下：

```jsx
 const  {Provider, Consumer} = React.createContext(defaultValue);
```

#### 从父朝子传值（外朝内）

示例代码：

```jsx
context模块: ./src/utils/myContext;

import {createContext} from "react";

export const {Provider, Consumer} = createContext({
  name:"张三疯"
});


//顶层组件：./src/App.js

import {Provider} from "./utils/myContext"

function App() {
  let val={
    name:"hi"
  };
  return (
    <div className="App">
      <Provider value={val}>
        <Home  />
      </Provider>
    </div>
  );
}

//孙子组件： App->Home->Goodslist
import {Consumer} from "../utils/myContext";

export default class GoodsList extends React.Component {
   
    render = () => (
        <div className="goodsList-box">
            <h1>商品列表：</h1>            
            <Consumer>
                    {
 					  (val)  => <div> { val.name } </div>
					}
            </Consumer>
        </div>
    )
}
```

> 注意：
>
>  export const {Provider, Consumer} = createContext({
>
> ​      name:"张三疯"
> });
>
> ​    这个默认值是在顶层组件没有使用Provider组件时的值，而不是，没有给value属性赋值时的值。
>
> 即：顶层组件的代码如下：

```
function App() {
  let val={
      name:"hi"
  };
  return (
    <div className="App">  
         <Home />
    </div>
  );
}
```

#### 在子组件改变状态树的数据

````js
//根组件：
import {Provider} from "./utils/myContext"

export default class App extends React.Component {
  constructor(props){
    super(props);  
    this.state={
        name:"宋晨",
        setName:this.fn
    } 
  }

  fn=(str)=>{
      this.setState({
        name:str
      });
  }
  
  render = () => (
    <div className="App">
      <Provider value={this.state} >
        <Home />
      </Provider>
    </div>
  )
}

//子组件里：
import {Consumer} from "../../utils/myContext";

export default class Banner extends Component {

render = () => (
  <div className="box" >
     <Consumer>
        {
           (obj)=>(
              <div>
                  <p>姓名：{obj.name}</p>
                  <input type="button" value="修改" onClick={()=>obj.setName("hiwww")} />
              </div>
          )
        }
        </Consumer>
    </div>
   )
}
````



## 高阶组件（HOC）的构建与应用

https://www.reactjscn.com/docs/higher-order-components.html

​        高阶组件（HOC）是react中对组件逻辑进行重用的高级技术。但高阶组件本身并不是React API。它只是一种模式，这种模式是由react自身的组合性质必然产生的。

​        具体而言，**高阶组件就是一个函数**，且该函数**接受一个组件作为参数**，并**返回一个新的组件**，高阶组件会对传入的组件做一些通用的处理。比如：我们希望给组件的的下方增加一个版权信息，那么就可以使用高阶组件。把原始组件传入，然后，给组件增加一个版权信息后，再返回。

​         高阶组件是通过将原组件 包裹（wrapping） 在容器组件（container component）里面的方式来组合（composes） 使用原组件。高阶组件就是一个没有副作用的纯函数（有参数，有返回值，并且在函数内部不会修改参数）。

```jsx
如：
const EnhancedComponent = higherOrderComponent(WrappedComponent);
高阶函数是：higherOrderComponent
传入的组件是:WrappedComponent
返回的组件是：EnhancedComponent
```



示例代码：

```jsx
// 带上版权的高阶函数,并且给组件赋能（增加属性和方法）
function withCopyRight(OldCom){
    //给组件赋能
    OldCom.prototype.aaa = "hi";
    OldCom.prototype.fn = function(){

    };
    
    class NewCom extends React.Component{      
        render() {
            return (
               <div>
                    <OldCom />
                    <hr/>
                    <div>
                        Copyright © 2020 Sohu All Rights Reserved. 搜狐公司 版权所有
                    </div>
               </div>
            );
        }
    }   
    
    return NewCom;
}

//原始组件
class CommentList extends React.Component {
    componentDidMount(){
        console.log("this.aaa",this.aaa);
        console.log("this.fn",this.fn);
    }
    render() {      
       return (
           <div>
               新闻11111111111111111111
           </div>
       );
    }
}

//调用高阶函数后的组件
const CommentListWithCopyRight = withCopyRight(CommentList);

ReactDOM.render( <div> <CommentListWithCopyRight /></div> ,$("box"));
function $(id){
    return document.getElementById(id);
}
```

>高阶组件的优点：
>
>1、给组件赋能
>
>2、解耦
>
>3、开闭原则

高阶组件的注意点（这些特点慢慢理解消化，使用其做了项目后，回过头来再看一下）：

1、不要在render函数里使用（调用）高阶组件

2、必须将静态（static）方法做拷贝

​        当使用高阶组件包装组件，原始组件被容器组件包裹，也就意味着新组件会丢失原始组件的所有静态方法，解决这个问题的方法就是，将原始组件的所有静态方法全部拷贝给新组件。

3、Refs不能传递

​	一般来说，高阶组件可以传递所有的props属性给包裹的组件，但是不能传递refs引用。

4、不要在高阶组件内部修改（或以其它方式修改）原组件的原型属性（prototype）。

5、约定：将不相关的props属性传递给包裹组件

6、约定：最大化使用组合

7、约定：包装显示名字以便于调试



## 路由

[官网](https://reacttraining.com/react-router/) 中文

|              | vue-router                       | react-router                                   |
| ------------ | -------------------------------- | ---------------------------------------------- |
| **路由配置** | 分离式（在一个单独的文件里配置） | 嵌套式（路由配置在组件内部，任何组件都可以写） |
| **匹配**     | 排他性（只有一个路由被匹配上）   | 包容性（多个路由可能会同时被匹配上）           |

### **1、作用：**

路由最基本的职责就是当页面的 **访问地址** **与 Route 上的 path**	匹配上时，就渲染出对应的 UI 界面（组件）。 

实现SPA应用，整个项目只有一个完整页面。页面切换不会刷新页面，内容局部更新

### **2、react-router提供了两种路由模块**

**1)．React-Router：**

提供了一些router的核心API，包括Router, Route, Switch等，但是它没有提供 DOM 操作进行跳转的API。很少用

**2)．React-Router-DOM：**

提供了 BrowserRouter,HashRouter , Route, Link，Switch等 API,我们可以通过 DOM 的事件控制路由。例如点击一个按钮进行跳转，所以在开发过程中，我们**更多是使用React-Router-DOM**。

###  **3、路由的两种模式：**

**1)．HashRouter组件：** 

url中会有个#，例如localhost:3000/#，HashRouter就会出现这种情况，它是通过hash值来对路由进行控制。如果你使用HashRouter，你的路由就会默认有这个#。

 **2)．BrowserRouter组件：**

很多情况下我们则不是这种情况，我们不需要这个#，因为它看起来很怪，这时我们就需要用到BrowserRouter。

> 记住：去掉多余的BrowserRouter 标签，**整个项目中，写一个BrowserRouter 标签就行**，否则，react路由时，不能正常渲染，需要刷新，才能渲染。



### 4、路由的配置

**1）. Route组件:**

​      vue的路由配置是分离式的，在一个单独的文件里进行路由配置。react是嵌套式的，路由配置就写在组件内部，任何组件都可以写。

​      vue中是在json数组里写好配置，然后传入vueRouter对象，而React直接使用组件BrowserRouter（或HashRouter）和Route进行配置。


- ​	Route：路径和组件的对应关系。职责就是当页面的访问地址与 Route 上的 path 匹配时，就渲染出对应的组件

​       如：

```js
<BrowserRouter>
   <Route path="/" component={App}/>  
   <Route path="/About" component={About}/>  
</BrowserRouter>

```

### 路由匹配到的组件展示在何处

也是使用**Route**组件。即：Route组件既是路由配置，又是渲染组件的地方。



### 5、路由的跳转



- **声明式导航**

  vue中使用Router-Link组件，react中使用**Link或NavLink **组件进行路由跳转导航

**1)．Link：**

主要属性是to，to可以接受string或者一个object，来控制url。

```jsx
<Link to="/About">去About</Link>
<Link to={{pathname:'/About'}}>去About</Link>

```

**2)．NavLink：**

​       它可以为当前选中的路由设置类名、样式以及回调函数等。to属性跳转路径，activeClassName当元素处于活动状态时应用于元素的样式

```
<NavLink to="/About" activeClassName="red">去About</NavLink>
<NavLink to={{pathname:'/about'}} activeClassName="red">去About</NavLink>
```



- **编程式导航**

```js

this.props.history.push({pathname:'/about'})

```

>注意：
>
>​       如果当前组件不是通过路由跳转过来的，那么，当前组件的props里，没有history。
>
>​       



### **6、基本使用步骤：**

**1)、下载路由模块：**

npm install --save react-router-dom

**2)、创建若干个组件**

创建components文件夹，并建立若干个组件

如：About.js，InBox.js，Goodslist，

**3)、路由配置：**


```jsx
src/index.js
//引入模块
import { BrowserRouter, Route } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    <Route path="/" component={App}/>  
    <Route path="/About" component={About}/>  
    <Route path="/Inbox" component={Inbox}/>  
  </BrowserRouter>,
  document.getElementById('root')
);
```

**4）、路由跳转**

**声明式导航**

在App.js里做链接跳转

```jsx
src/app.js

import { Link } from 'react-router-dom'

function App() {
  return (
    <div className="App">
       <ul>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/Inbox">Inbox</Link></li>
        </ul>
    </div>
  );
}
```

扩展：

可以尝试把 app.js里的 link 标签换成a标签。看看页面是不是会有闪动。

（PS：如果使用a标签，在每次点击时，页面被重新加载，<Link>标签会避免这种状况发生。当 你点击<Link>时，url会更新，组件会被重新渲染，但是页面不会重新加载）



### 二级路由

直接在InBox组件里在写路由配置就行：

```jsx
<BrowserRouter>
	<Link to='/gla/Pencil' >铅笔</Link> &nbsp;|&nbsp;
	<Link to='/gla/Eraser' >橡皮</Link>
	<hr/>
	<Route path='/gla/Pencil' component={Pencil} />
	<Route path='/gla/Eraser' component={Eraser} />
</BrowserRouter>
```



### 7、路由传参

路由传参完成的是组件之间的数据传递（组件传值）

**1）、params**

​	  **路由配置：**           

```
<Route path='/Inbox/:id' component={Inbox} />
```

​     **路由跳转（传值）：**

```
声明式导航：
<NavLink to={'/Inbox/01008'} >铅笔</NavLink>
编程式导航：
this.props.history.push( '/Inbox/'+'01008' )
```

​     **取值（接值）：**

```
this.props.match.params.id 
```

>优势 ： 刷新，参数依然存在
>
>缺点 ： 只能传字符串，并且，如果传的值太多的话，url会变得长而丑陋。
>
>场景：传递一个参数时使用。



**2）、query**

   **路由配置：**

​    不用改变路由配置表。

```
<Route path='/Inbox' component={Inbox} />
```

​    **路由跳转（传值）：**

```
声明式导航：
<Link to={{pathname:'/Inbox',query:{id:'01009'}}} >铅笔</Link>
编程式导航：
this.props.history.push( {pathname:'/Inbox',query:{id:'01009'}} )
```

   **取值：**

```
this.props.location.query.id
```

>优势：传参优雅，传递参数可传对象；
>
>缺点：刷新地址栏，参数丢失



**3）、state**

​        同query差不多，只是属性名不一样，而且state传的参数是加密的，query传的参数是公开的，只需要把query改为state即可。

  **路由配置：**

​    不用改变路由配置表。

```
<Route path='/Inbox' component={Inbox} />
```

​    **路由跳转（传值）：**

```
声明式导航
<Link to={{pathname:'/Inbox',state:{id:'01009'}}} >铅笔</Link>
编程式导航
this.props.history.push({pathname:'/Inbox',state:{id:"01"}});
```

   **取值：**

```
this.props.location.state.id
```

>优势：传参优雅，传递参数可传对象
>
>缺点：刷新地址栏，（hash方式会丢失参数，Browser模式不会丢失参数）



**4）、search**

 **路由配置：**

​    不用改变路由配置表。

```
<Route path='/Inbox' component={Inbox} />
```

​    **传值：**

```jsx
声明式导航
<Link to='/Inbox?a=1&b=2' >铅笔</Link>
编程式导航
this.props.history.push({pathname:'/Inbox',search:'?a=1&b=2'})
```

   **取值：**

```
this.props.location.search
```

> 用location.search所获取的是查询字符串（如：?a=1&b=2），所以，还需要进一步的解析，自己自行解析，也可以使用第三方模块：qs，或者nodejs里的query-string



### **8、路由上下文**

​       在react-router里面，如果**组件是通过路由跳转的**，那么它会把路由相关的API挂在了组件的**props**上（vue-Router使用的$router和$route），并且分为history，location，match。

**history**：历史，用来跳转的，并做历史记录。有函数：push()，replace()，go() …………

**location**：地址，地址栏上路径，并保存着query，state，search等数据。

**match**：匹配，匹配到的路径，有params



### 9、非路由跳转的组件（标签的方式）如何获取路由上下文



**先看一下，组件在页面上展示的两种情况下，props对象的内容**

1）、标签名的方式直接展示的组件（没有属性），props是空

2）、路由跳转的方式展示的组件（就算没有属性），props会自动增加属性：history,match,location。



**如果用标签名的方式，还想获取到路由上下文，有以下解决方案：**

1. 通过属性传递

   首先要求，当前组件是路由跳转过来的，然后把路由上下文通过属性的方式传递给子组件

   ```jsx
    <AboutSub02 {...this.props} />
   ```

   

2. 通过withRouter包装

   接收路由上下文的组件用withRouter函数（这是一个高阶组件）进行包裹

```jsx
import {withRouter} from 'react-router-dom'

class 组件 extends Component{
    
}

export default withRouter(组件)
```



### exact属性：

​        react的路由**匹配（path后的路径和地址路径）**默认是**模糊的**（path后面的路径只要包含在地址栏的路径里就能匹配上对应的组件）

如果想使用严格匹配（path后面的路径和地址栏的路径完全相同），那么，把Route组件的exact属性设置为true。

```js
<Route exact={true} path="/" component={App} />
```

假如，有如下路由配置：

```js
<BrowserRouter>
    <Route  path="/" component={App} />
    <Route path="/My" component={My}/>
</BrowserRouter>
```

地址栏中输入：

```js
http://localhost:3000/My
```

那么路径 “/My”，匹配到的路径是： “/” 和 “/My”，并且，在浏览器会把匹配到的所有组件的内容进行显示。

如果希望 路径  /My 值匹配 path=“/My”，那么，这么写：<Route exact={true} path="/" component={App} />



### 404

在路由配置里**不设置path属性**，那么，就总是会匹配上。404页面就需要这样做（当然还得结合Switch）

```jsx
<Route  component={Error}/> 总是会匹配
```



### Switch

​       **排他性匹配**。

​	   react默认的路由匹配是包容性的，即：匹配到的**多个路由**对应的**所有组件**会同时被显示在页面上。如果只希望显示匹配到的第一个组件（换句话说：匹配到的第一个符合要求的路径后，其它路径就不再做匹配），那么使用switch。

假如，有如下路由配置：

```js
<BrowserRouter>
    <!-- exact={true} 表示地址栏的路径和 / 必须相等 -->
    <Route exact={true} path="/" component={App} />
    <Route path="/My" component={My}/>
    <Route component={Error}/> 总是会匹配
</BrowserRouter>
```

地址栏中输入：

```js
http://localhost:3000/My
```

​       那么路径 “/My”，匹配到的路径是：“/My” 和最后一个<Route>，即，在浏览器上从上到下会显示组件My和Error的内容。



路由配置改成如下：

```jsx
  <BrowserRouter>
    <Switch>
       <Route  exact={true} path="/" component={App} />
       <Route path="/My" component={My}/>
       <Route component={Error}/> 总是会匹配
    </Switch>
  </BrowserRouter>
```

>Switch应该写在Route的外层，而且，在Switch和Route中间不能有任何组件



地址栏中输入：

```js
http://localhost:3000/My
```

那么路径 “/My”，只会让浏览器显示匹配到的第一个组件My。



>区分：exact和switch
>
>exact：表示路径匹配规则，exact={true} 表**地址栏的路径**和 **路由配置中path**一定要**完全相等**
>
>switch：表示排他性，即：一旦地址栏上路径和某个path匹配成功后，就不再匹配其它path。



### Redirect

```js
<Redirect exact={true} from="/" to="/Home" />
```



### 附：路由提供组件的详解（自行研究）

**组件及其作用：**

|            | 组件          | 作用                                                         |
| ---------- | ------------- | ------------------------------------------------------------ |
| 路由模式   | BrowserRouter | 约定模式 为 history，使用 HTML5 提供的 history API 来保持 UI 和 URL 的同步 |
| 路由模式   | HashRouter    | 约定模式 为 hash，使用 URL 的 hash 来保持 UI 和URL 的同步    |
| 声明式跳转 | NavLink       | 声明式跳转 还可以约定 路由激活状态                           |
| 声明式跳转 | Link          | 声明式跳转    无激活状态                                     |
| 重定向     | Redirect      | 重定向    ~~ replace                                         |
| 匹配并展示 | Route         | 路由配置和展示。匹配组件，并展示组件。即匹配成功后，<Route>组件立即被替换成匹配的组件 |
| 排他性匹配 | Switch        | 排他性匹配。如果不想使用包容性，那么使用Switch。             |
| 高阶组件   | withRouter    | 把不是通过路由切换过来的组件中，将 history、location、match 三个对象传入props对象上（高阶组件） |
|            |               |                                                              |

**结构**

- BrowserRouter|HashRouter （整个项目里只能有一个BrowserRouter|HashRouter ）
  
  App（或其它组件）
  - NavLink|Link
  - Route
  - Redirect
    - 子组件
      - NavLink|Link
      - Route
      - ...

**BrowserRouter**

| 属性                | 类型     | 作用                                                         |
| ------------------- | -------- | ------------------------------------------------------------ |
| basename            | string   | 所有位置的基本URL。如果您的应用是从服务器上的子目录提供的，则需要将其设置为子目录。格式正确的基本名称应以斜杠开头，但不能以斜杠结尾 |
| getUserConfirmation | Function | 用于确认导航的功能。默认使用[`window.confirm`](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)。 |
|                     |          |                                                              |

**Route**

| 属性      | 类型                    | 作用                                                         |
| --------- | ----------------------- | ------------------------------------------------------------ |
| path      | string  \|object        | 路由匹配路径。没有path属性的Route 总是会 匹配                |
| exact     | boolean                 | 为true时，要求全路径匹配(/home)。路由默认为“包含”的(/和/home都匹配)，这意味着多个 Route 可以同时进行匹配和渲染 |
| component | Function    \|component | 在地址匹配的时候React的组件才会被渲染，route props也会随着一起被渲染 |
| render    | Function                | 内联渲染和包装组件，要求要返回目标组件的调用                 |

**Link**

| 属性    | 类型                                    | 作用               |
| ------- | --------------------------------------- | ------------------ |
| to      | string \| 对象{pathname:,search:,hash:} | 要跳转的路径或地址 |
| replace | boolean                                 | 是否替换历史记录   |

**NavLink**

| 属性            | 类型                                  | 作用                                          |
| --------------- | ------------------------------------- | --------------------------------------------- |
| to              | string\|对象{pathname:,search:,hash:} | 要跳转的路径或地址                            |
| replace         | boolean                               | 是否替换历史记录                              |
| activeClassName | string                                | 当元素被选中时，设置选中样式，默认值为 active |
| activeStyle     | object                                | 当元素被选中时，设置选中样式                  |
|                 |                                       |                                               |

**Switch**   

该组件用来渲染匹配地址的第一个Route或者Redirect，仅渲染一个路由，排他性路由,默认全匹配(场景：侧边栏，引导选项卡等)

| 属性     | 类型          | 作用 |
| -------- | ------------- | ---- |
| location | string object |      |
| children | node          |      |

**Redirect**

该组件用来渲染匹配地址的第一个Route或者Redirect，仅渲染一个路由，排他性路由,默认全匹配(场景：侧边栏和面包屑，引导选项卡等

| 属性      | 类型          | 作用         |
| --------- | ------------- | ------------ |
| from      | string        | 来自         |
| to        | string object | 去向         |
| push      | boolean       | 添加历史记录 |
| exact     | boolean       | 严格匹配     |
| sensitive | boolean       | 区分大小写   |



## 补一下：async和await

### 概念：

**async和await是回调地狱的终极解决方案**，是ES7新增的两个关键字。

async：异步

await：等待

### **功能：**

将异步操作按同步操作的方式书写，即上一步未执行完，会阻塞当前函数的线程，不影响主线程

### **知识点：**

​    1）、async：修饰的函数表示函数里面有异步操作，返回值是Promise对象

​    2）、await：

​		2.1）、await必须放在async修饰的函数里。

​        2.2）、await修饰代码后，await朝后的代码（函数体里的代码）会等待。

​        2.3）、一般来说，await后跟 promise对象，或者返回Promise对象的函数；

​		2.4）、await 修饰函数（或者promise对象）后，那么，返回值变成了Promise对象中resolve的参数；

​			  如 Let res = await fn(); //res是函数fn返回的Promise对象里的resolve的参数。

​	    2.5）、如果要拿到reject里参数，就使用try catch。

如：

```js
try{

	Let res = await fn(); //res是 fn函数里Promise的resolve的参数

}catch(err){

	err 是 fn函数里Promise的reject的参数

}
```



### **完整代码：**

```js
原理使用promise的axios代码：

created(){   
    axios({
        url:"/books",
        method:"get",
        params:{
            type:this.type
        }
    })
   .then(res=>{
        this.books = res.data;        
    })
    .catch(err=>{
        console.log("服务器出错");
    })       
},

经过async和await改造后的代码(没有了回调）；

async created(){   
    try {
        // 1、发送请求
        let res = await axios({
            url:"/books",
            method:"get",
            params:{
                type:this.type
            }
        })

        //2、把获得的数据赋给变量
        this.books = res.data;

    } catch (error) {
        console.log("服务器出错",error);   
    }
}
```



如果想看细节，请查看以下两篇文章：

async和await的理解

 https://blog.csdn.net/jiang7701037/article/details/105909612 



原生JS面试题：什么是async，什么是await，async和await的区别，async和await的理解

 https://blog.csdn.net/jiang7701037/article/details/103148245 





## 选择器冲突解决方案

### 一、命名空间  BEM

BEM(Block, Element, Modifier)是由Yandex团队提出的一种前端命名规范。其核心思想是将页面 拆分成一个个独立的富有语义的块（blocks）。在某种程度上，BEM和OOP是相似的。
        Block：代表块（Block）：也是模块的意思
        Element：元素（Element）：
        Modifier： 修饰符（Modifier）
无论是什么网站页面，都可以拆解成这三部分 

https://segmentfault.com/a/1190000012705634

### 二、模块化

```jsx
import 变量  from './css/xx.module.css' 

<jsx className={变量.类名|id}

//配置1 (脚手架默认配置上了)
//webpack配置 "style-loader!css-loader?modules" | module:true
//问题：所有css都需要模块化使用

//配置2 
//改名xx.css -> xx.module.css 
//需要模块化的才修改,不影响其他非模块化css写法
```

### 三、scss

**安装: node-sass**

```scss
/*定义scss*/
$bg-color: #399;

.box{
  background: $bg-color;
}

```

**引入sass**

```jsx

//1）、普通引入
import 'xx/xx.scss'

   <jsx className="box" />



//2）、模块化
import style from xx.module.scss

   <jsx className={style.box}

```



**引入全局的（公共的）scss**

- 局部scss文件内部： @import './全局.scss'

- webpack配置一次，局部scss内部直接使用

```js
//1. 安装插件 : sass-resources-loader
//2. 配置修改webpack.config.js

{
  test:sassRegex,
  ...
  use: [
    {loader:'style-loader'},
    {loader:'css-loader'}, 
    {loader:'sass-loader'},
    {
      loader: 'sass-resources-loader',
      options:{
        resources:'./src/xx/全局主题.scss'
      }
    }
  ]
}

```

> 注意: 
> 		loader:'css-loader?modules'    ?modules 模块化时需要添加
> 		resources 指向作用域在项目环境下



## react hooks（重中之重）

### Hooks 介绍

 https://zh-hans.reactjs.org/docs/hooks-intro.html 

​       react hooks是v16.8新增的特性， 他允许你 在不写类组件（即：函数式组件）的情况下操作state 和react的其他特性（如：生命周期的钩子函数）。

​        hooks 只是多了一种写组件的方法，使编写一个组件**更简单更方便**，同时可以自定义hook把公共的逻辑提取出来，让逻辑在多个组件之间共享。

​        **Hook 是什么？** Hook 是一个特殊的**函数**，它可以让你“钩入” React 的特性。例如，`useState` 是允许你在 React 函数组件中添加 state 的 Hook。 

​        函数式组件里面没有state，所以，无状态组件我们用函数写，或者说函数式组件是无状态组件。而现在有了Hook后，函数式组件里，也可以使用state了。当然还有其它Hook。 

### **使用规则**

- Hook可让您在不编写类（组件）的情况下使用状态（state）和其他React功能
- 只能在顶层调用Hooks 。不要在循环，条件或嵌套函数中调用Hook
- 只能在functional component或者自定义钩子中使用Hooks
- 钩子在类内部不起作用，没有计划从React中删除类

 

### **useState （使用状态）：**

格式：

```jsx
1、定义状态：
const [状态名，更新状态的函数] = React.useState(初始值|函数);

如：
1）、基本类型的状态
声明一个新的叫做 “count” 的 state 变量，初始值为0 。

const [count, setCount] = React.useState(0); //useState函数返回的是数组


相当于类组件中的
this.state={
    count :0
}

2）、引用类型的状态
const [person, setPerson] = React.useState({name: '张三疯', age: 18,sex:"女"})
const [person, setPerson] = React.useState(() => ({name: '张三疯', age: 18,sex:"女"}))

2、读取值：
{count}
{person.name}   {person.age}

3、修改值：  
  setCount(5);

  //对于引用类型，不能局部更新（即：不能只改某个属性），所以，需要使用扩展运算符先拷贝以前所有的属性
  setPerson({
     ...person, //拷贝之前的所有属性
     age:person.age+1,
     name: '张四疯' //这里的name覆盖之前的name
 })
```

 >注意：
 >
 >​         首先，需要知道，函数式组件重新渲染时，会执行函数里的所有代码，
 >
 >​         那么，当函数式组件重新渲染时，会不会再次把状态的值恢复成初始值呢？答案是：不会。后续组件重新渲染时，会使用最后一次更新的状态值
 >
 >  【官网解释： React 会确保 `setState` 函数的标识是稳定的，并且不会在组件重新渲染时发生变化 】



示例代码：

```jsx
import React,{useState} from 'react';

function App() {
// 声明一个叫 "count" 的 state 变量
  const [count,setCount] = useState(0); //在App组件重新后，useState 返回的第一个值将始终是更新后最新的 count。

  return (
    <div className="App">
      <p>{count}</p>
      <input type="button" value="测试" onClick={()=>{setCount(count+1)}} />
    </div>
  );
}
```

对应的函数class组件：

```jsx
class App extends React.Component {
  state = {
      count:0
  }
  render = () => (
    <div>
       <p>{this.state.count}</p>
       <input type="button" value="测试" 
			onClick={()=>this.setState({count:this.state.count+1})} />
    </div>
  )
}
```

>  我们之前把函数式的组件叫做“无状态组件”。但现在我们为它们引入了使用 React state 的能力 



再如：

```jsx

function App() {
  const [person, setPerson] = React.useState({name: '张三疯', age: 18})
 
  const onClick = () =>{
    //setPerson不可以局部更新，如果只改变其中一个，那么整个数据都会被覆盖，所以，需要使用扩展运算符先拷贝以前所有的属性
    setPerson({
        ...person, //拷贝之前的所有属性
        age:person.age+1,
        name: '张四疯' //这里的name覆盖之前的name
    })
  }

  return (
    <div className="App">
        <p>name:{person.name}</p>
        <p>age:{person.age}</p>
        <input type="button"  value="测试" onClick={onClick} />
    </div>
  );
}
```



### useEffect 处理副作用 （生命周期钩子函数）

​        可以使得你在函数组件中执行一些带有副作用的方法，天哪，“副作用”（大脑中无数个？？？？）。

​        每当 React组件更新之后，就会触发 useEffect，在第一次的render 和每次 update 后，useEffect都会触发，不用再去考虑“初次挂载”还是“更新”。React 保证了每次运行 effect 的同时，DOM 都已经更新完毕。

​        你可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合。

​         我们在函数式组件里，没有 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount`，用useEffect。即：当数据发生变化后，渲染到组件上，组件渲染完毕后，就会调用useEffect。

格式：

```js
useEffect(回调函数，[依赖]); //在render之后触发useEffect，进一步调用回调函数
```



1、useEffect的无条件执行（只有一个参数）

```js
import React,{useState,useEffect} from 'react';

function App() {
  const [count,setCount] = useState(0);
  
  //useEffect:相当于 componentDidMount，componentDidUpdate
  useEffect(()=>{
      console.log("userEffect");
      document.title = count;
  });

  return (
    <div className="App">
      <p>{count}</p>
      <input type="button" value="测试" onClick={()=>{setCount(count+1)}} />
    </div>
  );
}
```



2、useEffect的条件执行（useEffect的第二个参数）

​         当useEffect只有一个参数时，会无条件执行，那么，当发送请求时（页面的初始数据来自后端），一旦把请求放在useEffect里，就会无休止的执行。因为，当请求的数据回来后，引起组件的更新，组件更新后，再次触发useEffect，再次发送请求，再次组件更新………………，陷入到了无限的死循环。怎么办呢？可以使用useEffect的第二个参数。

​         第二个参数表示：useEffect是否再次触发 是 依赖于某个状态的变化。当为空数组时，表示不会二次触发。即：componentDidMount时会触发，componentDidUpdate不会触发。

​	  换种解释方式：

​    1、这是格式： useEffect(回调函数，依赖的数组)

​    2、数组里是回调函数是否执行的条件，（即：数组里的状态发生变化时，才调用回调函数）

​    3、如果是空数组，表示任何状态发生变化，都不会调用回调函数。



​      如下代码，由于依赖是空，所以，useEffect只表示componentDidMount。

```js
 useEffect( async ()=>{
      let data = await getBooks();  //发送请求的代码已经封装     
      setBooks(data);      
  },[]);
```

​      如下代码，表示componentDidMount，和 count变化后引起的componentDidUpdate。

```js
 useEffect( async ()=>{
      let data = await getBooks();  //发送请求的代码已经封装     
      setBooks(data);      
  },[count]);
```



### useContext（使用状态树传参）

Context状态树的使用，比较复杂，特别是使用Consumer时。

useContext这个hook能让Context使用起来变得非常简单。不需要再使用Consumer。使用useContext就能拿到context状态树里的值。

```js
const value = useContext(context对象);
```

​       useContext函数的解释：

​         参数： context 对象（`React.createContext` 的返回值）

​        返回值： context对象的当前值（由上层组件中距离当前组件最近的 的 `value` prop 决定） 

 当组件上层最近的 Provider 更新时，该 Hook 会触发重新渲染 。

示例：

````JS
//context/index.js 创建context对象。

import {createContext} from "react";

export default createContext({count:0});

//主入口文件 index.js

import ReactDOM from 'react-dom';
import App from './App';
import Context from "./context"

let count = 10;

ReactDOM.render(
    <Context.Provider value={count}>
     <App/>
    </Context.Provider>,
  document.getElementById('root')
);


//孙子组件 App-->User-->UserAdd.js

import myContext from "../../../context";
import {useContext} from "react";

export default (props)=>{
    let context = useContext(myContext);
    console.log(context);
    return (
        <div>
            <h1>我是用户添加页面</h1>
            <p>count:{context}</p> //此处使用比起consumer是不是简单的多得多得多了呢？
            <p></p>
        </div>
    );
}
````



### useCallBack

#### 一、概念和作用

##### **1、memo高阶函数：**

memo解决的是函数式组件的无效渲染问题，当函数式组件重新渲染时，会先判断数据是否发生了变化。相当于类组件的PureComponent（默认提供ShouldComponentUpdate）



##### **2、useCallback：**

1）、useCallback会返回一个函数的memoized(记忆的)值 

2）、在依赖不变的情况下,多次定义（如：函数）的时候,返回的值是相同的 。

3）、格式：

```js
let 新的函数 = useCallback(曾经的函数, [依赖的值]) 
```



#### 二、使用场景：

##### 1、memo高阶函数的使用场景：

**不论父组件是什么类型的组件，子组件是否渲染 ：**

###### **1）、 如果子组件是类组件**（继承自PureComponent）

​                      那么是否渲染由props和state是否改变决定；

######  **2）、如果子组件是函数式组件**

​                   只要父组件渲染，**子组件会无条件渲染**。如下是代码示例：

```js
//父组件：

import { useState } from "react";
import SonFn from "./SonFn";

export default () => {
    console.log("父组件");
    const [count, setCount] = useState(1);

    let changeCount = () => {
        setCount(count + 1);
    }

    return (
        <>
            <h1>useCallback</h1>
            <p>{count}</p>
            <input type="button" value="修改count" onClick={changeCount} />
            <hr />
            <SonFn />
        </>
    )
}

//子组件：
./SonFn.js

export default ()=>{
    console.log("子组件");
    return (
        <div>
            <h5>子组件(函数式组件)</h5>
        </div>
    )
}   
```

只要点击按钮"修改count”，父组件就会刷新，而子组件SonFn也会无条件渲染（这是无效的渲染）。

######  **3）、解决方案：**

​    把子组件用高阶函数**memo**进行包裹，就能解决子组件的无条件渲染问题，即：子组件的渲染就会由props和state决定，有点像类组件继承自PureComponent的感觉。

如下是代码(只需要把子组件的代码进行修改就行)：

```js
//子组件：
import React,{memo} from 'react'

const SonFn = ()=>{
    console.log("子组件");
    return (
        <div>
            <h5>子组件(函数式组件)</h5>
        </div>
    )
}

export default memo(SonFn);
```



##### 2、useCallback的使用场景：

**父组件是函数式组件，子组件也是函数式组件（并且用memo包裹）**

###### 1）、子组件的属性是数据：

​        如果数据不变化，那么子组件不渲染，如果数据发生变化，那么子组件渲染。这里就没有性能问题。

```js
//父组件

import { useState,useCallback } from "react";
import SonFn from "./SonFn";

export default () => {
    console.log("父组件UseCallback");
    const [count, setCount] = useState(1);

    let changeCount = () => {
        setCount(count + 1);
    }

    return (
        <>
            <h1>useCallback1</h1>
            <p>{count}</p>
            <input type="button" value="修改count" onClick={changeCount} />
            <hr/>
        	{/*此处给子组件传入了数据count，count只要发生变化，子组件就会重新渲染*/}
            <SonFn count={count} />
        </>
    )
}

//子组件：
./SonFn.js

import React,{memo} from 'react'

const SonFn = ({count})=>{
    console.log("子组件");
    return (
        <div>
            <h5>子组件(函数式组件)</h5>
            <p>{count}</p>
        </div>
    )
}

export default memo(SonFn);


```



###### 2）、子组件的属性是函数（其实，只要是引用类型）时，就会出现问题：

​        父组件刷新(重新渲染)了，子组件依然会刷新（重新渲染）。因为，父组件（函数式）每次刷新时，函数都会重新定义，那么传给子组件的函数属性必然会发生变化。所以子组件会刷新，如下是示例代码：

```js
//父组件：

import { useState } from "react";
import SonFn from "./SonFn";

export default () => {
    console.log("父组件");
    const [count, setCount] = useState(1);

    let changeCount = () => {
        setCount(count + 1);
    }

    let increment = ()=>{
        console.log("increment");
    }

    return (
        <>
            <h1>useCallback</h1>
            <p>{count}</p>
            <input type="button" value="修改count" onClick={changeCount} />
            <hr/>
            <SonFn onMyClick={increment} />
        </>
    )
}

//子组件：
./SonFn.js

import React,{memo} from 'react'

const SonFn = ()=>{
    console.log("子组件");
    return (
        <div>
            <h5>子组件(函数式组件)</h5>
        </div>
    )
}

export default memo(SonFn);

```



###### 3）、解决方案：把传给子组件的函数属性，用useCallback包裹。

格式：

```js
let 新的函数 = useCallback(曾经的函数, [依赖的值]) 
```

如下是修改后的代码（只需要修改父组件的代码）：

以下代码把increment函数进行了包裹

```js
export default () => {
    console.log("父组件");
    const [count, setCount] = useState(1);

    let changeCount = () => {
        setCount(count + 1);
    }

    let increment = useCallback(()=>{
        console.log("increment");
    },[]) // 该函数永远不会重新定义(第二个参数空数组表示，任何数据发生变化，回调函数都不会再次定义)
    
    /*
    let increment = useCallback(()=>{
        console.log("increment");
    },[count]) // 当count的值发生变化是，该函数才会重新定义
	*/
    
    return (
        <>
            <h1>useCallback</h1>
            <p>{count}</p>
            <input type="button" value="修改count" onClick={changeCount} />
            <hr/>
            <SonFn onMyClick={increment} />
        </>
    )
}
```



#### 三、总结：

1、**“万恶之源”** ：函数式组件每次重新渲染时，都会把函数体里的所有代码执行一遍。

2、useCallback解决的是 防止函数式组件里的 子函数（闭包） 多次被定义。既就是：useCallback是保证函数式组件重新渲染时，组件里的函数（闭包）只被定义一次。





### useCallback 记忆函数（详细版的）

https://blog.csdn.net/jiang7701037/article/details/118461262 



### useMemo 记忆组件

```js
  //格式
  useMemo(函数,数组);

  //意思:
  // 当数组中的其中一个元素，发生变化时，就会 调用 函数 。
```

> 如：  const nameStr =  useMemo(()=>genName(name),[name]) 

> 表示，当name发生变化时，才会调用 ()=>genName(name)函数

> 如：  const nameStr =  useMemo(()=>genName(name),[name，age]) 

> 表示，当name或者age发生变化时，都会调用 ()=>genName(name)函数



​        以下代码中，如果不使用useMemo，当我们点击“修改年龄”的按钮时，也调用了函数genName()。这其实是性能的损耗。



```js
import React,{useState,useMemo} from "react";
import './App.css';


function Person({ name, age}) {
    
  console.log("Person函数");
    
  function genName(name) {
    console.log('genName')
    return '姓名：'+name;
  }

  let nameStr = genName(name);  //没有使用useMemo
    // 以下代码有点vue中的watch的感觉：当name发生变化时，才调用回调函数.
 // const nameStr =  useMemo(()=>genName(name),[name]) //此处使用  useMemo

  return (
    <>
      <div>{nameStr}</div>
      <hr/>
      <div>年龄：{age}</div>
    </>

  )
}


function App() {
  const [name, setName] = useState('张三疯')
  const [age, setAge] = useState(12)

  return (
    <>
      <button onClick={() => setName("姓名"+age)}>修改姓名</button>
      <button onClick={() => setAge(age+1)}>修改年龄</button>
      <hr/>
      <Person name={name} age={age}></Person>
    </>
  )
}

export default App;
```



useMemo：解决的是：防止无效函数调用

useCallback：解决的是：防止无效函数定义



### useRef 保存引用值 

​	   

 https://reactjs.bootcss.com/docs/hooks-reference.html#useref 

​	`useRef` 返回一个可变的 ref 对象，其(ref 对象) `.current` 属性被初始化为传入的参数（`initialValue`）。返回的 ref 对象在组件的整个生命周期内保持不变。

```jsx
import {useRef} from "react";

let refContainer = useRef(initialValue)  

<JSX ref={refContainer} ...

refContainer.current.dom操作
```

一个常见的用例便是命令式地访问子组件：

```jsx
function TextInputWithFocusButton() {
    //定义了一个ref变量：inputEl
  const inputEl = useRef(null);
  
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
    
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```



**扩展：**

https://blog.csdn.net/jiang7701037/article/details/121594727

为什么react选择了函数式组件（剖析原理）



###### 面试题：

```js
//请问以下组件里，在执行时，会打印的
function App() {
  console.log("app");
  const [count,setCount] = React.useState(0);
  
  useEffect(()=>{
    setInterval(()=>{
        setCount(5)
      }
    ,1000)
  },[]);

  console.log("app:",count);//此次会打印多少：？？

  return (
    <div className="App">
      <p>{count}</p>
    </div>
  );
}

```



后面的hooks，以后的以后有余力，再看吧



### useLayoutEffect 同步执行副作用



### 路由相关的hooks

​        在非路由跳转的组件里，要获取路由上下文对象，除了可以使用高阶组件withRouter外，react-router-dom里还提供了hooks。

- [`useHistory`](https://reactrouter.com/web/api/Hooks/usehistory)
- [`useLocation`](https://reactrouter.com/web/api/Hooks/uselocation)
- [`useParams`](https://reactrouter.com/web/api/Hooks/useparams)
- [`useRouteMatch`](https://reactrouter.com/web/api/Hooks/useroutematch)

useHistory()： 

​         useHistory 返回一个 路由上下文上的history 对象

````JS
import { useHistory } from "react-router-dom";

export default (props)=>{
    let history = useHistory();
    console.log("我是用户添加页面:props",props);
    return (
        <div>
            <h1>我是用户添加页面</h1>
            <input type="button" value="跳转" onClick={()=>history.push("/")} />
        </div>
    );
}
````



 useLocation()

​      useLocation() 返回一个路由上下文的 location 对象。

```JS
<Link to={{pathname:"/User",query:{"id":"01002"}}}>用户管理</Link>

import { useHistory,useLocation } from "react-router-dom";

export default ()=>{
    let location = useLocation();
    let history = useHistory();
    console.log("location",location);
    return (
        <div>
            <h1>我是用户添加页面</h1>
            <input type="button" value="跳转" onClick={()=>history.push("/")} />
        </div>
    );
}
```



useParams()

​     useParams() 返回当前匹配的路径上的 params 


 <Link to={{pathname:"/User/01003"}}>用户管理</Link>

import { useHistory,useLocation,useParams } from "react-router-dom";

export default ()=>{
    let location = useLocation();
    let history = useHistory();
    let params = useParams();

    console.log("location",location);
    console.log("params",params);
    
    return (
        <div>
            <h1>我是用户添加页面</h1>
            <input type="button" value="跳转" onClick={()=>history.push("/")} />
        </div>
    );
}




useRouteMatch

​        useRouteMatch 可以有一个参数 path，如果什么都不传，会返回当前 context 上的 match 的值，一定是 true。如果传了 path，会比较这个 path 和当前 location 是否 match。 





视情况手写redux





## 状态管理（重中之重）

- 思想：flux
- 实现：vuex  redux

### redux

​	1. Redux是为javascript应用程序提供一个可预测（给一个相同的输入，必然会得到一个相同的结果）的状态容器。可以运行于服务端，客户端，原生应用，从Flux演变而来。简单容易上手。

2.    集中的管理react中多个组件的状态 

3.  **redux是专门作状态管理的js库，并不是react的插件库，也可以用在其他js框架中，例如vue，但是基本用在react中**

​        可以同一个地方查询状态，改变状态，传播状态，用在中大项目，如下场景：组件状态需要共享，在任何地方都可以拿到，组件需要改变全局状态，一个组件需要改变另外一个组件的状态。创建store实例，其它组件导入并共享这个store实例



#### **redux成员**

| 成员                       | 作用                                                         | 类型 |
| -------------------------- | ------------------------------------------------------------ | ---- |
| createStore(reducer,state) | 创建**store**实例(state:仓库存放的数据；reducer：对数据的操作) | 函数 |
| combineReducers            | 合并多个reducer                                              | 函数 |
| applyMiddleware            | 安装中间件，改装增强redux                                    | 函数 |

#### **store成员**

| 成员                | 作用                                           | 类型 |
| ------------------- | ---------------------------------------------- | ---- |
| subscribe(回调函数) | 订阅state变化                                  | 函数 |
| dispatch(action)    | 发送action 给 reducer                          | 函数 |
| getState()          | 获取**一次**state的值                          | 函数 |
| replaceReducer      | 一般在 Webpack Code-Splitting 按需加载的时候用 | 函数 |

#### **数据流动**

| component（views）  | action              | reducer                                        | state    | component（views） |
| ------------------- | ------------------- | ---------------------------------------------- | -------- | ------------------ |
| 展示state           | 转发的动作,异步业务 | 同步业务处理逻辑, 修改state，并且返回新的state | 状态收集 |                    |
| store.dispatch---》 | -------------》     |                                                |          | 《--subscribe      |
|                     |                     |                                                |          | 《--getState       |



安装：

```js
yarn add redux
```



#### **示例代码：**

**1）、组件从store中获取state(数据)）：**

```jsx
//创建一个store，
//创建store需要reducer和state。即就是：创建仓库时，需要说清楚仓库中存储的数据（state），以及对数据的操作（reducer）

./src/store/index.js

import { createStore} from "redux";
import state from "../store/state";
import reducer from "../store/reducer";

//1、 创建仓库，并且说清楚，仓库里存储的数据，以及，对仓库数据的操作
// createStore ：创建仓库的
// state：仓库里存储的数据
// reducer:对仓库数据的操作
let store = createStore(reducer,state);

export default store;

//2、创建state
//state就是仓促存储的数据（对象）
./src/store/state.js

export default {
    count:0
}

//3、创建reducer
//对仓库数据进行操作的函数（函数）。
//要求：传入旧的state，返回新的state。

./src/store/reducer.js

// reducer要求是个纯函数（在函数内部不能修改函数的参数(输入)，要有返回值），它的功能是：传入旧的state，根据action对state做操作，返回新的state。
// 参数：
// state：原始的state
// action：要做的事情，动作的类型
// 返回值：必须要有，是新的state（修改后的state）。getState()函数会调用reducer

// 因为是纯函数，所以，在函数内部，不能修改state和action。
let reducer = (state,action)=>{
    if(action.type){
      //对state的操作 
    }
    switch(action.type){
        case 添加:……………………return 新的state;break;       
        case 删除:……………………return 新的state;break;       
    }
    return state;//返回新的state
}

export default reducer;


//组件
./src/App.js
import store from "./store/index";

class App extends React.Component {
  state = {

  }
  render = () => (
    <div>
      <div className="App">
        <p>{store.getState().count}</p>
      </div>
    </div>
  )
}
```

>注意：getState()函数内部也调用了reducer。即：getState()获取的值是reducer的返回值。
>
>

**2）、通过组件修改store中的state(数据)）：**

注意：修改store中的state（数据）后，还需要把数据响应到组件上，就需要使用 subscribe，并使用组件中状态（否则，组件不会重新渲染）。



```jsx
修改reducer
./src/store/reducer.js

let reducer = (state,action)=>{
    let {type,payload} = action;
    switch(type){
        case "INCREMENT":{
            console.log("加一");
            return {
                ...state,
                count:state.count+payload
            }
        }
        default:return state;
    }
}

//修改App组件的代码
./src/App.js
class App extends React.Component {
  constructor(props){
    super(props);
    this.state = {
         count:store.getState().count
    }

    store.subscribe(()=>{
        this.setState({
          count:store.getState().count
        });
    })
  }

    inc(){
        store.dispatch({type:"INCREMENT",payload:10});
    }
  
    render = () => (
      <div>
        <div className="App">
          <p>{this.state.count}</p>
          <input type="button" value="增加" onClick={()=>{this.inc()}} />  
        </div>
      </div>
    )
  }
```



#### **操作流程总结**

```jsx
安装：
yarn add redux

import {createStore} from 'redux'

//一、创建reducer
//reducer：对仓库（数据）的操作
//参数：
// state：传入的旧数据（原始数据）
// action:对数据的操作
//返回值：操作后的新的数据

const reducer = (state,action)=>{
  let {type,payload}=action    
  swtich (type){
  	case XXXXX :{
    	  //数据的逻辑处理
      	return {
	          ...state,
	          属性名：新的值
	      }
  	}   
  	default:
	    return state
  }
}

//二、创建state对象
// 仓库里的数据
export default {
    count:0
}

//三、创建store对象（仓库）
//使用createStore(对仓库的操作，仓库的数据)
store = createStore(reducer,state)
export default store;

//四、在组件内部使用仓库（如：获取仓库的数据，修改仓库的数据，添加，删除）

import store from '...'

store.getState() //获取状态，执行一次

store.dispatch({type:xxx,payload:ooo}) //发送action给reducer  type是必传参数

store.subscribe(回调)  //订阅 state  更新state时触发
```



#### 提取并定义 **Action **

```jsx
./src/store/actions.js

export const increment = payload =>({
    type:"INCREMENT",
    payload
})

export const decrement = payload =>({
    type:"DECREMENT",
    payload
})

App.js组件
./src/App.js

import { increment } from "./store/actions";

store.dispatch(increment(10));
```



#### **action里处理异步**

需要安装中间件 redux-thunk ，redux-thunk可以增强dispatch的功能：让dispatch可以接受一个函数作为参数。

```jsx
./src/store/index.js

//安装中间件改装 redux
import {createStore,applyMiddleware} from 'redux'
import thunk from 'redux-thunk'

let store = createStore(reducer,state,applyMiddleware(thunk));

./src/store/actions.js

//处理异步
export let ADD =()=>((dispatch)=>{
    axios({
        url:"http://localhost:3000/inc",
    })
    .then(res=>{
        dispatch({
            type:"INCREMENT",
            payload:res.data.num
        })
    })
})


App组件
./src/App.js

import { increment,decrement,ADD } from "./store/actionCreators";

store.dispatch(ADD());
```



#### **combineReducers**提取reducer

​         当应用逻辑逐渐复杂的时候，我们就要考虑将巨大的 Reducer 函数拆分成一个个独立的单元，这在算法中被称为 ”分而治之“，拆分后的reducer 在 Redux 中实际上是用来处理 Store 中存储的 State 中的某一个数据，一个 Reducer 和 State 对象树中的某个属性对应。
​         把一个大的reducer拆成若干个小的。注意：把大的state也分开，并且放在每个reducer。也就是说，每个reducer里面写上它要操作的数据，这样的话，在一个reducer里包含了，数据（state）和数据的操作（reducer）。

```jsx
// ./src/plugins/myRedux.js

import {createStore,applyMiddleware,combineReducers} from 'redux'
import thunk from 'redux-thunk'
import count from "../store/reducers/count";
import todos from "../store/reducers/todos";

let rootReducer = combineReducers({
    todos:todos,
    count:count
});
//去掉了第二个参数state（因为state拆分到了每个reducer里了）
export default createStore(rootReducer,applyMiddleware(thunk));


// ./src/store/reducers/count.js

// 是当前reducer要操作的数据
let initCount = 8;

const count = (count=initCount,action)=>{    
    switch(action.type){
        case "INCREMENT":{
            return count+action.payload;
        }
        case "DECREMENT":{
            return count-action.payload;
        }
        default: return count;
    }
}

export default count;


// ./src/store/reducers/todos
let initState=[] //当前reducer所操作的数据，放在里自己的模块里。

const todos = (todos, action) => {
  switch (action.type) {
    case "ADD_TODO": {
      return [
        ...todos,
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ]
    }

    case "REMOVE_TODO": {
      const { id } = action;
      todos.map((item,index) => item.id ===id && todos.splice(index, 1));
      return [...todos]
    }

    case "CHECK_TODO": {
      const { id } = action;
      todos.map((item,index) => item.id ===id && (todos[index].completed=!todos[index].completed));
      return [...todos]
    }

    default:
      return todos;
  }
};

export default todos;

//删除掉state文件。

//组件里：
写法基本上不变
store.getState().todos
```

> state数据不写在组件内部订阅，可以写在主入口文件 订阅store数据的更新

```jsx
let render = ()=>{
    ReactDOM.render(
      <App/>,
      document.getElementById('root')
    )
};
render();
store.subscribe(render);
```

### react-redux

​        基于redux,专门为react使用redux而生，react-redux是连接redux和react组件的**桥梁**

​       react-redux做了哪些事情？

首先，看看redux里的问题：

1、组件中出现了大量的store对象

2、 在redux里，凡是使用state里数据的组件，必须加上 store.subscribe() 函数，否则，数据不是响应式的



#### **react-redux的API**

（react-redux仅有2个API）

- ​        **<Provider/>组件**：可以让组件拿到state（不需要使用传统的subscribe（）来监听state重绘组件）

  ```jsx
  import {Provider} from "react-redux";
  import store from './redux/store'
  
  ReactDOM.render((
      <Provider store={store}>
          <App/>
      </Provider>
  ), document.getElementById('root'));
  ```

  


- **connect():** 链接 ，（返回值）是个高阶组件，用来链接react组件和redux（组件状态要从redux中获取）


​	        connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])

​          **功能：**把store和react组件联系在一起。只要store发生了变化就会调用mapStateToProps方法。Connect方法（的返回值）就是个高阶组件。

​         **参数1：**mapStateToProps是个函数，

​                     **功能: ** 把仓库里的state合并到当前组件里的**props**上。给mapStateToProps函数**传入所有state**，它**返回指定的state数**据（需要合并到组件props中的state）。返回的state与组件的 props 合并（**联系的体现**）。另外，当store发生变化时，mapStateToProps方法就会更新组件里的props，那么组件就更新了（因为props变了）。

​                      **参数：**

​						state：所有的state    

​					**返回值：**

​						指定的state（组件里需要的state）。

​				**示例代码：**

​					

```jsx
const mapStateToProps = (state)=>{
      return {
           count:state.count
      }
}
```

**参数2：mapDispatchToProps函数**

**功能：**

​         把dispatch和props联系起来。传入dispatch，返回绑定好的action方法。

​         更改数据必须要触发action，所以，mapDispatchToProps把 action 作为组件的props 进行绑定（联系的体现）,要派发的函数名，可以是多个函数。mapDispatchToProps 就是用于建立组件跟store.dispatch(是action)的映射关系。

 **参数：**

​	**dispatch:** 派发

​	**ownProps:**当前组件的props，即使用标签时，传入的props

 **返回值：**

​          对象：表示所有dispatch的对象

 **示例代码：**

```jsx
import React from "react";
import './App.css';
import {ADD,REDUCE} from "./store/action";
import {connect} from "react-redux";

class App extends React.Component {

  add(){
    this.props.add();
  }

  reduce(){
    this.props.reduce();
  }

  render = () => {
      return (
          <div className="App">
            <p>{this.props.count}</p>
            <input type="button" value=" 加 " onClick={()=>this.add()} />
            <input type="button" value=" 减 " onClick={()=>this.reduce()} />            
          </div>
        )
    }
}

let mapStateToProps= state=>{
  return {
    count:state.count
  }
}

let mapDispatchToProps = dispatch=> ({
  add: () => dispatch(ADD()),
  reduce: () => dispatch(REDUCE())  
})

export default connect(mapStateToProps,mapDispatchToProps)(App);
```

​	

**react-redux的思路：**

1）、用Provider包裹最顶层的组件，提供一个store属性。这样在任何组件里都可以使用store了。

2）、使用connect()函数来**链接react的组件和redux的store**。记住：connect不能单独使用，必须要有Provider



#### 最佳实现（完整的代码）

```jsx
安装：npm install --save react-redux

//1、主入口文件 index.js
import {Provider} from 'react-redux'
import store from './plugins/redux'

<Provider store={store}>
  <App/>
</Provider>
  
 //2、容器组件里：App组件
 
import {connect} from "react-redux";
 
class App extends React.Component {
  add(){
    //直接用props来调用dispatch，而不需要store
    this.props.dispatch({
      type:"INCREMENT",
      payload:2
    });
  }
    
  render = () => (
   	 <div className="App">
        <p>{this.props.count}</p>  //  使用props可以直接拿到state里的数据,而不需要store
        <input type="button" value=" 加 " onClick={()=>this.add()} />
     </div>
   )    
}

 //容器组件对外开放时，（把redux里的state转到props） 
export default connect((state)=>{
  return {
    count :state.count    
  }
})(App);
 
```



在react-redux里，把组件进行拆分（容器组件和UI组件）

容器组件：处理业务逻辑，有状态（在redux里存放）组件，也叫智能组件

UI组件：只做展示，就是无状态组件，也叫木偶组件



## ui组件  ant-design Ant-Design-Mobile  element-ui (react)等



[ant-design](https://ant.design/docs/react/introduce-cn)



## immutable

###  Immutable.js 介绍 



​      引用类型的变量的优点是节约内存，我们称这样的方式是Mutable（可变的）。但是当一个项目越来越复杂的时候，Mutable带来的内存优势，消失殆尽。虽然我们可以进行deepCopy（深拷贝），但是这样会造成CPU和内存的浪费。Immutable就是来解决这样的问题的。

​        Immutable（ 不可改变的 ） Data 就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是 Persistent Data Structure（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了Structural Sharing（**结构共享**），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享 。

 ![immutable](https://segmentfault.com/img/bVsXeZ) 

（1）Immutable优点：

 

减少内存的使用（深拷贝消耗内存）

并发安全

降低项目的复杂度

 

（2）Immutable缺点：

库的大小（建议使用seamless-immutable）

对现有项目入侵严重

容易与原生的对象进行混淆



###  深拷贝与浅拷贝的关系 

面试题：深拷贝和浅拷贝(超级详细，有内存图)

https://blog.csdn.net/jiang7701037/article/details/98738487 



###  Immutable 中常用类型(Map，List) 

**Map()**

作用：复制一个对象(键值对)

参数：json对象

返回值：Map对象（经过immutable包装了的Map对象） ,该Map对象也可以使用set，get方法。但是set方法调用后会产生一个新的Map对象

```
Immutable.Map(json对象);
```

 示例：

```jsx
npm i --save immutable

function f(){
    //定义一个对象obj1
     var obj1 = {
        id:"007",
        name:"张三疯",
        address:{
            province:"陕西省",
            city:"西安市"
        }
    };
    //浅复制：let obj2 = obj1;  两个对象会共享同一块内存区域
    //深拷贝：两个对象的内存是独立的，
    
    //使用Immutable.Map()进行复制，相同的数据会共享。
    let obj2 = Immutable.map(obj1).toJS();
    //修改数据时，只把改动数据及其父级数据部分进行复制，其它部分不做复制，依然共享，节约了内存。
    obj1.address.province="北京";
    console.log(obj1);//obj1.address.province是北京
    console.log(obj2);//obj2.address.province是陕西
}
```



Map对象的set函数会产生一个新的对象

示例：

```js

function f(){
   var obj1 = {
        id:"007",
        name:"张三疯"
    };

  let obj2 = Immutable.Map(obj1);  
    
  let obj3 = obj2.set("name","李思峰");  
  let obj4 = obj2.set("sex","男");

  console.log(obj2.toJS());   //{id: "007", name: "张三疯"}
  console.log(obj3.toJS());    //{id: "007", name: "李思峰"}
  console.log(obj4.toJS());    //{id: "007", name: "张三疯", sex: "男"}
}
```



**List**

可重复的有序列表。对应原生JS的 Array

作用：复制一个数组

参数：数组

返回值：List。

是有序的索引密集集合，类似于JavaScript数组，针对List类型有set和get方法，分别表示设置和获取

```jsx
function f(){
    
  const persons = ['芙蓉姐姐','春哥','犀利哥']
  let ipersons = Immutable.List(persons);
  
  let ipersons2 = ipersons.set( 1, '干露露');

  console.log("ipersons",ipersons);//List对象
  console.log("ipersons.toJS()",ipersons.toJS()) 

  console.log("ipersons2",ipersons2);//List对象
  console.log("ipersons2.toJS()",ipersons2.toJS()) 
}
```



###  Immutable 中常用方法(fromJS,toJS，is())

```jsx
// (1) Immutable.fromJs()，把一个js对象(数组)转化为Immutable对象。

    let map = Immutable.fromJS({ a: 1,b: 1,c: 1 });
    console.log(map);

    let list = Immutable.fromJS(['芙蓉姐姐','春哥','犀利哥']);
     console.log(list);

// (2) immutable对象.toJS()，把一个Immutable对象转化为js对象(数组)。
 	 let o =  map.toJS();
     console.log(o);
	 let arr2 = list.toJS();
      console.log(arr2);

// (3) Immutable.is():比较两个Map（或List）的值是否相等
   var map1 = Immutable.Map({ a: 1,b: 1,c: 1 }); 
   var map2 = Immutable.Map({ a: 1,b: 1,c: 1 });
   console.log(Immutable.is(map1, map2));//true
```

###  Immutable + Redux 的开发方式

​       如果我们希望，redux中的state也是用immutable的方式。那就需要使用redux-immutable提供的`combineReducers`来代替。 

```jsx
//创建仓库

import {combineReducers} from 'redux-immutable';
import { createStore } from "redux";

import reducer from "../store/reducer";


let rootReducers = combineReducers({reducer});

let store = createStore(rootReducers);

export default store;
```

reducers中的state也需要使用immutable

```js
//reducer

// 初始化reducer中的initialState时建议用List方法而不是fromJS方法，效率更高
import Immutable from "immutable";

let initState=Immutable.List([
    {
        text:"吃饭",
        isComplete:false
    },
    {
        text:"睡觉",
        isComplete:true
    }
])

export default (state=initState,action)=>{
    //let state1 = state.toJS();
    let {type,payload} = action;
    switch(type){
        case "ADDTODO":{
            let obj = {
                text:payload,
                isComplete:false
            }
            return state.set(state.size,obj]);
        }
        default:return state;
    }
}
```

组件里面，获取到数据后，就需要使用toJS()进行转换。或者使用immutable的函数进行操作。

```js
组件：
class App extends React.Component {
  constructor(props){
    super(props);
    console.log("store.getState()",store.getState().toJS().reducer);
    this.state ={
        todos:store.getState().toJS().reducer,
        text:""
    }
  }

  componentDidMount(){
    store.subscribe(()=>{
      this.setState({
        todos:store.getState().toJS().reducer
      })
    });
  }
 …………………………
```



## mobx（视情况介绍一下就行）

https://cn.mobx.js.org/intro/overview.html

###      介绍 

​         一款可以与redux媲美的数据流方案。Flux思想**单向数据流方案，以 Redux 为代表**；Reactive**响应式数据流方案，以 [Mobx](https://cn.mobx.js.org/) 为代表**；

- 单向数据流实现：redux + react-redux + react-thunk

- 响应式数据流实现：mobx + mobx-react

​        MobX 的理念是通过观察者模式对数据做出追踪处理，在对可观察属性作出变更或者引用的时候，触发其依赖的监听函数，整体的store注入机制采用react提供的context来进行传递

适用场景可以是react vue angular mpvue 小程序 taro 



### 装饰器Decorator

是个**函数**,用来装饰类或者类成员 ，是Object.defineProperty的语法糖

```jsx
//给对象添加或修改属性
Object.defineProperty(target, prop, desc)
//target 需要定义属性的当前对象
//prop 当前需要定义的属性名 类型：字符
```

| desc         | 默认值    | 说明                                                      |
| ------------ | --------- | --------------------------------------------------------- |
| configurable | false     | 描述属性是否可以被删除，默认为 false                      |
| enumerable   | false     | 描述属性是否可以被for...in或Object.keys枚举，默认为 false |
| writable     | false     | 描述属性是否可以修改，默认为 false                        |
| get          | undefined | 当访问属性时触发该方法，默认为undefined                   |
| set          | undefined | 当属性被修改时触发该方法，默认为undefined                 |
| value        | undefined | 属性值，默认为undefined                                   |

```jsx
//定义装饰器(以下代码会被转成Object.defineProperty的写法)
function 装饰器名 (target,prop,descriptor){
  descriptor.writable=false;//writable属性是否可以写入
}

//使用装饰器
@装饰器名 类
@装饰器名 实例属性|静态属性（类的属性）
@装饰器名 实例方法|静态方法（类的方式）

//使用场景
mobx / angluarTs / vueTs / reactTs / java ...

```



**配置**

create-react-app脚手架 不支持装饰器语法，需要小配一下

```jsx
yarn add @babel/plugin-proposal-decorators --save
```

package.json

```json
babel: {
  "presets":...

  +
  "plugins": [
      ["@babel/plugin-proposal-decorators", { "legacy": true }],
   ]

  ....
}
```

**vscode**编辑配置

```jsx
去掉vscode中使用装饰器写法时的，波浪线提示
vscode->文件->首选项-->设置->搜索：experimentalDecorators->勾上  
//webstrom 无需设置
```



```bat
如果提示：
Parsing error: Using the export keyword between a decorator and a class is not allowed. Please use `export @dec class` instead.

请安装：
npm install --save-dev babel-plugin-transform-decorators-legacy

```



>模块的版本：
>
> "@babel/core": "7.12.3",
>
>  "@babel/plugin-proposal-decorators": "^7.12.12",
>
>  "@pmmmwh/react-refresh-webpack-plugin": "0.4.2",
>
>  "@svgr/webpack": "5.4.0",
>
>  "@testing-library/jest-dom": "^5.11.4",
>
>  "@testing-library/react": "^11.1.0",
>
>  "@testing-library/user-event": "^12.1.10",
>
>  "@typescript-eslint/eslint-plugin": "^4.5.0",
>
>  "@typescript-eslint/parser": "^4.5.0",
>
>  "antd": "^4.10.0",
>
>  "axios": "^0.21.1",
>
>  "babel-eslint": "^10.1.0",
>
>  "babel-jest": "^26.6.0",
>
>  "babel-loader": "8.1.0",
>
>  "babel-plugin-named-asset-import": "^0.3.7",
>
>  "babel-preset-react-app": "^10.0.0",
>
>  "bfj": "^7.0.2",
>
>  "camelcase": "^6.1.0",
>
>  "case-sensitive-paths-webpack-plugin": "2.3.0",
>
>  "css-loader": "4.3.0",
>
>  "dotenv": "8.2.0",
>
>  "dotenv-expand": "5.1.0",
>
>  "echarts": "^5.0.0",
>
>  "eslint": "^7.11.0",
>
>  "eslint-config-react-app": "^6.0.0",
>
>  "eslint-plugin-flowtype": "^5.2.0",
>
>  "eslint-plugin-import": "^2.22.1",
>
>  "eslint-plugin-jest": "^24.1.0",
>
>  "eslint-plugin-jsx-a11y": "^6.3.1",
>
>  "eslint-plugin-react": "^7.21.5",
>
>  "eslint-plugin-react-hooks": "^4.2.0",
>
>  "eslint-plugin-testing-library": "^3.9.2",
>
>  "eslint-webpack-plugin": "^2.1.0",
>
>  "file-loader": "6.1.1",
>
>  "fs-extra": "^9.0.1",
>
>  "html-webpack-plugin": "4.5.0",
>
>  "http-proxy-middleware": "^1.0.6",
>
>  "identity-obj-proxy": "3.0.0",
>
>  "immutable": "^4.0.0-rc.12",
>
>  "jest": "26.6.0",
>
>  "jest-circus": "26.6.0",
>
>  "jest-resolve": "26.6.0",
>
>  "jest-watch-typeahead": "0.6.1",
>
>  "mini-css-extract-plugin": "0.11.3",
>
>  "mobx": "^6.0.4",
>
>  "mobx-react": "^7.0.5",
>
>  "optimize-css-assets-webpack-plugin": "5.0.4",
>
>  "pnp-webpack-plugin": "1.6.4",
>
>  "postcss-flexbugs-fixes": "4.2.1",
>
>  "postcss-loader": "3.0.0",
>
>  "postcss-normalize": "8.0.1",
>
>  "postcss-preset-env": "6.7.0",
>
>  "postcss-safe-parser": "5.0.2",
>
>  "prompts": "2.4.0",
>
>  "react": "^17.0.1",
>
>  "react-app-polyfill": "^2.0.0",
>
>  "react-dev-utils": "^11.0.1",
>
>  "react-dom": "^17.0.1",
>
>  "react-redux": "^7.2.2",
>
>  "react-refresh": "^0.8.3",
>
>  "react-router-dom": "^5.2.0",
>
>  "redux": "^4.0.5",
>
>  "redux-thunk": "^2.3.0",
>
>  "resolve": "1.18.1",
>
>  "resolve-url-loader": "^3.1.2",
>
>  "sass-loader": "8.0.2",
>
>  "semver": "7.3.2",
>
>  "style-loader": "1.3.0",
>
>  "terser-webpack-plugin": "4.2.3",
>
>  "ts-pnp": "1.2.0",
>
>  "typescript": "^4.1.3",
>
>  "url-loader": "4.1.1",
>
>  "web-vitals": "^0.2.4",
>
>  "webpack": "4.44.2",
>
>  "webpack-dev-server": "3.11.0",
>
>  "webpack-manifest-plugin": "2.2.0",
>
>  "workbox-webpack-plugin": "5.1.4"
>
>



### 示例：

```jsx
定义一个只读装饰器（其实就是个函数），设定属性是只读的。
function  readonly(target,prop,desc) {
  desc.writable = false;
}

class Dog{
  //使用装饰器，修饰dark属性为只读的
   @readonly dark="wangwang";
}

let d =  new Dog();
d.dark="mie"; //这个是不能修改成功的
console.log(d.dark);
```



### mobx成员

`observable` 装饰类和其成员

@observable 装饰store类的成员，让store中的成员成为**被观察者**，即：在组件里可以订阅该数据的变化。

Mbox的思想是：把state和reducer都放在一个类中。state就是数据，reducer就是方法。

示例：创建仓库

```js
./src/mobxstore/index.js
    
import { observable } from 'mobx';

//1)、state：
//（在对象外面套上observable()函数，就可以，让对象里的数据可以被订阅）
let appStore = observable({
    count:0
});

//2）、reducer:
appStore.increment = function () {
    console.log("increment");
    appStore.count += 1;
}

export default appStore;
```



###  mobx-react成员

`Provider`  `inject` `observer`  

**Provider**，顶层提供appStore的服务

```jsx
<Provider appStore={appStore}>

</Provider>
```

**inject**：在组件里使用仓库对象（appStore）。 Provider和inject结合起来，就可以让顶层提供的仓库对象（appStore）在组件里使用

```js
@inject('appStore')
class Index extend React.Component{
     render(){
         <div>
         	{this.props.appStore.属性名}
         </div>
     }
}
```

**observer**：能够监听到数据的变化，并响应到组件里。即：在组件里订阅了store中的数据，当store中的数据发生变化时，就会发布给组件。

```jsx
@observer
class Index extend React.Component{
     render(){
         <div>
         	{this.props.appStore.属性名}
         </div>
     }
}
```



###  Mobx 的使用 

**1、安装：**npm install mobx --save   或者  yarn  add  mobx 
**2、创建仓库**

  文件名： ./mobxstore/index.js

```jsx

import { observable } from 'mobx';
//1)、定义状态并使其可观察
//state：（在对象外面套上observable()函数，就可以，让对象里的数据可以被订阅）

const appStore = observable({
    count: 0
});

appStore.increment = function () {
    console.log("increment");
    appStore.count += 1;
}

export default appStore;
```

**3、入口文件：**

在顶层提供 仓库对象（appStore）

```jsx
//index.js

import appStore from './mobxstore';
import { Provider } from "mobx-react";

ReactDOM.render(
  <BrowserRouter>
    <Provider appStore = {appStore}>
      <Route path="/" component={App} />
    </Provider>
  </BrowserRouter>,
  document.getElementById('root')
);
```

4、使用数据的组件

需要使用 observer,inject

```jsx

import { Component } from "react";
import {observer,inject} from "mobx-react";

@inject('appStore')  // 使用appStore对象，对应着顶层组件的Provider提供的appStore，同时，把appStore映射到当前组件的props上了
@observer   //监听appStore中数据的变化，即：只要appStore中的数据发生变化，就会重新渲染该组件
class Index extends Component {    
    //这个不能用箭头函数     
    render(){
        return(
                <div>
                    count：{this.props.appStore.count}                    
                </div>
            )
    }
}

export default Index;

```

5、修改数据的组件：

需要使用 inject

```jsx
import {observer,inject} from "mobx-react";

@inject('appStore')  // 使用appStore对象，对应着顶层组件的Provider提供的appStore
export default class Layout extends Component {

    incCount(){
        this.props.appStore.increment();
    }
    
    render = () => (
        <div className="box">
          <input type="button" value=" 加 " onClick={this.incCount.bind(this)} />                 
        </div>
    )
}
```



## React17

​        React 17版本不同寻常，因为它没有添加任何面向开发人员的**新功能**。取而代之的是，该发行版主要致力于简化React本身的升级。 

​       react17 ，不建议使用componentWillMount，componentWillUpdate，这些生命周期钩子函数，因为，这些钩子函数函数有安全问题



### 事件代理更改

​         在React 16和更早的版本中，React将对大多数事件代理执行**document**.addEventListener（）。 React 17将在后调用rootNode.addEventListener（）。 

………………

 https://segmentfault.com/a/1190000037552821 



## react+typescript

### 搭建项目（支持typescript）：

```
create-react-app 项目名称 --typescript
```



### 不同

文件扩展名：js变成了ts，jsx变成了tsx



### 代码示例：

1）、类组件里使用ts

```tsx
//类组件里使用ts

import React from "react";

interface IProps {
    name:string,
    age?:number
}

interface IState {
    color:string
}

export default class Home extends React.Component<IProps,IState>{
    constructor(props:IProps){
        super(props);
        this.state = {
            color:"red"
        }
    }

    changeColor=()=>{
        this.setState({
            color:"blue"
        });
    }

    render=()=>{
        return (
        <div>
            <p style={{color:this.state.color}} >姓名：{this.props.name}</p>
            <p>姓名：{this.props.age || 0}</p>
            <input type="button" value="改色" onClick={this.changeColor} />
        </div>
        )
    }
}
```

2）、函数式组件

```tsx
//Counter组件代码：

import React from "react";

interface IProps {
    count:number,
    increment:()=>void
}
 
const Counter=(props:IProps) =>{
//const Counter=({count,increment}:IProps) =>{       
    return(
        <>
            <p>{props.count}</p>
            <input type="button" value=" 加 " onClick={props.increment} />
        </>
    )
}

export default Counter;

App组件代码：

import React from 'react';
import logo from './logo.svg';
import './App.css';
import Home from './pages/Home/Home';
import Counter from './pages/Counter/Counter';

interface IProps {
}

interface IState {
  count:number
}

export default class App extends React.Component<IProps,IState>{
  constructor(props:IProps){
      super(props);
      this.state = {
          count:0
      }
  }

  increment=()=>{
    this.setState({
      count:this.state.count+1
    });
  }

  render=()=>{
    return (
      <div className="App">
          <Home name="张三丰" age={12} />
          <Counter count={this.state.count} increment={this.increment}/>
      </div>
     );
   }
}
```

高阶组件：

```tsx
import React from 'react';

class News extends React.Component{

  render=()=>{
    return (
              <div className="App">
                  <p>我是新闻，我拍谁</p>
                  <p>我是新闻，我拍谁</p>
                  <p>我是新闻，我拍谁</p>
              </div>
            );
    }
}

//高阶组件
const withCopyRight = (WrappedCom:React.ComponentType)=>{
    return class extends React.Component{
        render=()=>(
                <>
                    <WrappedCom></WrappedCom>
                    <div>
                        @copyright:版权所有 2002A
                    </div>
                </>
            );
    }
}

export default withCopyRight(News);
```



事件处理，传递事件对象（React.mouseEvent）

```tsx
定义 Event组件

import React, { ReactNode } from "react";

interface IProps{
    name:string,
    click:(e:React.MouseEvent):void
}

export default class Event extends React.Component<IProps>{

    render=()=>{
      return (
        <div className="App">
            <input type="button" value="测试" onClick={this.props.click} />
        </div>
      );
      }
  }
  
 
 //引用Event组件
import React from 'react';
import logo from './logo.svg';
import './App.css';
import Event from './pages/Event/Event';

interface IProps {
}


export default class App extends React.Component<IProps>{
  constructor(props:IProps){
      super(props);
   }


  handClick=(e:React.MouseEvent)=>{
    console.log(e);
    console.log(e.target);
  }

  render=()=>{
    return (
      <div className="App">
    
          <Event name="" click={this.handClick}/>
      </div>
    );
}
}
// export default App;
  
```



## umi

[官网](https://umijs.org/zh/guide/) [项目](C:\Users\Admin\Desktop\umitest)

### 介绍：

 https://umijs.org/zh-CN/docs  

​        Umi，中文可发音为**乌米**，是可扩展的企业级前端应用框架。Umi 以路由为基础的，同时支持**配置式路由**和**约定式路由**，保证路由的功能完备，并以此进行功能扩展。然后配以生命周期完善的插件体系，覆盖从源码到构建产物的每个生命周期，支持各种功能扩展和业务需求。

​        Umi 是蚂蚁金服的底层前端框架，已直接或间接地服务了 3000+ 应用，包括 java、node、H5 无线、离线（Hybrid）应用、纯前端 assets 应用、CMS 应用等。他已经很好地服务了我们的内部用户，同时希望他也能服务好外部用户



### 安装umi

1）、安装：

```
yarn add global umi@2.12.9  （react 整理推荐用yarn   瑕疵node-sass）
或

npm i -g  umi@2.12.9
```

2）、验证

```
umi -v
```

>顺便补充：yarn和npm 全局安装的文件在哪儿呢？
>
>yarn全局位置： yarn global bin
>
>npm全局位置：npm config list 
>
>

### 手工（命令）搭建项目

#### 创建项目目录

```dos
mkdir demoprj01
```

#### 创建页面

```js
//命令的方式创建页面。

//创建组件index
umi g page index
//创建组件users
umi g page users
```

#### 启动项目

```
umi dev
```

> ​        用命令`umi dev`启动项目 后，大家会发现 项目目录 下多了个 `.umi` 的目录。这是啥？这是 umi 的临时目录，可以在这里做一些验证，但请不要直接在这里修改代码，umi 重启或者 pages 下的文件修改都会重新生成这个文件夹下的文件。 
>
> ​        这个文件夹里有自动生成的路由配置（.umi/core/routes.ts）



#### 约定式路由

​     umi 里约定默认情况下 pages 下所有的 js 文件即路由 。



https://github.com/umijs/umi/blob/umi%402.12.9/packages/umi/src/link.js



```js
1)、直接使用react-router-dom

import {Link} from 'react-router-dom';

<Link to="/user">go to /users</Link>

2）、使用umi的link模块
  
    在umi的脚手架里使用

```



### 脚手架创建项目



```dos
创建项目目录：mkdir project
转换项目目录：cd project
使用yarn产生umi的项目：yarn create umi
```

![1598432192342](img\03umi01.png)





![1598432232898](C:\Users\31759\AppData\Roaming\Typora\typora-user-images\1598432232898.png)

![1598432273098](img/03umi02.png)



```
安装所有的依赖：yarn install
运行项目：yarn start
```



### 项目结构

```jsx
|-public 本地数据资源
|-mock umi支持mock数据，无需代理
|-src
  |-assets 开发资源
  |-components 通用组件
  |-layouts 为根布局,根据不同的路由选择return不同的布局,可以嵌套一些和布局相关的组件
  |-pages 页面 约定式路由的页面
  |-global.css 全局样式
|-umirc 编译时配置
```



### 路由

#### 约定式路由

​	umi里使用的约定式（固定规则）路由。 规则如下：

```jsx
|-pages
  |-index.js   "/" 路由 页面
  |-index/index.js "/" 路由 页面
  
  |-goods.js  //  /goods  路由
  |-goods/index.js  // /goods  路由页
    //goods 路由的默认页 return null 代表没有默认页

  |-goods/$id.js  // /goods/1 路由页 
  |-goods/$id$.js  // /goods 路由 goods下没有index 时 展示了 /goods 或 /goods/2 代表id可选
  |-goods/_layout.js  // /goods 路由页 有自己的展示区 {props.children} 不然会在父的展示区展示

  |-goods/category.js   // /goods/category 路由
  |-goods/category/index.js // /goods/category 路由

  |-404.js 生产模式下的404页，开发模式默认umi的
```



> 注意：
>
> 一定要关闭默认路由配置，否则，目录自动生成路由（约定式路由）失效，
>
> 如何关闭：修改.umirc.js文件：注释routes键数据即可



> 当在地址栏输入： http://localhost:8000/ 时，
>
> 会找layouts/index.js组件（布局）。在layouts/index.js里的{props.children}里会显示pages/index.js



#### 路由跳转

##### 声明式

```jsx
<Link to={{pathname:'/goods/2',query:{a:11,b:22}}}>商品 002</Link>
```

##### 编程式

```jsx
import router from 'umi/router';
					
router.push('/login')

router.push({
  pathname:'/goods/1',
  query:{a:11,b:22},
  search:'a=111&b=222'
})
// search:'a=111&b=222' 如果传递了search 会覆盖query,query只是对search的封装引用了search的值

props.history.push() //可用
```

#### 传接参

```jsx
props.match.params
props.location.query 返回对象
```



### 案例需求

基础路由        -   练习：创建文章模块列表、添加

动态路由        -   练习：文章查看详情

嵌套路由        -   练习：面包屑（列表、添加、查看详情都要）

全局layout     -   练习：后台三栏布局（头部、左侧、右侧（内容））

404路由         -    练习：搞404（留心坑） 、不同的全局 layout



### 实现步骤

**1、创建文章模块列表、添加**(基础路由 )

1）、列表页面

umi g page arts/index 

运行：yarn start

![1598535244051](img/04umi01.png)

2）、添加页面

umi g page arts/create

运行：yarn start

![1598535294804](img/04umi02.png)

**2、文章详情(动态路由)**

umi g page **'arts/$id**' --less

在产生的$id.js文件里，写如下代码：

```
<h1>Page {props.match.params.id}</h1>  //获取路由上的id的值
```

运行：yarn start，

在地址栏输入 localhost:8000/arts/0001

![1598535608948](img/04umi03.png)



**3、面包屑 (嵌套路由)**

后台项目文章模块，列表、添加详情都需要面包屑 ，我们做一个公共的父组件 _layout

普及：公共的父组件不管是在后端、还是前端专业术语叫layout    

语法命令：umi g page **arts/_layout** --less

> 在arts下创建的文件 _layout.js，如果访问，http://localhost:8000/arts 开头的路径，都会默认找 arts文件夹下的 _layout 组件。所以，在该组件里配置子路由，子路由对应的页面显示在{props.children}里。

在产生的_layout.js文件里，修改代码：

```
export default function(props) {
  return (
    <div className={styles.normal}>
      <h1>Page _layout</h1>
      <div>{props.children}</div>
    </div>
  );
}
```

在地址栏输入： http://localhost:8000/arts/ 

那么默认，会找arts文件夹下_layout.js文件，在  <div>{props.children}</div> 显示index.js文件。

![1598536099069](img/04umi04.png)



如果在地址输入： http://localhost:8000/arts/01001

会找arts下_layout.js文件，在  <div>{props.children}</div> 显示$id.js文件。

![1598536191539](img/04umi05.png)





**4、后台管理系统常见的三栏布局（头部，下（左边导航，右边页面））**

1）、总体布局

```js
//src/index.js
import styles from './index.less';

function BasicLayout(props) {
  return (
    <div className={styles.normal}>
      <div className={styles.header}>头部</div>
      <div className={styles.main}>
        <div className={styles.left}>导航</div>
        <div className={styles.right}>
          {props.children}
        </div>
      </div>      
    </div>
  );
}

export default BasicLayout;
```



```less
src/layout/index.less

.normal {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;

  .header{
    width: 100%;
    height: 120px;
    background-color: skyblue;
  }

  .main{
    flex: 1;
    width: 100%;
    background-color: red;
    display: flex;

    .left{
      width: 200px;
      height: 100%;
      background-color: blue;
    }
    
    .right{
      flex: 1;
      height: 100%;
      background-color: #ccc;
    } 
  }
}
```

![1598537599838](img/04umi06.png)



**5、404 页面**

约定 `pages/404.js` 为 404 页面，需返回 React 组件。

创建404页面

```
umi g page 404 --less // 这个不行，404不能产生，因为，在命令行里要求文件名不能以数字开头。
```

比如：

```js
export default () => {
  return (
    <div>I am a customized 404 page</div>
  );
};
```

> 注意：开发模式下，umi 会添加一个默认的 404 页面来辅助开发，但你仍然可通过精确地访问 `/404` 来验证 404 页面。上线后就不会这样了。



另外，需要增加判断，才能让整个页面都显示404页面。

修改 src/layout/index.js文件的内容

```js
function BasicLayout(props) {
  if(props.location.pathname==="/404"){
    return props.children
  }
  return (
    <div className={styles.normal}>
      <div className={styles.header}>头部</div>
      <div className={styles.main}>
        <div className={styles.left}>导航</div>
        <div className={styles.right}>
          {props.children}
        </div>
      </div>      
    </div>
  );
}
```



## dva

官网：

 https://dvajs.com/guide/ 





## 扩展

### 契合antd的富文本编辑器braft-editor 



## vue和react的区别



|            |      | reactJs              | vueJs        |
| ---------- | ---- | -------------------- | ------------ |
| 控制器     |      | -                    | -            |
| 过滤器     |      | -                    | √            |
| 指令       |      | -                    | √            |
| 模板语法   |      | -                    | √            |
| 服务       |      | -                    | -            |
| 组件       |      | √                    | √            |
| jsx        |      | √                    | 2.0之后加入  |
| 响应式原理 |      | 必须调用setState函数 | 真正的响应式 |

