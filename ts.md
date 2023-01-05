### 一、Ts基础类型

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

**void也可以定义null和undefined类型**

```tsx
let a:void = null;
let b:void = undefined;
```

#### 5.null和undefined类型：

```tsx
let a:null = null; //定义null类型
let b:undefined = undefined; //定义undefined类型
```

**void与undefined和null最大的区别是：**

undefined和null是所有类型的字类型，也就是说undefined类型的变量，可以复制给string类型的变量：

```tsx
let a:void = undefined;
let b:string = '1';
b=a
//这样写会报错，void类型不可以分给其他类型
let a:null = null;
let b:string = '1';
a=b;
//这样写是可以的，或者也可以使用undeffined类型也是可以的；
let a:undefined = undefined;
let b:string = '1'
a = b;
```

**注意如果开启了严格模式null不能赋予void类型**

### 二、Ts（任意类型）

#### 1.any类型和unknow顶级类型：

**1.在没有强制限制使用哪种类型时，随时切换类型时，我们可以使用any类型进行定义，不需要进行类型检查。**

```tsx
let anys:any = 123;
anys=[]; //ok
anys='123'; //ok
anys=true; //ok
anys={}; //ok
anys=Symol('123'); //ok
```

**2.声名变量时没有制定类型时默认为any**

```tsx
let anys;
anys = [];
anys = 123;
```

**3.any类型的弊端，使用any类型就失去了ts类型检测的作用**

**4.unknow类型与any一样，所有类型都可以分配，但是unknow类型更加安全**

```tsx
let unknows:unknown = 123;
unknows=[];
unknows='123';
unknows=true;
unknows=Symbol('正邦')；

// let a:unknown = '牛蛙';
// let b:string = a; //unknow类型不可以赋值

// let a:any = '牛蛙';
// let b:string = a; //any类型可以

// let a:unknown = 'niubi';
// let b:any = a;
//unknow可以赋值给any类型

// let a:unknown = {x:1};
//a.x; //unknow类型不可以调用对象属性以及函数的方法；
let a:any = {x:1};
console.log(a.x) //any类型可以调用

// let x:unknown = {a:'suibian',B:():void=>{
//     return
//     }}
// console.log(x.a);
// console.log(x.B)
//unknow类型不可以调用对象属性以及函数的方法，ts中会进行报错；
let x:any = {a:'suibian',B:():void=>{
        return
    }}
console.log(x.a);
console.log(x.B)
//any类型不受影响
```

#### 2.unknow类型与any类型的区别：

1:unknow类型只能做为**子类型**不能做为**父类型，**any可以做为子类型和父类型；

2:unknow类型**不能赋值给其他类型**，any可以；

3:unknow**不能调用对象的属性以及函数的方法**，any可以；

4:unknow类型赋值的对象只能是unknow和any；

