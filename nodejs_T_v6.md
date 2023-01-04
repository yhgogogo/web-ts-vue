# nodeJS

**大纲：**

1、搭建服务器

2、mongodb 注册和登录增删改查新闻(bootstrap)

3、api server 注册和登录增删改查新闻(bootstrap)

4、bcrypt加密 注册和登录

5、session 登录和其它需要验证身份的模块（如：添加，删除）

6、token 登录，其它需要验证身份的模块（如：添加，删除）

7、上传图片：添加新闻

8、socket ：做聊天室



## 服务器

### 什么是服务器

服务器就是提供服务的，有web服务器，数据库服务器等等…………



### web服务端保存的资源：

**静态资源**

​      `      xx.css`	`xx.html`	`xx.js`	`xx.图片`	`xx.json`	`xx.字体`	...  等等在前端使用的文件。这些文件的内容每次打开时，都是一样的，所以叫静态资源。

**动态资源（也有接口）**

​      后端的程序，如：二阶段学习的php文件



###  web服务器（软件）的作用：

​      1、接收前端请求
​      2、查找文件
​      3、执行服务器端代码
​      4、响应

![1608469841837](img/01B&S.png)

**node完成了二阶段中apache和php的功能**

![1608469861652](img/02B&s.png)



## nodeJS

### 介绍

​        后端的编程语言，与之类似有 php  c#   java ，python，go。不但是后端的编程语言，还可以搭建服务器，如：apche等。

​       nodeJS就是ECMAScirpt。在原来学习的ES的基础上增加了后端相关的API，如：HTTP，fs，URL等等。让javascript还可以做后端的开发，即一门语言搞定前后端，即**全栈**。



### 目标

数据服务：连接数据库

文件服务：文件上传等等。

web服务：web服务器（如：apache）

### 优势

性能高，方便、对于前端人员来说入门难度低、大公司都在用（BAT）。

坊间传说：一台node的服务器顶得上86台php服务器。特别是在处理**并发**的场景，尤为明显。

###	劣势

- 服务器提供的相对较少
- 相对其他语言，能用的上的学习资料少
- 对程序员的要求高了

### 特点：

  单线程：nodeJS是单线程的，那么如何面对并发，靠的是事件循环（事件队列）。
              https://blog.csdn.net/jiang7701037/article/details/95887439       
  非阻塞  :
        NodeJS在访问高IO（input/output）操作后不会等待其完成，而是立即去执行其他代码，操作完成后会使用回调函数返回，保证高效的利用当前线程的cpu 不造成硬件浪费。跟异步差不多。

事件驱动：
       通过事件来驱动整个程序的进行, 由于是单线程，所以把高IO的操作 就会移动到事件队列中等待完成，完成后通过回调函数的方式返回给线程来进行处理。这个循环处理的过程称之为：事件循环



### 环境安装

官网：[英文](https://nodejs.org/en/) [中文](http://nodejs.cn/) [镜像](http://npm.taobao.org/)

测试环境： `win+r->命令行(运行->cmd)->node -v`

###	版本

Vx(主).x(子).x（修正）

> 主版本:   变化了，1/3的API发生巨变 , 使用方式变化了
>
> 子版本： API没有删减，使用方式没变化，内部实现发生了变化
>
> 修正版： 什么都没变，处理一下bug
>
> V6.8.0  稳定
>
> V6.9.1 非稳定版
>
> alpha：内部测试版本
>
> beta ： 公测版本
>
> rc  测试稳定































###	运行

#####	window

```javascript
a. 找到目标目录-》地址栏输入cmd-》node 文件名.js | node 文件名

b. 当前目录->右键->git bash-> node 文件名
```

#####	苹果

```
终端->cd 目录-> node 文件名.js | node 文件名
```

#####	vscode

```
新建终端->cd 目录->node 文件名.js | node 文件名 
调试->运行
```

#####	webstrom

```
terminal| run
```





### **注意**

`nodejs` 使用的是`ECMA`语法，不可使用`DOM`，`BOM`  因为后端开发没有浏览器。



即：前端js代码在浏览器上运行；后端js代码在node环境里运行的。















































## web服务器的开发

(回顾二阶段)

### 1、搭建服务器 （http模块）

​        搭建服务器要使用http模块，http模块，可以创建服务器对象，同时，可以处理请求和响应

1）、引入http模块	

```
let http = require('http')
```

2）、创建web服务   返回http对象

```js
let app = http.createServer((req,res)=>{
	req 请求体（请求对象）  浏览器->服务器，服务器端把浏览器发送来的信息整理到req对象里。
	
	req.url  地址   提取地址栏数据
	req.on('data') 提取非地址栏数据 所有的http[s]都会触发end事件
	req.on('end') 
	
	res 响应体（响应对象）  服务器->浏览器，服务器端给浏览端发送的信息都整理在res里。
	
	res.writeHead(200,{'Content-Type':'text/html;charset=utf-8'});响应头设置
	res.write(字符/数据<string><buffer>) 返回数据
	res.end() 结束响应 必须
	
})
```

3）、监听服务器

```
app.listen(端口，[地址]，[回调])
```

> 端口: 1-65535	1024以下系统占用 
>
> 地址：localhost  真实域名xx.duapp.com，ip地址也行。
>
> 回调：监听成功，回调一次



4）、启动服务器的命令：

   node 文件名



**搭建服务器的示例代码：**

```js
// 1、引入http模块（nodeJS官方的模块，是个对象）
const http = require("http");

// 2、创建服务器对象
let server = http.createServer((req,res)=>{
    // req:请求对象（包含所有前端请求的信息，如：url，method，）
    console.log("req.url",req.url);
    console.log("req.method",req.method);
    // res：响应对象（包括响应所使用的函数等等，如：res.write() ）
    res.writeHeader(200,{"Content-type":"text/html;charset=utf-8"})
    // res.setHeader("Content-type","text/html;charset=utf-8")
    // res.write():给前端响应数据的
    res.write("<h1>欢迎光临1！</h1>");
    res.write("<h1>欢迎光临2！</h1>");
    res.write("<h1>欢迎光临3！</h1>");
    res.write("<h1>欢迎光临4！</h1>");
    res.end();
});

// 3、启动服务器
server.listen(9000,"10.35.165.56",()=>{
    console.log("启动成功，请访问……");
})
```



热部署和启动命令

> 由于，每次更新代码后, 需要重新启动服务器，很麻烦
>
> 推荐命令行工具：`supervisor`  或者`nodemon`	
>
> 安装方式: `npm install supervisor -g`	或者   `npm install nodemon -g`
>
> 启动服务器  nodemon 文件名   





###	2、静态资源托管（fs模块）

fs:fileSystem; 这个模块可以读取文件的内容，给文件写内容，创建文件夹，删除文件夹等等有关文件和文件夹的一切操作。

**一般来说，前端的如下代码都会请求一个静态资源**

```html
<a href=".."></a>
<img src="..."/>
```

```javascript
location.href="..."
```

```css
body{
    background:url(....)
}
```

**后端资源读取静态资源文件就要使用fs模块**

```
fs.readFile(文件名,[编码方式],回调(err,data));
```



####	2.1 fs模块

磁盘操作，文件操作

**读取**

```
异步的方式：fs.readFile('文件路径',[编码方式],(err,data)=>{})
```

[^err ]: err 错误 ,null没有错误
[^data]: 数据, 默认是buffer流；如果希望是unicode编码的结果，那么可以把编码方式使用  utf-8 

```
同步的方式： let data = fs.readFileSync('文件路径') 
```

> 处理错误
>
> try{
>
> ​	要排错的代码，如果try分支里有错误，那么就会进入到catch分支。
>
> }catch(e){     
>
> }



#### 2.2 path模块

操作系统磁盘路径

windows: 	`c:\user\admin `               磁盘的路径的分割是： \    网络上的路径分割是  /
		mac: 	`~/desktop/1901`

**API**

磁盘路径解析 **parse**

```js
path.parse('c:\\wamp\\xx.png') // string -> object

//返回
{
   root: 'c:\\', 盘符
   dir: 'c:\\wamp', 目录
   base: 'xx.png',  文件名
   ext: '.png', 扩展名
   name: 'xx' 	文件，不含扩展名
}
```

片段合并**join**

```js
path.join('磁盘路径1','磁盘路径2'，'磁盘路径n')
```

> __dirname ：nodeJS的官方全局变量， 返回当前文件所在的磁盘路径。



片段合并 **resolve**

```js
path.resolve('磁盘路径1','磁盘路径n')
```

> 合并磁盘片段,右到左找，如果找到了根，那就不再朝前找了；拼接时，是从左到右拼接，如果没有给根路径，以当前文件路径为根（即默认：加上 __dirname）。





**静态资源的示例代码：**

```js
//一、在目录下建立  www 目录，写几个html文件

//二、服务器代码
// 1、引入http模块（nodeJS官方的模块，是个对象）
const http = require("http");
const fs = require("fs");

// 2、创建服务器对象
let server = http.createServer((req, res) => {
    if (req.url != "/favicon.ico") {
        try {
            let filename = "./www" + req.url;
            let data = fs.readFileSync(filename, "utf8");
            res.write(data);
            res.end();
        } catch (error) {
            res.write("服务器出错了");
            res.end();
        } 
    }
});

// 3、启动服务器
server.listen(9000, "10.35.165.56", () => {
    console.log("启动成功，请访问……");
})
```































###	3、动态资源（接口实现）

**前端**



表单：get/post/put/delete/...

js： ajax/jsonp 





**后端（如同二阶段的php）**



处理方式：`http[s]`

​	地址栏上的数据：	`req.url`  抓取 get请求的数据  切字符 	| url模块

​	非地址栏的数据 :    `req.on('data',(chunk)=>{CHUNK==每次收到的数据buffer})`

​					             	`req.on('end',()=>{	接收完毕 切字符 querystring })`

> postman 一个不用写前端，就可以发出各种请求的软件 [下载](https://www.getpostman.com/downloads/)



#### 3.1 url模块

**作用**

处理 url格式的字符串

**用法**

```
url.parse(str,true)  返回 对象	true处理query为对象
```

> str -> obj  返回 对象  true 
> 		protocol: 'http:',	协议
> 		slashes: true,	双斜杠
> 		auth: null,   作者
> 		host: 'localhost:8002',  主机
> 		port: '8002',	端口
> 		hostname: 'localhost',  baidu
> 		hash: '#title',	哈希（锚)
> 		search: '?username=sdfsdf&content=234234',	查询字符串
> 		query: 'username=sdfsdf&content=234234',	数据
> 		pathname: '/aaa',	文件路径
> 		path: '/aaa?username=sdfsdf&content=234234',	文件路径
> 		href: 'http://localhost:8002/aaa?username=sdfsdf&content=234234#title'

```
url.format(obj) 返回字符
```

> obj -> str   返回str 







































####	3.2 querystring 模块

**作用**

处理查询字符串 如：`?key=value&key2=value2`

**用法**

```
querystring.parse(str) 返回对象
```

```
querystring.stringify(obj) 返回字符串
```



#### 3.3 获取非地址栏的数据：

如：前端使用post发来的数据



使用：

```js
//用req.on绑定事件data：接收一次数据（一般来说：65536字节），触发一次data
req.on('data',(chunk)=>{
   
})

//用req.on绑定事件end:接收完毕。
req.on('end',(err)=>{

})
```



**动态资源的示例代码：**

```js
const http = require("http");
const url = require("url");

let server = http.createServer((req, res) => {

    // 1、根据书籍编号获取书籍详情
   
    // url: bookdetail
    // method:get
    // params:bookid  (书籍的编号)
    // 返回数据格式：
    // {
    //     status:"success",
    //     data:{
    //         bookid:"01001",
    //         bookname:"论语",
    //         bookauthor:"敖东",
    //     }
    // }

    // console.log("req.url",req.url);
    let urlObj = url.parse(req.url, true);

    if (urlObj.pathname === "/bookdetail") {
        // 1、接收前端的数据
        let bookid = urlObj.query.bookid;
        // 2、查询数据库

        // 3、响应
        res.setHeader("Conent-type","text/plain");
        if (bookid == "01001") {
            res.write(`{
                status: "success",
                data: {
                    bookid: "01001",
                    bookname: "论语",
                    bookauthor: "敖东",
                }
            }`)
        }else if (bookid == "01002") {
            res.write(`{
                status: "success",
                data: {
                    bookid: "01002",
                    bookname: "中庸",
                    bookauthor: "王冰芊"
                }
            }`)
        }else{
            res.write(`{
                status: "fail"
            }`) 
        }

        res.end();
    }


    // 2、注册
    // url: regSave
    // method:post
    // params:
    //   username：用户名
    //   userpass：密码
    // 返回数据格式：
    // {
    //     status:"success",//fail
    // }

    if (urlObj.pathname === "/regSave") {
        // 1、接收前端发来的数据
        //用req.on绑定事件data：接收一次数据（一般来说：65536字节），触发一次data
        let str = "";
        req.on("data",(chunk)=>{
            console.log("chunk",chunk);
            // chunk: 本次接收到一块的数据
            str += chunk;
        });

        //用req.on绑定事件end:接收完毕。
        req.on("end",(err)=>{
            if(!err){
                console.log("接到数据是：",str);
                // 2、处理（连接数据库）
                
                // 3、响应 
                let obj = JSON.parse(str);
                if(obj.username=="敖东" && obj.userpass=="123"){
                    // res.write(`{
                    //        status:"success",
                    // }`);
                    // res.end();

                    res.end(`{
                               status:"success",
                        }`);

                }else{
                    res.end(`{
                            status:"fail",
                    }`);
                }
            }
        });

    
    }

});


