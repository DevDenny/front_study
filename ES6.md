# ES6 课程概述

1. ECMAScript、JavaScript、NodeJs，它们的区别是什么？

ECMAScript：简称ES，是一个语言标准（循环、判断、变量、数组等数据类型）

JavaScript：运行在浏览器端的语言，该语言使用ES标准。 ES + web api = JavaScript

NodeJs：运行在服务器端的语言，该语言使用ES标准。 ES + node api = JavaScript


无论JavaScript，还是NodeJs，它们都是ES的超集（super set）

2. ECMAScript有哪些关键的版本？

ES3.0： 1999

ES5.0:  2009

ES6.0:  2015, 从该版本开始，不再使用数字作为编号，而使用年份

ES7.0:  2016

3. 为什么ES6如此重要？

ES6解决JS无法开发大型应用的语言层面的问题。

4. 如何应对兼容性问题？

之后的课程会介绍如何解决

5. 学习本课程需要的前置知识有哪些？

HTML+CSS、JavaScript（ES 5）

6. 这套课程难不难？

难度和《JavaScript基础》差不多


# 块级绑定
## 声明变量的问题

使用var声明变量

1. 允许重复的变量声明：导致数据被覆盖

2. 变量提升：怪异的数据访问、闭包问题

- 典型闭包问题：i会提升，导致点击输出11，点击时for已经结束。
```js
var div = document.getElementById("divButtons")

for (var i = 1; i <= 10; i++) {
    var btn = document.createElement("button");
    btn.innerHTML = "按钮" + i;
    div.appendChild(btn);
    btn.onclick = function () {
        console.log(i); //输出11
    }
}
```

3. 全局变量挂载到全局对象：全局对象成员污染问题


## 使用let声明变量

ES6不仅引入let关键字用于解决变量声明的问题，同时引入了块级作用域的概念

块级作用域：代码执行时遇到花括号，会创建一个块级作用域，花括号结束，销毁块级作用域

声明变量的问题

1. 全局变量挂载到全局对象：全局对象成员污染问题

***let声明的变量不会挂载到全局对象***

2. 允许重复的变量声明：导致数据被覆盖

***let声明的变量，不允许当前作用域范围内重复声明***

***在块级作用域中用let定义的变量，在作用域外不能访问***

3. 变量提升：怪异的数据访问、闭包问题

***使用let不会有变量提升，因此，不能在定义let变量之前使用它***

底层实现上，let声明的变量实际上也会有提升，但是，提升后会将其放入到“暂时性死区”，如果访问的变量位于暂时性死区，则会报错：“Cannot access 'a' before initialization”。当代码运行到该变量的声明语句时，会将其从暂时性死区中移除。

在循环中，用let声明的循环变量，会特殊处理，每次进入循环体，都会开启一个新的作用域，并且将循环变量绑定到该作用域（每次循环，使用的是一个全新的循环变量）

在循环中使用let声明的循环变量，在循环结束后会销毁

## 使用const声明常量

const和let完全相同，仅在于用const声明的变量，必须在声明时赋值，而且不可以重新赋值。

实际上，在开发中，应该尽量使用const来声明变量，以保证变量的值不会随意篡改，原因如下：

1. 根据经验，开发中的很多变量，都是不会更改，也不应该更改的。
2. 后续的很多框架或者是第三方JS库，都要求数据不可变，使用常量可以一定程度上保证这一点。

注意的细节：

1. 常量不可变，是指声明的常量的内存空间不可变，并不保证内存空间中的地址指向的其他空间不可变。
- 对像内容可变：
```js
const a = {
    name: "kevin",
    age: 123
};
a.name = "abc";
console.log(a)
```

3. 常量的命名
   1. 特殊的常量：该常量从字面意义上，一定是不可变的，比如圆周率、月地距地或其他一些绝不可能变化的配置。通常，**该常量的名称全部使用大写，多个单词之间用下划线分割**
   2. 普通的常量：使用和之前一样的命名即可
4. 在for循环中，循环变量不可以使用常量

# 字符串和正则表达式
## 更好的Unicode支持

早期，由于存储空间宝贵，Unicode使用16位二进制来存储文字。我们将一个16位的二进制编码叫做一个码元（Code Unit）。

后来，由于技术的发展，Unicode对文字编码进行了扩展，将某些文字扩展到了32位（占用两个码元），并且，将某个文字对应的二进制数字叫做码点（Code Point）。

ES6为了解决这个困扰，为字符串提供了方法：codePointAt，根据字符串码元的位置得到其码点。

同时，ES6为正则表达式添加了一个flag: u，如果添加了该配置，则匹配时，使用码点匹配

## 更多的字符串API

以下均为字符串的实例（原型）方法

- includes

判断字符串中是否包含指定的子字符串（第二个参数表示从第几个字符开始查找）

- startsWith

判断字符串中是否以指定的字符串开始

- endsWith

判断字符串中是否以指定的字符串结尾

- repeat

将字符串重复指定的次数，然后返回一个新字符串。

## 模板字符串

ES6之前处理字符串繁琐的两个方面：

1. 多行字符串
2. 字符串拼接


在ES6中，提供了模板字符串的书写，可以非常方便的换行和拼接，要做的，仅仅是将字符串的开始或结尾改为 ` 符号

如果要在字符串中拼接js表达式，只需要在模板字符串中使用```${JS表达式}```

## 3-5. [扩展]模板字符串标记

在模板字符串书写之前，可以加上标记:

```js
标记名`模板字符串`
```

标记是一个函数，函数参数如下：

1. 参数1：被插值分割的字符串数组
2. 后续参数：所有的插值


# 函数
## 4-1. 参数默认值

### 使用

在书写形参时，直接给形参赋值，附的值即为默认值

这样一来，当调用函数时，如果没有给对应的参数赋值（给它的值是undefined），则会自动使用默认值。
```js
function getContainer() {
    console.log("abc");
    return document.getElementById("container");
}

/**
 * 创建一个元素
 * @param {*} name 元素的名称 
 * @param {*} container 元素的父元素
 * @param {*} content 元素的内容 
 */
function createElement(name = "div", container = getContainer(), content = "") {
    const ele = document.createElement(name)
    if (content) {
        ele.innerHTML = content;
    }
    container.appendChild(ele);
}

createElement(undefined, undefined, "手动阀手动阀十分")//不能只传一个参数，会放到name参数中。出错。
createElement(undefined, undefined, "234242342424")
createElement(undefined, document.getElementById("container"), "234242342424")
```

### [扩展]对arguments的影响

只要给函数加上参数默认值，该函数会自动变成严格模式下的规则：arguments和形参脱离（argument不建议开发中使用）

### [扩展]留意暂时性死区

形参和ES6中的let或const声明一样，具有作用域，并且根据参数的声明顺序，存在暂时性死区。

## 4-2. 剩余参数

arguments的缺陷：

1. 如果和形参配合使用，容易导致混乱
2. 从语义上，使用arguments获取参数，由于形参缺失，无法从函数定义上理解函数的真实意图


ES6的剩余参数专门用于手收集末尾的所有参数，将其放置到一个形参数组中。

语法:

```js
function (...形参名){

}
```

**细节：**

1. 一个函数，仅能出现一个剩余参数
2. 一个函数，如果有剩余参数，剩余参数必须是最后一个参数

# 4-3. 展开运算符

使用方式：```  ...要展开的东西  ```

```js
/**
 * 对所有数字求和
 * @param  {...any} args 
 */
