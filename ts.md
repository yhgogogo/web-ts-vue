### ts基础类型

#### 1.字符串类型：

字符串使用string进行定义：

```tsx
let a:string = '杨恒' //普通声名
let str:string = `d${a}` //也可以使用es6模版字符串
```

其中``用来定义es6中的模版字符串，使用${}用来在模版字符串中嵌入表达式

#### 2.数字类型：

数字使用number进行定义，支持十六进制，十进制，八进制和二进制；

```tsx
let a:number = 123; //普通数字
let b:number = NaN; //NaN
let c:number = Infinity; //Infinity表示无穷大
let d:number = 6; //十进制
let e:number = 0xf00d; //十六进制
let f:number = 0b010; //二进制
let g:number = 0o744; //八进制
```

 #### 3.布尔类型：

布尔类型使用Boolean进行定义，**注意布尔类型创造的对象不是布尔值**

```tsx
let a:boolean = true;//使用boolean定义true或false
let creatBoolean:boolean = new Boolean(1);//	注意这样写会报错，因为事实上 new Boolean（）返回的是一个Boolean对象，可以改写成如下方法：
let creatBoolean:Boolean = new Boolean(1); //使用大写Boolean来接收布尔值对象
let boolean:boolean = true; //可以直接使用布尔值
let boolean2:boolean = Boolean(1); //也可以通过函数返回布尔值
```

#### 4.空值类型：

**JavaStricpt**中没有空值（**Void**）的概念，在**TypeScripct**中可以使用（**Void**）来表示没有任何返回值的函数

```tsx
function voidFn():void = {
  console.log('tst void')
};
//void用法主要是不希望调用者管血函数返回值的情况下使用，比如通常是异步回调函数；
```