server.listen(9000, "localhost", () => {
    console.log("启动成功，请访问……");
})
```



## 模块化规范	commonJS

### 1、模块化的作用

  1）、防止全局变量和全局函数重名，不污染全局变量。

  2）、隐藏了细节

  3）、js文件引用js文件。如果说前端还可以用html引入js文件的话，那么后端就不可能了，因为后端代码里没有html。

### 2、常见的模块化

前端模块化规范：CMD，AMD，ES6

后端的模块化规范：commonJS，ES6



### 3、规范是什么

如：

​     AMD是个规范，requireJS是AMD规范的体现

​     CMD是个规范，seaJS是CMD规范的体现

​     commonJS是个规范，node和webpack是commonJS规范的体现

​     ECMA是个规范，JS/AS是ECMA实现了它     



### 4、commonJS的模块：

**系统模块**

`http`	`fs`	`querystring`	`url`	

**第三方模块：**

​     gulp 

**自定义模块：**





### 5、commonJS模块化格式：

#### 1）、导入（引入）

```javascript
let 变量名 = require('模块名')    // 得到的是个对象  ES6中import
let 变量名 = require('模块名').xx  按需引用  //得到的是对象的某个属性
```

> 
>
> 不指定路径：先找系统模块-> 再从项目环境找node_modules|bower_components (依赖模块)->not found
>
> 指定路径 : 找指定路径 -> not found
>
> 支持任何类型
>
> 所以：系统模块和第三方模块不需要写路径。而我们 的自定义模块需要写路径。

#### 2）、导出（输出，对外开放）

```js
exports.自定义属性 = 值( 任意类型)   //ES6 的 export
```

> 可输出多次
>
> 
>
> 如：exports.name = "张三疯";
>
> ​        exports.sex  = "男";
>
> ​        exports.person= {
>
> ​          }        
>

```js
module.exports = 值( 任意类型)   //ES6 的 export default

```

> 只能输出一次，输出的为任意类型的数据，或者类。
>
> 如：
>
> module.exports = {
>
> }
>
> 
>
> module.exports = class Person{
>
> ​	   constructor(){
>
> ​       }
>
> }



**模块化规范的代码：**

把上面的代码进行提取

```js
//一、routes/bookdetail.js
// 1、根据书籍编号获取书籍详情
    // url: bookdetail
    // method:get
    // params:bookid  (书籍的编号)
    // 返回数据格式：
    // {
    //     status:"success",
    //     data:{
    //         bookid:"01001",
    //         bookname:"论语",
    //         bookauthor:"敖东",
    //     }
    // }
const url = require("url");

module.exports = function(req,res){
    
       let urlObj = url.parse(req.url, true);
        // 1、接收前端的数据
        let bookid = urlObj.query.bookid;
        // 2、查询数据库
        // 3、响应
        res.setHeader("Conent-type","text/plain");
        if (bookid == "01001") {
            res.write(`{
                status: "success",
                data: {
                    bookid: "01001",
                    bookname: "论语",
                    bookauthor: "敖东",
                }
            }`)
        }else if (bookid == "01002") {
            res.write(`{
                status: "success",
                data: {
                    bookid: "01002",
                    bookname: "中庸",
                    bookauthor: "王冰芊"
                }
            }`)
        }else{
            res.write(`{
                status: "fail"
            }`) 
        }
        res.end(); 
}

//二、routes/regSave.js

// 2、注册
// url: regSave
// method:post
// params:
//   username：用户名
//   userpass：密码
// 返回数据格式：
// {
//     status:"success",//fail
// }
module.exports = function (req, res) {
    // 1、接收前端发来的数据
    //用req.on绑定事件data：接收一次数据（一般来说：65536字节），触发一次data
    let str = "";
    req.on("data", (chunk) => {
        console.log("chunk", chunk);
        // chunk: 本次接收到一块的数据
        str += chunk;
    });

    //用req.on绑定事件end:接收完毕。
    req.on("end", (err) => {
        if (!err) {
            console.log("接到数据是：", str);
            // 2、处理（连接数据库）

            // 3、响应 
            let obj = JSON.parse(str);
            if (obj.username == "敖东" && obj.userpass == "123") {
                // res.write(`{
                //        status:"success",
                // }`);
                // res.end();

                res.end(`{
                               status:"success",
                        }`);

            } else {
                res.end(`{
                            status:"fail",
                    }`);
            }
        }
    });

}

//三、 server.js（服务器diam）
const http = require("http");
const url = require("url");
const bookdetail = require("./routes/bookdetail");
const regSave = require("./routes/regSave");

let routes ={
    "/bookdetail":bookdetail,
    "/regSave":regSave
}

let server = http.createServer((req, res) => {
    let urlObj = url.parse(req.url);
    
    routes[urlObj.pathname](req,res)
    
    // if (urlObj.pathname === "/bookdetail") {
    //     bookdetail(req,res);
    // }else  if (urlObj.pathname === "/regSave") {
    //     regSave(req,res);
    // }

});