function sum(...args) {
    let sum = 0;
    for (let i = 0; i < args.length; i++) {
        sum += args[i];
    }
    return sum;
}

/**
 * 获取一个指定长度的随机数组成的数组
 * @param {*} length 
 */
function getRandomNumbers(length) {
    const arr = [];
    for (let i = 0; i < length; i++) {
        arr.push(Math.random());
    }
    return arr;
}

const numbers = getRandomNumbers(10);
//将数组的每一项展开，依次作为参数传递，而不是把整个数组作为一个参数传递
// sum(numbers)

console.log(sum(...numbers))//相当于传递了10个参数
console.log(sum(1, 3, ...numbers, 3, 5))
```


## 对数组展开 ES6
- 数组拷贝
```js
const arr1 = [3, 67, 8, 5];

//克隆arr1数组到arr2

const arr2 = [0, ...arr1, 1];

console.log(arr2, arr1 === arr2)
```
## 对对象展开 ES7
```js
const obj1 = {
    name: "成哥",
    age: 18,
    love: "邓嫂",
    address: {
        country: "中国",
        province: "黑龙江",
        city: "哈尔滨"
    }
}

// 浅克隆到obj2

const obj2 = {
    ...obj1,
    name: "邓哥"   //对像属性不能重复，后面的会覆盖前面的。
};

console.log(obj2)

console.log(obj1.address === obj2.address) // address是浅克隆，所以地址相同，指向同一个对像
```
## 4-5. 明确函数的双重用途

ES6提供了一个特殊的API，可以使用该API在函数内部，判断该函数是否使用了new来调用

```js
new.target 
//该表达式，得到的是：如果没有使用new来调用函数，则返回undefined
//如果使用new调用函数，则得到的是new关键字后面的函数本身
```

## 4-6. 箭头函数

回顾：this指向

1. 通过对象调用函数，this指向对象
2. 直接调用函数，this指向全局对象
3. 如果通过new调用函数，this指向新创建的对象
4. 如果通过apply、call、bind调用函数，this指向指定的数据
5. 如果是DOM事件函数，this指向事件源

### 使用语法

箭头函数是一个函数表达式，理论上，任何使用函数表达式的场景都可以使用箭头函数

完整语法：

```js
(参数1, 参数2, ...)=>{
    //函数体
}
```

如果参数只有一个，可以省略小括号

```js
参数 => {

}
```

如果箭头函数只有一条返回语句，可以省略大括号，和return关键字

```js
参数 => 返回值
```

### 注意细节

- 箭头函数中，不存在this、arguments、new.target，如果使用了，则使用的是函数外层的对应的this、arguments、new.target
- 箭头函数没有原型
- 箭头函数不能作用构造函数使用

### 应用场景

1. 临时性使用的函数，并不会可以调用它，比如：
   1. 事件处理函数
   2. 异步处理函数
   3. 其他临时性的函数
2. 为了绑定外层this的函数
3. 在不影响其他代码的情况下，保持代码的简洁，最常见的，数组方法中的回调函数


# 对象
## 5-1. 新增的对象字面量语法

1. 成员速写

如果对象字面量初始化时，成员的名称来自于一个变量，并且和变量的名称相同，则可以进行简写

2. 方法速写

对象字面初始化时，方法可以省略冒号和function关键字

3. 计算属性名

有的时候，初始化对象时，某些属性名可能来自于某个表达式的值，在ES6，可以使用中括号来表示该属性名是通过计算得到的。

## 5-2. Object的新增API

1. Object.is

用于判断两个数据是否相等，基本上跟严格相等（===）是一致的，除了以下两点：

1) NaN和NaN相等
2) +0和-0不相等

2. Object.assign

用于混合对象

3. Object.getOwnPropertyNames 的枚举顺序

Object.getOwnPropertyNames方法之前就存在，只不过，官方没有明确要求，对属性的顺序如何排序，如何排序，完全由浏览器厂商决定。

ES6规定了该方法返回的数组的排序方式如下：

- 先排数字，并按照升序排序
- 再排其他，按照书写顺序排序

4. Object.setPrototypeOf

该函数用于设置某个对象的隐式原型

比如： Object.setPrototypeOf(obj1, obj2)，
相当于：  ``` obj1.__proto__ = obj2 ```

## 类：构造函数的语法糖

### 传统的构造函数的问题

1. 属性和原型方法定义分离，降低了可读性
2. 原型成员可以被枚举
3. 默认情况下，构造函数仍然可以被当作普通函数使用

### 类的特点

1. 类声明不会被提升，与 let 和 const 一样，存在暂时性死区
2. 类中的所有代码均在严格模式下执行
3. 类的所有方法都是不可枚举的
4. 类的所有方法都无法被当作构造函数使用
5. 类的构造器必须使用 new 来调用

## 5-5. 类的其他书写方式

1. 可计算的成员名

2. getter和setter

Object.defineProperty 可定义某个对象成员属性的读取和设置

使用getter和setter控制的属性，不在原型上

3. 静态成员

构造函数本身的成员

使用static关键字定义的成员即静态成员

4. 字段初始化器（ES7）

注意：

1). 使用static的字段初始化器，添加的是静态成员
2). 没有使用static的字段初始化器，添加的成员位于对象上
3). 箭头函数在字段初始化器位置上，指向当前对象

5. 类表达式

6. [扩展]装饰器（ES7）(Decorator)

横切关注点

装饰器的本质是一个函数

## 5-6. 类的继承

如果两个类A和B，如果可以描述为：B 是 A，则，A和B形成继承关系

如果B是A，则：

1. B继承自A
2. A派生B
3. B是A的子类
4. A是B的父类

如果A是B的父类，则B会自动拥有A中的所有实例成员。


新的关键字：

- extends：继承，用于类的定义
- super
  - 直接当作函数调用，表示父类构造函数
  - 如果当作对象使用，则表示父类的原型


注意：ES6要求，如果定义了constructor，并且该类是子类，则必须在constructor的第一行手动调用父类的构造函数

如果子类不写constructor，则会有默认的构造器，该构造器需要的参数和父类一致，并且自动调用父类构造器

【冷知识】

- 用JS制作抽象类
  - 抽象类：一般是父类，不能通过该类创建对象
- 正常情况下，this的指向，this始终指向具体的类的对象
- 
# 解构
## 6-1. 对象解构

### 什么是解构

使用ES6的一种语法规则，将一个对象或数组的某个属性提取到某个变量中

**解构不会对被解构的目标造成任何影响**

### 在解构中使用默认值

```js

{同名变量 = 默认值}

```

### 非同名属性解构

```js

