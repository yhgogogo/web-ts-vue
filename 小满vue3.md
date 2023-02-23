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

<span>Hello world!</span>
<span>Hello world!</span>
<span>Hello world!</span>
<span>Hello world!</span>
<span>{{msg}}</span>
<span>Hello world!</span>
<span>Hello world! </span>
Vue3 编译后的 Vdom 是这个样子的

export function render(_ctx，_cache，$props，$setup，$data，$options){return (_openBlock(),_createBlock(_Fragment,null，[
_createvNode( "span", null,"Hello world ! "),
_createvNode( "span",null，"Hello world! "),
_createvNode( "span"，null，"Hello world! "),
_createvNode( "span", null，"Hello world! "),
_createVNode("span", null，_toDisplaystring(_ctx.msg)，1/* TEXT */)，
_createvNode( "span", null，"Hello world! "),
_createvNode( "span", null，"Hello world! ")]，64/*STABLE_FRAGMENT */))
新增了 patch flag 标记

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