server.listen(9000, "localhost", () => {
    console.log("启动成功，请访问……");
})
```





## 第三方模块化的管理工具

### 1、NPM

#### 作用

帮助你安装模块（包），并且自动安装所有的依赖，管理包（增，删，更新，项目所有包)。

npm服务器上放置了50多万的包，进行统一管理。



> 类似：	[bower](http://bower.io)		[yarn](https://yarn.bootcss.com/)

#### 安装到全局（操作系统）环境

- 安装到电脑系统环境下（相当于给操作系统安装了一个软件，只不过，此时用的是npm安装而已）
- 使用时在任何位置都可以使用（因为，默认配置了环境变量）
- 被全局安装的通常是：命令行工具，脚手架

```
npm install 包名 -g				安装
npm uninstall 包名 -g	 			卸载
```

[^g]: global



#### 安装到项目环境（本地安装）

只能在当前目录使用，需要使用npm 代 运行

##### 初始化项目环境

```   
npm init   //会自动产生package.json文件
```



**package.json文件示例**

```json
{
  "name": "npm",	//项目名称
  "version": "0.0.1",	//版本
  "description": "test and play",	//描述
  "main": "index.js", //入口文件
  "dependencies": {  //项目依赖(生产依赖)  上线也要用
    "jquery": "^3.2.1"
  },
  "devDependencies": { //开发依赖 上线就不用
    "gulp": "^3.5.2"
  },
  "scripts": {	//命令行
    "test": "命令行"
  },
  "repository": {	//仓库信息
    "type": "git",
    "url": "git+https://github.com/tianwater.github.io/2017-8-28.git"
  },
  "keywords": [  //关键词，github上搜索时，可以使用该关键字
    "test",'xx','oo'
  ],
  "author": "tianwater",
  "license": "ISC",	//认证
  "bugs": {
    "url": "https://github.com/alexwa9.github.io/2017-8-28/issues"//问题提交
  },
  "homepage": "https://github.com/alexwa9.github.io/2017-8-28#readme"//首页
}
```



##### 项目依赖（生产依赖）

不但在当前项目下需要使用，上线了，也需要这个依赖  `--save`

```
//安装
npm install 包名 --save      //安装最新版本的包
npm install 包名 -S    // --save可以简写为 -S   
npm install 包名@x.x.x -S    //安装指定版本的包

//卸载
npm uninstall 包名 --save
npm uninstall 包名 -S
```



安装了项目依赖后，会产生一个package-lock.json，这个文件千万不要删除

```
{
  "name": "nodetest",
  "version": "1.0.0",
  "lockfileVersion": 1,
  "requires": true,
  "dependencies": {
    "jquery": { //包名
      "version": "3.5.0",  //版本号
      "resolved": "https://registry.npm.taobao.org/jquery/download/jquery-3.5.0.tgz?cache=0&sync_timestamp=1586533502771&other_urls=https%3A%2F%2Fregistry.npm.taobao.org%2Fjquery%2Fdownload%2Fjquery-3.5.0.tgz",   //安装来源
      "integrity": "sha1-mYC5fZ5BlGEcNlMOfcRqWNc0D8k=" //迭代的id
    }
  }
}
```



**package-lock.json文件的作用（这个可以以后再理解）：**



 Node.js v8.0 后，npm 也升级到了5.0，其中package-lock.json文件就是npm5开始有的。

1. package.json文件下载到的依赖包可能在不同的情况下，各库包的版本语义可能并不相同，有的库包开发者并不严格遵守这一原则：相同大版本号的同一个库包，其接口符合兼容要求。所以说，在不同时间或者不同npm下载源之下，下载的各依赖包版本可能有所不同，因此其依赖库包行为特征也不同，有时候甚至完全不兼容。
2. package-lock.json文件所标识的具体版本下载依赖库包（包的安装来源），就能确保到一个新的机器上所有库包与上次的安装完全一样。
3. npm的下载源改为私服地址，这样产生的package-lock.json文件的版本号是这个私服上设置好的版本号



​      场景：

​       如果在git上下载了别人的代码，或者你的代码拷贝到其它机子上，一般不会拷贝node_modules文件夹。在新的机子上执行命令： npm i 。那么npm会根据package.json里的配置安装所有的依赖，具体的包来自哪个来源，在package-lock.json里。





##### 开发依赖

只能在当前项目下使用	上线了，依赖不需要了 `--save-dev`

```
npm install 包名 --save-dev
npm install 包名 -D
```





**npm代运行**

即：使用npm执行 package.json文件里的scripts属性里的内容（叫作 npm脚本）



如：

package.json里这样写：

```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node ./app.js",
    "n":"node"  //全局
    "j":"jQuery" //项目依赖
  },
```

命令行执行  npm run start  就相当于执行  node ./app.js  

另外，start属性可以不用加run，其它属性需要加run。即：npm start 就相当于npm run start

命令行执行  npm run n  就相当于执行  node 

命令行执行  npm run j  就相当于执行  jQuery   



















#### 查看包

```
npm list  列出项目里所有已装包
npm outdated 版本对比(安装过得包)，查看项目中安装的包和npm服务器上的包有没有版本上的不同
npm info 包名 查看当前包概要信息 
npm view 包名 versions 查看包历史版本，如果想知道jQuery都有哪些版本：npm view jquery versions
```

![1588393896079](img/03package01.png)

![1588394040179](img/03package02.png)



current：当前项目中安装的版本号；

wanted：在你安装的当前主版本号里，推荐你使用的子版本号

latest：最新版本号



#### 安装所有依赖

```	js
npm install 
```

> 安装package.json里面指定的所有包

#### 版本约束

```
^x.x.x   约束主版本，重新安装时，后面两位找最新
~x.x.x   保持前两位不变，重新安装时，最后一位找最新
*		 重新安装时，永远装最新的版本
x.x.x 	 定死了一个版本
```















#### 选择源（选择最快的源进行安装）

```
npm install nrm -g     安装选择源的工具包
nrm ls 查看所有源
nrm test 测试所有源，可以看到当前哪个源的速度快
nrm use 切换源名 ，切换到最快的源
```

#### 安装卡顿时

```
ctrl+c -> npm uninstall 包名 -> npm cache clean 清除npm的缓存 -> 换移动数据（开热点）-> npm install 包名
```























#### 发布包

- 官网 [注册]()	

- 登录（用命令行）
  - 在项目目录下输入：npm login    （注意：一定把源切换到npm，不要用其它的镜像）
  - 输入 user/password/email
  
- 创建包（写项目）
  - `npm init -y`
  - 创建入口index.js
  - 编写，输出
  
- 发布
  
  - `npm publish`
  
    注意：发布前，首先需要确定 包名（package.json里的name属性）是否在npm的服务器上存在。进入 npm 官网，搜索一下包名：![1588408182571](C:\Users\31759\AppData\Roaming\Typora\typora-user-images\1588408182571.png)
  
  ​      把目前切换到当前项目下，然后，输入npm publish。
  
- 迭代
  
  - 修改版本号
  - `npm publish`
  
- 删除
  
  - `npm unpublish`

> 包的发布、迭代、删除，需要在包目录下进行
>
> 删除包，有时需要发送邮件











































### 2、YARN

[官网](https://classic.yarnpkg.com/)

#### 安装

[去官网安装](https://classic.yarnpkg.com/zh-Hans/)

> 注意：为省事，不要用npm i yarn -g，去安装yarn，而是去下载压缩包，保证注册表和环境变量的硬写入，后期通过yarn安装全局包时方便（否则，可能会装不上）

#### 使用

**初始化一个新项目**

```
yarn init
```

**添加依赖包**

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

**将依赖项添加到不同依赖项类别中**

分别添加到 ``dependencies``,`devDependencies`、`peerDependencies` 和 `optionalDependencies` 类别中：

```
yarn add [package] --save   | -S 
yarn add [package] --dev    | -D 
yarn add [package] --peer
yarn add [package] --optional
```

**升级依赖包**

```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

**移除依赖包**

```
yarn remove [package]
```

**安装项目的全部依赖**

```
yarn
```

或者

```
yarn install
```

**安装到全局**

```js
yarn global add [package]				//global的位置测试不能变，global不能写在最后
yarn global remove [package]
```















### 3、BOWER