{属性名:变量名}

```

# 符号
## 7-1. 普通符号

符号是ES6新增的一个数据类型，它通过使用函数 ```Symbol(符号描述)``` 来创建

符号设计的初衷，是为了给对象设置私有属性

私有属性：只能在对象内部使用，外面无法使用

符号具有以下特点：

- 没有字面量
- 使用 typeof 得到的类型是 symbol
- **每次调用 Symbol 函数得到的符号永远不相等，无论符号名是否相同**
- 符号可以作为对象的属性名存在，这种属性称之为符号属性
  - 开发者可以通过精心的设计，让这些属性无法通过常规方式被外界访问
  - 符号属性是不能枚举的，因此在 for-in 循环中无法读取到符号属性，Object.keys 方法也无法读取到符号属性
  - Object.getOwnPropertyNames 尽管可以得到所有无法枚举的属性，但是仍然无法读取到符号属性
  - ES6 新增 Object.getOwnPropertySymbols 方法，可以读取符号
- 符号无法被隐式转换，因此不能被用于数学运算、字符串拼接或其他隐式转换的场景，但符号可以显式的转换为字符串，通过 String 构造函数进行转换即可，console.log 之所以可以输出符号，是它在内部进行了显式转换

## 7-2. 共享符号

根据某个符号名称（符号描述）能够得到同一个符号

```js
Symbol.for("符号名/符号描述")  //获取共享符号
```

## 7-3. 知名（公共、具名）符号

知名符号是一些具有特殊含义的共享符号，通过 Symbol 的静态属性得到

ES6 延续了 ES5 的思想：减少魔法，暴露内部实现！

因此，ES6 用知名符号暴露了某些场景的内部实现

1. Symbol.hasInstance

该符号用于定义构造函数的静态成员，它将影响 instanceof 的判定

```js

obj instanceof A

//等效于

A[Symbol.hasInstance](obj) // Function.prototype[Symbol.hasInstance]

```

2. [扩展] Symbol.isConcatSpreadable

该知名符号会影响数组的 concat 方法

3. [扩展] Symbol.toPrimitive

该知名符号会影响类型转换的结果

4. [扩展] Symbol.toStringTag

该知名符号会影响 Object.prototype.toString 的返回值

5. 其他知名符号


# 异步处理
## [回顾]事件循环

JS运行的环境称之为宿主环境。

执行栈：call stack，一个数据结构，用于存放各种函数的执行环境，每一个函数执行之前，它的相关信息会加入到执行栈。函数调用之前，创建执行环境，然后加入到执行栈；函数调用之后，销毁执行环境。

JS引擎永远执行的是执行栈的最顶部。

异步函数：某些函数不会立即执行，需要等到某个时机到达后才会执行，这样的函数称之为异步函数。比如事件处理函数。异步函数的执行时机，会被宿主环境控制。

浏览器宿主环境中包含5个线程：

1. JS引擎：负责执行执行栈的最顶部代码
2. GUI线程：负责渲染页面 
3. 事件监听线程：负责监听各种事件
4. 计时线程：负责计时
5. 网络线程：负责网络通信

当上面的线程发生了某些事请，如果该线程发现，这件事情有处理程序，它会将该处理程序加入一个叫做事件队列的内存。当JS引擎发现，执行栈中已经没有了任何内容后，会将事件队列中的第一个函数加入到执行栈中执行。

JS引擎对事件队列的取出执行方式，以及与宿主环境的配合，称之为事件循环。


事件队列在不同的宿主环境中有所差异，大部分宿主环境会将事件队列进行细分。在浏览器中，事件队列分为两种：

- 宏任务（队列）：macroTask，计时器结束的回调、事件回调、http回调等等绝大部分异步函数进入宏队列
- 微任务（队列）：MutationObserver，Promise产生的回调进入微队列

> MutationObserver用于监听某个DOM对象的变化

当执行栈清空时，JS引擎首先会将微任务中的所有任务依次执行结束，如果没有微任务，则执行宏任务。

## Promise基础
> 本节课的任务：
>
> 1. 理解Promise A+规范的基本概念
> 2. 学会创建Promise
> 3. 学会针对Promise进行后续处理

### 邓哥的烦恼

邓哥心中有很多女神，他今天下定决心，要向这些女神表白，他认为，只要女神够多，根据概率学原理，总有一个会接收他

稳妥起见，邓哥决定使用**串行**的方式进行表白：先给第1位女神发送短信，然后等待女神的回应，如果成功了，就结束，如果失败了，则再给第2位女神发送短信，依次类推

![image-20210618150543263](http://mdrs.yuanjin.tech/img/20210618150543.png)

邓哥的女神一共有4位，名字分别是：李建国、王富贵、周聚财、刘人勇

发短信是一个重复性的劳动，邓哥是个程序员，因此决定用函数封装这个动作

```js
// 向某位女生发送一则表白短信
// name: 女神的姓名
// onFulffiled: 成功后的回调
// onRejected: 失败后的回调
function sendMessage(name, onFulffiled, onRejected) {
  // 模拟 发送表白短信
  console.log(
    `邓哥 -> ${name}：最近有谣言说我喜欢你，我要澄清一下，那不是谣言😘`
  );
  console.log(`等待${name}回复......`);
  // 模拟 女神回复需要一段时间
  setTimeout(() => {
    // 模拟 有10%的几率成功
    if (Math.random() <= 0.1) {
      // 成功，调用 onFuffiled，并传递女神的回复
      onFulffiled(`${name} -> 邓哥：我是九，你是三，除了你还是你😘`);
    } else {
      // 失败，调用 onRejected，并传递女神的回复
      onRejected(`${name} -> 邓哥：你是个好人😜`);
    }
  }, 1000);
}
```

有了这个函数后，邓哥于是开始编写程序发送短信了

```js
// 首先向 李建国 发送消息
sendMessage(
  '李建国',
  (reply) => {
    // 如果成功了，输出回复的消息后，结束
    console.log(reply);
  },
  (reply) => {
    // 如果失败了，输出回复的消息后，向 王富贵 发送消息
    console.log(reply);
    sendMessage(
      '王富贵',
      (reply) => {
        // 如果成功了，输出回复的消息后，结束
        console.log(reply);
      },
      (reply) => {
        // 如果失败了，输出回复的消息后，向 周聚财 发送消息
        console.log(reply);
        sendMessage(
          '周聚财',
          (reply) => {
            // 如果成功了，输出回复的消息后，结束
            console.log(reply);
          },
          (reply) => {
            // 如果失败了，输出回复的消息后，向 刘人勇 发送消息
            console.log(reply);
            sendMessage(
              '刘人勇',
              (reply) => {
                // 如果成功了，输出回复的消息后，结束
                console.log(reply);
              },
              (reply) => {
                // 如果失败了，就彻底没戏了
                console.log(reply);
                console.log('邓哥命犯天煞孤星，注定孤独终老！！');
              }
            );
          }
        );
      }
    );
  }
);
```

该程序完成后，邓哥内心是崩溃的

这一层一层的回调嵌套，形成了传说中的「**回调地狱 callback hell**」

邓哥是个完美主义者，怎么能忍受这样的代码呢？

要解决这样的问题，需要Promise出马

### Promise规范

Promise是一套专门处理异步场景的规范，它能有效的避免回调地狱的产生，使异步代码更加清晰、简洁、统一

这套规范最早诞生于前端社区，规范名称为[Promise A+](https://promisesaplus.com/)

该规范出现后，立即得到了很多开发者的响应

Promise A+ 规定：

1. 所有的异步场景，都可以看作是一个异步任务，每个异步任务，在JS中应该表现为一个**对象**，该对象称之为**Promise对象**，也叫做任务对象

   <img src="http://mdrs.yuanjin.tech/img/20210618154556.png" alt="image-20210618154556558" style="zoom:50%;" />

2. 每个任务对象，都应该有两个阶段、三个状态

   <img src="http://mdrs.yuanjin.tech/img/20210618155145.png" alt="image-20210618155145355" style="zoom:50%;" />

   根据常理，它们之间存在以下逻辑：

   - 任务总是从未决阶段变到已决阶段，无法逆行
   - 任务总是从挂起状态变到完成或失败状态，无法逆行
   - 时间不能倒流，历史不可改写，任务一旦完成或失败，状态就固定下来，永远无法改变

3. `挂起->完成`，称之为`resolve`；`挂起->失败`称之为`reject`。任务完成时，可能有一个相关数据；任务失败时，可能有一个失败原因。

   ![image-20210618160538875](http://mdrs.yuanjin.tech/img/20210618160538.png)

4. 可以针对任务进行后续处理，针对完成状态的后续处理称之为onFulfilled，针对失败的后续处理称之为onRejected

   ![image-20210618161125894](http://mdrs.yuanjin.tech/img/20210618161125.png)

### Promise API

ES6提供了一套API，实现了Promise A+规范

基本使用如下：

```js
// 创建一个任务对象，该任务立即进入 pending 状态
const pro = new Promise((resolve, reject) => {
  // 任务的具体执行流程，该函数会立即被执行
  // 调用 resolve(data)，可将任务变为 fulfilled 状态， data 为需要传递的相关数据
  // 调用 reject(reason)，可将任务变为 rejected 状态，reason 为需要传递的失败原因
});

