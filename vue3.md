## 学习vue3第一章

#### 1.介绍vue

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

MVVM（Model-View-ViewModel）架构

『View』：视图层（UI 用户界面）
『ViewModel』：业务逻辑层（一切 js 可视为业务逻辑）
『Model』：数据层（存储数据及对数据的处理如增删改查）
![img](https://img-blog.csdnimg.cn/a5c02dc81b9547a6bebfd9dbc3502687.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 2.回顾vue2对比<u>vue3</u>

我们看如下图

发现传统的vue2 逻辑比较分散 可读性差 可维护性差

对比vue3 逻辑分明 可维护性 高

![img](https://img-blog.csdnimg.cn/img_convert/e8ad905d83aaec45451797517ef453aa.png)

#### 3.vue3新特性介绍

![img](https://img-blog.csdnimg.cn/f2c9d2e9576d46cf8a9a3e8abc5b31b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

- [ ] #### 重写双向数据绑定

```
vue2
基于Object.defineProperty()实现
 
vue3 基于Proxy
proxy与Object.defineProperty(obj, prop, desc)方式相比有以下优势：
 
//丢掉麻烦的备份数据
//省去for in 循环
//可以监听数组变化
//代码更简化
//可以监听动态新增的属性；
//可以监听删除的属性 ；
//可以监听数组的索引和 length 属性；
 
    let proxyObj = new Proxy(obj,{
        get : function (target,prop) {
            return prop in target ? target[prop] : 0
        },
        set : function (target,prop,value) {
            target[prop] = 888;
        }
    })
```

Vue3 优化Vdom
在Vue2中,每次更新diff,都是全量对比,Vue3则只对比带有标记的,这样大大减少了非动态内容的对比消耗

Vue Template Explorer 我们可以通过这个网站看到静态标记



patch flag 优化静态树

```html
<span>Hello world!</span>
<span>Hello world!</span>
<span>Hello world!</span>
<span>Hello world!</span>
<span>{{msg}}</span>
<span>Hello world!</span>
<span>Hello world! </span>
```

Vue3 编译后的 Vdom 是这个样子的

```js
export function render(_ctx，_cache，$props，$setup，$data，$options){return (_openBlock(),_createBlock(_Fragment,null，[
_createvNode( "span", null,"Hello world ! "),
_createvNode( "span",null，"Hello world! "),
_createvNode( "span"，null，"Hello world! "),
_createvNode( "span", null，"Hello world! "),
_createVNode("span", null，_toDisplaystring(_ctx.msg)，1/* TEXT */)，
_createvNode( "span", null，"Hello world! "),
_createvNode( "span", null，"Hello world! ")]，64/*STABLE_FRAGMENT */))
```

新增了 patch flag 标记

```js
TEXT = 1 // 动态文本节点
CLASS=1<<1,1 // 2//动态class
STYLE=1<<2，// 4 //动态style
PROPS=1<<3,// 8 //动态属性，但不包含类名和样式
FULLPR0PS=1<<4,// 16 //具有动态key属性，当key改变时，需要进行完整的diff比较。
HYDRATE_ EVENTS = 1 << 5，// 32 //带有监听事件的节点
STABLE FRAGMENT = 1 << 6, // 64 //一个不会改变子节点顺序的fragment
KEYED_ FRAGMENT = 1 << 7, // 128 //带有key属性的fragment 或部分子字节有key
UNKEYED FRAGMENT = 1<< 8, // 256 //子节点没有key 的fragment
NEED PATCH = 1 << 9, // 512 //一个节点只会进行非props比较
DYNAMIC_SLOTS = 1 << 10 // 1024 // 动态slot
HOISTED = -1 // 静态节点
BALL = -2
```

我们发现创建动态 dom 元素的时候，Vdom 除了模拟出来了它的基本信息之外，还给它加了一个标记： 1 /* TEXT */

这个标记就叫做 patch flag（补丁标记）

patch flag 的强大之处在于，当你的 diff 算法走到 _createBlock 函数的时候，会忽略所有的静态节点，只对有标记的动态节点进行对比，而且在多层的嵌套下依然有效。

尽管 JavaScript 做 Vdom 的对比已经非常的快，但是 patch flag 的出现还是让 Vue3 的 Vdom 的性能得到了很大的提升，尤其是在针对大组件的时候。


Vue3 Fragment
vue3 允许我们支持多个根节点


<template>
  <div>12</div>
  <div>23</div>
</template>

同时支持render JSX 写法

```
render() {
        return (
            <>
                {this.visable ? (
                    <div>{this.obj.name}</div>
                ) : (
                    <div>{this.obj.price}</div>
                )}
                <input v-model={this.val}></input>
                {[1, 2, 3].map((v) => {
                   return <div>{v}-----</div>;
                })}
            </>
        );
    },

```

同时新增了Suspense teleport  和  多 v-model 用法

#### Vue3 Tree shaking

简单来讲，就是在保持代码运行结果不变的前提下，去除无用的代码

在Vue2中，无论我们使用什么功能，它们最终都会出现在生产代码中。主要原因是Vue实例在项目中是单例的，捆绑程序无法检测到该对象的哪些属性在代码中被使用到

而Vue3源码引入tree shaking特性，将全局 API 进行分块。如果你不使用其某些功能，它们将不会包含在你的基础包中

就是比如你要用watch 就是import {watch} from 'vue' 其他的computed 没用到就不会给你打包减少体积

#### Vue 3 Composition Api

Setup 语法糖式编程 

## 学习vue3第二章（配置环境）

1.安装nodejs（建议装14，16，版本稳定）
装过的同学可以忽略

下载 | Node.js 中文网

装完之后会有一个命令叫 npm

可以在终端输入npm -v 来检查是否安装成功

![img](https://img-blog.csdnimg.cn/7d2fc2a1c353444b8e9dd56e0f63145f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

2.构建vite项目
官方文档开始 [Vite中文网]()https://vitejs.cn/guide/#overview

vite 的优势

 冷服务   默认的构建目标浏览器是能 在 script 标签上支持原生 ESM 和 原生 ESM 动态导入

 HMR  速度快到惊人的 模块热更新（HMR）

Rollup打包  它使用 Rollup 打包你的代码，并且它是预配置的 并且支持大部分rollup插件

使用vite初始化一个项目

npm

```
npm init vite@latest
运行之后
```

项目名称

![img](https://img-blog.csdnimg.cn/79a9f7a215224aafb904217075b3816b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

构建的项目模板

![img](https://img-blog.csdnimg.cn/229e41aa7c124fb8a0ed11d89a37e342.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

切换目录

npm install 安装依赖包

npm run dev 启动

![img](https://img-blog.csdnimg.cn/06271d7048fb4dbda0447badfd4c0e1d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

 package json 命令解析

```jsx
{
  "scripts": {
    "dev": "vite", // 启动开发服务器，别名：`vite dev`，`vite serve`
    "build": "vite build", // 为生产环境构建产物
    "preview": "vite preview" // 本地预览生产构建产物
  }
}3.安装Vue cli脚手架 
npm install @vue/cli -g
```

检查是否安装成功

![img](https://img-blog.csdnimg.cn/89487aa89f184aa9bd5f90df990a0380.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

 vue create <project>

构建我们的cli 项目可以去对比一下

### nodejs 底层原理（非重要）

Node.js 主要由 **V8**、**Libuv** 和第三方库组成：

1.Libuv：跨平台的异步 IO 库，但它提供的功能不仅仅是 IO，还包括进程、线程、信号、定时器、进程间通信，线程池等。
2.第三方库：异步 DNS 解析（ cares ）、HTTP 解析器（旧版使用 http_parser，新版使用 llhttp）、HTTP2 解析器（ nghttp2 ）、 解压压缩库( zlib )、加密解密库( openssl )等等。
3.V8：实现 JS 解析、执行和支持自定义拓展，得益于 V8 支持自定义拓展，才有了 Node.js。
你也可以理解成 js应用层  桥C/C++  底层C/C++ 

![img](https://img-blog.csdnimg.cn/d3718cc40bd74ad884adb38d07a5b0cb.png)



libuvC语言源码解析 

[libuv源码地址]: GitHub-libuv/libuv:Cross-platformasynchronousI/O

 


![img](https://img-blog.csdnimg.cn/4a958b1a38a8469fb0bd9a9a8b53667c.png)

##	学习Vue3 第三章（Vite目录 & Vue单文件组件 & npm run dev 详解）

#### 1.vite目录

public 下面的不会被编译 可以存放静态资源

assets 下面可以存放可编译的静态资源

components 下面用来存放我们的组件

App.vue 是全局组件

main ts 全局的ts文件

**index.html 非常重要的入口文件 （webpack，rollup 他们的入口文件都是enrty input 是一个js文件 而Vite 的入口文件是一个html文件，他刚开始不会编译这些js文件 只有当你用到的时候 如script src="xxxxx.js" 会发起一个请求被vite拦截这时候才会解析js文件）**

vite config ts 这是vite的配置文件具体配置项 后面会详解

VsCode Vue3 插件推荐 Vue Language Features (Volar)

####	2.SFC 语法规范

*.vue 件都由三种类型的顶层语法块所组成：<template>`、`<script>`、`<style>、<template>

1.每个 `*.vue` 文件最多可同时包含一个顶层 `<template>` 块。

2.其中的内容会被提取出来并传递给 `@vue/compiler-dom`，预编译为 JavaScript 的渲染函数，并附属到导出的组件上作为其 `render` 选项。

------

其中的内容会被提取出来并传递给 @vue/compiler-dom，预编译为 JavaScript 的渲染函数，并附属到导出的组件上作为其 render 选项。

**script**

- 每一个 `*.vue` 文件可以有多个 `<script>` 块 (不包括[](https://v3.cn.vuejs.org/api/sfc-script-setup.html))。

- 该脚本将作为 ES Module 来执行。

- 其**默认导出**的内容应该是 Vue 组件选项对象，它要么是一个普通的对象，要么是 [defineComponent](https://v3.cn.vuejs.org/api/global-api.html#definecomponent) 的返回值 

  ------

  

**script setup**

每个 *.vue 文件最多只能有一个 <script setup> 块 (不包括常规的 <script>)

该脚本会被预处理并作为组件的 setup() 函数使用，也就是说它会在每个组件实例中执行。<script setup> 的顶层绑定会自动暴露给模板。更多详情请查看 <script setup> 文档。

------

**style**
一个 *.vue 文件可以包含多个 <style> 标签。

style 标签可以通过 scoped 或 module attribute (更多详情请查看 SFC 样式特性) 将样式封装在当前组件内。多个不同封装模式的 <style> 标签可以在同一个组件中混

------

npm run dev 详解
在我们执行这个命令的时候会去找 package json 的scripts 然后执行对应的dev命令

 ![img](https://img-blog.csdnimg.cn/6acb806d8ffa485ca5d82080fa812889.png)

 那为什么我们不直接执行vite 命令不是更方便吗

应为在我们的电脑上面并没有配置过相关命令 所以无法直接执行

![img](https://img-blog.csdnimg.cn/a336be16f8464405be0adea96e7ceed6.png)

 其实在我们执行npm install 的时候（包含vite） 会在node_modules/.bin/ 创建好可执行文件

.bin 目录，这个目录不是任何一个 npm 包。目录下的文件，表示这是一个个软链接，打开文件可以看到文件顶部写着 #!/bin/sh ，表示这是一个脚本 

![img](https://img-blog.csdnimg.cn/a16de7a134e44c81b1501da1767432a6.png)

![img](https://img-blog.csdnimg.cn/4cfa1c01ea6d46938fcbdf972c49db48.png)



在我们执行npm run xxx  npm 会通过软连接 查找这个软连接存在于源码目录node_modules/vite

![img](https://img-blog.csdnimg.cn/e5fdf294051e4953b6e70e31345d220c.png)

 所以npm run xxx 的时候，就会到 node_modules/bin中找对应的映射文件，然后再找到相应的js文件来执行

1.查找规则是先从当前项目的node_modlue /bin去找,

2.找不到去全局的node_module/bin 去找

3.再找不到 去环境变量去找


![img](https://img-blog.csdnimg.cn/07db23d9dcd142eda151cb0203c91495.png)

 node_modules/bin中 有三个vite文件。为什么会有三个文件呢？

```js
# unix Linux macOS 系默认的可执行文件，必须输入完整文件名
vite
 
# windows cmd 中默认的可执行文件，当我们不添加后缀名时，自动根据 pathext 查找文件
vite
 
# Windows PowerShell 中可执行文件，可以跨平台
vite
```

我们使用windows 一般执行的是第二个

MacOS Linux 一般是第一个

##	学习Vue3 第四章（模板语法 & vue指令）

#### 1.模板插值语法

在script 声明一个变量可以直接在template 使用用法为{{变量名称}}

```jsx
<template>
  <div>{{ message }}</div>
</template>
 
 
<script setup lang="ts">
const message = "我是小满"
</script>
 
 
 
<style>
</style>
```

模板语法是可以编写条件运算的

```jsx
<template>
  <div>{{ message == 0 ? '我是小满0' : '我不是小满other' }}</div>
</template>
 
 
<script setup lang="ts">
const message:number = 1
</script>
 
 
 
<style>
</style>
```

运算也是支持的

```jsx
<template>
  <div>{{ message  + 1 }}</div>
</template>
 
 
<script setup lang="ts">
const message:number = 1
</script>
 
 
 
<style>
</style>
```

操作API 也是支持的

```jsx
<template>
  <div>{{ message.split('，') }}</div>
</template>
 
 
<script setup lang="ts">
const message:string = "我，是，小，满"
</script>
 
 
 
<style>
</style>
```

#### 2.指令

v- 开头都是vue 的指令

v-text 用来显示文本

v-html 用来展示富文本

v-if 用来控制元素的显示隐藏（切换真假DOM）

v-else-if 表示 v-if 的“else if 块”。可以链式调用

v-else v-if条件收尾语句

v-show 用来控制元素的显示隐藏（display none block Css切换）

v-on 简写@ 用来给元素添加事件

v-bind 简写:  用来绑定元素的属性Attr

v-model 双向绑定

v-for 用来遍历元素

v-on修饰符 冒泡案例

v-once 性能优化只渲染一次

v-memo 这个指令是`Vue3.2`**新增的内置指令，大致的作用就是小幅度手动提升一部分**`性能`

**用法**

在组件和元素都可以使用，主要是可以`缓存` 期望的类型是个数组`any[]`，该指令需要传入一个固定长度的依赖值数组进行比较。如果数组里的每个值都与最后一次的渲染相同，那么他的更新将会被跳过，甚至虚拟 DOM 的 vnode 创建也将被跳过，提升了性能。

tips：如果`v-memo="[]"` 传入的是一个空数组，那么他的效果和`v-once` 一样

```html
<div v-memo="[val]"></div>
```

**配合v-for**

配合v-for 属于最常见的情况，但是只有助于大数据量的情况例如`（1000条以上）`。

tips:   当搭配 `v-for` 使用 `v-memo`，确保两者都绑定在同一个元素上。**`v-memo` 不能用在 `v-for` 内部。**

测试代码 `未加v-memo 一万条数据`

当组件的 `selected` 状态改变，默认会重新创建大量的 vnode，尽管绝大部分都跟之前是一模一样的。`v-memo` 用在这里本质上是在说“只有当该项的被选中状态改变时才需要更新”。这使得每个选中状态没有变的项能完全重用之前的 vnode 并跳过差异比较。

```jsx
<template>
  <div>
    <div @click="select(item.id)"   :key="item.id" v-for="(item) in arr">
      {{ item.id }} - selected： {{ item.id == active }}
    </div>
  </div>
</template>

<script setup lang='ts'>
import { ref, reactive } from 'vue'

const arr = reactive<any[]>([])
for (let i = 0; i < 10000; i++) {
  arr.push({
    id: i + 1,
    name: "test"
  })
}

const active = ref(1)

const select = async (index: number) => {
  active.value = index;
  console.time()
  await Promise.resolve()
  console.timeEnd()
}

</script>

<style scoped lang='less'>

</style>
```

**总结**

如果你的项目对性能要求非常严格可以使用，但也只是小部分提升性能，如果你的项目平时没有那么大的数据量，感觉没什么有用

阻止表单提交案例

```cobol
<template>
  <form action="/">
    <button @click.prevent="submit" type="submit">submit</button>
  </form>
</template>
 
 
<script setup lang="ts">
const submit = () => {
  console.log('child');
 
}
 
 
</script>
 
 
 
<style>
</style>
```

v-bind 绑定class 案例 1

```cobol
<template>
  <div :class="[flag ? 'active' : 'other', 'h']">12323</div>
</template>
 
 
<script setup lang="ts">
const flag: boolean = false;
</script>
 
 
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```

v-bind 绑定class 案例 2

```cobol
<template>
  <div :class="flag">{{flag}}</div>
</template>
 
 
<script setup lang="ts">
type Cls = {
  other: boolean,
  h: boolean
}
const flag: Cls = {
  other: false,
  h: true
};
</script>
 
 
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```

v-bind 绑定style案例

```cobol
<template>
  <div :style="style">2222</div>
</template>
 
 
 
<script setup lang="ts">
 
 
type Style = {
  height: string,
  color: string
}
 
const style: Style = {
  height: "300px",
  color: "blue"
}
 
</script>
 
 
<style>
</style>
```

v-model 案例

```cobol
<template>
  <input v-model="message" type="text" />
  <div>{{ message }}</div>
</template>
 
 
<script setup lang="ts">
import { ref } from 'vue'
const message = ref("v-model")
</script>
 
 
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```

## 学习Vue3 第五章（Vue核心虚拟Dom和 diff 算法）

为什么要学习源码

1.可以提升自己学习更优秀的API设计和代码逻辑

2.面试的时候也会经常问源码相关的东西

3.更快的掌握vue和遇到问题可以定位

#### 介绍[虚拟DOM](https://so.csdn.net/so/search?q=虚拟DOM&spm=1001.2101.3001.7020)

虚拟DOM就是通过JS来生成一个[AST](https://so.csdn.net/so/search?q=AST&spm=1001.2101.3001.7020)节点树

![img](https://img-blog.csdnimg.cn/b3fde3141a0740568fdc42c5eb3b23c5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

[Vue3 Template Explorer]: https://vue-next-template-explorer.netlify.app/#eyJzcmMiOiI8ZGl2PlxyXG4gICAgPGRpdj4gXHJcbiAgICAgICAgIDxzZWN0aW9uPnRlc3Q8L3NlY3Rpb24+XHJcbiAgICAgIDwvZGl2PiAgXHJcbjwvZGl2PiIsIm9wdGlvbnMiOnt9fQ==

为什么要有虚拟DOM？

我们可以通过下面的例子

```cobol
let div = document.createElement('div')
let str = ''
for (const key in div) {
  str += key + ''
}
console.log(str)
```

发现一个dom上面的属性是非常多的

```
aligntitlelangtranslatedirhiddenaccessKeydraggablespellcheckautocapitalizecontentEditableisContentEditableinputModeoffsetParentoffsetTopoffsetLeftoffsetWidthoffsetHeightstyleinnerTextouterTextonbeforexrselectonabortonbluroncanceloncanplayoncanplaythroughonchangeonclickoncloseoncontextmenuoncuechangeondblclickondragondragendondragenterondragleaveondragoverondragstartondropondurationchangeonemptiedonendedonerroronfocusonformdataoninputoninvalidonkeydownonkeypressonkeyuponloadonloadeddataonloadedmetadataonloadstartonmousedownonmouseenteronmouseleaveonmousemoveonmouseoutonmouseoveronmouseuponmousewheelonpauseonplayonplayingonprogressonratechangeonresetonresizeonscrollonsecuritypolicyviolationonseekedonseekingonselectonslotchangeonstalledonsubmitonsuspendontimeupdateontoggleonvolumechangeonwaitingonwebkitanimationendonwebkitanimationiterationonwebkitanimationstartonwebkittransitionendonwheelonauxclickongotpointercaptureonlostpointercaptureonpointerdownonpointermoveonpointeruponpointercancelonpointeroveronpointeroutonpointerenteronpointerleaveonselectstartonselectionchangeonanimationendonanimationiterationonanimationstartontransitionrunontransitionstartontransitionendontransitioncanceloncopyoncutonpastedatasetnonceautofocustabIndexattachInternalsblurclickfocusenterKeyHintvirtualKeyboardPolicyonpointerrawupdatenamespaceURIprefixlocalNametagNameidclassNameclassListslotattributesshadowRootpartassignedSlotinnerHTMLouterHTMLscrollTopscrollLeftscrollWidthscrollHeightclientTopclientLeftclientWidthclientHeightattributeStyleMaponbeforecopyonbeforecutonbeforepasteonsearchelementTimingonfullscreenchangeonfullscreenerroronwebkitfullscreenchangeonwebkitfullscreenerrorchildrenfirstElementChildlastElementChildchildElementCountpreviousElementSiblingnextElementSiblingafteranimateappendattachShadowbeforeclosestcomputedStyleMapgetAttributegetAttributeNSgetAttributeNamesgetAttributeNodegetAttributeNodeNSgetBoundingClientRectgetClientRectsgetElementsByClassNamegetElementsByTagNamegetElementsByTagNameNSgetInnerHTMLhasAttributehasAttributeNShasAttributeshasPointerCaptureinsertAdjacentElementinsertAdjacentHTMLinsertAdjacentTextmatchesprependquerySelectorquerySelectorAllreleasePointerCaptureremoveremoveAttributeremoveAttributeNSremoveAttributeNodereplaceChildrenreplaceWithrequestFullscreenrequestPointerLockscrollscrollByscrollIntoViewscrollIntoViewIfNeededscrollTosetAttributesetAttributeNSsetAttributeNodesetAttributeNodeNSsetPointerCapturetoggleAttributewebkitMatchesSelectorwebkitRequestFullScreenwebkitRequestFullscreenariaAtomicariaAutoCompleteariaBusyariaCheckedariaColCountariaColIndexariaColSpanariaCurrentariaDescriptionariaDisabledariaExpandedariaHasPopupariaHiddenariaKeyShortcutsariaLabelariaLevelariaLiveariaModalariaMultiLineariaMultiSelectableariaOrientationariaPlaceholderariaPosInSetariaPressedariaReadOnlyariaRelevantariaRequiredariaRoleDescriptionariaRowCountariaRowIndexariaRowSpanariaSelectedariaSetSizeariaSortariaValueMaxariaValueMinariaValueNowariaValueTextgetAnimationsnodeTypenodeNamebaseURIisConnectedownerDocumentparentNodeparentElementchildNodesfirstChildlastChildpreviousSiblingnextSiblingnodeValuetextContentELEMENT_NODEATTRIBUTE_NODETEXT_NODECDATA_SECTION_NODEENTITY_REFERENCE_NODEENTITY_NODEPROCESSING_INSTRUCTION_NODECOMMENT_NODEDOCUMENT_NODEDOCUMENT_TYPE_NODEDOCUMENT_FRAGMENT_NODENOTATION_NODEDOCUMENT_POSITION_DISCONNECTEDDOCUMENT_POSITION_PRECEDINGDOCUMENT_POSITION_FOLLOWINGDOCUMENT_POSITION_CONTAINSDOCUMENT_POSITION_CONTAINED_BYDOCUMENT_POSITION_IMPLEMENTATION_SPECIFICappendChildcloneNodecompareDocumentPositioncontainsgetRootNodehasChildNodesinsertBeforeisDefaultNamespaceisEqualNodeisSameNodelookupNamespaceURIlookupPrefixnormalizeremoveChildreplaceChildaddEventListenerdispatchEventremoveEventListener
```

所以直接操作DOM非常浪费性能

解决方案就是 我们可以用`JS`的计算性能来换取操作`DOM`所消耗的性能，既然我们逃不掉操作`DOM`这道坎,但是我们可以尽可能少的操作DOM

```
操作JS是非常快的
```

#### 介绍Diff算法

[Vue3](https://so.csdn.net/so/search?q=Vue3&spm=1001.2101.3001.7020) 源码地址[ https://github.com/vuejs/core](https://github.com/vuejs/core)

```
<template>
  <div>
    <div :key="item" v-for="(item) in Arr">{{ item }}</div>
  </div>
</template>
 
 
 
<script setup lang="ts">
const Arr: Array<string> = ['A', 'B', 'C', 'D']
Arr.splice(2,0,'DDD')
</script>
 
 
<style>
</style>
```

![img](https://img-blog.csdnimg.cn/16158a3afbfb4558b094dd4203aab442.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

splice 用法 太贴心了 

![img](https://img-blog.csdnimg.cn/1fe57a274d8644bfacf44526e79d57bc.png)

##	学习Vue3 第六章（认识Ref全家桶）

#### ref

接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象仅有一个 .value property，指向该内部值。

案例

我们这样操作是无法改变message  的值 应为message 不是响应式的无法被vue 跟踪要改成ref
　

```jsx
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>
 
 
 
<script setup lang="ts">
let message: string = "我是message"
 
const changeMsg = () => {
   message = "change msg"
}
</script>
 
 
<style>
</style>
```

改为ref

Ref TS对应的接口

```cobol
interface Ref<T> {



  value: T



}
```

**注意被ref包装之后需要.value 来进行赋值**

####	isRef

判断是不是一个ref对象

```jsx
import { ref, Ref,isRef } from 'vue'
let message: Ref<string | number> = ref("我是message")
let notRef:number = 123
const changeMsg = () => {
  message.value = "change msg"
  console.log(isRef(message)); //true
  console.log(isRef(notRef)); //false

} 
```

#### ref 小妙招

我们console 输出 

![img](https://img-blog.csdnimg.cn/35f5e6f6f97047baa56afc1ca37f5b59.png)

 是个这玩意 查看起来很不方便 Vue 已经想到 了 帮我们做了格式化



![img](https://img-blog.csdnimg.cn/39e9aba85eed4f81a4f8754d00a3455d.png)

![img](https://img-blog.csdnimg.cn/026bf7a841d44473a2b16c205a341620.png)

![img](https://img-blog.csdnimg.cn/2a0dc0cb2f49456a8c0a49669d61d7d5.png)

![img](https://img-blog.csdnimg.cn/4a6410d67e18454aac5985a6c0bf0aed.png)

此时观看打印的值就很明了

#### `shallowRef`

创建一个跟踪自身 `.value` 变化的 ref，但不会使其值也变成响应式的

例子

修改其属性是非响应式的这样是不会改变的

```cobol
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>
 
 
 
<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value.name = '大满'
}
</script>
 
 
<style>
</style>
```

#### triggerRef 

强制更新页面DOM

这样也是可以改变值的

```cobol
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>
 
 
 
<script setup lang="ts">
import { Ref, shallowRef,triggerRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value.name = '大满'
 triggerRef(message)
}
</script> 
 
 
<style>
</style>
```

#### customRef

自定义ref 

customRef 是个工厂函数要求我们返回一个对象 并且实现 get 和 set 适合去做防抖之类的

```cobol
<template>
 
  <div ref="div">小满Ref</div>
  <hr>
  <div>
    {{ name }}
  </div>
  <hr>
  <button @click="change">修改 customRef</button>
 
</template>
 
<script setup lang='ts'>
import { ref, reactive, onMounted, shallowRef, customRef } from 'vue'
 
function myRef<T = any>(value: T) {
  let timer:any;
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newVal) {
        clearTimeout(timer)
        timer =  setTimeout(() => {
          console.log('触发了set')
          value = newVal
          trigger()
        },500)
      }
    }
  })
}
 
 
const name = myRef<string>('小满')
 
 
const change = () => {
  name.value = '大满'
}
 
</script>
<style scoped>
</style>
```

## 学习Vue3 第七章（认识Reactive全家桶）

#### reactive

```
用来绑定复杂的数据类型 例如 对象 数组
```

reactive 源码约束了我们的类型

![img](https://img-blog.csdnimg.cn/4bea460f515a479d82e650c29ee00a99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

`他是不可以绑定普通的数据类型`这样是不允许 会给我们报错

```cobol
import { reactive} from 'vue'
 
let person = reactive('sad')
```

![img](https://img-blog.csdnimg.cn/c6b228aafa654e9c893614116cac764c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

绑定普通的数据类型 我们可以 使用昨天讲到ref

你如果用ref去绑定对象 或者 数组 等复杂的数据类型 我们看源码里面其实也是 去调用reactive

使用reactive 去修改值无须.value

reactive 基础用法

```cobol
import { reactive } from 'vue'
let person = reactive({
   name:"小满"
})
person.name = "大满"
```

数组异步赋值问题

这样赋值页面是不会变化的因为会脱离[响应式](https://so.csdn.net/so/search?q=响应式&spm=1001.2101.3001.7020)

```
let person = reactive<number[]>([])
setTimeout(() => {
  person = [1, 2, 3]
  console.log(person);
  
},1000)
```

解决方案1

使用push

```
import { reactive } from 'vue'
let person = reactive<number[]>([])
setTimeout(() => {
  const arr = [1, 2, 3]
  person.push(...arr)
  console.log(person);
  
},1000)
```

方案2

包裹一层对象

```js
type Person = {
  list?:Array<number>
}
let person = reactive<Person>({
   list:[]
})
setTimeout(() => {
  const arr = [1, 2, 3]
  person.list = arr;
  console.log(person);
  
},1000)
```

#### readonly

拷贝一份proxy对象将其设置为只读

```jsx
import { reactive ,readonly} from 'vue'
const person = reactive({count:1})
const copy = readonly(person)
 
 //person.count++
 
 copy.count++
```

#### shallowReactive 

只能对浅层的数据 如果是深层的数据只会改变值 不会改变视图

案例

```cobol
<template>
  <div>
    <div>{{ state }}</div>
    <button @click="change1">test1</button>
    <button @click="change2">test2</button>
  </div>
</template>
 
 
 
<script setup lang="ts">
import { shallowReactive } from 'vue'
 
 
const obj = {
  a: 1,
  first: {
    b: 2,
    second: {
      c: 3
    }
  }
}
 
const state = shallowReactive(obj) 
function change1() {
  state.a = 7
}
function change2() {
  state.first.b = 8
  state.first.second.c = 9
  console.log(state);
} 
</script> 
<style>
</style>
```

##	学习Vue3 第八章（认识to系列全家桶）

#### toRef toRefs toRaw

#### toRef

如果原始对象是非[响应式](https://so.csdn.net/so/search?q=响应式&spm=1001.2101.3001.7020)的就不会更新视图 数据是会变的

```cobol
<template>
   <div>
      <button @click="change">按钮</button>
      {{state}}
   </div>
</template>
 
<script setup lang="ts">
import { reactive, toRef } from 'vue'
 
const obj = {
   foo: 1,
   bar: 1
}
 
 
const state = toRef(obj, 'bar')
// bar 转化为响应式对象
 
const change = () => {
   state.value++
   console.log(obj, state);
 
}
</script>
```

如果原始对象是响应式的是会更新视图并且改变数据的

####	toRefs
可以帮我们批量创建ref对象主要是方便我们解构使用

```jsx
import { reactive, toRefs } from 'vue'
const obj = reactive({
   foo: 1,
   bar: 1
})
 
let { foo, bar } = toRefs(obj)
 
foo.value++
console.log(foo, bar);
```

####	toRaw

将响应式对象转化为普通对象

```cobol
import { reactive, toRaw } from 'vue'
 
const obj = reactive({
   foo: 1,
   bar: 1
})
 
 
const state = toRaw(obj)
// 响应式对象转化为普通对象
 
const change = () => {
 
   console.log(obj, state);
 
}
```

#### 源码解析 toRef

如果是ref 对象直接返回 否则 调用 ObjectRefImpl 创建一个类ref 对象

```scala
export function toRef<T extends object, K extends keyof T>(
  object: T,
  key: K,
  defaultValue?: T[K]
): ToRef<T[K]> {
  const val = object[key]
  return isRef(val)
    ? val
    : (new ObjectRefImpl(object, key, defaultValue) as any)
}
```

 类ref 对象只是做了值的改变 并未处理 收集依赖 和 触发依赖的过程 所以 普通对象无法更新视图

```scala
class ObjectRefImpl<T extends object, K extends keyof T> {
  public readonly __v_isRef = true
 
  constructor(
    private readonly _object: T,
    private readonly _key: K,
    private readonly _defaultValue?: T[K]
  ) {}
 
  get value() {
    const val = this._object[this._key]
    return val === undefined ? (this._defaultValue as T[K]) : val
  }
 
  set value(newVal) {
    this._object[this._key] = newVal
  }
}
```

[toRefs](https://so.csdn.net/so/search?q=toRefs&spm=1001.2101.3001.7020) 源码解析

其实就是把reactive 对象的每一个属性都变成了ref 对象循环 调用了toRef

```js
export type ToRefs<T = any> = {
  [K in keyof T]: ToRef<T[K]>
}
export function toRefs<T extends object>(object: T): ToRefs<T> {
  if (__DEV__ && !isProxy(object)) {
    console.warn(`toRefs() expects a reactive object but received a plain one.`)
  }
  const ret: any = isArray(object) ? new Array(object.length) : {}
  for (const key in object) {
    ret[key] = toRef(object, key)
  }
  return ret
}
```

toRaw 源码解析

通过 ReactiveFlags 枚举值 取出 proxy 对象的 原始对象

```cobol
export const enum ReactiveFlags {
  SKIP = '__v_skip',
  IS_REACTIVE = '__v_isReactive',
  IS_READONLY = '__v_isReadonly',
  IS_SHALLOW = '__v_isShallow',
  RAW = '__v_raw'
}
 
export function toRaw<T>(observed: T): T {
  const raw = observed && (observed as Target)[ReactiveFlags.RAW]
  return raw ? toRaw(raw) : observed
}
```

## 学习Vue3 第九章（认识computed计算属性）

#### computed用法

[计算属性](https://so.csdn.net/so/search?q=计算属性&spm=1001.2101.3001.7020)就是当依赖的属性的值发生变化的时候，才会触发他的更改，如果依赖的值，不发生变化的时候，使用的是缓存中的属性值。

1 函数形式

```cobol
import { computed, reactive, ref } from 'vue'
let price = ref(0)//$0
 
let m = computed<string>(()=>{
   return `$` + price.value
})
 
price.value = 500
```

2 对象形式

```jsx
<template>
   <div>{{ mul }}</div>
   <div @click="mul = 100">click</div>
</template>
 
<script setup lang="ts">
import { computed, ref } from 'vue'
let price = ref<number | string>(1)//$0
let mul = computed({
   get: () => {
      return price.value
   },
   set: (value) => {
      price.value = 'set' + value
   }
})
</script>
 
<style>
</style>
```

####	computed购物车案例

```jsx
<template>
   <div>
      <table style="width:800px" border>
         <thead>
            <tr>
               <th>名称</th>
               <th>数量</th>
               <th>价格</th>
               <th>操作</th>
            </tr>
         </thead>
         <tbody>
            <tr :key="index" v-for="(item, index) in data">
               <td align="center">{{ item.name }}</td>
               <td align="center">
                  <button @click="AddAnbSub(item, false)">-</button>
                  {{ item.num }}
                  <button @click="AddAnbSub(item, true)">+</button>
               </td>
               <td align="center">{{ item.num * item.price }}</td>
               <td align="center">
                  <button @click="del(index)">删除</button>
               </td>
            </tr>
         </tbody>
         <tfoot>
            <tr>
               <td></td>
               <td></td>
               <td></td>
               <td align="center">总价:{{ $total }}</td>
            </tr>
         </tfoot>
      </table>
   </div>
</template>
 
<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
type Shop = {
   name: string,
   num: number,
   price: number
}
let $total = ref<number>(0)
const data = reactive<Shop[]>([
   {
      name: "小满的袜子",
      num: 1,
      price: 100
   },
   {
      name: "小满的裤子",
      num: 1,
      price: 200
   },
   {
      name: "小满的衣服",
      num: 1,
      price: 300
   },
   {
      name: "小满的毛巾",
      num: 1,
      price: 400
   }
])
 
const AddAnbSub = (item: Shop, type: boolean = false): void => {
   if (item.num > 1 && !type) {
      item.num--
   }
   if (item.num <= 99 && type) {
      item.num++
   }
}
const del = (index: number) => {
   data.splice(index, 1)
}
 
 
 
$total = computed<number>(() => {
   return data.reduce((prev, next) => {
      return prev + (next.num * next.price)
   }, 0)
})
 
 
 
</script>
 
<style>
</style>
```

## 学习Vue3 第十章（认识watch侦听器）

#### watch用法

watch 需要侦听特定的数据源，并在单独的回调函数中执行副作用

watch第一个参数监听源

watch第二个参数回调函数cb（newVal,oldVal）

watch第三个参数一个options配置项是一个对象{

immediate:true //是否立即调用一次

deep:true //是否开启深度监听

}

####	监听Ref 案例

```jsx
import { ref, watch } from 'vue'
 
let message = ref({
    nav:{
        bar:{
            name:""
        }
    }
})
 
 
watch(message, (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
},{
    immediate:true,
    deep:true
})
```

####	监听多个ref 注意变成数组

```cobol
import { ref, watch ,reactive} from 'vue'
 
let message = ref('')
let message2 = ref('')
 
watch([message,message2], (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

监听Reactive

使用reactive监听深层对象开启和不开启deep 效果一样

```jsx
import { ref, watch ,reactive} from 'vue'
 
let message = reactive({
    nav:{
        bar:{
            name:""
        }
    }
})
 
 
watch(message, (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

#### 案例2 监听reactive 单一值

```cobol
import { ref, watch ,reactive} from 'vue'
 
let message = reactive({
    name:"",
    name2:""
})
 
 
watch(()=>message.name, (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

## 学习Vue3 第十一章（认识watchEffect高级侦听器）

####	watchEffect

立即执行传入的一个函数，同时[响应式](https://so.csdn.net/so/search?q=响应式&spm=1001.2101.3001.7020)追踪其依赖，并在其依赖变更时重新运行该函数。

如果用到message 就只会监听message 就是用到几个监听几个 而且是非惰性 会默认调用一次

```cobol
let message = ref<string>('')
let message2 = ref<string>('')
 watchEffect(() => {
    //console.log('message', message.value);
    console.log('message2', message2.value);
})
```

#### 清除副作用

就是在触发监听之前会调用一个函数可以处理你的逻辑例如[防抖](https://so.csdn.net/so/search?q=防抖&spm=1001.2101.3001.7020)

```jsx
import { watchEffect, ref } from 'vue'
let message = ref<string>('')
let message2 = ref<string>('')
 watchEffect((oninvalidate) => {
    //console.log('message', message.value);
    oninvalidate(()=>{
        
    })
    console.log('message2', message2.value);
})
```

#### 停止跟踪 watchEffect 返回一个函数 调用之后将停止更新

```cobol
const stop =  watchEffect((oninvalidate) => {
    //console.log('message', message.value);
    oninvalidate(()=>{
 
    })
    console.log('message2', message2.value);
},{
    flush:"post",
    onTrigger () {
 
    }
})
stop()
```

#### **更多的配置项**

**副作用刷新时机 flush 一般使用post**

|          | pre                | sync                 | post               |
| :------- | :----------------- | :------------------- | :----------------- |
| 更新时机 | 组件**更新前**执行 | 强制效果始终同步触发 | 组件**更新后**执行 |

**onTrigger 可以帮助我们调试 watchEffect**

```cobol
import { watchEffect, ref } from 'vue'
let message = ref<string>('')
let message2 = ref<string>('')
 watchEffect((oninvalidate) => {
    //console.log('message', message.value);
    oninvalidate(()=>{
 
    })
    console.log('message2', message2.value);
},{
    flush:"post",
    onTrigger () {
        
    }
})
```



## 学习Vue3 第十二章（认识组件&Vue3生命周期）

####  组件基础

每一个.vue 文件呢都可以充当组件来使用

每一个组件都可以复用

![img](https://img-blog.csdnimg.cn/d71e48ec34ef47e4a59720f54f9ef13a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

例如 helloWorld 充当子组件

![img](https://img-blog.csdnimg.cn/3afb80a0e75444669636a67488b4d4eb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

 父组件使用

引入子组件 helloWorld 然后直接就可以去当标签去使用 （切记组件名称不能与html元素标签名称一样）

![img](https://img-blog.csdnimg.cn/72459aa0048c47f88c487b37bf53d3e8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

####	组件的生命周期

简单来说就是一个组件从创建 到 销毁的 过程 成为生命周期

在我们使用[Vue3](https://so.csdn.net/so/search?q=Vue3&spm=1001.2101.3001.7020) 组合式API 是没有 `beforeCreate 和 created 这两个生命周期的`

![img](https://img-blog.csdnimg.cn/2dc0048de8824543aa60d9e4e4e656a3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

**onBeforeMount**()

在组件DOM实际渲染安装之前调用。在这一步中，根元素还不存在。

**onMounted**()

在组件的第一次渲染后调用，该元素现在可用，允许直接DOM访问

**onBeforeUpdate**()

数据更新时调用，发生在虚拟 DOM 打补丁之前。

**onUpdated**()

DOM更新后，updated的方法即会调用。

**onBeforeUnmount**()

在卸载组件实例之前调用。在这个阶段，实例仍然是完全正常的。

**onUnmounted**()
卸载组件实例后调用。调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载.

| 选项式 API      | Hook inside `setup` |
| --------------- | ------------------- |
| beforeCreate    | Not needed*         |
| Created         | Not needed*         |
| beforeMount     | onBeforeMount       |
| Mounted         | onMounted           |
| Beforeupdate    | `onBeforeUpdate`    |
| Updated         | onUpdated           |
| Beforeunmount   | onBeforeUnmount     |
| Unmounted       | onUnmounted         |
| errorcaptured   | onErrorCaptured     |
| renderTracked   | onRenderTracked     |
| renderTriggered | onRenderTriggered   |
| activated       | onActivated         |
| deactivated     | onDeactivated       |

##	学习Vue3 第十三章（实操组件和认识less 和 scoped）

#### 概览
什么是less

**Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。这里呈现的是 Less 的官方文档（中文版），包含了 Less 语言以及利用 JavaScript 开发的用于将 Less 样式转换成 CSS 样式的 Less.js 工具。**

因为 Less 和 CSS 非常像，因此很容易学习。而且 Less 仅对 CSS 语言增加了少许方便的扩展，这就是 Less 如此易学的原因之一。

[Less官方文档]: https://less.bootcss.com/#%E6%A6%82%E8%A7%88

在vite中使用less

npm install less less-loader -D 安装即可

在style标签注明即可

```
<style lang="less">
 
</style>
```

####	什么是scoped

实现组件的私有化, 当前style属性只属于当前模块.

　在DOM结构中可以发现,vue通过在DOM结构以及css样式上加了唯一标记,达到样式私有化,不污染全局的作用,

![img](https://img-blog.csdnimg.cn/7d9cadcadd594044843cc225efabca6a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)



## 学习Vue3 第十四章（父子组件传参）

#### 正向传值

父组件通过[v-bind](https://so.csdn.net/so/search?q=v-bind&spm=1001.2101.3001.7020)绑定一个数据，然后子组件通过defineProps接受传过来的值，

##### 父组件传递如以下代码

**给Menu组件 传递了一个title 字符串类型是不需要v-bind**

```cobol
<template>
    <div class="layout">
        <Menu  title="我是标题"></Menu>
        <div class="layout-right">
            <Header></Header>
            <Content></Content>
        </div>
    </div>
</template>
```

**传递非字符串类型需要加v-bind 简写 冒号**

```cobol
<template>
    <div class="layout">
        <Menu v-bind:data="data"  title="我是标题"></Menu>
        <div class="layout-right">
            <Header></Header>
            <Content></Content>
        </div>
    </div>
</template>
 
<script setup lang="ts">
import Menu from './Menu/index.vue'
import Header from './Header/index.vue'
import Content from './Content/index.vue'
import { reactive } from 'vue';
 
const data = reactive<number[]>([1, 2, 3])
</script>
```

##### 子组件接受值

通过[defineProps](https://so.csdn.net/so/search?q=defineProps&spm=1001.2101.3001.7020) 来接受 **defineProps是无须引入的直接使用即可**

如果我们使用的[TypeScript](https://so.csdn.net/so/search?q=TypeScript&spm=1001.2101.3001.7020)

可以使用传递字面量类型的纯类型语法做为参数

如 这是TS特有的

```cobol
<template>
    <div class="menu">
        菜单区域 {{ title }}
        <div>{{ data }}</div>
    </div>
</template>
 
<script setup lang="ts">
defineProps<{
    title:string,
    data:number[]
}>()
</script>
```

如果你使用的不是TS

```cobol
defineProps({
    title:{
        default:"",
        type:string
    },
    data:Array
})
```

**TS 特有的默认值方式**

**withDefaults是个函数也是无须引入开箱即用接受一个props函数第二个参数是一个对象设置默认值**

#### 逆向传值

##### 子传父方法

```cobol
<template>
    <div class="menu">
        <button @click="clickTap">派发给父组件</button>
    </div>
</template>
 
<script setup lang="ts">
import { reactive } from 'vue'
const list = reactive<number[]>([4, 5, 6])
 
const emit = defineEmits(['on-click'])
 
//如果用了ts可以这样两种方式
// const emit = defineEmits<{
//     (e: "on-click", name: string): void
// }>()
const clickTap = () => {
    emit('on-click', list)
}
 
</script>
```

我们在子组件绑定了一个click 事件 然后通过defineEmits 注册了一个自定义事件

点击click 触发 emit 去调用我们注册的事件 然后传递参数

##### **父组件接受子组件的事件**

```cobol
<template>
    <div class="layout">
        <Menu @on-click="getList"></Menu>
        <div class="layout-right">
            <Header></Header>
            <Content></Content>
        </div>
    </div>
</template>
 
<script setup lang="ts">
import Menu from './Menu/index.vue'
import Header from './Header/index.vue'
import Content from './Content/index.vue'
import { reactive } from 'vue';
 
const data = reactive<number[]>([1, 2, 3])
 
const getList = (list: number[]) => {
    console.log(list,'父组件接受子组件');
}
</script>
```

我们从Menu 组件接受子组件派发的事件on-click 后面是我们自己定义的函数名称getList

会把参数返回过来

子组件暴露给父组件内部属性

通过defineExpose

我们从父组件获取子组件实例通过ref

```jsx
 <Menu ref="refMenu"></Menu>
//这样获取是有代码提示的
<script setup lang="ts">
import MenuCom from '../xxxxxxx.vue'
//注意这儿的typeof里面放的是组件名字(MenuCom)不是ref的名字 ref的名字对应开头的变量名(refMenu)
const refMenu = ref<InstanceType<typeof MenuCom>>()
</script>
```

然后打印menus.value 发现没有任何属性

这时候父组件想要读到子组件的属性可以通过 defineExpose暴露

```cobol
const list = reactive<number[]>([4, 5, 6])
 
defineExpose({
    list
})
```

这样父组件就可以读到了

####	案例封装瀑布流组件

父组件

```jsx
<template>
    <waterFallVue :list="list"></waterFallVue>
</template>
 
<script setup lang='ts'>
import { ref, reactive } from 'vue'
import waterFallVue from './components/water-fall.vue';
const list = [
    {
        height: 300,
        background: 'red'
    },
    {
        height: 400,
        background: 'pink'
    },
    {
        height: 500,
        background: 'blue'
    },
    {
        height: 200,
        background: 'green'
    },
    {
        height: 300,
        background: 'gray'
    },
    {
        height: 400,
        background: '#CC00FF'
    },
    {
        height: 200,
        background: 'black'
    },
    {
        height: 100,
        background: '#996666'
    },
    {
        height: 500,
        background: 'skyblue'
    },
    {
        height: 300,
        background: '#993366'
    },
    {
        height: 100,
        background: '#33FF33'
    },
    {
        height: 400,
        background: 'skyblue'
    },
    {
        height: 200,
        background: '#6633CC'
    },
    {
        height: 300,
        background: '#666699'
    },
    {
        height: 300,
        background: '#66CCFF'
    },
    {
        height: 300,
        background: 'skyblue'
    },
    {
        height: 200,
        background: '#CC3366'
    },
    {
        height: 200,
        background: '#CC9966'
    },
    {
        height: 200,
        background: '#FF00FF'
    },
    {
        height: 500,
        background: '#990000'
    },
    {
        height: 400,
        background: 'red'
    },
    {
        height: 100,
        background: '#999966'
    },
    {
        height: 200,
        background: '#CCCC66'
    },
    {
        height: 300,
        background: '#FF33FF'
    },
    {
        height: 400,
        background: '#FFFF66'
    },
    {
        height: 200,
        background: 'red'
    },
    {
        height: 100,
        background: 'skyblue'
    },
    {
        height: 200,
        background: '#33CC00'
    },
    {
        height: 300,
        background: '#330033'
    },
    {
        height: 100,
        background: '#0066CC'
    },
    {
        height: 200,
        background: 'skyblue'
    },
    {
        height: 100,
        background: '#006666'
    },
    {
        height: 200,
        background: 'yellow'
    },
    {
        height: 300,
        background: 'yellow'
    },
    {
        height: 100,
        background: '#33CCFF'
    },
    {
        height: 400,
        background: 'yellow'
    },
    {
        height: 400,
        background: 'yellow'
    },
    {
        height: 200,
        background: '#33FF00'
    },
    {
        height: 300,
        background: 'yellow'
    },
    {
        height: 100,
        background: 'green'
    }
 
]
</script>
 
<style  lang='less'>
#app,
html,
body {
    height: 100%;
}
 
* {
    padding: 0;
    margin: 0;
}
</style>
```

 子组件

```cobol
<template>
    <div class="wraps">
        <div :style="{height:item.height+'px',background:item.background,top:item.top+'px',left:item.left + 'px'}"
            v-for="item in waterList" class="items"></div>
    </div>
</template>
 
<script setup lang='ts'>
import { ref, reactive, onMounted } from 'vue'
const props = defineProps<{
    list: any[]
}>()
const waterList = reactive<any[]>([])
const init = () => {
    const heightList: any[] = []
    const width = 130;
    const x = document.body.clientWidth
    const column = Math.floor(x / width)
 
    for (let i = 0; i < props.list.length; i++) {
        if (i < column) {
            props.list[i].top = 10;
            props.list[i].left = i * width;
            heightList.push(props.list[i].height + 10)
            waterList.push(props.list[i])
        } else {
            let current = heightList[0]
            let index = 0;
            heightList.forEach((h, inx) => {
                if (current > h) {
                    current = h;
                    index = inx;
                }
            })
            console.log(current,'c')
            props.list[i].top = (current + 20);
            console.log(props.list[i].top,'top',i)
            props.list[i].left = index * width;
            heightList[index] =  (heightList[index] + props.list[i].height + 20);
            waterList.push(props.list[i])
        
        }
    }
    console.log(props.list)
}
 
onMounted(() => {
    window.onresize = () => init()
    init()
})
 
</script>
 
<style scoped lang='less'>
.wraps {
    position: relative;
     height: 100%;
    .items {
        position: absolute;
        width: 120px;
    }
}
</style>
```

![img](https://img-blog.csdnimg.cn/a1528d99952c4af68e3ee04d113af82a.png)





## 学习Vue3 第十五章（全局组件，局部组件，递归组件）

#### 配置全局组件

例如组件使用频率非常高（table，Input，button，等）这些组件 几乎每个页面都在使用便可以封装成全局组件

案例------我这儿封装一个Card组件想在任何地方去使用

```cobol
<template>
  <div class="card">
     <div class="card-header">
         <div>标题</div>
         <div>副标题</div>
     </div>
     <div v-if='content' class="card-content">
         {{content}}
     </div>
  </div>
</template>
 
<script setup lang="ts">
type Props = {
    content:string
}
defineProps<Props>()
 
</script>
 
<style scoped lang='less'>
@border:#ccc;
.card{
    width: 300px;
    border: 1px solid @border;
    border-radius: 3px;
    &:hover{
        box-shadow:0 0 10px @border;
    }
 
    &-content{
        padding: 10px;
    }
    &-header{
        display: flex;
        justify-content: space-between;
        padding: 10px;
        border-bottom: 1px solid @border;
    }
}
</style>
```

![img](https://img-blog.csdnimg.cn/62c1e614b98b4547a261c42145aed662.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

使用方法

在main.ts 引入我们的组件跟随在createApp(App) 后面 切记不能放到mount 后面这是一个链式调用用

其次调用 component 第一个参数组件名称 第二个参数组件实例

```cobol
import { createApp } from 'vue'
import App from './App.vue'
import './assets/css/reset/index.less'
import Card from './components/Card/index.vue'
 
 
createApp(App).component('Card',Card).mount('#app')
```

使用方法

直接在其他vue页面 立即使用即可 无需引入

```cobol
<template>
 <Card></Card>
</template>
```

#### 批量注册全局组件

可以参考element ui 其实就是遍历一下然后通过 app.component 注册

![img](https://img-blog.csdnimg.cn/5b4de39d5f8b48c38f751d510b4f1f61.png)

#### 配置局部组件

```cobol
<template>
  <div class="wraps">
    <layout-menu :flag="flag" @on-click="getMenu" @on-toogle="getMenuItem" :data="menuList" class="wraps-left"></layout-menu>
    <div class="wraps-right">
      <layout-header> </layout-header>
      <layout-main class="wraps-right-main"></layout-main>
    </div>
  </div>
</template>
 
<script setup lang="ts">
import { reactive,ref } from "vue";
import layoutHeader from "./Header.vue";
import layoutMenu from "./Menu.vue";
import layoutMain from "./Content.vue";
```

就是在一个组件内（A） 通过import 去引入别的组件(B) 称之为局部组件

应为B组件只能在A组件内使用 所以是局部组件

如果C组件想用B组件 就需要C组件也手动import 引入 B 组件

#### 配置递归组件

原理跟我们写[js递归](https://so.csdn.net/so/search?q=js递归&spm=1001.2101.3001.7020)是一样的 自己调用自己 通过一个条件来结束递归 否则导致内存泄漏

案例递归树

在父组件配置数据结构 数组对象格式 传给子组件

```cobol
type TreeList = {
  name: string;
  icon?: string;
  children?: TreeList[] | [];
};
const data = reactive<TreeList[]>([
  {
    name: "no.1",
    children: [
      {
        name: "no.1-1",
        children: [
          {
            name: "no.1-1-1",
          },
        ],
      },
    ],
  },
  {
    name: "no.2",
    children: [
      {
        name: "no.2-1",
      },
    ],
  },
  {
    name: "no.3",
  },
]);
```

子组件接收值 第一个script

```jsx
type TreeList = {
  name: string;
  icon?: string;
  children?: TreeList[] | [];
};
 
type Props<T> = {
  data?: T[] | [];
};
 
defineProps<Props<TreeList>>();
const clickItem = (item: TreeList) => {
  console.log(item)
}
```

#### 子组件增加一个script 定义组件名称为了 递归用 给我们的组件定义名称有好几种方式

##### 1.在增加一个script 通过 export 添加name

```cobol
<script lang="ts">
export default {
  name:"TreeItem"
}
</script>
```

##### 2.直接使用文件名当组件名

![img](https://img-blog.csdnimg.cn/42572d5f39b94ccca67608ea8b0b8d6a.png)

##### 3.使用插件 

 unplugin-vue-define-options

```jsx
import DefineOptions from 'unplugin-vue-define-options/vite'
import Vue from '@vitejs/plugin-vue'
 
export default defineConfig({
  plugins: [Vue(), DefineOptions()],
})
```

ts支持

```js
 "types": ["unplugin-vue-define-options/macros-global"],
```

![img](https://img-blog.csdnimg.cn/703a8f3167e8448eb0fc1616884dee1a.png)

template 

TreeItem 其实就是当前组件 通过import 把自身又引入了一遍 如果他没有children 了就结束

```cobol
  <div style="margin-left:10px;" class="tree">
    <div :key="index" v-for="(item,index) in data">
      <div @click='clickItem(item)'>{{item.name}}
    </div>
    <TreeItem @on-click='clickItem' v-if='item?.children?.length' :data="item.children"></TreeItem>
  </div>
  </div>
```

![img](https://img-blog.csdnimg.cn/e70cd74130e84cd2a6a4be220ca2a312.png)

## 学习Vue3 第十六章（动态组件）

什么是动态组件 就是：让多个组件使用同一个挂载点，并动态切换，这就是动态组件。

在挂载点使用[component](https://so.csdn.net/so/search?q=component&spm=1001.2101.3001.7020)标签，然后使用[v-bind](https://so.csdn.net/so/search?q=v-bind&spm=1001.2101.3001.7020):is=”组件”

用法如下

引入组件

```jsx
import A from './A.vue'
import B from './B.vue'
```

```jsx
  <component :is="A"></component>
```

通过is 切换 A B 组件

使用场景

tab切换 居多

**注意事项** 

**1.在Vue2 的时候is 是通过组件名称切换的 在Vue3 setup 是通过组件实例切换的**

**2.如果你把组件实例放到Reactive Vue会给你一个警告runtime-core.esm-bundler.js:38 [Vue warn]: Vue received a Component which was made a reactive object. This can lead to unnecessary performance overhead, and should be avoided by marking the component with `markRaw` or using `shallowRef` instead of `ref`.** 
**Component that was made reactive:** 

**这是因为reactive 会进行proxy 代理 而我们组件代理之后毫无用处 节省性能开销 推荐我们使用shallowRef 或者  markRaw 跳过proxy 代理**

**修改如下**

```jsx
const tab = reactive<Com[]>([{
    name: "A组件",
    comName: markRaw(A)
}, {
    name: "B组件",
    comName: markRaw(B)
}])
```

## 学习Vue3 第十七章（插槽slot）

插槽就是子组件中的提供给父组件使用的一个[占位符](https://so.csdn.net/so/search?q=占位符&spm=1001.2101.3001.7020)，用<slot></slot> 表示，父组件可以在这个占位符中填充任何模板代码，如 HTML、组件等，填充的内容会替换子组件的<slot></slot>标签。

#### 匿名插槽

1.在子组件放置一个插槽

```jsx
<template>
    <div>
       <slot></slot>
    </div>
</template>
```

父组件使用插槽

在父组件给这个插槽填充内容

```cobol
        <Dialog>
           <template v-slot>
               <div>2132</div>
           </template>
        </Dialog>
```

#### [具名插槽](https://so.csdn.net/so/search?q=具名插槽&spm=1001.2101.3001.7020)·

具名插槽其实就是给插槽取个名字。一个子组件可以放多个插槽，而且可以放在不同的地方，而父组件填充内容时，可以根据这个名字把内容填充到对应插槽中

```cobol
    <div>
        <slot name="header"></slot>
        <slot></slot>
 
        <slot name="footer"></slot>
    </div>
```

父组件使用需对应名称

```cobol
        <Dialog>
            <template v-slot:header>
               <div>1</div>
           </template>
           <template v-slot>
               <div>2</div>
           </template>
           <template v-slot:footer>
               <div>3</div>
           </template>
        </Dialog>
```

 插槽简写

```cobol
        <Dialog>
            <template #header>
               <div>1</div>
           </template>
           <template #default>
               <div>2</div>
           </template>
           <template #footer>
               <div>3</div>
           </template>
        </Dialog>
```

#### 作用域插槽

在子组件[动态绑定](https://so.csdn.net/so/search?q=动态绑定&spm=1001.2101.3001.7020)参数 派发给父组件的slot去使用

```cobol
    <div>
        <slot name="header"></slot>
        <div>
            <div v-for="item in 100">
                <slot :data="item"></slot>
            </div>
        </div>
 
        <slot name="footer"></slot>
    </div>
```

通过结构方式取值

```cobol
         <Dialog>
            <template #header>
                <div>1</div>
            </template>
            <template #default="{ data }">
                <div>{{ data }}</div>
            </template>
            <template #footer>
                <div>3</div>
            </template>
        </Dialog>
```

#### 动态插槽

插槽可以是一个变量名

```cobol
        <Dialog>
            <template #[name]>
                <div>
                    23
                </div>
            </template>
        </Dialog>
```

```
const name = ref('header')
```

## 学习Vue3 第十八章（异步组件&代码分包&suspense）

这一章节建议观看视频教程Vue3 + vite + Ts + pinia + 实战 + 源码_哔哩哔哩_bilibili

#### 异步组件

在大型应用中，我们可能需要将应用分割成小一些的代码块 并且减少主包的体积

这时候就可以使用异步组件

#### 顶层 await

在setup语法糖里面 使用方法

<script setup> 中可以使用顶层 await。结果代码会被编译成 async setup()

```cobol
<script setup>
const post = await fetch(`/api/post/1`).then(r => r.json())
</script>
```

父组件引用子组件 通过defineAsyncComponent加载异步配合import 函数模式便可以分包

```cobol
<script setup lang="ts">
import { reactive, ref, markRaw, toRaw, defineAsyncComponent } from 'vue'
 
const Dialog = defineAsyncComponent(() => import('../../components/Dialog/index.vue'))
 
//完整写法
 
const AsyncComp = defineAsyncComponent({
  // 加载函数
  loader: () => import('./Foo.vue'),
 
  // 加载异步组件时使用的组件
  loadingComponent: LoadingComponent,
  // 展示加载组件前的延迟时间，默认为 200ms
  delay: 200,
 
  // 加载失败后展示的组件
  errorComponent: ErrorComponent,
  // 如果提供了一个 timeout 时间限制，并超时了
  // 也会显示这里配置的报错组件，默认值是：Infinity
  timeout: 3000
})
```

#### suspense

`<suspense>` 组件有两个插槽。它们都只接收一个直接子节点。`default` 插槽里的节点会尽可能展示出来。如果不能，则展示 `fallback` 插槽里的节点。

```cobol
     <Suspense>
            <template #default>
                <Dialog>
                    <template #default>
                        <div>我在哪儿</div>
                    </template>
                </Dialog>
            </template>
 
            <template #fallback>
                <div>loading...</div>
            </template>
        </Suspense>
```

## 学习Vue3 第十九章（Teleport传送组件）

Teleport Vue 3.0新特性之一。

Teleport 是一种能够将我们的模板渲染至指定DOM节点，不受父级style、v-show等属性影响，但data、prop数据依旧能够共用的技术；类似于 React 的 Portal。

主要解决的问题 因为Teleport节点挂载在其他指定的DOM节点下，完全不受父级style样式影响

#### 使用方法

通过to 属性 插入指定元素位置 to="body" 便可以将Teleport 内容传送到指定位置

```jsx
<Teleport to="body">
    <Loading></Loading>
</Teleport>
```

也可以自定义传送位置 支持 class id等 选择器

```cobol
    <div id="app"></div>
    <div class="modal"></div>
```

```jsx
<template>
 
    <div class="dialog">
        <header class="header">
            <div>我是弹框</div>
            <el-icon>
                <CloseBold />
            </el-icon>
        </header>
        <main class="main">
            我是内容12321321321
        </main>
        <footer class="footer">
            <el-button size="small">取消</el-button>
            <el-button size="small" type="primary">确定</el-button>
        </footer>
    </div>
 
</template>
 
<script setup lang='ts'>
import { ref, reactive } from 'vue'
 
</script>
<style lang="less" scoped>
.dialog {
    width: 400px;
    height: 400px;
    background: #141414;
    display: flex;
    flex-direction: column;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -200px;
    margin-top: -200px;
 
    .header {
        display: flex;
        color: #CFD3DC;
        border-bottom: 1px solid #636466;
        padding: 10px;
        justify-content: space-between;
    }
 
    .main {
        flex: 1;
        color: #CFD3DC;
        padding: 10px;
    }
 
    .footer {
        border-top: 1px solid #636466;
        padding: 10px;
        display: flex;
        justify-content: flex-end;
    }
}
</style>
```

![img](https://img-blog.csdnimg.cn/55de4fed983546d4946595c59dbbfb80.png)

#### 多个使用场景

```cobol
<Teleport to=".modal1">
     <Loading></Loading>
</Teleport>
<Teleport to=".modal2">
     <Loading></Loading>
</Teleport>
```

#### 动态控制teleport

使用disabled 设置为 true则 to属性不生效 false 则生效

```cobol
    <teleport :disabled="true" to='body'>
      <A></A>
    </teleport>
```

#### 源码解析

在创建teleport 组件的时候会经过patch 方法 然后调用teleport 的process 方法

![img](https://img-blog.csdnimg.cn/149a7a7d23fa4ab4b1f55e972e6d6998.png)

 主要是创建 更新 和删除的逻辑

![img](https://img-blog.csdnimg.cn/5730b33d5b3145ad883a3634c92f53bd.png)

 他通过 resolveTarget 函数 获取了props.to 和 querySelect 获取 了目标元素

然后判断是否有disabled 如果有则 to 属性不生效 否则 挂载新的位置

![img](https://img-blog.csdnimg.cn/05904a4de04141868e31c64083ad5a9e.png)

 新节点disabled 为 true  旧节点disabled false 就把子节点移动回容器

如果新节点disabled 为 false 旧节点为true 就把子节点移动到目标元素

![img](https://img-blog.csdnimg.cn/a9723354a6ac493a9dea28601f1b084b.png)

 遍历teleport 子节点进行unmount方法去移除 

![img](https://img-blog.csdnimg.cn/6bb80aa3efe5451089f80e3a1341e3f2.png)



## 学习Vue3 第二十章（keep-alive缓存组件）

#### 内置组件keep-alive

有时候我们不希望组件被重新渲染影响使用体验；或者处于性能考虑，避免多次重复渲染降低性能。而是希望组件可以缓存下来,维持当前的状态。这时候就需要用到keep-alive组件。

开启**keep-alive** 生命周期的变化

- 初次进入时： onMounted> onActivated

- 退出后触发 deactivated
- 再次进入：
- 只会触发 onActivated
- 事件挂载的方法等，只执行一次的放在 onMounted中；组件每次进去执行的方法放在 onActivated中

```jsx
<!-- 基本 -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>
 
<!-- 多个条件判断的子组件 -->
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>
 
<!-- 和 `<transition>` 一起使用 -->
<transition>
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
</transition>
```

#### **`include` 和 `exclude`**

```jsx
 <keep-alive :include="" :exclude="" :max=""></keep-alive>

```

include 和 exclude 允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示：

#### **`max`**

```jsx
<keep-alive :max="10">
  <component :is="view"></component>
</keep-alive>
```

#### keep-alive 源码讲解

源码目录runtime-core/src/components/[KeepAlive](https://so.csdn.net/so/search?q=KeepAlive&spm=1001.2101.3001.7020).ts

##### props

```tsx
 props: {
    include: [String, RegExp, Array], // 配置了该属性，那么只有名称匹配的组件会被缓存
    exclude: [String, RegExp, Array], // 配置了该属性，那么任何名称匹配的组件都不会被缓存
    max: [String, Number]// 最多可以缓存多少组件实例
  },
 
  setup(props: KeepAliveProps, { slots }: SetupContext) {
    const instance = getCurrentInstance()!
    const sharedContext = instance.ctx as KeepAliveContext
  })
```


```tsx
  const { include, exclude, max } = props
    //如果include 子组件名称 不包含， 和  exclude包含的名字 就不该缓存 直接返回
  if (
    (include && (!name || !matches(include, name))) ||
    (exclude && name && matches(exclude, name))
  ) {
    current = vnode
    return rawVNode
  }
```

```tsx
    watch(
      () => [props.include, props.exclude],
      ([include, exclude]) => {
        //props 是响应式的 所以这个操作需要在做一遍
        include && pruneCache(name => matches(include, name))
        exclude && pruneCache(name => !matches(exclude, name))
      },
      // prune post-render after `current` has been updated
      { flush: 'post', deep: true }
    )
```

#### 缓存设计

```cobol
    //KeepAlive 组件的缓存管理
    const cache: Cache = new Map()
    const keys: Keys = new Set()
```

```tsx
    let pendingCacheKey: CacheKey | null = null
    //缓存函数
    const cacheSubtree = () => {
      // fix #1621, the pendingCacheKey could be 0
      if (pendingCacheKey != null) {
        cache.set(pendingCacheKey, getInnerChild(instance.subTree))
      }
    }
 
    //KeepLive 组件中对缓存的管理时，首先会在组件的 onMounted 或 onUpdated 生命周期钩子函数中设置缓存，如下代码所示：
    onMounted(cacheSubtree)
    onUpdated(cacheSubtree)
```

```tsx
   //Vnode 的key 作为缓存的key
      pendingCacheKey = key
      //KeepLive组件返回的函数中根据 vnode 对象的 key 去缓存中查找是否有缓存的组件，
       //如果缓存存在，则继承组件实例，并将用于描述组件的 vnode 对象标记为 COMPONENT_KEPT_ALIVE，
       //这样渲染器就不会重新创建新的组件实例；如果缓存不存在，则将 vnode 对象的key 添加到 keys 集合中
      if (cachedVNode) {
        // 如果缓存中存在缓存的组件
        vnode.el = cachedVNode.el
        vnode.component = cachedVNode.component
        //无须创建组件实例，继承缓存的组件即可
        if (vnode.transition) {
          // 如果组件上有动画，处理动画
          setTransitionHooks(vnode, vnode.transition!)
        }
        // 标记Vnode 不会重新渲染
        vnode.shapeFlag |= ShapeFlags.COMPONENT_KEPT_ALIVE
        // make this key the freshest
        keys.delete(key)
        keys.add(key)
      } else {
        //将 vnode 的key 添加到 keys 集合中，keys 集合用户维护缓存组件的 key
        keys.add(key)
        // prune oldest entry
        if (max && keys.size > parseInt(max as string, 10)) {
          pruneCacheEntry(keys.values().next().value)
        }

```

#### 生命周期

```tsx
   //隐藏容器
    const storageContainer = createElement('div')
    //在实例上注册两个钩子函数 activate，  deactivate
    sharedContext.activate = (vnode, container, anchor, isSVG, optimized) => {
      const instance = vnode.component!
      move(vnode, container, anchor, MoveType.ENTER, parentSuspense)
      // props 可能会发生变化，因此需要执行 patch 过程
      patch(
        instance.vnode,
        vnode,
        container,
        anchor,
        instance,
        parentSuspense,
        isSVG,
        vnode.slotScopeIds,
        optimized
      )
      queuePostRenderEffect(() => {
        instance.isDeactivated = false
        if (instance.a) {
          invokeArrayFns(instance.a)
        }
        const vnodeHook = vnode.props && vnode.props.onVnodeMounted
        if (vnodeHook) {
          invokeVNodeHook(vnodeHook, instance.parent, vnode)
        }
      }, parentSuspense)
 
      if (__DEV__ || __FEATURE_PROD_DEVTOOLS__) {
        // Update components tree
        devtoolsComponentAdded(instance)
      }
    }
 
    sharedContext.deactivate = (vnode: VNode) => {
      const instance = vnode.component!
      //将组件移动到隐藏容器中
      //在 “卸载” 组件时，并不是真正的卸载，而是调用 move 方法，将组件搬运到一个隐藏的容器中
      move(vnode, storageContainer, null, MoveType.LEAVE, parentSuspense)
      queuePostRenderEffect(() => {
        if (instance.da) {
          invokeArrayFns(instance.da)
        }
        const vnodeHook = vnode.props && vnode.props.onVnodeUnmounted
        if (vnodeHook) {
          invokeVNodeHook(vnodeHook, instance.parent, vnode)
        }
        instance.isDeactivated = true
      }, parentSuspense)
 
      if (__DEV__ || __FEATURE_PROD_DEVTOOLS__) {
        // Update components tree
        devtoolsComponentAdded(instance)
      }
    }
```



## 学习Vue3 第二十一章（transition动画组件）

 Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡:

- 条件渲染 (使用 v-if)

- 条件展示 (使用 v-show)
- 动态组件
- 组件根节点

自定义 transition 过度效果，你需要对transition组件的name属性自定义。并在css中写入对应的样式

#### 1.过渡的类名

在进入/离开的过渡中，会有 6 个 class 切换。

1. **过渡 class**
   在进入/离开的过渡中，会有 6 个 class 切换。
2. v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

3. v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

4. v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/动画完成之后移除。

5. v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

6. v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

7. v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被移除)，在过渡/动画完成之后移除。


如下

```vue
       <button @click='flag = !flag'>切换</button>
       <transition name='fade'>
         <div v-if='flag' class="box"></div>
       </transition>
```

```css
//开始过度
.fade-enter-from{
   background:red;
   width:0px;
   height:0px;
   transform:rotate(360deg)
}
//开始过度了
.fade-enter-active{
  transition: all 2.5s linear;
}
//过度完成
.fade-enter-to{
   background:yellow;
   width:200px;
   height:200px;
}
//离开的过度
.fade-leave-from{
  width:200px;
  height:200px;
  transform:rotate(360deg)
}
//离开中过度
.fade-leave-active{
  transition: all 1s linear;
}
//离开完成
.fade-leave-to{
  width:0px;
   height:0px;
}
```

小满zs

已于 2022-02-22 00:05:27 修改

6451
 收藏 21
分类专栏： Vue3 文章标签： 学习 动画 vue.js
版权

Vue3
专栏收录该内容
49 篇文章2001 订阅
已订阅
 Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡:

条件渲染 (使用 v-if)
条件展示 (使用 v-show)
动态组件
组件根节点
自定义 transition 过度效果，你需要对transition组件的name属性自定义。并在css中写入对应的样式

1.过渡的类名
在进入/离开的过渡中，会有 6 个 class 切换。

#过渡 class
在进入/离开的过渡中，会有 6 个 class 切换。

v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/动画完成之后移除。

v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被移除)，在过渡/动画完成之后移除。

如下

       <button @click='flag = !flag'>切换</button>
       <transition name='fade'>
         <div v-if='flag' class="box"></div>
       </transition>
```css
//开始过度
.fade-enter-from{
   background:red;
   width:0px;
   height:0px;
   transform:rotate(360deg)
}
//开始过度了
.fade-enter-active{
  transition: all 2.5s linear;
}
//过度完成
.fade-enter-to{
   background:yellow;
   width:200px;
   height:200px;
}
//离开的过度
.fade-leave-from{
  width:200px;
  height:200px;
  transform:rotate(360deg)
}
//离开中过度
.fade-leave-active{
  transition: all 1s linear;
}
//离开完成
.fade-leave-to{
  width:0px;
   height:0px;
}
```

#### 2.自定义过渡 class 类名

trasnsition props

- enter-from-class
- enter-active-class
- enter-to-class
- leave-from-class
- leave-active-class
- leave-to-class

自定义过度时间 单位毫秒

你也可以分别指定进入和离开的持续时间：

```vue
<transition :duration="1000">...</transition>
 
 
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

通过自定义class 结合css动画库animate css

安装库 npm install animate.css

引入 import 'animate.css'

使用方法

[官方文档 Animate.css | A cross-browser library of CSS animations.]: https://animate.style/

```vue
        <transition
            leave-active-class="animate__animated animate__bounceInLeft"
            enter-active-class="animate__animated animate__bounceInRight"
        >
            <div v-if="flag" class="box"></div>
        </transition>
```

#### 3.transition 生命周期8个

```tsx
  @before-enter="beforeEnter" //对应enter-from
  @enter="enter"//对应enter-active
  @after-enter="afterEnter"//对应enter-to
  @enter-cancelled="enterCancelled"//显示过度打断
  @before-leave="beforeLeave"//对应leave-from
  @leave="leave"//对应enter-active
  @after-leave="afterLeave"//对应leave-to
  @leave-cancelled="leaveCancelled"//离开过度打断
```

当只用 JavaScript 过渡的时候，在 **`enter` 和 `leave` 钩子中必须使用 `done` 进行回调**

结合gsap 动画库使用 [GreenSock](https://greensock.com/)

```cobol
const beforeEnter = (el: Element) => {
    console.log('进入之前from', el);
}
const Enter = (el: Element,done:Function) => {
    console.log('过度曲线');
    setTimeout(()=>{
       done()
    },3000)
}
const AfterEnter = (el: Element) => {
    console.log('to');
}
```

#### 4.appear

通过这个属性可以设置初始节点过度 就是页面加载完成就开始动画 对应三个状态

```cobol
appear-active-class=""
appear-from-class=""
appear-to-class=""
appear
```



## 学习Vue3 第二十二章（transition-group过度列表）

- 单个节点

- 多个节点，每次只渲染一个

那么怎么同时渲染整个列表，比如使用 v-for？在这种场景下，我们会使用 <transition-group> 组件。在我们深入例子之前，先了解关于这个组件的几个特点：

- 默认情况下，它不会渲染一个包裹元素，但是你可以通过 tag attribute 指定渲染一个元素。
- 过渡模式不可用，因为我们不再相互切换特有的元素。
- 内部元素总是需要提供唯一的 key attribute 值。
- CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身。

```vue
<transition-group>
     <div style="margin: 10px;" :key="item" v-for="item in list">{{ item }</div>
</transition-group>
```

```tsx
const list = reactive<number[]>([1, 2, 4, 5, 6, 7, 8, 9])
const Push = () => {
    list.push(123)
}
const Pop = () => {
    list.pop()
}
```

#### 2.列表的移动过渡

<transition-group> 组件还有一个特殊之处。除了进入和离开，它还可以为定位的改变添加动画。只需了解新增的 v-move 类就可以使用这个新功能，它会应用在元素改变定位的过程中。像之前的类名一样，它的前缀可以通过 name attribute 来自定义，也可以通过 move-class attribute 手动设置

下面代码很酷炫

```vue
<template>
    <div>
        <button @click="shuffle">Shuffle</button>
        <transition-group class="wraps" name="mmm" tag="ul">
            <li class="cell" v-for="item in items" :key="item.id">{{ item.number }}</li>
        </transition-group>
    </div>
</template>
  
<script setup  lang='ts'>
import _ from 'lodash'
import { ref } from 'vue'
let items = ref(Array.apply(null, { length: 81 } as number[]).map((_, index) => {
    return {
        id: index,
        number: (index % 9) + 1
    }
}))
const shuffle = () => {
    items.value = _.shuffle(items.value)
}
</script>
  
<style scoped lang="less">
.wraps {
    display: flex;
    flex-wrap: wrap;
    width: calc(25px * 10 + 9px);
    .cell {
        width: 25px;
        height: 25px;
        border: 1px solid #ccc;
        list-style-type: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
 
.mmm-move {
    transition: transform 0.8s ease;
}
</style>
```

#### 3.状态过渡

Vue 也同样可以给数字 Svg 背景颜色等添加过度动画 今天演示数字变化

```cobol
<template>
    <div>
        <input step="20" v-model="num.current" type="number" />
        <div>{{ num.tweenedNumber.toFixed(0) }}</div>
    </div>
</template>
    
<script setup lang='ts'>
import { reactive, watch } from 'vue'
import gsap from 'gsap'
const num = reactive({
    tweenedNumber: 0,
    current:0
})
 
watch(()=>num.current, (newVal) => {
    gsap.to(num, {
        duration: 1,
        tweenedNumber: newVal
    })
})
 
</script>
    
<style>
</style>
```



## 学习Vue3 第二十三章（依赖注入Provide / Inject）

#### Provide / Inject

通常，当我们需要从父组件向子组件传递数据时，我们使用 props。想象一下这样的结构：有一些深度嵌套的组件，而深层的子组件只需要父组件的部分内容。在这种情况下，如果仍然将 prop 沿着组件链逐级传递下去，可能会很麻烦。

官网的解释很让人疑惑，那我翻译下这几句话：

provide 可以在祖先组件中指定我们想要提供给后代组件的数据或方法，而在任何后代组件中，我们都可以使用 inject 来接收 provide 提供的数据或方法。 


![img](https://img-blog.csdnimg.cn/img_convert/5f4ea9e16eda6e37ab336075a788ae15.png)

 看一个例子

#### 父组件传递数据

```vue
<template>
    <div class="App">
        <button>我是App</button>
        <A></A>
    </div>
</template>
    
<script setup lang='ts'>
import { provide, ref } from 'vue'
import A from './components/A.vue'
let flag = ref<number>(1)
provide('flag', flag)
</script>
    
<style>
.App {
    background: blue;
    color: #fff;
}
</style>
```

#### 子组件接受

```cobol
<template>
    <div style="background-color: green;">
        我是B
        <button @click="change">change falg</button>
        <div>{{ flag }}</div>
    </div>
</template>
    
<script setup lang='ts'>
import { inject, Ref, ref } from 'vue'
 
const flag = inject<Ref<number>>('flag', ref(1))
const change = () => {
    flag.value = 2
}
</script>
    
<style>
</style>
```

 **TIPS 你如果传递普通的值 是不具有响应式的 需要通过ref reactive 添加响应式**

#### 使用场景

当父组件有很多数据需要分发给其子代组件的时候， 就可以使用provide和inject。

## 学习Vue3 第二十四章（兄弟组件传参和Bus）

**两种方案**

#### 1.借助父组件传参

例如父组件为App 子组件为A 和 B他两个是同级的

```cobol
<template>
    <div>
        <A @on-click="getFalg"></A>
        <B :flag="Flag"></B>
    </div>
</template>
    
<script setup lang='ts'>
import A from './components/A.vue'
import B from './components/B.vue'
import { ref } from 'vue'
let Flag = ref<boolean>(false)
const getFalg = (flag: boolean) => {
   Flag.value = flag;
}
</script>
    
<style>
</style>
```

A 组件派发事件通过App.vue 接受A组件派发的事件然后在Props 传给B组件 也是可以实现的

缺点就是比较麻烦 ，无法直接通信，只能充当桥梁

#### 2.Event Bus

我们在Vue2 可以使用$emit 传递 $on监听 emit传递过来的事件

这个原理其实是运用了JS设计模式之发布订阅模式

我写了一个简易版

```tsx
 
type BusClass<T> = {
    emit: (name: T) => void
    on: (name: T, callback: Function) => void
}
type BusParams = string | number | symbol 
type List = {
    [key: BusParams]: Array<Function>
}
class Bus<T extends BusParams> implements BusClass<T> {
    list: List
    constructor() {
        this.list = {}
    }
    emit(name: T, ...args: Array<any>) {
        let eventName: Array<Function> = this.list[name]
        eventName.forEach(ev => {
            ev.apply(this, args)
        })
    }
    on(name: T, callback: Function) {
        let fn: Array<Function> = this.list[name] || [];
        fn.push(callback)
        this.list[name] = fn
    }
}
 
export default new Bus<number>()
```

然后挂载到Vue config 全局就可以使用啦



## 学习Vue3 第二十五章（TSX）<vuehook>

**完整版用法 请看 [@vue/babel-plugin-jsx - npm](https://www.npmjs.com/package/@vue/babel-plugin-jsx)**

我们之前呢是使用Template去写我们模板。现在可以扩展另一种风格TSX风格

vue2 的时候就已经支持jsx写法，只不过不是很友好，随着vue3对typescript的支持度，tsx写法越来越被接受

#### 1.安装插件

npm install @vitejs/plugin-vue-jsx -D

vite.config.ts 配置


![img](https://img-blog.csdnimg.cn/35486eb0bd1a4dabac0035384f877cbe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

```tsx
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx';
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),vueJsx()]
})
```

#### 2.修改tsconfig.json 配置文件

```cobol
    "jsx": "preserve",
    "jsxFactory": "h",
    "jsxFragmentFactory": "Fragment",
```

![img](https://img-blog.csdnimg.cn/e6d0ae1a4c254f13bde802f408fed4b3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 3.使用TSX

**TIPS tsx不会自动解包使用ref加.vlaue ! ! !**

##### tsx支持 v-model 的使用

```tsx
 
import { ref } from 'vue'
 
let v = ref<string>('')
 
const renderDom = () => {
    return (
        <>
           <input v-model={v.value} type="text" />
           <div>
               {v.value}
           </div>
        </>
    )
}
 
export default renderDom
```

#### v-show

```cobol
 
import { ref } from 'vue'
 
let flag = ref(false)
 
const renderDom = () => {
    return (
        <>
           <div v-show={flag.value}>景天</div>
           <div v-show={!flag.value}>雪见</div>
        </>
    )
}
 
export default renderDom
```

#### v-if是不支持的

所以需要改变风格

```tsx
import { ref } from 'vue'
 
let flag = ref(false)
 
const renderDom = () => {
    return (
        <>
            {
                flag.value ? <div>景天</div> : <div>雪见</div>
            }
        </>
    )
}
 
export default renderDom
```

#### v-for也是不支持的

需要使用Map

```cobol
import { ref } from 'vue'
 
let arr = [1,2,3,4,5]
 
const renderDom = () => {
    return (
        <>
            {
              arr.map(v=>{
                  return <div>${v}</div>
              })
            }
        </>
    )
}
 
export default renderDom
```

#### v-bind使用

直接赋值就可以

```tsx
import { ref } from 'vue'
 
let arr = [1, 2, 3, 4, 5]
 
const renderDom = () => {
    return (
        <>
            <div data-arr={arr}>1</div>
        </>
    )
}
 
export default renderDom
```

#### v-on绑定事件 所有的事件都按照react风格来

- 所有事件有on开头
- 所有事件名称首字母大写

```cobol
 
const renderDom = () => {
    return (
        <>
            <button onClick={clickTap}>点击</button>
        </>
    )
}
 
const clickTap = () => {
    console.log('click');
}
 
export default renderDom
```

#### Props 接受值

```tsx
 
import { ref } from 'vue'
 
type Props = {
    title:string
}
 
const renderDom = (props:Props) => {
    return (
        <>
            <div>{props.title}</div>
            <button onClick={clickTap}>点击</button>
        </>
    )
}
 
const clickTap = () => {
    console.log('click');
}
 
export default renderDom
```

#### Emit派发

```tsx
type Props = {
    title: string
}
 
const renderDom = (props: Props,content:any) => {
    return (
        <>
            <div>{props.title}</div>
            <button onClick={clickTap.bind(this,content)}>点击</button>
        </>
    )
}
 
const clickTap = (ctx:any) => {
 
    ctx.emit('on-click',1)
}
```

#### Slot

```tsx
const A = (props, { slots }) => (
  <>
    <h1>{ slots.default ? slots.default() : 'foo' }</h1>
    <h2>{ slots.bar?.() }</h2>
  </>
);
 
const App = {
  setup() {
    const slots = {
      bar: () => <span>B</span>,
    };
    return () => (
      <A v-slots={slots}>
        <div>A</div>
      </A>
    );
  },
};
 
// or
 
const App = {
  setup() {
    const slots = {
      default: () => <div>A</div>,
      bar: () => <span>B</span>,
    };
    return () => <A v-slots={slots} />;
  },
};
 
// or you can use object slots when `enableObjectSlots` is not false.
const App = {
  setup() {
    return () => (
      <>
        <A>
          {{
            default: () => <div>A</div>,
            bar: () => <span>B</span>,
          }}
        </A>
        <B>{() => "foo"}</B>
      </>
    );
  },
};
```

#### 加餐实现一个vite插件解析tsx

1.需要用到的第三方插件

```tsx
npm install @vue/babel-plugin-jsx
npm install @babel/core
npm install @babel/plugin-transform-typescript
npm install @babel/plugin-syntax-import-meta
npm install @types/babel__core
```

插件代码

```tsx
import type { Plugin } from 'vite'
import * as babel from '@babel/core'; //@babel/core核心功能：将源代码转成目标代码。
import jsx from '@vue/babel-plugin-jsx'; //Vue给babel写的插件支持tsx v-model等
export default function (): Plugin {
    return {
        name: "vite-plugin-tsx",
        config (config) {
           return {
              esbuild:{
                 include:/\.ts$/
              }
           }
        },
        async transform(code, id) {
            if (/.tsx$/.test(id)) {
                //@ts-ignore
                const ts = await import('@babel/plugin-transform-typescript').then(r=>r.default)
                const res = babel.transformSync(code,{
                    plugins:[jsx,[ts, { isTSX: true, allowExtensions: true }]], //添加babel插件
                    ast:true, // ast: 抽象语法树，源代码语法结构的一种抽象表示。babel内部就是通过操纵ast做到语法转换。
                    babelrc:false, //.babelrc.json
                    configFile:false //默认搜索默认babel.config.json文件
                })
                return res?.code //code: 编译后的代码
            }
           
            return code
        }
    }
}
```

![img](https://img-blog.csdnimg.cn/74dcc7da3e8d46999c651cb92c8f676b.png)

![img](https://img-blog.csdnimg.cn/c8f6b367a44c4317ae526b6501d85d16.png)

![img](https://img-blog.csdnimg.cn/737283315f6d4affac79c42801fdfa5a.png)

## 学习Vue3 第二十六章（深入v-model）

#### Vue3自动引入插件

unplugin-auto-import/[vite](https://so.csdn.net/so/search?q=vite&spm=1001.2101.3001.7020)

vite配置

```cobol
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import VueJsx from '@vitejs/plugin-vue-jsx'
import AutoImport from 'unplugin-auto-import/vite'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),VueJsx(),AutoImport({
    imports:['vue'],
    dts:"src/auto-import.d.ts"
  })]
})
```

配置完成之后使用ref reactive watch 等 无须import 导入 可以直接使用 

[链接]: GitHub-antfu/unplugin-auto-import:AutoimportAPIson-demandforVite,WebpackandRollup

#### v-model

![img](https://img-blog.csdnimg.cn/a494e90ff5ba4e4b8ec945aba6e48e76.png)

TIps 在Vue3 v-model 是破坏性更新的

v-model在组件里面也是很重要的

v-model 其实是一个语法糖 通过props 和 emit组合而成的

1.默认值的改变

- prop：value -> modelValue；
- 事件：input -> update:modelValue；
- v-bind 的 .sync 修饰符和组件的 model 选项已移除
- 新增 支持多个v-model
- 新增 支持自定义 修饰符 Modifiers

案例 

#### 子组件 

```vue
<template>
     <div v-if='propData.modelValue ' class="dialog">
         <div class="dialog-header">
             <div>标题</div><div @click="close">x</div>
         </div>
         <div class="dialog-content">
            内容
         </div>
         
     </div>
</template>
 
<script setup lang='ts'>
 
type Props = {
   modelValue:boolean
}
 
const propData = defineProps<Props>()
 
const emit = defineEmits(['update:modelValue'])
 
const close = () => {
     emit('update:modelValue',false)
}
 
</script>
 
<style lang='less'>
.dialog{
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    position: fixed;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
    &-header{
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: 10px;
    }
    &-content{
        padding: 10px;
    }
}
</style>
```

#### 父组件

```vue
<template>
  <button @click="show = !show">开关{{show}}</button>
  <Dialog v-model="show"></Dialog>
</template>
 
<script setup lang='ts'>
import Dialog from "./components/Dialog/index.vue";
import {ref} from 'vue'
const show = ref(false)
</script>
 
<style>
</style>
```

#### 绑定多个案例 子组件

```cobol
<template>
     <div v-if='modelValue ' class="dialog">
         <div class="dialog-header">
             <div>标题---{{title}}</div><div @click="close">x</div>
         </div>
         <div class="dialog-content">
            内容
         </div>
         
     </div>
</template>
 
<script setup lang='ts'>
 
type Props = {
   modelValue:boolean,
   title:string
}
 
const propData = defineProps<Props>()
 
const emit = defineEmits(['update:modelValue','update:title'])
 
const close = () => {
     emit('update:modelValue',false)
     emit('update:title','我要改变')
}
 
</script>
 
<style lang='less'>
.dialog{
    width: 300px;
    height: 300px;
    border: 1px solid #ccc;
    position: fixed;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
    &-header{
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: 10px;
    }
    &-content{
        padding: 10px;
    }
}
</style>
```

#### 父组件

```cobol
<template>
  <button @click="show = !show">开关{{show}} ----- {{title}}</button>
  <Dialog v-model:title='title' v-model="show"></Dialog>
</template>
 
<script setup lang='ts'>
import Dialog from "./components/Dialog/index.vue";
import {ref} from 'vue'
const show = ref(false)
const title = ref('我是标题')
</script>
 
<style>
</style>
```

#### 自定义修饰符

添加到组件 `v-model` 的修饰符将通过 `modelModifiers` prop 提供给组件。在下面的示例中，我们创建了一个组件，其中包含默认为空对象的 `modelModifiers` prop

```cobol
<script setup lang='ts'>
 
type Props = {
    modelValue: boolean,
    title?: string,
    modelModifiers?: {
        default: () => {}
    }
    titleModifiers?: {
        default: () => {}
    }
 
}
 
const propData = defineProps<Props>()
 
const emit = defineEmits(['update:modelValue', 'update:title'])
 
const close = () => {
    console.log(propData.modelModifiers);
 
    emit('update:modelValue', false)
    emit('update:title', '我要改变')
}
```



## 学习Vue3 第二十七章（自定义指令directive）

#### directive-自定义指令（属于破坏性更新）

Vue中有v-if,v-for,**v-bind**，v-show,v-model 等等一系列方便快捷的指令 今天一起来了解一下vue里提供的自定义指令

#### 1.Vue3指令的钩子函数

- created 元素初始化的时候
- beforeMount 指令绑定到元素后调用 只调用一次
- mounted 元素插入父级dom调用
- beforeUpdate 元素被更新之前调用
- update 这个周期方法被移除 改用updated
- beforeUnmount 在元素被移除前调用
- unmounted 指令被移除后调用 只调用一次

Vue2 指令 bind inserted update componentUpdated unbind

####   2.在setup内定义局部指令

但这里有一个需要注意的限制：必须以 vNameOfDirective 的形式来命名本地自定义指令，以使得它们可以直接在模板中使用。

```vue
<template>
  <button @click="show = !show">开关{{show}} ----- {{title}}</button>
  <Dialog  v-move-directive="{background:'green',flag:show}"></Dialog>
</template>
```

```tsx
 
const vMoveDirective: Directive = {
  created: () => {
    console.log("初始化====>");
  },
  beforeMount(...args: Array<any>) {
    // 在元素上做些操作
    console.log("初始化一次=======>");
  },
  mounted(el: any, dir: DirectiveBinding<Value>) {
    el.style.background = dir.value.background;
    console.log("初始化========>");
  },
  beforeUpdate() {
    console.log("更新之前");
  },
  updated() {
    console.log("更新结束");
  },
  beforeUnmount(...args: Array<any>) {
    console.log(args);
    console.log("======>卸载之前");
  },
  unmounted(...args: Array<any>) {
    console.log(args);
    console.log("======>卸载完成");
  },
};
```

#### 3.生命周期钩子参数详解

第一个 el  当前绑定的DOM 元素

第二个 binding

- instance：使用指令的组件实例。

- value：传递给指令的值。例如，在 v-my-directive="1 + 1" 中，该值为 2。
- oldValue：先前的值，仅在 beforeUpdate 和 updated 中可用。无论值是否有更改都可用。
- arg：传递给指令的参数(如果有的话)。例如在 v-my-directive:foo 中，arg 为 "foo"。
- modifiers：包含修饰符(如果有的话) 的对象。例如在 v-my-directive.foo.bar 中，修饰符对象为 {foo: true，bar: true}。
- dir：一个对象，在注册指令时作为参数传递。例如，在以下指令中
- ![img](https://img-blog.csdnimg.cn/f9e5558801104f81ac32c54d6cf5f587.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)


第三个 当前元素的虚拟DOM 也就是Vnode

第四个 prevNode 上一个虚拟节点，仅在 beforeUpdate 和 updated 钩子中可用 

#### 4.函数简写

你可能想在 `mounted` 和 `updated` 时触发相同行为，而不关心其他的钩子函数。那么你可以通过将这个函数模式实现

```cobol
<template>
   <div>
      <input v-model="value" type="text" />
      <A v-move="{ background: value }"></A>
   </div>
</template>
   
<script setup lang='ts'>
import A from './components/A.vue'
import { ref, Directive, DirectiveBinding } from 'vue'
let value = ref<string>('')
type Dir = {
   background: string
}
const vMove: Directive = (el, binding: DirectiveBinding<Dir>) => {
   el.style.background = binding.value.background
}
</script>
 
 
 
<style>
</style>
```

#### 1.案例自定义拖拽指令 

```cobol
<template>
  <div v-move class="box">
    <div class="header"></div>
    <div>
      内容
    </div>
  </div>
</template>
 
<script setup lang='ts'>
import { Directive } from "vue";
const vMove: Directive = {
  mounted(el: HTMLElement) {
    let moveEl = el.firstElementChild as HTMLElement;
    const mouseDown = (e: MouseEvent) => {
      //鼠标点击物体那一刻相对于物体左侧边框的距离=点击时的位置相对于浏览器最左边的距离-物体左边框相对于浏览器最左边的距离
      console.log(e.clientX, e.clientY, "-----起始", el.offsetLeft);
      let X = e.clientX - el.offsetLeft;
      let Y = e.clientY - el.offsetTop;
      const move = (e: MouseEvent) => {
        el.style.left = e.clientX - X + "px";
        el.style.top = e.clientY - Y + "px";
        console.log(e.clientX, e.clientY, "---改变");
      };
      document.addEventListener("mousemove", move);
      document.addEventListener("mouseup", () => {
        document.removeEventListener("mousemove", move);
      });
    };
    moveEl.addEventListener("mousedown", mouseDown);
  },
};
</script>
 
<style lang='less'>
.box {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  border: 1px solid #ccc;
  .header {
    height: 20px;
    background: black;
    cursor: move;
  }
}
</style>
```

#### 2.案例权限按钮

```cobol
<template>
   <div class="btns">
       <button v-has-show="'shop:create'">创建</button>
 
       <button v-has-show="'shop:edit'">编辑</button>
 
       <button v-has-show="'shop:delete'">删除</button>
   </div>
</template>
 
<script setup lang='ts'>
import { ref, reactive,  } from 'vue'
import type {Directive} from 'vue'
//permission
localStorage.setItem('userId','xiaoman-zs')
 
//mock后台返回的数据
const permission = [
    'xiaoman-zs:shop:edit',
    'xiaoman-zs:shop:create',
    'xiaoman-zs:shop:delete'
]
const userId = localStorage.getItem('userId') as string
const vHasShow:Directive<HTMLElement,string> = (el,bingding) => {
   if(!permission.includes(userId+':'+ bingding.value)){
       el.style.display = 'none'
   }
}
 
</script>
 
<style scoped lang='less'>
.btns{
    button{
        margin: 10px;
    }
}
</style>
```



## 学习Vue3 第二十八章（自定义Hooks）

Vue3 自定义Hook

**主要用来处理复用代码逻辑的一些封装**

这个在vue2 就已经有一个东西是Mixins

**mixins**就是将这些多个相同的逻辑抽离出来，各个组件只需要引入mixins，就能实现一次写代码，多组件受益的效果。

**弊端就是 会涉及到覆盖的问题**

组件的data、methods、filters会覆盖mixins里的同名data、methods、filters。
![img](https://img-blog.csdnimg.cn/3d89c4d5dd8b4050b866d64b250d7391.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

第二点就是 变量来源不明确（隐式传入），不利于阅读，使代码变得难以维护。

#### Vue3 的自定义的hook

- Vue3 的 hook函数 相当于 vue2 的 mixin, 不同在与 hooks 是函数

- Vue3 的 hook函数 可以帮助我们提高代码的复用性, 让我们能在不同的组件中都利用 hooks 函数

Vue3 hook 库 [VueUse](https://vueuse.org/guide/)

案例

```tsx
import { onMounted } from 'vue'
 
 
type Options = {
    el: string
}
 
type Return = {
    Baseurl: string | null
}
export default function (option: Options): Promise<Return> {
 
    return new Promise((resolve) => {
        onMounted(() => {
            const file: HTMLImageElement = document.querySelector(option.el) as HTMLImageElement;
            file.onload = ():void => {
                resolve({
                    Baseurl: toBase64(file)
                })
            }
 
        })
 
 
        const toBase64 = (el: HTMLImageElement): string => {
            const canvas: HTMLCanvasElement = document.createElement('canvas')
            const ctx = canvas.getContext('2d') as CanvasRenderingContext2D
            canvas.width = el.width
            canvas.height = el.height
            ctx.drawImage(el, 0, 0, canvas.width,canvas.height)
            console.log(el.width);
            
            return canvas.toDataURL('image/png')
 
        }
    })
 
 
}
```



## 学习Vue3 第二十九章（Vue3定义全局函数和变量）

#### globalProperties

由于[Vue3](https://so.csdn.net/so/search?q=Vue3&spm=1001.2101.3001.7020) 没有Prototype 属性 使用 app.config.globalProperties 代替 然后去定义变量和函数

[Vue2](https://so.csdn.net/so/search?q=Vue2&spm=1001.2101.3001.7020)

```tsx
// 之前 (Vue 2.x)
Vue.prototype.$http = () => {}
```

Vue3

```cobol
// 之后 (Vue 3.x)
const app = createApp({})
app.config.globalProperties.$http = () => {}
```

#### 过滤器

在Vue3 移除了

我们正好可以使用全局函数代替Filters

案例

```tsx
app.config.globalProperties.$filters = {
  format<T extends any>(str: T): string {
    return `$${str}`
  }
}
```

声明文件 不然TS无法正确类型 推导

```tsx
type Filter = {
    format<T>(str: T): string
}
 
// 声明要扩充@vue/runtime-core包的声明.
// 这里扩充"ComponentCustomProperties"接口, 因为他是vue3中实例的属性的类型.
declare module 'vue' {
    export interface ComponentCustomProperties {
        $filters: Filter
    }
}
 
 
```

setup 读取值

```tsx
import { getCurrentInstance, ComponentInternalInstance } from 'vue';
 
const { appContext } = <ComponentInternalInstance>getCurrentInstance()
 
console.log(appContext.config.globalProperties.$env);
 
推荐第二种方式
 
import {ref,reactive,getCurrentInstance} from 'vue'
const app = getCurrentInstance()
console.log(app?.proxy?.$filters.format('js'))
```



## 学习Vue3 第三十章（编写Vue3插件）

#### 插件

插件是自包含的代码，通常向 Vue 添加全局级功能。你如果是一个对象需要有install方法Vue会帮你自动注入到install 方法 你如果是function 就直接当install 方法去使用

#### 使用插件

在使用 createApp() 初始化 Vue 应用程序后，你可以通过调用 use() 方法将插件添加到你的应用程序中。

实现一个Loading

#### Loading.Vue

```vue
<template>
    <div v-if="isShow" class="loading">
        <div class="loading-content">Loading...</div>
    </div>
</template>
    
<script setup lang='ts'>
import { ref } from 'vue';
const isShow = ref(false)//定位loading 的开关
 
const show = () => {
    isShow.value = true
}
const hide = () => {
    isShow.value = false
}
//对外暴露 当前组件的属性和方法
defineExpose({
    isShow,
    show,
    hide
})
</script>
 
 
    
<style scoped lang="less">
.loading {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    &-content {
        font-size: 30px;
        color: #fff;
    }
}
</style>
```

#### Loading.ts

```cobol
import {  createVNode, render, VNode, App } from 'vue';
import Loading from './index.vue'
 
export default {
    install(app: App) {
        //createVNode vue提供的底层方法 可以给我们组件创建一个虚拟DOM 也就是Vnode
        const vnode: VNode = createVNode(Loading)
        //render 把我们的Vnode 生成真实DOM 并且挂载到指定节点
        render(vnode, document.body)
        // Vue 提供的全局配置 可以自定义
        app.config.globalProperties.$loading = {
            show: () => vnode.component?.exposed?.show(),
            hide: () => vnode.component?.exposed?.hide()
        }
 
    }
}
```

#### Main.ts

```ts
import Loading from './components/loading'
 
 
let app = createApp(App)
 
app.use(Loading)
 
 
type Lod = {
    show: () => void,
    hide: () => void
}
//编写ts loading 声明文件放置报错 和 智能提示
declare module '@vue/runtime-core' {
    export interface ComponentCustomProperties {
        $loading: Lod
    }
}
 
 
 
app.mount('#app')
```

#### 使用方法

```cobol
<template>
 
  <div></div>
 
</template>
 
<script setup lang='ts'>
import { ref,reactive,getCurrentInstance} from 'vue'
const  instance = getCurrentInstance()  
instance?.proxy?.$Loading.show()
setTimeout(()=>{
  instance?.proxy?.$Loading.hide()
},5000)
 
 
// console.log(instance)
</script>
<style>
*{
  padding: 0;
  margin: 0;
}
</style>
```

#### Vue use 源码手写

```cobol
import type { App } from 'vue'
import { app } from './main'
 
interface Use {
    install: (app: App, ...options: any[]) => void
}
 
const installedList = new Set()
 
export function MyUse<T extends Use>(plugin: T, ...options: any[]) {
    if(installedList.has(plugin)){
      return console.warn('重复添加插件',plugin)
    }else{
        plugin.install(app, ...options)
        installedList.add(plugin)
    }
}
```



## 学习Vue3 第三十一章（详解Scoped和样式 穿透）

主要是用于修改很多vue常用的组件库（element, vant, AntDesigin），虽然配好了样式但是还是需要更改其他的样式

就需要用到样式穿透

#### scoped的原理

vue中的scoped 通过在DOM结构以及css样式上加唯一不重复的标记:data-v-hash的方式，以保证唯一（而这个工作是由过PostCSS转译实现的），达到样式私有化模块化的目的。

总结一下scoped三条渲染规则：

- 给HTML的DOM节点加一个不重复data属性(形如：data-v-123)来表示他的唯一性

- 在每句css选择器的末尾（编译后的生成的css语句）加一个当前组件的data属性选择器（如[data-v-123]）来私有化样式
- 如果组件内部包含有其他组件，只会给其他组件的最外层标签加上当前组件的data属性

PostCSS会给一个组件中的所有dom添加了一个独一无二的动态属性data-v-xxxx，然后，给CSS选择器额外添加一个对应的属性选择器来选择该组件中dom，这种做法使得样式只作用于含有该属性的dom——组件内部dom, 从而达到了'样式模块化'的效果.

案例修改Element ui Input样式

发现没有生效

![img](https://img-blog.csdnimg.cn/566e8a5d6e5349638615ff65771896a0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

![img](https://img-blog.csdnimg.cn/a6ad6292e41e46089251cd7627074ee6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

 如果不写Scoped 就没问题

原因就是Scoped 搞的鬼 他在进行PostCss转化的时候把元素选择器默认放在了最后

![img](https://img-blog.csdnimg.cn/c896c971d3ce407989d3439101a180ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

#### Vue 提供了样式穿透:deep() 他的作用就是用来改变 属性选择器的位置

![img](https://img-blog.csdnimg.cn/100a1402669844c2a0da76d7959c978e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)

![img](https://img-blog.csdnimg.cn/185233f9afe8422fa815be3a22249e4a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bCP5ruhenM=,size_20,color_FFFFFF,t_70,g_se,x_16)





## 学习Vue3 第三十二章（css Style完整新特性）

#### :deep(),其实还有两个选择器可以补充

### 1.插槽选择器

A 组件定义一个插槽

```cobol
<template>
    <div>
        我是插槽
        <slot></slot>
    </div>
</template>
 
<script>
export default {}
</script>
 
<style scoped>
 
</style>
```

在App.vue 引入

```cobol
<template>
    <div>
        <A>
            <div class="a">私人定制div</div>
        </A>
    </div>
</template>
 
<script setup>
import A from "@/components/A.vue"
</script>
 
 
<style lang="less" scoped>
</style>
```

在A组件修改class a 的颜色

```cobol
<style scoped>
.a{
    color:red
}
</style>
```

无效果

![img](https://img-blog.csdnimg.cn/b4f7142c2205486ea9d5e3ffc19740a8.png)

 默认情况下，作用域样式不会影响到 `<slot/>` 渲染出来的内容，因为它们被认为是父组件所持有并传递进来的。

解决方案 slotted

```cobol
<style scoped>
 :slotted(.a) {
    color:red
}
</style>
```

![img](https://img-blog.csdnimg.cn/57d5f7f9cd7f48f9ad7e2ef11f5ba338.png)

####  2.全局选择器

在之前我们想加入全局 样式 通常都是新建一个style 标签 不加[scoped](https://so.csdn.net/so/search?q=scoped&spm=1001.2101.3001.7020) 现在有更优雅的解决方案

```cobol
<style>
 div{
     color:red
 }
</style>
 
<style lang="less" scoped>
 
</style>
```

```css
<style lang="less" scoped>
:global(div){
    color:red
}
</style>
```

效果等同于上面 

#### 3.动态 CSS

单文件组件的 `<style>` 标签可以通过 `v-bind` 这一 CSS 函数将 CSS 的值关联到动态的组件状态上：

```vue
<template>
    <div class="div">
       小满是个弟弟
    </div>
</template>
 
<script lang="ts" setup>
import { ref } from 'vue'
const red = ref<string>('red')
</script>
 
<style lang="less" scoped>
.div{
   color:v-bind(red)
}
 
</style>
```

如果是对象 [v-bind](https://so.csdn.net/so/search?q=v-bind&spm=1001.2101.3001.7020) 请加引号

```vue
 <template>
    <div class="div">
        小满是个弟弟
    </div>
</template>
 
<script lang="ts" setup>
import { ref } from "vue"
const red = ref({
    color:'pink'
})
</script>
 
    <style lang="less" scoped>
.div {
    color: v-bind('red.color');
}
</style>
```

#### 4.css `module`

<style module> 标签会被编译为 CSS Modules 并且将生成的 CSS 类作为 $style 对象的键暴露给组件

```vue
<template>
    <div :class="$style.red">
        小满是个弟弟
    </div>
</template>
 
<style module>
.red {
    color: red;
    font-size: 20px;
}
</style>
```

自定义注入名称（多个可以用数组）

你可以通过给 `module` attribute 一个值来自定义注入的类对象的 property 键

```cobol
<template>
    <div :class="[zs.red,zs.border]">
        小满是个弟弟
    </div>
</template>
 
<style module="zs">
.red {
    color: red;
    font-size: 20px;
}
.border{
    border: 1px solid #ccc;
}
</style>
```

与组合式 API 一同使用

注入的类可以通过 useCssModule API 在 setup() 和 <script setup> 中使用。对于使用了自定义注入名称的 <style module> 模块，useCssModule 接收一个对应的 module attribute 值作为第一个参数

```vue
<template>
    <div :class="[zs.red,zs.border]">
        小满是个弟弟
    </div>
</template>
 
 
<script setup lang="ts">
import { useCssModule } from 'vue'
const css = useCssModule('zs')
</script>
 
<style module="zs">
.red {
    color: red;
    font-size: 20px;
}
.border{
    border: 1px solid #ccc;
}
</style>
```

使用场景一般用于TSX  和 render  函数 居多



# 学习Vue3 第三十三章（Evnet Loop 和 nextTick）

#### JS 执行机制

在我们学js 的时候都知道js 是单线程的如果是多线程的话会引发一个问题在同一时间同时操作DOM 一个增加一个删除JS就不知道到底要干嘛了，所以这个语言是单线程的但是随着HTML5到来js也支持了多线程webWorker 但是也是不允许操作DOM

单线程就意味着所有的任务都需要排队，后面的任务需要等前面的任务执行完才能执行，如果前面的任务耗时过长，后面的任务就需要一直等，一些从用户角度上不需要等待的任务就会一直等待，这个从体验角度上来讲是不可接受的，所以JS中就出现了异步的概念。

#### 同步任务

代码从上到下按顺序执行

#### 异步任务

##### 1.宏任务

script(整体代码)、setTimeout、setInterval、UI交互事件、postMessage、Ajax

##### 2.微任务

Promise.then catch finally、MutaionObserver、process.nextTick(Node.js 环境)

#### 运行机制

所有的同步任务都是在主进程执行的形成一个执行栈，主线程之外，还存在一个"任务队列"，异步任务执行队列中先执行宏任务，然后清空当次宏任务中的所有微任务，然后进行下一个tick如此形成循环。

nextTick 就是创建一个异步任务，那么它自然要等到同步任务执行完成后才执行。

```ts
<template>
   <div ref="xiaoman">
      {{ text }}
   </div>
   <button @click="change">change div</button>
</template>
   
<script setup lang='ts'>
import { ref,nextTick } from 'vue';
 
const text = ref('小满开飞机')
const xiaoman = ref<HTMLElement>()
 
const change = async () => {
   text.value = '小满不开飞机'
   console.log(xiaoman.value?.innerText) //小满开飞机
   await nextTick();
   console.log(xiaoman.value?.innerText) //小满不开飞机
}
 
 
</script>
 
 
<style  scoped>
</style>
```

源码地址 core\packages\runtime-core\src\scheduler.ts

```cobol
const resolvedPromise: Promise<any> = Promise.resolve()
let currentFlushPromise: Promise<void> | null = null
 
export function nextTick<T = void>(
  this: T,
  fn?: (this: T) => void
): Promise<void> {
  const p = currentFlushPromise || resolvedPromise
  return fn ? p.then(this ? fn.bind(this) : fn) : p
}
```

nextTick 接受一个参数fn（函数）定义了一个变量P 这个P最终返回都是Promise，最后是return 如果传了fn 就使用变量P.then执行一个微任务去执行fn函数，then里面this 如果有值就调用bind改变this指向返回新的函数，否则直接调用fn，如果没传fn，就返回一个promise，最终结果都会返回一个promise

在我们之前讲过的ref源码中有一段 triggerRefValue  他会去调用 triggerEffects

```tsx
export function triggerRefValue(ref: RefBase<any>, newVal?: any) {
  ref = toRaw(ref)
  if (ref.dep) {
    if (__DEV__) {
      triggerEffects(ref.dep, {
        target: ref,
        type: TriggerOpTypes.SET,
        key: 'value',
        newValue: newVal
      })
    } else {
      triggerEffects(ref.dep)
    }
  }
}
```

```tsx
export function triggerEffects(
  dep: Dep | ReactiveEffect[],
  debuggerEventExtraInfo?: DebuggerEventExtraInfo
) {
  // spread into array for stabilization
  for (const effect of isArray(dep) ? dep : [...dep]) {
    if (effect !== activeEffect || effect.allowRecurse) {
      if (__DEV__ && effect.onTrigger) {
        effect.onTrigger(extend({ effect }, debuggerEventExtraInfo))
      }
      //当响应式对象发生改变后，执行 effect 如果有 scheduler 这个参数，会执行这个 scheduler 函数
      if (effect.scheduler) {
        effect.scheduler()
      } else {
        effect.run()
      }
    }
  }
}
```

那么scheduler 这个函数从哪儿来的 我们看这个类 ReactiveEffect

```cobol
export class ReactiveEffect<T = any> {
  active = true
  deps: Dep[] = []
  parent: ReactiveEffect | undefined = undefined
 
  /**
   * Can be attached after creation
   * @internal
   */
  computed?: ComputedRefImpl<T>
  /**
   * @internal
   */
  allowRecurse?: boolean
 
  onStop?: () => void
  // dev only
  onTrack?: (event: DebuggerEvent) => void
  // dev only
  onTrigger?: (event: DebuggerEvent) => void
 
  constructor(
    public fn: () => T,
    public scheduler: EffectScheduler | null = null, //我在这儿 
    scope?: EffectScope
  ) {
    recordEffectScope(this, scope)
  }
```

scheduler 作为一个参数传进来的

```cobol
   const effect = (instance.effect = new ReactiveEffect(
      componentUpdateFn,
      () => queueJob(instance.update),
      instance.scope // track it in component's effect scope
    ))
```

他是在初始化 effect 通过 queueJob 传进来的

```cobol
//queueJob 维护job列队，有去重逻辑，保证任务的唯一性，每次调用去执行，被调用的时候去重，每次调用去执行 queueFlush
export function queueJob(job: SchedulerJob) {
  // 判断条件：主任务队列为空 或者 有正在执行的任务且没有在主任务队列中  && job 不能和当前正在执行任务及后面待执行任务相同
  // 重复数据删除：
  // - 使用Array.includes(Obj, startIndex) 的 起始索引参数：startIndex
  // - startIndex默认为包含当前正在运行job的index，此时，它不能再次递归触发自身
  // - 如果job是一个watch()回调函数或者当前job允许递归触发，则搜索索引将+1，以允许他递归触发自身-用户需要确保回调函数不会死循环
  if (
    (!queue.length ||
      !queue.includes(
        job,
        isFlushing && job.allowRecurse ? flushIndex + 1 : flushIndex
      )) &&
    job !== currentPreFlushParentJob
  ) {
    if (job.id == null) {
      queue.push(job)
    } else {
      queue.splice(findInsertionIndex(job.id), 0, job)
    }
    queueFlush()
  }
}
```

 queueJob 维护job列队 并且调用 queueFlush

```cobol
function queueFlush() {
  // 避免重复调用flushJobs
  if (!isFlushing && !isFlushPending) {
    isFlushPending = true
     //开启异步任务处理flushJobs
    currentFlushPromise = resolvedPromise.then(flushJobs)
  }
}
```

queueFlush 给每一个队列创建了微任务