[官网](https://bower.io/)

#### 安装bower

```bash
npm install -g bower
```

#### 安装包到全局环境

```
bower i 包名 -g		安装
bower uninstall 包名 -g	 卸载
```

#### 安装包到项目环境

##### 初始化项目环境

```
bower init
```

> bower.json  第三方包管理配置文件

##### 项目依赖

只能在当前项目下使用，上线了，也需要这个依赖  `--save`

```
//安装
同npm
bower install 包名#x.x.x -S 指定版本使用#

//卸载
同npm
```

##### 开发依赖

只能在当前项目下使用	，上线了，依赖不需要了 `--save-dev`

```
同npm
```

































## EXPRESS框架

nodejs库，不用基础做起，工作简单化，点击进入[官网](http://www.expressjs.com.cn/)，类似的还有 [koa](https://koa.bootcss.com/)

### 特点

二次封装，非侵入式，增强形

二次封装：保留了原来的功能，做了进一步的增加。

### 安装

1、全局安装

 

```
npm  install express  -g

npm  install express-generator  -g（mac系统中一般没有问题，windows系统下建议安装）

express  -h  测试是否安装成功
```



2、搭建项目，局部安装

```
1）、新建项目目录
2）、初始化项目：npm init -y
3）、本地安装express：npm i express --save
```



### 搭建服务器

```js
let express=require('express')
let app=express();  //相当于调用  http.createServer()
app.listen(端口,地址,回调)
```

### 静态资源托管

```js
app.use(express.static('./www'));
```































### 动态资源（接口实现）

#### 不同请求方式对应的函数

支持各种请求方式：get、post、put、delete...

```js
app.请求方式API(接口名称,处理函数)
app.get(url,(req,res,next)=>{
    
})
app.post(url,(req,res,next)=>{})
...
```

#### req	请求体

req 对象表示 HTTP 请求，把前端发送请求的一切信息封装到了该对象里。包含了请求查询字符串，参数，内容，HTTP 头部等属性。

```js
req.query //获取地址栏的数据
req.body //获取非地址栏的数据  依赖中间件（body-parser） 

req.params //获取动态接口名   如： /api/goods/:id 的请求，那么，用req.params.id 获取
req.method //获取前端提交方式
```

> req.body依赖中间件
>
> 中间件使用:body-parser  
>
> 1. npm install body-parser 
> 2.  let bodyParser = require('body-parser') 
> 3.  app.use(bodyParser ())

#### res	响应体

response 对象表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据

```js
res.send(any)  //send可以返回任何类型，包括对象 //对等 res.write + end
res.end(string|buffer) //原来的方法还在，这就是非侵入式的意思。
res.json(json) //只返回json

res.status(404).send({error:1,msg:"Sorry can't find that!"}) //返回一个404，并响应内容，这是链式写法

res.sendFile(path.resolve('public/error.html')) //发送文件，如：mp3，mp4等等，html也可以。

res.redirect(url)   后端跳转  指向一个接口
```





























#### 处理 多个接口的 公共业务

如：token的处理（前端发来token字符串，后端先验证token是否正确，如果正确，才进行接口的处理）

```js
app.all('/admin/*',(req,res,next)=>{}))  表示任何以admin开始的请求（任何类型）都会经过回调函数
```

> all 处理所有类型的 HTTP 请求（如：get，post，put，delete等等）
>
> 需要next 延续后续



```js

// 处理以admin开头请求的公共业务
app.all("/admin/*",(req,res,next)=>{
   req.name1="admin";
   res.common = "hi";
   next();
});

// 处理 /admin/a
app.get("/admin/a",(req,res,next)=>{
    console.log("req.name1",req.name1);
    console.log( "res.common",res.common);
    console.log("admin/a");
    res.json({
        status:"success",
        msg:"a"
    });
});

// 处理 /admin/b
app.post("/admin/b",(req,res,next)=>{
    console.log("req.name1",req.name1);
    console.log( "res.common",res.common);
    console.log("admin/b");
    res.json({
        status:"success",
        msg:"b"
    });
});

```

































### use函数

安装中间件、路由。接受一个**函数**， 中间件其实就是个函数

```
app.use([地址],中间件|路由|函数体)
```



```js
app.use('/api/*',(req,res,next)=>{ })  //等价于 app.all('/api/ *  ,(req,res,next)=>{});
```



```js
app.use('/',(req,res,next)=>{ })  //等价于  app.use((req,res,next)=>{ })  )
```



即：如果不写地址，那么表示  '/' ，即针对任何请求，都会执行该中间件



>app.use，app.all，app.get|app.post的关系
>
>app.use :处理中间件（关注点：在完成功能上，如body-parser）
>
>​    app.all：处理请求
>
>​         app.get ：处理某种类型请求
>
>​         app.post
>
>​         app.put
>
>​         ………………



### 中间件

middleware， 处理自定义业务，只处理从 请求 到 结束响应 的**中间部分**。

**举例**

```js
npm i body-parser -S //安装包

let bodyParser=require('body-parser')//引入中间件

app.use(bodyParser())//安装中间件（插入中间件）
```

> body-parser 使用方式，实时查询 [npm](http://npmjs.com)，可获得最新





































### 扩展

#### req

- req.app：当callback为外部文件时，用req.app访问express的实例
- req.baseUrl：获取路由当前安装的URL路径
- req.cookies：Cookies
- req.fresh / req.stale：判断请求是否还「新鲜」
- req.hostname / req.ip：获取主机名和IP地址
- req.originalUrl：获取原始请求URL
- req.path：获取请求路径
- req.protocol：获取协议类型
- req.route：获取当前匹配的路由
- req.subdomains：获取子域名
- req.accepts()：检查可接受的请求的文档类型
- req.acceptsCharsets / req.acceptsEncodings / req.acceptsLanguages：返回指定字符集的第一个可接受字符编码
- req.get()：获取指定的HTTP请求头
- req.is()：判断请求头Content-Type的MIME类型

#### res

- res.app：同req.app一样
- res.append()：追加指定HTTP头
- res.set()在res.append()后将重置之前设置的头
- res.cookie(name，value [，option])：设置Cookie
- opition: domain / expires / httpOnly / maxAge / path / secure / signed
- res.clearCookie()：清除Cookie
- res.download()：传送指定路径的文件
- res.get()：返回指定的HTTP头
- res.location()：只设置响应的Location HTTP头，不设置状态码或者close response
- res.render(view,[locals],callback)：渲染一个view，同时向callback传递渲染后的字符串，如果在渲染过程中有错误发生next(err)将会被自动调用。callback将会被传入一个可能发生的错误以及渲染后的页面，这样就不会自动输出了。
- res.sendFile(path [，options] [，fn])：传送指定路径的文件 -会自动根据文件extension设定Content-Type
- res.set()：设置HTTP头，传入object可以一次设置多个头
- res.status()：设置HTTP状态码
- res.type()：设置Content-Type的MIME类型

**作业**

把vue的项目（打包后的）利用express搭建的node服务器来管理，保证资源托管到位，部分接口实现。























## express脚手架

### 1、用express脚手架创建项目

```
 express  -e  项目名（如：express  -e   expressshop）
```

###  2、安装express的依赖

```
 npm i  
```

> 这是pakeage.json的依赖代码：
> "dependencies": {
>     "body-parser": "~1.18.2",  //专门解析前端提交的数据
>     "cookie-parser": "~1.4.3", //专门解析cookie的
>     "debug": "~2.6.9", //调试程序的，显示调试信息，代替console.log
>     "ejs": "~2.5.7", //nodeJS express的模板，可以用此模板渲染前端页面
>     "express": "~4.15.5",
>     "morgan": "~1.9.0", // 日志
>     "serve-favicon": "~2.4.5" //用来设置根目录下favicon.ico图标。
>   } 



### 3、 启动express的项目：

```
npm  start
```

### 4、访问项目

```
http://localhost:3000。
```

### 5、脚手架项目解析

/bin/www文件：启动服务器

/app.js文件： 所有的请求，都会经过该文件。 处理中间件、设置静态资源， 动态资源（ 路径和routes文件夹下的文件的对应关系）

routes文件夹 :  逻辑

 

## 后端渲染

​       通常，前端根据后端返回的json数据，然后来生成html被称为**前端渲染**，而后端渲染是后端把json与html结合渲染好后返回到浏览器，没前端什么事了。

### 模板引擎

无论前后谁来渲染页面，都会用到模板引擎，前端渲染页面实际上是**操作dom**，后端渲染页面是**把数据和html字符拼接**后丢给浏览器

| 引擎                      | 前端 | 后端 |
| ------------------------- | ---- | ---- |
| angularJs                 | √    | ×    |
| vue/mustach               | √    | √    |
| react                     | √    | √    |
| angularTs/mustach         | √    | √    |
| jade（现在重命名为了pug） | ×    | √    |
| **ejs**                   | ×    | √    |
| jquery + art-template     | √    | ×    |
| handlerbars               | √    | ×    |















### ejs



#### ejs介绍：

​    "E" 代表 "effective"，即【高效】。EJS 是一套简单的模板语言，帮你利用普通的 JavaScript 代码生成 HTML 页面。EJS 没有如何组织内容的教条；也没有再造一套迭代和控制流语法；有的只是普通的 JavaScript 代码而已。

#### ejs特点：

纯 JavaScript：
       js代码可以直接使用
语法简单：
    EJS 支持直接在标签内书写简单、直白的 JavaScript 代码。只需让 JavaScript 输出你所需要的 HTML ，完成工作很轻松

易于调试
     调试 EJS 错误（error）很容易：所有错误都是普通的 JavaScript 异常，并且还能输出异常发生的位置。



#### **原理**：

 fs  抓取前端静态页面 + ejs + 数据  	->	返回send(data)	 -> 	浏览器



#### **使用**

**1）、后端渲染结果**

```js
let ejs = require('ejs')
ejs.renderFile('ejs模板文件',renderData,回调(err,data))

```

> ejs模板 ：	后缀名为ejs的html文件
>
> renderData：json对象格式。要渲染到ejs模板文件里的数据，这个对象在ejs模块文件里就是个全局对象
>
> err：错误，null代表没有错误
>
> data:	渲染后的字符|流，即：渲染后的结果	
>

**2）、后端渲染结果发送给前端**

```js
//设置模板文件的路径
app.set('views', path.join(__dirname, 'views'));
//设置模板的引擎
app.set('view engine', 'ejs');

res.render('模板文件名',{数据}) //整合页面和数据，完成渲染，发往浏览器,并结束响应
```

> 模板文件名：不用写扩展名，直接写文件名就行
>
> 数据是json对象

```
 如：
 let data = {
     news:[
            {"id":"01","title":"特朗普又说谎了"},
            {"id":"02","title":"特朗普又又说谎了"}
          ]
 }
 res.render("news",data);
```



**3）、ejs模板文件语法**

- ejs 结构就是html

- 语句：所有的JS代码都写在<%与 %>中间，（与：php和html的混合写法一样）

- 输出:	<%= 数据名|属性名|变量名 + 表达式 %>

- 非转义输出:	<%- 数据名|变量名  + 表达式 %>

- 载入公共：<%- include('./hd.ejs',{数据}) %>

- 判断

  ```html
  <% if (user) { %>
  
    <h2><%= user.name %></h2>
  
  <% } %>
  ```




[其他扩展](https://www.npmjs.com/package/ejs)



###	前后端交互流程

**后端渲染的流程**

​	用户 - > 地址栏(http[s]请求) -> web服务器（收到) - > nodejs处理请求(返回静态、动态)->请求数据库服务(返回结果)->nodejs(接收到数据)->**node渲染页面**->浏览器（接收页面，完成最终渲染)

**前端渲染的流程**

​	用户 - > http[s]请求 -> web服务器（收到) - > nodejs处理请求(返回静态、动态)->请求数据库服务(返回结果)->nodejs(接收到数据)->返回给前端(渲染)->浏览器（接收页面，完成最终渲染)