pro.then(
  (data) => {
    // onFulfilled 函数，当任务完成后，会自动运行该函数，data为任务完成的相关数据
  },
  (reason) => {
    // onRejected 函数，当任务失败后，会自动运行该函数，reason为任务失败的相关原因
  }
);
```

### 邓哥的解决方案

学习了ES6的Promise后，邓哥决定对`sendMessage`函数进行改造，改造结果如下：

```js
// 向某位女生发送一则表白短信
// name: 女神的姓名
// 该函数返回一个任务对象
function sendMessage(name) {
  return new Promise((resolve, reject) => {
    // 模拟 发送表白短信
    console.log(
      `邓哥 -> ${name}：最近有谣言说我喜欢你，我要澄清一下，那不是谣言😘`
    );
    console.log(`等待${name}回复......`);
    // 模拟 女神回复需要一段时间
    setTimeout(() => {
      // 模拟 有10%的几率成功
      if (Math.random() <= 0.1) {
        // 成功，调用 resolve，并传递女神的回复
        resolve(`${name} -> 邓哥：我是九，你是三，除了你还是你😘`);
      } else {
        // 失败，调用 reject，并传递女神的回复
        reject(`${name} -> 邓哥：你是个好人😜`);
      }
    }, 1000);
  });
}
```

之后，就可以使用该函数来发送消息了

```js
sendMessage('李建国').then(
  (reply) => {
    // 女神答应了，输出女神的回复
    console.log(reply);
  },
  (reason) => {
    // 女神拒绝了，输出女神的回复
    console.log(reason);
  }
);
```

> 至此，回调地狱的问题仍然没能解决
>
> 要解决回调地狱，还需要进一步学习Promise的知识

## Promise链式调用
![image-20210618161125894](http://mdrs.yuanjin.tech/img/20210618161125.png)

### catch方法

`.catch(onRejected)` = `.then(null, onRejected)`

### 链式调用

![image-20210621103501094](http://mdrs.yuanjin.tech/img/20210621103501.png)

1. then方法必定会返回一个新的Promise

   可理解为`后续处理也是一个任务`

2. 新任务的状态取决于后续处理：

   - 若没有相关的后续处理，新任务的状态和前任务一致，数据为前任务的数据

   - 若有后续处理但还未执行，新任务挂起。
   - 若后续处理执行了，则根据后续处理的情况确定新任务的状态
     - 后续处理执行无错，新任务的状态为完成，数据为后续处理的返回值
     - 后续处理执行有错，新任务的状态为失败，数据为异常对象
     - 后续执行后返回的是一个任务对象，新任务的状态和数据与该任务对象一致

由于链式任务的存在，异步代码拥有了更强的表达力

```js
// 常见任务处理代码

/*
 * 任务成功后，执行处理1，失败则执行处理2
 */
pro.then(处理1).catch(处理2)

/*
 * 任务成功后，依次执行处理1、处理2
 */
pro.then(处理1).then(处理2)

/*
 * 任务成功后，依次执行处理1、处理2，若任务失败或前面的处理有错，执行处理3
 */
pro.then(处理1).then(处理2).catch(处理3)
```

### 邓哥的解决方案

```js
// 向某位女生发送一则表白短信
// name: 女神的姓名
// onFulffiled: 成功后的回调
// onRejected: 失败后的回调
function sendMessage(name) {
  return new Promise((resolve, reject) => {
    // 模拟 发送表白短信
    console.log(
      `邓哥 -> ${name}：最近有谣言说我喜欢你，我要澄清一下，那不是谣言😘`
    );
    console.log(`等待${name}回复......`);
    // 模拟 女神回复需要一段时间
    setTimeout(() => {
      // 模拟 有10%的几率成功
      if (Math.random() <= 0.1) {
        // 成功，调用 onFuffiled，并传递女神的回复
        resolve(`${name} -> 邓哥：我是九，你是三，除了你还是你😘`);
      } else {
        // 失败，调用 onRejected，并传递女神的回复
        reject(`${name} -> 邓哥：你是个好人😜`);
      }
    }, 1000);
  });
}

sendMessage('李建国')
  .catch((reply) => {
    // 失败，继续
    console.log(reply);
    return sendMessage('王富贵');
  })
  .catch((reply) => {
    // 失败，继续
    console.log(reply);
    return sendMessage('周聚财');
  })
  .catch((reply) => {
    // 失败，继续
    console.log(reply);
    return sendMessage('刘人勇');
  })
  .then(
    (reply) => {
      // 成功，结束
      console.log(reply);
      console.log('邓哥终于找到了自己的伴侣');
    },
    (reply) => {
      // 最后一个也失败了
      console.log(reply);
      console.log('邓哥命犯天煞孤星，无伴终老，孤独一生');
    }
  );