## 数据库

### 1、回顾mysql

​        关系数据库，二维表，不存在子表，每条数据的字段都是一样的，就算该字段没有值，但是字段依然存在。即：格式是固定的

**sql语句**

**建库**

```mysql
CREATE DATABASE  `2017-12-6` DEFAULT CHARACTER SET armscii8 COLLATE armscii8_general_ci;
```

**建表**

```mysql
CREATE TABLE  `2020-12-6`.`user` (
					`name` VARCHAR( 32 ) NOT NULL ,
					`age` INT( 3 ) NOT NULL ,
					`address` VARCHAR( 128 ) NOT NULL
					) ENGINE = INNODB
```

**增**

```mysql
INSERT INTO 表 (字段列表) VALUES(值列表)
INSERT INTO user (name,age,address) VALUES('苏菲',38,'外滩18号')
```

**删**

```mysql
DELETE FROM 表 WHERE 字段名=值
DELETE FROM user WHERE name='alex'
```

**改**

```mysql
UPDATE 表 SET 字段名=值 WHERE 字段名=值
UPDATE user set name='sufei' WHERE name='苏菲'
```

**查**

```mysql
SELECT ? FROM 表
SELECT * FROM user  查所有
```















































### 2、node + mysql客户端

安装+引入

```js
npm install mysql -S
var mysql = require('mysql');
```

创建库链接

```js
var connection = mysql.createConnection({
  host     : 'localhost:3306',//主机名
  user     : 'root',
  password : 'root',
  database : 'mydb2001'//库名
});
 
connection.connect();
```

表操作

```js
connection.query('SQL语句', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results==  查询array||  增删改object);
});
```

关闭库

```js
connection.end();
```



























### 3、mongodb

**非关系型数据库**，又叫nosql，缓存型，使用场景多是解决大规模数据集合多重数据种类

mongodb使用内存映射文件（memory-mapped file）来处理对磁盘文件中数据的读写请求。真正的存储(朝硬盘存储)交给操作系统

将MongoDB用作内存数据库（in-memory database），也即，根本就不让MongoDB把数据保存到磁盘中

1. [下载](https://www.mongodb.com/download-center)	[安装帮助](https://www.cnblogs.com/keyi/p/10984514.html)

2. 配置数据文件存储位置：

找到安装目录C:\Program Files\MongoDB\Server\4.0\bin  -> cmd回车-> mongod --dbpath c:\data\db

这句命令，同时也启动了数据库服务

> data和db目录要手动创建



3. 服务端启动: **可选**

找到安装目录C:\Program Files\MongoDB\Server\4.0\bin  -> cmd回车-> mongod 回车



> 一般开启会默认启动



4. 客户端连接:

找到安装目录C:\Program Files\MongoDB\Server\4.0\bin  -> cmd回车-> mongo 回车



5. 环境变量 **可选**

为了在任意盘符下去都可以启动  mongod服务端|mongo客户端，把安装目录添加到环境变量



**mysql vs mongodb**

| mysql                | mongoDb                            |              |
| -------------------- | ---------------------------------- | ------------ |
| database(库)         | database(库)                       |              |
| table(表)            | collection(集合)                   |              |
| column(字段)         | field(域)                          |              |
| row(一条数据)        | document(文档)                     |              |
| 二维表，每次存到磁盘 | json数组，存在缓存，关闭时存到磁盘 | **存储方式** |

mongodb**命令行操作**	 声明式 | obj.api()

**库操作**

```js
查: show dbs
  	db 查看当前库
建（存在该库，就是切换）:	use 库名	
```

**集合(表)操作**

```js
建：db.createCollection('表名',{配置})
  //配置：{size:集合的最大容量,capped:true,max:条数|文档数} capped定量
  //db.表(集合).isCapped() 返回 true/false 是否是定量
查：show collections / db.getCollectionNames()
删：db.集合.drop()
```

**文档(row)操作**

**增**

```js
db.集合.save({}) //添加一条
db.集合.insert({})  //添加一条
db.集合.insertOne({}) //添加一条

db.集合.save([{},{}]) //多条
db.集合.insert([{},{}]) //多条
//insert  不会替换相同ID	save会
```

**删**

```js
db.集合.deleteOne({查询条件}) //一条

db.集合.remove({查询条件},true)  //一条

db.集合.remove({查询条件}) //多条

db.集合.remove({}) //清空表
```

**改**

```js
db.集合.update({查询条件},{替换条件},[插入false],[全替换false])
```

> 查询条件
>
> ​        {age:22}		age == 22
> ​		{age:{$gt:22}}	age > 22
> ​		{age:{$lt:22}}    age < 22
> ​		{age:{$gte:22}}	age>=22
> ​		{age:{$lte:22}}	age<=22
> ​		{age:{$lte:122,$gte:22}}	age<=122 && age>=22
> ​		{$or:[{age:22},{age:122}]}	age==22 or age==122
> ​		{key:value,key2:value2} key== value && key2==value2
> ​		{name:/正则/}
>
> 替换条件
>
> {$set:{age:12},$inc:{age:-1}}

**查**

```js
所有：db.集合.find(条件)

条数: db.集合.find().count()

db.集合.find({条件},{指定要显示列区域})
```

> 指定要显示列区域
>
> username:1 显示这个区域，其他不显示
>
> username:0 不显示这个区域，其他显示
>
> _id 是默认显示

**排**

```js
db.集合.find().sort({key:1}) //升
db.集合.find().sort({key:-1})	//降
db.集合.find().sort({key1:1,key2:-1}) // 按照key1升序，key1相同时，按照key2降序
```

**限定**

```js
db.集合.find().limit(number)  //限定
db.集合.find().skip(number)	//跳过
db.集合.findOne()//找第一个
db.集合.find().limit(1)  //查询第一条
```



### 4、node+mongodb

#### 1）、Mongoose简介

mongoose是nodeJS提供连接 mongodb的一个模块，便捷操作MongoDB的对象模型工具(Mongoose的操作是以对象为单位的)

#### 2）、安装mongoose      

​     	  npm   i    mongoose  --save

#### 3）、mongoose连接数据完成增删改查

```js
//1、conndb.js  连接数据库
const mongoose = require('mongoose');
mongoose.connect(
		'mongodb://localhost:27017/db2106',
		{
            useNewUrlParser:true
        } //使用解析器来解析本次连接
);
module.exports = mongoose;

// 2、usersdb.js //针对集合users的所有操作（一般来说，增删改查）
let mongoose = require('./conndb'); //引入连接数据库的代码

//模板：创建集合users对应的模板（相当于表结构，在nodejs里创建一个表结构）
let userSchema = new mongoose.Schema({
    'userName':String,
    'userPass':String
});

//模型：（把数据库中集合users和模板进行对应和绑定）
let UserModel = mongoose.model('users',userSchema);

module.exports = {
	//1）、添加
	addUser:function(user,success,fail) {
		//1）、创建实体（要添加到数据库中的数据）
		let userEntity = new UserModel({
            userName:"敖东",
            userPass:"123"
        });
		userEntity.save((err,data)=>{
			if(err){  
                fail&&fail();
			}else{	
                success&&success(); 
            }
		});
    }
	//2）、查询
	queryUser:function(obj,success,fail){
		UserModel.find(obj,(err,data)=>{
			if(err){   
                fail&&fail();  
			}else{   
                success&&success(data); 
            }
		});
	},
//3、删除
	removeUser:function(obj,success,fail){
		UserModel.remove(obj,(err,data)=>{
			if(err){
				fail&&fail();
			}else{
				success&&success(data);
			}
		});
	},
//4、修改
	updateUser:function(condationObj,updateObj,success,fail){
		UserModel.update(condationObj,updateObj,(err,data)=>{
			if(err){
				fail&&fail();
			}else{
				success&&success(data);
			}
		});
	}

```



示例(作业)：

1、mongodb  注册和登录增删改查新闻(bootstrap)

2、理解api server和webserver

​       apiserver：只提供数据（完成的接口） ，上周做的vue联合项目中的java同学做的就是apiserver

​       webserver：有页面，上周做的vue联合项目中的我们前端做的是webserver



## MVC

项目架构，项目分层，不同的层的职责不同。

C：controller，控制器。（控制的就是某个功能的业务流程）
	routers文件夹下的文件。
	根据业务流程，调用不同的模块完成对应的功能。

V：view：视图（显示）
	views文件夹下的文件。完成显示的格式。使用ejs模板，jade模板

M：model，模型（只是数据处理）
	业务逻辑处理，更多体现的是数据库的增删改查，完成具体的功能

​     db文件夹



## web开发中的架构：

### **耦合架构**







### **半分离架构**

![image-20211110115658026](D:\2110third\02node\nodejs_T_v5\image-20211110115658026.png)

>web的工作流程：
>
>1、打开web，加载静态资源，如HTML,CSS，JS等；
>
>2、发起一个Ajax请求再到服务端请求数据，同时展示loading；
>
>3、得到JSON格式的数据后再根据逻辑选择模板渲染出DOM字符串；
>
>4、将DOM字符串插入HTML页面中渲染出DOM结构；
>
>



### **分离架构**

​       **前端渲染：**vue脚手架的项目是前后端分离，但是用的是前端的渲染

![image-20211110115552713](D:\2110third\02node\nodejs_T_v5\image-20211110115552713.png)



​       **后端渲染：**如果要使用后端渲染，那么就需要使用ejs。    



![image-20211110115419083](D:\2110third\02node\nodejs_T_v5\image-20211110115419083.png)

>web的工作流程：
>      1）浏览器请求服务器端的NodeJS；
>      2）NodeJS再发起HTTP去请求JSP；
>      3）JSP的API输出JSON给NodeJS；
>      4）NodeJS收到JSON后再渲染出HTML页面；
>      5）NodeJS直接将HTML页面发送给浏览器；
>      这样，浏览器得到的就是普通的HTML页面，而不用再发Ajax去请求服务器了。

 



## restful规范

###  概念：

RESTful (资源数据接口规范) 是目前最流行的 API 设计规范，用于 Web 数据接口的设计。      
         前端设备层出不穷（手机、平板、桌面电脑、其他专用设备......）。因此，必须有一种统一的机制，方便不同的前端设备与后端进行通信。这导致API构架的流行。RESTful API是目前比较成熟的API设计理论。要搞清楚restful规范，必须先了解REST。REST强调HTTP应当以**资源**为中心，并且规范了**资源URI**的风格；规范了HTTP请求动作（PUT，POST等）的使用，具有对应的语义。
	
如：
         员工信息的uri ：http://www.qianfeng.com/employees
         书籍信息的uri ：http://www.qianfeng.com/books
         编号为01001的书籍信息的uri ：http://www.qianfeng.com/books/01001

即：**不同的资源（信息）是不同的uri**

###  优点：

​       遵循REST规范的Web应用将会获得下面好处：        
​		  URL具有很强可读性的，
​		  具有自描述性；
  		资源描述与视图的松耦合
 		 可提供OpenAPI，
 		 便于第三方系统集成，提高互操作性；
​		  如果提供无状态的服务接口，可提高应用的水平扩展性；

### 规范

**URI**
    URI(Uniform Resource Identifiers) 统一资源标示符，能够统一标识一个资源的符号都是URI
    URL(Uniform Resource Locator)  统一资源定位符，是URI的一种体现形式。
         在restfulAPI的规范里，要让一个资源可以被识别，需要有个唯一标识，在Web中这个唯一标识就是       URI(Uniform Resource Identifier)，

**URI的格式定义如下：**

​		URI = scheme "://" 域名：端口号 "/" path [ "?" query ] [ "#" fragment ]

- ​	**协议：** API与用户的通信协议，总是使用HTTPs协议。
- ​	**域名：**  应该尽量将API部署在专用域名之下，如：https://api.example.com。（如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下，https://example.org/api/）
- ​	**版本号：**https://api.qianfeng.com/v1
-    **HTTP动词：**
    GET（SELECT）：从服务器取出资源（一项或多项）。
    POST（CREATE）：在服务器新建一个资源。
    PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
    PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
    DELETE（DELETE）：从服务器删除资源。
- **路径（Endpoint）：**
      路径又称"终点"（endpoint），表示API的具体网址
       如：https://api.qianfeng.com/v1/GET/employees  获取版本1里的所有employess的信息
       如：https://api.qianfeng.com/v1/GET/employees/01001  获取版本1里的id为01001的信息 
       如：https://api.qianfeng.com/v1/GET/employees?id=01001  获取版本1里的编号为01001的信息
       如：https://api.qianfeng.com/v1/GET/employees?name



> 总结一下：
>
> ​      restful规范，最终核心规定的是由**“动词”和“名词”**构成的需求，这个需求就体现在URI上。
>
>    即：要求在URI上能够体现出需求
>
> 
>
> 如:  https://api.qianfeng.com/v1/GET/employees   表示  获取 员工信息
>
> 如:  https://api.qianfeng.com/v1/POST/employees   表示  添加 员工信息
>
> 



## bcrypt模块

bcrypt模块对用户密码进行加密。

#### 介绍：

​        bcrypt算法相对来说是运算比较**慢**的算法，在密码学界有句常话：越慢的算法越安全。算法越慢，黑客破解成本越高.通过salt和cost这两个值来减缓加密过程，加密时间（百ms级）远远超过md5（大概1ms左右）。对于计算机来说，Bcrypt 的计算速度很慢，但是对于用户来说，这个过程不算慢。bcrypt是单向的，而且经过salt和cost的处理，使其受rainbow攻击破解的概率大大降低，同时破解的难度也提升不少，相对于MD5等加密方式更加安全，而且使用也比较简单。
​       bcrypt加密后的字符串形如：$2b$10$2UCl0qI6K7tgtFmcO.DzdOKmBxfQorIuUV8Hdb12go7sHJitOV9w.，其中：$是分割符，无意义；2b 是bcrypt加密版本号；10是cost的值；而后的前22位是salt值；再然后的字符串就是密码的密文了；
​      BCrypt算法将salt随机并混入最终加密后的密码，验证时也无需单独提供之前的salt，从而无需单独处理salt问题。



#### 安装：

```js
 npm   i   bcrypt   --save
```

#### 加密代码：

```js


//1、引入模块
let  bcrypt= require("bcrypt");

//2、 产生salt

let salt = bcrypt.genSaltSync(11); // 11 是迭代次数

//3、加密
// bcrypt.hashSync(用户注册时输入的密码,salt); 
let pass = bcrypt.hashSync("123456",salt); 

console.log("pass",pass);//pass就是加密后的结果，把这个可以存储到数据库中

// $2b$10$1MtFAztjfpDTm8z.PjQTwOo6k4FrRiXwbZKq0oAKWqWI94mhzJfTG
// $2b$11$jMC6xi32MVFDApY.valjJ.f7W5gGXQoLj3VZlrPQ8Fik.pVQ/szTK
```



查看用户输入密码和数据库中是否一致

```js
// 验证密码是否正确
//1、引入模块
let  bcrypt= require("bcrypt");

// let isMatch = bcrypt.compareSync(用户登录时输入的密码,数据库中拿到的密码);

let isMatch = bcrypt.compareSync("123456","$2b$11$jMC6xi32MVFDApY.valjJ.f7W5gGXQoLj3VZlrPQ8Fik.pVQ/szTK");

console.log(isMatch);//true：表示密码一致；false：密码不一致
```



示例：bcrypt加密 注册和登录



## 身份验证

​       HTTP 是一种没有状态的协议，也就是它并不知道是谁访问(或者说，没法区分，每次的访问和以前哪次的访问来自同一个客户端)。客户端用户名密码通过了身份验证，不过下回这个客户端再发送请求时候，还得再验证

### session（会话）

#### 思想

​        1、当用户打开浏览器，首次访问某个网站（服务器），服务器会产生一个唯一的编号（sessionId），在响应时，把该编号发给客户端，客户端把该编号存储在cookie里。

​        2、当用户二次访问（如：点击超链）服务器，那么，请求时，会携带保存在cookie里的编号，服务器端拿到该编号后，会跟session中的编号进行对比，就能知道本次访问和哪次访问是同一个客户端。



#### 业务场景：

​        1、客户端发送用户名和密码，请求登录

​		2、服务端收到请求，去库验证用户名与密码

​		3、验证成功后，服务端种一个cookie或发一个字符到客户端，同时服务器保留一份session

​		4、客户端收到 响应 以后可以把收到的字符存到cookie

​        5、客户端每次向服务端请求资源的cookie会自动携带

​		6、服务端收到请求，然后去验证cookie和session，如果验证成功，就向客户端返回请求的库数据



> Session存储位置: 服务器内存，磁盘，或者数据库里
>
> Session存储内容: id,存储时间，用户名等 
>
> 客户端携带 ： cookie自动带



#### express-session



1、引入session模块： var session = require('express-session');

2、创建session中间件：

```js
  app.use(session(options)); 
```

 示例：

```js
app.use(session({
	secret: 'recommend 128 bytes random string',
	cookie: { maxAge: 20 * 60 * 1000 },//session的过期时间(没有错，cookie和session息息相关，所以，是用cookie.maxAge来设置session的过期时间)
 	resave: true,
  	saveUninitialized: true
	})
);
```



​    3、保存变量：

```js
req.session.变量名= 值；
```

​    4、获取变量：

```
req.session.变量名
```



示例：session 登录和其它需要验证身份的模块（如：添加，删除）



### token

现在比较流行。

#### 思想

在服务端不需要存储用户的登录记录，全部发给客户端**由客户端自己存(**cookie,localStorage)

​        1、客户端使用用户名跟密码请求登录（前端）

​		2、服务端收到请求，去验证用户名与密码​	（后端）	

​        3、验证成功后，服务端会签发(**产生**)一个 **Token**（加了密的字符串），再把这个 Token 发送给客户端（后端）

​		4、客户端收到 Token 以后可以把它存储起来，比如放在 Cookie 里或者 Local Storage 里（前端）

​		5、客户端每次向服务端请求资源的时候需要带着服务端签发的 Token（前端）

​		6、服务端收到请求，然后去**验证**客户端请求里面带着的 **Token**，如果验证成功，就向客户端返回请求的数据



示例：vue项目（webserver）和nodejs项目（apiserver）使用token。

































#### 实现

**jsonwebtoken**的安装引入

```js
let jwt = require('jsonwebtoken')
```

**生成签名**（token）

```js
let token = jwt.sign(payload, secretOrPrivateKey, [options, callback])
```

[^payload]: 使用用户输入的信息（如：用户名），加密的原始字符串
[^secretOrPrivateKey]: 加密规则，字符串，或者私钥path模块，其实就是一串字符串（越乱越好）
[^options]: 可选配置项 ，如：expiresIn表示 过期时间（单位是秒）
[^callback]: 成功回调, 可选 返回制作后的token,也可同步返回；如果写了该回调函数就用的回调函数的异步操作，如果不写，那么同步返回

**校验token**

```js
jwt.verify(token, secretOrPublicKey, [options, callback])
```

[^token]: 制作后的token
[^secretOrPublicKey]: 解密规则，字符串，或者公钥
[^callback：]: 回调 err 错误信息 decode 成功后的信息

[^options]: expiresIn 过期时间



**token删除**

由客户端，负责删除，服务器端没有任何保存，所以，只需要在客户端删除就行。



示例：token 登录，其它需要验证身份的模块（如：添加，删除）































### session vs token

|                    | session | token  |
| ------------------ | ------- | ------ |
| 服务端保存用户信息 | √       | ×      |
| 避免CSRF攻击       | ×       | √      |
| 多服务器粘性问题   | 存在    | 不存在 |

> 多服务器粘性问题
>
> 当在应用中进行 session的读，写或者删除操作时，会有一个文件操作发生在操作系统的temp 文件夹下，至少在第一次时。假设有多台服务器并且 session 在第一台服务上创建。当你再次发送请求并且这个请求落在另一台服务器上，session 信息并不存在并且会获得一个“未认证”的响应。这时候，就需要把在第一台服务器上保存的session给其它服务器上也保存。然而，在基于 token 的认证中，这个问题很自然就被解决了。没有粘性 session 的问题，因为在每个发送到服务器的请求中这个请求的 token 都会被拦截



对于注销登录来说，token只需要在客户端删除就行。



















### 如何保存信息给浏览器（cookie）

**1、前端保存：** 

cookie/localstorage

**2、后端操作cookie:**   

服务器给浏览器种cookie:  	cookie-parser

 Cookie的创建（express会将其填入Response Header中的Set-Cookie）：

 **格式：** 

1、添加cookie

```js
res.cookie(name, value [, options]);
//参数：
   name: cookie名
   value: 类型为String和Object。
   Option: 类型为对象，可使用的属性如下：
   domain：cookie在什么域名下有效，类型为String,。默认为网站域名
   expires: cookie过期时间，类型为Date。如果没有设置或者设置为0，那么该cookie只在这个session有效
   maxAge: 实现expires的功能，设置cookie过期的时间，类型为String，指明从现在开始，多少毫秒以后，cookie到期。
   path: cookie在什么路径下有效，默认为'/'，类型为String
   secure：只能被HTTPS使用，类型Boolean，默认为false
   

 
```

2、删除cookie

```js
res.clearCookie(name [, options]);
```

3、获取cookie

```js
req.cookies.键名  //cookie是在客户端保存，每次请求时会携带，所以，用req对象获取cookie
```

**示例：**

```js
设置键为name，值为：baobao，可访问的域名为：.example.com，可访问的路径：/admin'，只可以用https访问
res.cookie('name', 'baobao', { 
				domain: '.example.com', 
					maxAge:10000*1000,
				path: '/admin', 
				secure: true 
				});

cookie的value为对象
res.cookie('cart', { items: [1,2,3] });
```

服务器给浏览器种cookie的同时在服务器上生成session:  cookie-session



























## 文件上传

**思想**

前端表单->后端接收到文件本身->保存到服务器上->给数据库记录文件一些信息（如：文件路径）->库返回给nodejs相关信息->nodejs返回给前端

前端: 

```html
<form enctype="multipart/form-data" action="" method="post">
      <input type=file  name="fieldname"  />
    
</form>
```



**实现**

multer->文件名会随机->fs模块改名->path系统模块解析磁盘路径

> 后端：multer 接受 form-data编码数据 



### multer中间件

multer 接受 form-data编码数据,所以，要求前端携带时注意一下，如：

<form enctype="multipart/form-data" />
**使用**

```js
//1 引入(在app.js里)
let multer  = require('multer');
//2 实例化  
let objMulter = multer({ dest: './upload' }); //dest: 指定 保存位置（存到服务器)
//安装中间件， 
app.use(objMulter.any());  //允许上传什么类型文件,any 代表任何类型  

```

中间件扩展了req请求体 `req.files`

```js
app.post('/reg',(req,res)=>{
  req.files
})
```

> ​        fieldname: 表单name名
> ​		originalname: 上传的文件名（浏览器端选择的文件的名字）
> ​		encoding： 编码方式
> ​		mimetype: 文件类型
> ​		buffer: 文件本身
> ​		size：尺寸
> ​		destination: 上传后，文件保存的路径 （服务器端的路径）
> ​		filename： 上传后，保存后的文件名  不含后缀
> ​		path：	上传后，保存磁盘路径+保存后的文件名 不含后缀



前端使用form发送请求代码



```html
  <form action="/upload" enctype="multipart/form-data" method="post" >
        <input type="text" name="username" >
        <input type="file" name="files" >
        <input type="submit" value="上传">
    </form>    
```



前端使用ajax发送请求：

```html
<body>
    <form action="regSave" method="post" >
        
        
        
        用户名<input type="text" name="username" ><br/>
        
        
        头像：
        <img style="width:50px;height:50px" id="logoImg" />
        
            <input type="file" id="photoFile" style="display: none;" onchange="upload()">
        
            <a href="javascript:void(0)" onclick="uploadPhoto()">选择图片</a>
        
        	<input name="logo" type="hidden" id="logoInput" >     <br/>  
        
        
        <input type="submit" value="注册" >
        
        
    </form>    
</body>
</html>
<script src="js/jquery-3.2.1.min.js"></script>
<script>
    function uploadPhoto() {
        $("#photoFile").click();//选择文件的按钮
    }
    //上传
   function upload(){
        if ($("#photoFile").val() == '') {
            return;
        }
        console.log($("#photoFile").val());
       
        var formData = new FormData();
       
        formData.append('photo', document.getElementById('photoFile').files[0]);
       
        $.ajax({
            url:"/upload",
            type:"post",
            data:formData,  
            contentType:false,
            processData:false,        
            success:function(str){
                console.log(str);//图片文件路径
                $("#logoImg").attr("src",str);
                $("#logoInput").val(str);
            }
        });
    };
</script>
```





## BSR&&SSR&&SEO



### BSR

 客户端渲染（BSR:Browser Side Render)