```

## Promise静态方法

### 邓哥的新问题

邓嫂出门时，给邓哥交待了几个任务：

1. 做饭

   可交给电饭煲完成

2. 洗衣服

   可交给洗衣机完成

3. 打扫卫生

   可交给扫地机器人完成

邓哥需要在所有任务结束后给邓嫂汇报工作，哪些成功了，哪些失败了

为了最大程度的节约时间，邓哥希望这些任务同时进行，最终汇总结果统一处理

<img src="http://mdrs.yuanjin.tech/img/20210621142519.png" alt="image-20210621142519937" style="zoom:50%;" />

每个任务可以看做是一个返回Promise的函数

```js
// 做饭
function cook() {
  return new Promise((resolve, reject) => {
    console.log('邓哥打开了电饭煲');
    setTimeout(() => {
      if (Math.random() < 0.5) {
        resolve('饭已ok');
      } else {
        reject('做饭却忘了加水，米饭变成了爆米花');
      }
    }, 2000);
  });
}

// 洗衣服
function wash() {
  return new Promise((resolve, reject) => {
    console.log('邓哥打开了洗衣机');
    setTimeout(() => {
      if (Math.random() < 0.5) {
        resolve('衣服已经洗好');
      } else {
        reject('洗衣服时停水了，洗了个寂寞');
      }
    }, 2500);
  });
}

// 打扫卫生
function sweep() {
  return new Promise((resolve, reject) => {
    console.log('邓哥打开了扫地机器人');
    setTimeout(() => {
      if (Math.random() < 0.5) {
        resolve('地板扫的非常干净');
      } else {
        reject('扫地机器人被哈士奇一爪掀翻了');
      }
    }, 3000);
  });
}

```

如何利用这三个函数实现邓哥的要求呢？

### Promise的静态方法

| 方法名                       | 含义                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| Promise.resolve(data)        | 直接返回一个完成状态的任务                                   |
| Promise.reject(reason)       | 直接返回一个拒绝状态的任务                                   |
| Promise.all(任务数组)        | 返回一个任务<br />任务数组全部成功则成功<br />任何一个失败则失败 |
| Promise.any(任务数组)        | 返回一个任务<br />任务数组任一成功则成功<br />任务全部失败则失败 |
| Promise.allSettled(任务数组) | 返回一个任务<br />任务数组全部已决则成功<br />该任务不会失败 |
| Promise.race(任务数组)       | 返回一个任务<br />任务数组任一已决则已决，状态和其一致       |
|                              |                                                              |

### 邓哥的解决方案

```js
Promise.allSettled([cook(), wash(), sweep()]).then((result) => {
  // 处理汇总结果
  const report = result
    .map((r) => (r.status === 'fulfilled' ? r.value : r.reason))
    .join(';');
  console.log(report);
});
```



## async和await

![image-20210618161125894](http://mdrs.yuanjin.tech/img/20210618161125.png)

### 消除回调

有了Promise，异步任务就有了一种统一的处理方式

有了统一的处理方式，ES官方就可以对其进一步优化

ES7推出了两个关键字`async`和`await`，用于更加优雅的表达Promise

#### async

async关键字用于修饰函数，被它修饰的函数，一定返回Promise

```js
async function method1(){
  return 1; // 该函数的返回值是Promise完成后的数据
}

method1(); // Promise { 1 }

async function method2(){
  return Promise.resolve(1); // 若返回的是Promise，则method得到的Promise状态和其一致
}

method2(); // Promise { 1 }

async function method3(){
  throw new Error(1); // 若执行过程报错，则任务是rejected
}

method3(); // Promise { <rejected> Error(1) }
```

#### await

`await`关键字表示等待某个Promise完成，**它必须用于`async`函数中**

```js
async function method(){
  const n = await Promise.resolve(1);
  console.log(n); // 1
}

// 上面的函数等同于
function method(){
  return new Promise((resolve, reject)=>{
    Promise.resolve(1).then(n=>{
      console.log(n);
      resolve(1)
    })
  })
}
```

`await`也可以等待其他数据

```js
async function method(){
  const n = await 1; // 等同于 await Promise.resolve(1)
}
```

如果需要针对失败的任务进行处理，可以使用`try-catch`语法

```js
async function method(){
  try{
    const n = await Promise.reject(123); // 这句代码将抛出异常
    console.log('成功', n)
  }
  catch(err){
    console.log('失败', err)
  }
}

method(); // 输出： 失败 123
```



### 邓哥表白的完美解决方案

邓哥的女神可不是只有4位，而是40位！

为了更加方便的编写表白代码，邓哥决定把这40位女神放到一个数组中，然后利用async和await轻松完成代码

```js
// 女神的名字数组
const beautyGirls = [
  '梁平',
  '邱杰',
  '王超',
  '冯秀兰',
  '赖军',
  '顾强',
  '戴敏',
  '吕涛',
  '冯静',
  '蔡明',
  '廖磊',
  '冯洋',
  '韩杰',
  '江涛',
  '文艳',
  '杜秀英',
  '丁艳',
  '邓静',
  '江刚',
  '乔刚',
  '史平',
  '康娜',
  '袁磊',
  '龙秀英',
  '姚静',
  '潘娜',
  '萧磊',
  '邵勇',
  '李芳',
  '谭芳',
  '夏秀英',
  '程娜',
  '武杰',
  '崔军',
  '廖勇',
  '崔强',
  '康秀英',
  '余磊',
  '邵勇',
  '贺涛',
];

// 向某位女生发送一则表白短信
// name: 女神的姓名
function sendMessage(name) {
  return new Promise((resolve, reject) => {
    // 模拟 发送表白短信
    console.log(
      `邓哥 -> ${name}：最近有谣言说我喜欢你，我要澄清一下，那不是谣言😘`
    );
    console.log(`等待${name}回复......`);
    // 模拟 女神回复需要一段时间
    setTimeout(() => {
      // 模拟 有10%的几率成功
      if (Math.random() <= 0.1) {
        // 成功，调用 onFuffiled，并传递女神的回复
        resolve(`${name} -> 邓哥：我是九，你是三，除了你还是你😘`);
      } else {
        // 失败，调用 onRejected，并传递女神的回复
        reject(`${name} -> 邓哥：你是个好人😜`);
      }
    }, 1000);
  });
}

// 批量表白的程序
async function proposal() {
  let isSuccess = false;
  for (const girl of beautyGirls) {
    try {
      const reply = await sendMessage(girl);
      console.log(reply);
      console.log('表白成功!');
      isSuccess = true;
      break;
    } catch (reply) {
      console.log(reply);
      console.log('表白失败');
    }
  }
  if (!isSuccess) {
    console.log('邓哥注定孤独一生');
  }
}
proposal();
```



## Promise相关面试题

### 面试题考点

#### Promise的基本概念

![image-20210618161125894](http://mdrs.yuanjin.tech/img/20210618161125.png)

#### 链式调用规则

![image-20210621103501094](http://mdrs.yuanjin.tech/img/20210621103501.png)

![image-20210621103501094](http://mdrs.yuanjin.tech/img/20210621103501.png)

1. then方法必定会返回一个新的Promise

   可理解为`后续处理也是一个任务`

2. 新任务的状态取决于后续处理：

   - 若没有相关的后续处理，新任务的状态和前任务一致，数据为前任务的数据

   - 若有后续处理但还未执行，新任务挂起。
   - 若后续处理执行了，则根据后续处理的情况确定新任务的状态
     - 后续处理执行无错，新任务的状态为完成，数据为后续处理的返回值
     - 后续处理执行有错，新任务的状态为失败，数据为异常对象
     - 后续执行后返回的是一个任务对象，新任务的状态和数据与该任务对象一致

#### Promise的静态方法

| 方法名                       | 含义                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| Promise.resolve(data)        | 直接返回一个完成状态的任务                                   |
| Promise.reject(reason)       | 直接返回一个拒绝状态的任务                                   |
| Promise.all(任务数组)        | 返回一个任务<br />任务数组全部成功则成功<br />任何一个失败则失败 |
| Promise.any(任务数组)        | 返回一个任务<br />任务数组任一成功则成功<br />任务全部失败则失败 |
| Promise.allSettled(任务数组) | 返回一个任务<br />任务数组全部已决则成功<br />该任务不会失败 |
| Promise.race(任务数组)       | 返回一个任务<br />任务数组任一已决则已决，状态和其一致       |
|                              |                                                              |

#### async和await

有了Promise，异步任务就有了一种统一的处理方式

有了统一的处理方式，ES官方就可以对其进一步优化

ES7推出了两个关键字`async`和`await`，用于更加优雅的表达Promise

##### async

async关键字用于修饰函数，被它修饰的函数，一定返回Promise

```js
async function method1(){
  return 1; // 该函数的返回值是Promise完成后的数据
}

method1(); // Promise { 1 }

async function method2(){
  return Promise.resolve(1); // 若返回的是Promise，则method得到的Promise状态和其一致
}

method2(); // Promise { 1 }

async function method3(){
  throw new Error(1); // 若执行过程报错，则任务是rejected
}

method3(); // Promise { <rejected> Error(1) }
```

##### await

`await`关键字表示等待某个Promise完成，**它必须用于`async`函数中**

```js
async function method(){
  const n = await Promise.resolve(1);
  console.log(n); // 1
}

// 上面的函数等同于
function method(){
  return new Promise((resolve, reject)=>{
    Promise.resolve(1).then(n=>{
      console.log(n);
      resolve(1)
    })
  })
}
```

`await`也可以等待其他数据

```js
async function method(){
  const n = await 1; // 等同于 await Promise.resolve(1)
}
```

如果需要针对失败的任务进行处理，可以使用`try-catch`语法

```js
async function method(){
  try{
    const n = await Promise.reject(123); // 这句代码将抛出异常
    console.log('成功', n)
  }
  catch(err){
    console.log('失败', err)
  }
}