客户端渲染的意思：就是客户端在操作DOM.
渲染过程：在服务端放了一个html 页面，客户端发起请求，服务端把页面发送过去，客户端从上到下依次解析，如果在解析的过程中，发现ajax请求，再次向服务器发送新的请求，客户端拿到ajax 响应的结果，渲染在页面上，这个过程中至少和服务端交互了两次

缺点：请求次数会增加，如果渲染逻辑复杂，数量大的话，尽量不要使用客户端渲染（因为，客户端电脑的配置低）。
优点：网络上传输的数据量小，局部刷新



### SSR 

服务端渲染（SSR）

渲染过程：前端发送请求，服务器端从数据库中拿出数据，通过render（）函数，把数据渲染在模板（ejs）里，产生了HTML代码，把渲染结果发给了前端，整个过程只有一次交互。 

优点：请求的次数少，提高渲染速度（服务器的配置高）

缺点：网络上传输的数据量大了，页面全部刷新。



### SSR与BSR的区别

 大多数网站里，既有服务端渲染又有客户端渲染 。

服务端渲染（SSR）和客户端渲染（BSR）的区别

客户端渲染不利于 SEO 搜索引擎优化，服务器端渲染有利于SEO搜索引擎优化

服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的



>例如：
>
>京东网站：
>
>1、商品列表就采用的是服务端渲染，目的了为了 SEO 搜索引擎优化
>
>2、商品评论列表为了用户体验，而且也不需要 SEO 优化，所以采用是客户端渲染
>
>



### SEO

SEO（Search Engine Optimization）：搜索引擎优化（1994年诞生的）

作用：提升网站关键词排名，提高访问量

搜索引擎不友好的网站有哪些特征：

1、网页中大量采用图片或者Flash等富媒体（Rich Media）形式，没有可以检索的文本信息，而SEO最基本的就是文章SEO和图片SEO( img标签的alt )；

2、网页没有标题，或者标题中没有包含有效的关键词；

3、网页正文中有效关键词比较少（最好自然而重点分布，不需要特别的堆砌关键词）；

4、网站导航系统让搜索引擎“看不懂”；

5、大量动态网页影响搜索引擎检索；

6、没有被其他已经被搜索引擎收录的网站提供的链接；

7、网站中充斥大量欺骗搜索引擎的垃圾信息，如“过渡页”、“桥页”、颜色与背景色相同的文字；

8、网站中缺少原创的内容，完全照搬硬抄别人的内容等。

9、网站地图，网站日记提取蜘蛛爬行轨迹。



## SOCKET

###  socket应用

​        Web领域的实时推送技术，也被称作Realtime技术。这种技术要达到的目的是让用户不需要刷新浏览器就可以获得实时更新。它有着广泛的应用场景，比如WebIM( 网页聊天 )、在线客服系统、评论系统等。
​         而曾经的HTTP协议，是无状态的协议，一次交互完成后，连接就断开了。所以，服务器端和客户端就没有连接（也就是说，服务器端并不能拿到聊天的客户端）。只有客户端发送请求，服务器才知道那些客户端连接了。所以http是被动的协议，客户端不请求，服务器端不响应，显然不能满足需求。HTML5提供的websocket协议可以主动推送信息。



### socket介绍

​        socket：插座；套接字
​        网络上的两个程序通过一个双向（全双工）的通信连接实现数据的交换，这个连接的一端称为一个socket。就像用座机打电话，给两个座机都插上电话线，就可以打电话，即语音信息的交流。



### socket的通讯流程 

![1608630467559](img/04socket01.png)



服务器端（电话的一端，接听电话者）：
1、创建socket，表示有了一个电话
2、绑定socket和端口号，相当于，电话对应了一个电话号码
3、监听端口号，相当于，把电话插上电话线，可以随时等待有人拨通电话
4、接收客户端的连接请求，相当于有人打来了电话
5、从socket中读取字符，相当于，接起电话，有语音信息传输过来了
6、关闭socket，相当于通话完成后，挂掉电话

客户端：
1、 创建socket，表示有了一个电话（当然也默认绑定了端口号）
2、连接指定的计算机端口（服务器的地址和端口），相当于拨打电话
3、向socket中写入信息，相当于给对方说话
4、关闭socket，相当于通话完成后，挂掉电话



### webSocket

WebSocket是HTML5新增加的原生的对象，是一个通信的协议（ws协议）。是HTML5开始提供的一种浏览器与服务器间进行全双工通讯的网络技术。在WebSocket API中，浏览器和服务器只需要做一个握手(handshaking)的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。
         
    单工： 只能单向， 传呼机
    半双工：双向，但是同时，只能单向，对讲机
    全双工：双向，可以同时传输信息，电话



> 对比webSocket协议和http协议：
>
> HTTP协议是不支持持久连接的（长连接）
>
> Websocket是一个持久化的协议，相对于HTTP这种非持久的协议来说。



### webSocket对象介绍

#### 属性

| **属性**       | **描述**                                                     |
| -------------- | ------------------------------------------------------------ |
| readyState     | 只读属性 readyState 表示连接状态，可以是以下值：0 - 表示连接尚未建立。1 - 表示连接已建立，可以进行通信。2 - 表示连接正在进行关闭。3 - 表示连接已经关闭或者连接不能打开。 |
| bufferedAmount | 只读属性 bufferedAmount 已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数 |

#### 事件

| **事件**   | **事件处理程序** | **描述**                   |
| ---------- | ---------------- | -------------------------- |
| connection |                  | 连接事件                   |
| open       | Socket.onopen    | 连接建立时触发             |
| message    | Socket.onmessage | 客户端接收服务端数据时触发 |
| error      | Socket.onerror   | 通信发生错误时触发         |
| close      | Socket.onclose   | 连接关闭时触发             |

#### 方法

| **方法** | **描述**                                |
| -------- | --------------------------------------- |
| send()   | 使用连接发送数据                        |
| close()  | 关闭连接，同时会触发另外一端的close事件 |

#### 使用步骤:

**1、安装ws模块（webSocket）**

```
 npm  i ws  --save
```

>  ws：是nodejs的一个WebSocket库，可以用来创建服务。
>
> 客户端send函数触发 服务器官方事件message。

**2、服务器端代码**

```js
var WebSocketServer = require('ws').Server;
var wss = new WebSocketServer({ port: 9000 });

let clients = [];  //记录客户端对象  
let i =0; //记录客户端 序号

//绑定事件connection，此事件是当有客户端连接时，触发
wss.on('connection', function (ws) { //ws就是连接的客户端对象
        ws.name = ++i; 
        clients.push(ws) //把客户端对象保存起来
    
        //给客户端对象绑定事件message，当客户端发送信息时，触发该事件
        ws.on('message', function (message) { //给客户端对象绑定message事件，有信息发过来了。
            broadcast(message, ws) //把客户端发送来的信息，广播给其它客户端。 
        })
    
        ws.on('close', function () { //给客户端对象绑定close事件，客户端关闭了
            console.log('离开');
        })
    
})
    
//广播信息
function broadcast(msg, ws) {//broadcast是把信息发布（广播）给其它客户端
       for (var key in clients) { //clients: 记录着所有的客户端对象
           clients[key].send(ws.name + '说: ' + msg)
       }
}
```



**3、客户端代码**

```js
//js代码：WsClient.js
//new WebSocket()，就触发了服务器端的connection事件
var ws = new WebSocket('ws://127.0.0.1:9000/')

//当连接上服务器端，即打开连接后，触发
ws.onopen = function () {
  ws.send('大家好')
}

//当接收到（服务器端的）信息后，触发
ws.onmessage = function (event) {
  var chatroot = document.getElementById('chatroom');
  chatroom.innerHTML += '<br/>' + event.data
}
//当服务器端关闭时，触发
ws.onclose = function () {
  console.log('Closed')
}
//当出错时，触发
ws.onerror = function (err) {
  alert('Error:' + err)
}

//html代码
<h1>WebSocket</h1>
  <div id="chatroom" style="width:400px;height:300px;overflow:auto;border:1px solid blue"></div>
  <input type="text" name="sayinput" id="sayinput" value="">
  <input type="button" name="send" id="sendbutton" value="发送">
      
  <script src="WsClient.js"></script>

  <script type="text/javascript">
   
   function send() {
      ws.send(sayinput.value);
      sayinput.value = '';
    }

    document.onkeyup = function (event) {
      if (event.keycode == 13) {
        send();
      }
    }
    sendbutton.onclick = function () {
      send();
    }
  </script>


```



![1625452889320](C:\Users\31759\AppData\Roaming\Typora\typora-user-images\1625452889320.png)



### socket.io

#### 介绍：

​       如果浏览器不支持HTML5，即没法直接使用websocket。我们还需要使用socket.io来考虑兼容性。即，如果支持HTML5，就用websocket，如果不支持HTML5就使用socket.io。Socket.io使用检测功能来判断是否建立WebSocket连接。Socket.IO还提供了一个NodeJS API



#### 思路：

服务器和客户端用触发自定义事件的方式传递数据

![1608630934509](img/04socket02.png)



#### 代码：

1、  使用node 的express和socket.io
 2、 安装express
 3、 安装socket.io
	

```js
   npm   i   express  socket.io
```

  前端代码需要引入“socket.io.js”



 **服务器端：**

```js
const server = require('http').createServer();
const io = require("socket.io")(server, {
    allowEIO3: true,
    // 解决跨域问题
    cors: {
      origin: "*", // from the screenshot you provided
      methods: ["GET", "POST"]
    }
});


server.listen(9009,()=>{
    console.log("启动成功！");
});


io.on('connection', () => { 
   console.log("") 
});




//保存所有的客户端
var clients = {};

io.sockets.on('connection', function (socket) {
        console.log("connection被触发了");
        //有客户端连接，发送消息 (f 是自定义事件)
        socket.on('f', function (sayer, fn) {
            //如果没有保存改客户端，那么就保存该客户端
            if (!clients[sayer]) {
                clients[sayer] = this;
            }
            console.log(clients);
            fn();
     });
        
	socket.on('message', function (data) {
        console.log("message被触发了");
        broadcast(data);
	});
    
function  broadcast(data){
         for(let key in clients){
              //给客户端发送消息，使用事件，emit触发事件 news	
            clients[key].emit('news',data);
         }
     }
 });
```



**客户端：**

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="chatroom" style="width:400px;height:300px;overflow:auto;border:1px solid blue"></div>
    <input type="text"  id="sayer" >说：
    <input type="text"  id="sayConent" >
    <input type="button"  id="sendbutton" value="发送">
</body>
</html>
<script src="./js/socket.io.js"></script>
<script>

sayer.value = "游客"+Date.parse((new Date()).toUTCString());
// 创建对象，并连接服务器端（触发服务器端的connection事件）
const socket = io.connect("http://127.0.0.1:9009");
// 连接事件
socket.on("connect",function(){
    console.log("connect");
    // 连接建立起来后，打个招呼
    socket.emit('f',sayer.value, function () {
        socket.send(sayer.value+"说：hello ！"); //send函数触发的是服务器端message事件
    });
});

socket.on("news",function(data){
    console.log("news被触发了");
    chatroom.innerHTML += data+"<br/>";
})

sendbutton.onclick = function(){
    socket.send(sayer.value+"说："+sayConent.value); 
    sayConent.value="";
}

</script>
```



![1608630940999](img/04socket03.png)



![1608630947589](img/04socket04.png)

