method(); // 输出： 失败 123
```

#### 事件循环

根据目前所学，进入事件队列的函数有以下几种：

- `setTimeout`的回调，宏任务（macro task）
- `setInterval`的回调，宏任务（macro task）
- Promise的`then`函数回调，**微任务**（micro task）
- `requestAnimationFrame`的回调，宏任务（macro task）
- 事件处理函数，宏任务(macro task)

### 面试题

1. 下面代码的输出结果是什么

   ```js
   const promise = new Promise((resolve, reject) => {
       console.log(1); 
       resolve(); 
       console.log(2);
   })
   
   promise.then(() => {
       console.log(3);
   })
   
   console.log(4);
   ```

2. 下面代码的输出结果是什么

   ```js
   const promise = new Promise((resolve, reject) => {
       console.log(1); 
       setTimeout(()=>{
         console.log(2)
         resolve(); 
       	console.log(3);
       })
   })
   
   promise.then(() => {
       console.log(4);
   })
   
   console.log(5);
   ```

3. 下面代码的输出结果是什么

   ```js
   const promise1 = new Promise((resolve, reject) => {
   	setTimeout(() => {
       resolve()
     }, 1000)
   })
   const promise2 = promise1.catch(() => {
     return 2;
   })
   
   console.log('promise1', promise1) 
   console.log('promise2', promise2) 
   
   setTimeout(() => {
     console.log('promise1', promise1) 
     console.log('promise2', promise2) 
   }, 2000)
   ```

4. 下面代码的输出结果是什么

   ```js
   async function m(){
     const n = await 1;
     console.log(n);
   }
   
   m();
   console.log(2);
   ```

5. 下面代码的输出结果是什么

   ```js
   async function m(){
     const n = await 1;
     console.log(n);
   }
   
   (async ()=>{
     await m();
     console.log(2);
   })();
   
   console.log(3);
   ```

   

6. 下面代码的输出结果是什么

   ```js
   async function m1(){
     return 1;
   }
   
   async function m2(){
     const n = await m1();
     console.log(n)
     return 2;
   }
   
   async function m3(){
     const n = m2();
     console.log(n);
     return 3;
   }
   
   m3().then(n=>{
     console.log(n);
   });
   
   m3();
   
   console.log(4);
   ```

7. 下面代码的输出结果是什么

   ```js
   Promise.resolve(1)	
     .then(2)
     .then(Promise.resolve(3))
     .then(console.log)
   ```

8. 下面代码的输出结果是什么

   ```js
   var a;
   var b = new Promise((resolve, reject) => {
     console.log('promise1');
     setTimeout(()=>{
       resolve();
     }, 1000);
   }).then(() => {
     console.log('promise2');
   }).then(() => {
     console.log('promise3');
   }).then(() => {
     console.log('promise4');
   });
   
   a = new Promise(async (resolve, reject) => {
     console.log(a);
     await b;
     console.log(a);
     console.log('after1');
     await a
     resolve(true);
     console.log('after2');
   });
   
   console.log('end');
   ```

9. 下面代码的输出结果是什么

   ```js
   async function async1() {
       console.log('async1 start');
       await async2();
       console.log('async1 end');
   }
   async function async2() {
   	console.log('async2');
   }
   
   console.log('script start');
   
   setTimeout(function() {
       console.log('setTimeout');
   }, 0)
   
   async1();
   
   new Promise(function(resolve) {
       console.log('promise1');
       resolve();
   }).then(function() {
       console.log('promise2');
   });
   console.log('script end');
   ```



# FetchApi
## 9-1. Fetch Api 概述

**XMLHttpRequest的问题**

1. 所有的功能全部集中在同一个对象上，容易书写出混乱不易维护的代码
2. 采用传统的事件驱动模式，无法适配新的 Promise Api

**Fetch Api 的特点**

1. 并非取代 AJAX，而是对 AJAX 传统 API 的改进
2. 精细的功能分割：头部信息、请求信息、响应信息等均分布到不同的对象，更利于处理各种复杂的 AJAX 场景
3. 使用 Promise Api，更利于异步代码的书写
4. Fetch Api 并非 ES6 的内容，属于 HTML5 新增的 Web Api
5. 需要掌握网络通信的知识

## 基本使用

> 请求测试地址：http://study.yuanjin.tech/api/local

使用 `fetch` 函数即可立即向服务器发送网络请求

### 参数

该函数有两个参数：

1. 必填，字符串，请求地址
2. 选填，对象，请求配置

**请求配置对象**

- method：字符串，请求方法，默认值 GET
- headers：对象，请求头信息
- body: 请求体的内容，必须匹配请求头中的 Content-Type
- mode：字符串，请求模式
  - cors：默认值，配置为该值，会在请求头中加入 origin 和 referer
  - no-cors：配置为该值，不会在请求头中加入 origin 和 referer，跨域的时候可能会出现问题
  - same-origin：指示请求必须在同一个域中发生，如果请求其他域，则会报错
- credentials: 如何携带凭据（cookie）
  - omit：默认值，不携带 cookie
  - same-origin：请求同源地址时携带 cookie
  - include：请求任何地址都携带 cookie
- cache：配置缓存模式
  - default: 表示 fetch 请求之前将检查下 http 的缓存.
  - no-store: 表示 fetch 请求将完全忽略 http 缓存的存在. 这意味着请求之前将不再检查下 http 的缓存, 拿到响应后, 它也不会更新 http 缓存.
  - no-cache: 如果存在缓存, 那么 fetch 将发送一个条件查询 request 和一个正常的 request, 拿到响应后, 它会更新 http 缓存.
  - reload: 表示 fetch 请求之前将忽略 http 缓存的存在, 但是请求拿到响应后, 它将主动更新 http 缓存.
  - force-cache: 表示 fetch 请求不顾一切的依赖缓存, 即使缓存过期了, 它依然从缓存中读取. 除非没有任何缓存, 那么它将发送一个正常的 request.
  - only-if-cached: 表示 fetch 请求不顾一切的依赖缓存, 即使缓存过期了, 它依然从缓存中读取. 如果没有缓存, 它将抛出网络错误(该设置只在 mode 为”same-origin”时有效).

### 返回值

fetch 函数返回一个 Promise 对象

- 当收到服务器的返回结果后，Promise 进入 resolved 状态，状态数据为 Response 对象
- 当网络发生错误（或其他导致无法完成交互的错误）时，Promise 进入 rejected 状态，状态数据为错误信息

**Response 对象**

- ok：boolean，当响应消息码在 200~299 之间时为 true，其他为 false
- status：number，响应的状态码
- text()：用于处理文本格式的 Ajax 响应。它从响应中获取文本流，将其读完，然后返回一个被解决为 string 对象的 Promise。
- blob()：用于处理二进制文件格式（比如图片或者电子表格）的 Ajax 响应。它读取文件的原始数据，一旦读取完整个文件，就返回一个被解决为 blob 对象的 Promise。
- json()：用于处理 JSON 格式的 Ajax 的响应。它将 JSON 数据流转换为一个被解决为 JavaScript 对象的 promise。
- redirect()：可以用于重定向到另一个 URL。它会创建一个新的 Promise，以解决来自重定向的 URL 的响应。

## Request 对象

除了使用基本的fetch方法，还可以通过创建一个Request对象来完成请求（实际上，fetch的内部会帮你创建一个Request对象）

```js
new Request(url地址, 配置)
```

注意点：

尽量保证每次请求都是一个新的Request对象

## Headers 对象

在Request和Response对象内部，会将传递的请求头对象，转换为Headers

Headers对象中的方法：

- has(key)：检查请求头中是否存在指定的key值
- get(key): 得到请求头中对应的key值
- set(key, value)：修改对应的键值对
- append(key, value)：添加对应的键值对
- keys(): 得到所有的请求头键的集合
- values(): 得到所有的请求头中的值的集合
- entries(): 得到所有请求头中的键值对的集合

## 文件上传

流程：

1. 客户端将文件数据发送给服务器
2. 服务器保存上传的文件数据到服务器端
3. 服务器响应给客户端一个文件访问地址

> 测试地址：http://study.yuanjin.tech/api/upload
> 键的名称（表单域名称）：imagefile

请求方法：POST
请求的表单格式：multipart/form-data
请求体中必须包含一个键值对，键的名称是服务器要求的名称，值是文件数据

> HTML5 中，JS 仍然无法随意的获取文件数据，但是可以获取到 input 元素中，被用户选中的文件数据
> 可以利用 HTML5 提供的 FormData 构造函数来创建请求体

# 迭代器与生成器

## 迭代器

### 背景知识

1. 什么是迭代？

从一个数据集合中按照一定的顺序，不断取出数据的过程

2. 迭代和遍历的区别？

迭代强调的是依次取数据，并不保证取多少，也不保证把所有的数据取完

遍历强调的是要把整个数据依次全部取出

3. 迭代器

对迭代过程的封装，在不同的语言中有不同的表现形式，通常为对象

4. 迭代模式

一种设计模式，用于统一迭代过程，并规范了迭代器规格：

- 迭代器应该具有得到下一个数据的能力
- 迭代器应该具有判断是否还有后续数据的能力

### JS中的迭代器

JS规定，如果一个对象具有next方法，并且该方法返回一个对象，该对象的格式如下：

```js
{value: 值, done: 是否迭代完成}
```

则认为该对象是一个迭代器

含义：

- next方法：用于得到下一个数据
- 返回的对象
  - value：下一个数据的值
  - done：boolean，是否迭代完成

## 可迭代协议 与 for-of 循环

### 可迭代协议

**概念回顾**

- 迭代器(iterator)：一个具有next方法的对象，next方法返回下一个数据并且能指示是否迭代完成
- 迭代器创建函数（iterator creator）：一个返回迭代器的函数

**可迭代协议**

ES6规定，如果一个对象具有知名符号属性```Symbol.iterator```，并且属性值是一个迭代器创建函数，则该对象是可迭代的（iterable）

> 思考：如何知晓一个对象是否是可迭代的？
> 思考：如何遍历一个可迭代对象？

### for-of 循环

for-of 循环用于遍历可迭代对象，格式如下

```js
//迭代完成后循环结束
for(const item of iterable){
    //iterable：可迭代对象
    //item：每次迭代得到的数据
}
```

### 展开运算符与可迭代对象

展开运算符可以作用于可迭代对象，这样，就可以轻松的将可迭代对象转换为数组。

## 生成器 (Generator)

1. 什么是生成器？

生成器是一个通过构造函数Generator创建的对象，生成器既是一个迭代器，同时又是一个可迭代对象

2. 如何创建生成器？

生成器的创建，必须使用生成器函数（Generator Function）

3. 如何书写一个生成器函数呢？

```js
//这是一个生成器函数，该函数一定返回一个生成器
function* method(){

}
```

4. 生成器函数内部是如何执行的？

生成器函数内部是为了给生成器的每次迭代提供的数据

每次调用生成器的next方法，将导致生成器函数运行到下一个yield关键字位置

yield是一个关键字，该关键字只能在生成器函数内部使用，表达“产生”一个迭代数据。

5. 有哪些需要注意的细节？

1). 生成器函数可以有返回值，返回值出现在第一次done为true时的value属性中
2). 调用生成器的next方法时，可以传递参数，传递的参数会交给yield表达式的返回值
3). 第一次调用next方法时，传参没有任何意义
4). 在生成器函数内部，可以调用其他生成器函数，但是要注意加上*号


6. 生成器的其他API

- return方法：调用该方法，可以提前结束生成器函数，从而提前让整个迭代过程结束
- throw方法：调用该方法，可以在生成器中产生一个错误

# 更多的集合类型

## set 集合

> 一直以来，JS只能使用数组和对象来保存多个数据，缺乏像其他语言那样拥有丰富的集合类型。因此，ES6新增了两种集合类型（set 和 map），用于在不同的场景中发挥作用。

**set用于存放不重复的数据**

1. 如何创建set集合

```js
new Set(); //创建一个没有任何内容的set集合

new Set(iterable); //创建一个具有初始内容的set集合，内容来自于可迭代对象每一次迭代的结果

```

2. 如何对set集合进行后续操作

- add(数据): 添加一个数据到set集合末尾，如果数据已存在，则不进行任何操作
  - set使用Object.is的方式判断两个数据是否相同，但是，针对+0和-0，set认为是相等
- has(数据): 判断set中是否存在对应的数据
- delete(数据)：删除匹配的数据，返回是否删除成功
- clear()：清空整个set集合
- size: 获取set集合中的元素数量，只读属性，无法重新赋值

3. 如何与数组进行相互转换

```js
const s = new Set([x,x,x,x,x]);
// set本身也是一个可迭代对象，每次迭代的结果就是每一项的值
const arr = [...s];
```

4. 如何遍历

1). 使用for-of循环
2). 使用set中的实例方法forEach

注意：set集合中不存在下标，因此forEach中的回调的第二个参数和第一个参数是一致的，均表示set中的每一项

## map集合

键值对（key value pair）数据集合的特点：键不可重复

map集合专门用于存储多个键值对数据。

在map出现之前，我们使用的是对象的方式来存储键值对，键是属性名，值是属性值。

使用对象存储有以下问题：

1. 键名只能是字符串
2. 获取数据的数量不方便
3. 键名容易跟原型上的名称冲突


1. 如何创建map

```js
new Map(); //创建一个空的map
new Map(iterable); //创建一个具有初始内容的map，初始内容来自于可迭代对象每一次迭代的结果，但是，它要求每一次迭代的结果必须是一个长度为2的数组，数组第一项表示键，数组的第二项表示值
```

2. 如何进行后续操作

- size：只读属性，获取当前map中键的数量
- set(键, 值)：设置一个键值对，键和值可以是任何类型
  - 如果键不存在，则添加一项
  - 如果键已存在，则修改它的值
  - 比较键的方式和set相同
- get(键): 根据一个键得到对应的值
- has(键)：判断某个键是否存在
- delete(键)：删除指定的键
- clear(): 清空map


3. 和数组互相转换

和set一样

4. 遍历

- for-of，每次迭代得到的是一个长度为2的数组
- forEach，通过回调函数遍历
  - 参数1：每一项的值
  - 参数2：每一项的键
  - 参数3：map本身

## WeakSet 和 WeakMap

### WeakSet

使用该集合，可以实现和set一样的功能，不同的是：

1. **它内部存储的对象地址不会影响垃圾回收**
2. 只能添加对象
3. 不能遍历（不是可迭代的对象）、没有size属性、没有forEach方法

### WeakMap

类似于map的集合，不同的是：

1. **它的键存储的地址不会影响垃圾回收**
2. 它的键只能是对象
3. 不能遍历（不是可迭代的对象）、没有size属性、没有forEach方法


# 代理与反射
## 属性描述符

Property Descriptor 属性描述符  是一个普通对象，用于描述一个属性的相关信息

通过```Object.getOwnPropertyDescriptor(对象, 属性名)```可以得到一个对象的某个属性的属性描述符

- value：属性值
- configurable：该属性的描述符是否可以修改
- enumerable：该属性是否可以被枚举
- writable：该属性是否可以被重新赋值

> ```Object.getOwnPropertyDescriptors(对象)```可以得到某个对象的所有属性描述符

如果需要为某个对象添加属性时 或 修改属性时， 配置其属性描述符，可以使用下面的代码:

```js
Object.defineProperty(对象, 属性名, 描述符);
Object.defineProperties(对象, 多个属性的描述符)
```

### 存取器属性

属性描述符中，如果配置了 get 和 set 中的任何一个，则该属性，不再是一个普通属性，而变成了存取器属性。

get 和 set配置均为函数，如果一个属性是存取器属性，则读取该属性时，会运行get方法，将get方法得到的返回值作为属性值；如果给该属性赋值，则会运行set方法。

存取器属性最大的意义，在于可以控制属性的读取和赋值。

## Reflect

1. Reflect是什么？

Reflect是一个内置的JS对象，它提供了一系列方法，可以让开发者通过调用这些方法，访问一些JS底层功能

由于它类似于其他语言的**反射**，因此取名为Reflect

2. 它可以做什么？

使用Reflect可以实现诸如 属性的赋值与取值、调用普通函数、调用构造函数、判断属性是否存在与对象中  等等功能

3. 这些功能不是已经存在了吗？为什么还需要用Reflect实现一次？

有一个重要的理念，在ES5就被提出：减少魔法、让代码更加纯粹

这种理念很大程度上是受到函数式编程的影响

ES6进一步贯彻了这种理念，它认为，对属性内存的控制、原型链的修改、函数的调用等等，这些都属于底层实现，属于一种魔法，因此，需要将它们提取出来，形成一个正常的API，并高度聚合到某个对象中，于是，就造就了Reflect对象

因此，你可以看到Reflect对象中有很多的API都可以使用过去的某种语法或其他API实现。

4. 它里面到底提供了哪些API呢？

- Reflect.set(target, propertyKey, value): 设置对象target的属性propertyKey的值为value，等同于给对象的属性赋值
- Reflect.get(target, propertyKey): 读取对象target的属性propertyKey，等同于读取对象的属性值
- Reflect.apply(target, thisArgument, argumentsList)：调用一个指定的函数，并绑定this和参数列表。等同于函数调用
- Reflect.deleteProperty(target, propertyKey)：删除一个对象的属性
- Reflect.defineProperty(target, propertyKey, attributes)：类似于Object.defineProperty，不同的是如果配置出现问题，返回false而不是报错
- Reflect.construct(target, argumentsList)：用构造函数的方式创建一个对象
- Reflect.has(target, propertyKey): 判断一个对象是否拥有一个属性
- 其他API：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

## Proxy 代理

代理：提供了修改底层实现的方式

```js

//代理一个目标对象
//target：目标对象
//handler：是一个普通对象，其中可以重写底层实现
//返回一个代理对象
new Proxy(target, handler)
```


# 增强的数组功能


