# JS进阶

## 与现阶段区别

> 之前学习的是JS基础语法, 这个阶段学习一些高级用法和思想

>  学习目标

```
构造函数的作用
深入了解JS对象的各种组成关系: 【如：prototype, __proto__  】显式原型和隐式原型
封装自己插件！【如：new Swiper()  】
类的概念, 继承的概念, 实例化概念 
高阶函数, 回调函数, 递归函数, 立即执行函数, 闭包
深拷贝浅拷贝
正则表达式
ES6新增的语法使用
```

> JS进阶 前2天学习, 为了面试,更深层次的理解JS, 后2天学习, 为了学习更多的属性和方法

## 1. 构造函数 

* 目的: 创建一套模板, 用于批量创建对象

* 语法: function 大写开头函数名() {this身上绑定属性}
* 使用: new 大写开头函数名()
* 实战: 带着大家封装自己第一个插件 tab导航切换

### 1.0 为什么使用构造函数 - 目的

> 批量创建对象

```js

// 需求: 我想创建3个学生信息对象 (因为都是学生,所以有着共同的属性 => 年龄, 地址, 名字)

// 1. 一个个创建 过于麻烦 而且不好整体修改

let obj1 = {
    name: "小黑",
    age: 18,
    address: "上海"
}

let obj2 = {
    name: "小智",
    age: 21, 
    address: "北京"
}

let obj3 = {
    name: "老北",
    age: 23,
    address: "广州"
}

console.log(obj1, obj2, obj3);

```

> ## 工厂函数批量创建

```jsx
// 工厂函数批量创建对象
function createStudent(nameValue, ageValue, address) {
    // 创建一个新对象
    let student = {}; // new Object();
    // 给对象添加对应的属性
    student.name = nameValue;
    student.age = ageValue;
    student.address = address;
    // 返回该对象
    return student;
}

let zs = createStudent('zs', 18, '铁岭');
let ls = createStudent('ls', 20, '上海');
console.log(zs, ls);
// 不足:
// 1. 需要手动创建对象和返回这个对象
// 2. 创建出来的对象类型统一是object, 这样不方便方法共享,继承等...
```

> 自定义构造函数

```jsx
function Student(nameValue, ageValue, address) {
    this.name = nameValue;
    this.age = ageValue;
    this.address = address;
}

let zs = new Student('zs', 18, '铁岭');
let ls = new Student('ls', 20, '上海');

console.log(zs, ls);
// 优势: 
// 1. 不需要手动声明对象和返回对象
// 2. 有具体的类型, 类型是当前的构造函数名称 
// 小结: 通过构造函数配合new调用,可以实现批量创建对象效果,并且这些对象都有公共的属性名
```

> 给实例对象(new出来的对象我们都称之为实例对象)添加公共方法

```js
// 思考: 如果实例对象需要添加公共方法怎么做?
function Student(nameValue, ageValue, address) {
    this.name = nameValue;
    this.age = ageValue;
    this.address = address;
    this.sing = function () {
        console.log('我是一只小小小小鸟');
    };
}

let zs = new Student('zs', 18, '铁岭');
let ls = new Student('ls', 20, '上海');

console.log(zs, ls);
// 需求: 给实例对象添加方法
zs.sing();
ls.sing();    
```

## 2. 原型 

### 2.0 内存浪费问题

```js
// 构造函数
function Person(){
    this.sing = function(){
        console.log("学习唱歌");
    };
}

// 1. 判断多个对象之间的sing值是否相等 - 相等证明是同一个内存空间
// 判断是否相等:
// 基础类型判断: 值, 类型是否相等
// 引用类型判断: 内存地址是否相等
let p1 = new Person();
let p2 = new Person();
console.log(p1.sing === p2.sing); // false (不相同)

// 2. 如果有一万个实例对象, 那么会创建1万个对应的函数, 但是函数功能是一样的, 岂不是浪费?
// 可以回忆一下, 所有的数组都可以使用push,pop等方法... 难不成是每一个数组自己都有一个自己的push么??
```

![1593831109014](C:/Users/14517/Downloads/day01/01-教学资料/images/1593831109014.png)

```
解决方式: 将函数提取出来存到一个变量里面去
1. 写法不优雅
2. 如果有多个公共方法很难维护
```

### 2.1 prototype - 原型属性

* prototype是什么?
  1. 函数本质上也是对象,所以函数也有属性, prototype是函数的一个属性, 取值是一个对象
  2. 每一个函数天生就有prototype这个属性,是JS内置的

* prototype能干什么?

  1. 让所有由该构造函数实例化的对象都来共享这个prototype原型对象里面的值

```js
// 1. Person其实也是一个对象
function Person(){
}
// 查看Person的prototype属性 - 值是一个对象(原型对象)
console.log(Person.prototype);

// 2. 尝试在函数的原型属性上, 添加我们需要共享的数据
Person.prototype.sing = function(){
    console.log("学习唱歌");
}

// 3. 尝试判断
let p1 = new Person();
let p2 = new Person();
console.log(p1.sing === p2.sing); // true

// 总结:
// 在原型对象上添加, 让所有实例对象共享, 可以保证内存的不浪费
```

> 注意: 判断sing的值是否相等, 不要写成了sing()

> prototype是函数特有的属性, 只有函数能使用

### 2.2 `__proto__` 隐式原型

> 问:  为什么每一个实例对象都可以共享到原型对象上的方法呢?

* `__proto__`是什么?

  对象的一个属性,并且是每一个对象都有的一个属性

* `__proto__`能干什么?

  可以通过`__proto__`访问到构造函数的原型对象

```js
function Person(){
}
Person.prototype.sing = function(){
    console.log("学习唱歌");
}
// 1. 构造函数里的属性 - 直接在对象上
let per = new Person();
// 2. 打印实例对象的隐式原型 通过查询实例对象的隐式原型可以直观的看到这个对象的原型上有什么
console.log(per.__proto__);

// 总结:
// "对象的__proto__" 等于 "构造函数的prototype属性"
console.log(per.__proto__ === Person.prototype);   
```

* **`__proto__`, 构造函数, prototype 关系**

![image-20200826145049140](C:/Users/14517/Downloads/day01/01-教学资料/images/image-20200826152118148.png)

> 课堂练习

```
制作一个构造函数,由这个构造函数实例化的对象都拥有sayHi方法
```

## 2.3 原型链

> 问: 下面代码的zs为什么可以调用toString() 方法

> 原型链, 就是每个对象通过`__proto__`去寻找它自身没有的属性/方法来进行调用, 逐级查找, 直到对象的顶端, 没有就报错, 有就调用执行

```js
function Person(){
}
Person.prototype.sing = function(){
    console.log("学习唱歌");
}

let zs = new Person();

// zs调用一个不存在的方法
console.log(zs.toString());
console.log(zs);
```

> ### 查找过程

* zs 对象本身
* zs的`__proto__` 的对象本身
* zs的`__proto__`的`__proto__` 指向的对象 (Object的原型属性)
* zs的`__proto__` 的`__proto__`的`__proto__`的对象找 (发现是null)
* 直接报错: xxxx.xxx is not a function

> 通过隐式原型逐级查找的关系过程, 叫做原型链

> ### 总结

* 函数有prototype, 目的是为了让所有实例对象共享方法使用, 节省内存
* 对象有`__proto__`, 目的为了让对象调用更多的属性和方法

> 提问

```
为什么要有构造函数 ?
为什么需要原型对象(prototype) ?
为什么需要有隐式原型(__proto__)?

1. 为什么所有的日期对象都有getFullYear,getMonth,getDate...等方法
2. 为什么所有的数组都有push,pop,join,sort...等方法
3. 为什么所有的数组,对象,字符串,数字 都可以使用toString
```

### 2.4 instanceof 使用

> 需求: 判断一个变量里面是数组还是对象

> 作用:  用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上 

> 语法:  实例对象  instanceof  构造函数

```js




// ----------------------------------------
// 需求: 判断复杂数据的类型
function Person(){}
let per = new Person();
console.log(per instanceof Person); // true

// ----------------------------------------
// 所有的 字面量 {} 都是 new Object创建出来的
let obj = {};
console.log(obj instanceof Object); // true
console.log(obj instanceof Array); // false

// ----------------------------------------
// 注意:数组本质上也是对象, 所以返回的也是true
let arr = [];
console.log(arr instanceof Array); // true
console.log(arr instanceof Object); // true

// 需求: 制作一个函数 判断是数组还是对象

```

#### 检测类型-通式

```
function getType(target){
return Object.prototype.toString.call(target)
}
```



### 2.5 课上思考

```js
1. 制作一个函数: 接受一个参数, 返回这个参数的数据类型
2. 请正确的往小数组里, 随便添加一个数字, 保证代码不错 [{}, [], {}, []],使用循环判断)
```

## 3. this相关 

> this是一个关键字, 在函数中使用.  (全局的this是window),会根据函数的**调用方式**不同最终的指向也不同
>
> 绝大部分情况下: 函数的this是无法通过定义函数的上下文就可以知道的, **需要通过调用才能够知道** 

| 方式     | 代码                      | this指向   | 备注                                       |
| -------- | ------------------------- | ---------- | ------------------------------------------ |
| 默认绑定 | fn()/定时器/计时器        | window     | 前面默认有window                           |
| 隐式绑定 | 事件处理函数/函数方法调用 | 调用者     | 调用者.函数名()                            |
| new绑定  | new构造函数               | 新对象     | 返回这个新对象                             |
| 硬绑定   | 使用cell/apply/bind方法   | 传入的参数 | call和apply马上执行函数,bind会返回一个函数 |

* **万能口诀: 谁调用就是谁** (除了 new 以外)

```js
// 1. 普通函数调用
function myFn() {
    console.log(this); 
}
myFn();
// 因为前面省略了window. => window.myFn()

// 2. 事件处理函数中 - this指向事件源
let btn = document.querySelector("button");
btn.onclick = function () {
    console.log(this); // btn
}
// btn调用onclick方法执行, 调用者是btn => btn.onclick();

// 3. 对象中函数调用 - 找准函数调用者
let obj = {    
    fn: function () {
        console.log(this); 
    }, 
}
obj.fn();

// 4. 计时器/定时器函数中 - this指向window对象
setTimeout(function () {
    console.log(this); // window对象
}, 2000);

// window调用setTimeout上的函数执行

// 5. new 调用特殊记忆
function Person(){
    console.log(this);
}

new Person(); // 这里this指向的new出来的新对象
Person(); // 这样调用this是window

// 总结: 
// 看好函数在调用时, 点前面的调用者
// new调用函数时, 构造函数内this指向的是new新创建的空对象
```

 * 面试题

```js
let obj = {
    a: {
        fn: function () {
            console.log(this); 
        },
        b: 10
    }
}
obj.a.fn(); 
let temp = obj.a.fn;
temp();

// -------------------------------------------------------------

function Person(theName, theAge){
    this.name = theName;
    this.age = theAge;
}
Person.prototype.sayHello = function(){ // 定义函数
    console.log(this); 
    // console.log('我叫' + this.name + '我今年, ' + this.age + '岁');
}

let per = new Person("小黑", 18);
per.sayHello(); // 调用函数
```

### 3.1 课上练习

```js
练习: 定义ChuanTool构造函数, 属性接受2个数字(numA, numB), 定义2个方法(getMax, randomNum), getMax方法返回numA和numB其中比较大的值, 另一个方法返回一个随机数(范围就是numA到numB, 假设numA是小于numB的) 
要求: 所有由ChuanTool构造函数new出来的对象都可以共享到getMax和randomNum方法.

定义2个实例对象, 分别调用返回打印结果
function ChuanTool(theNumA, theNumB){}

let obj1 = new ChuanTool(10, 20);
let obj2 = new ChuanTool(40, 80);
obj1.getMax(); // 20
obj2.getMax(); // 80
obj1.randomNum(); // 10 - 20的随机数 
obj2.randomNum(); // 40 - 80的随机数

// 1. 方法应该写在谁的身上? 共享方法?
// 2. 如何在共享方法里面获取实例对象的属性
```

### 3.2 抽离面向对象模型

```
构造函数: 负责给实例对象添加公共属性.
原型对象: 负责给实例对象添加公共方法.
实例对象: 我们需要用的,通过 `new 构造函数`得到.

公共属性的取值不同: 在new 调用创建对象的时候传递进去
公共方法的参数不同: 在实例对象调用该方法的时候传递进去
如果公共方法需要用到当前对象自身的公共属性的时候, 可以通过this获取
```

> 练习

```
批量创建老师对象
公共属性: 工号,姓名,爱好
公共方法: 
	上课: 每个老师负责不同的阶段课程
```

## 4. 案例 - 封装tab栏 

> 最终效果: 用户调用new Tab()函数, 传递对应的选择器,不同的数据就可以得到一个新的tab栏

```js
<div class="box" id="box">
    <div class="header"></div>
    <div class="main"></div>
</div>

new Tab('#box', arr1, arr2) // arr1是header数据数组 arr2是main数据数组

//解题思路
//1.构造函数
//2.给构造函数添加公共属性
//3.执行函数
//4.优化
```

### 4.0 案例 - 封装tab栏 - 标签和css

> 准备好标签结构和CSS样式
>

```html
<style>
    .box {
        border: 1px solid #000;
        box-sizing: border-box;
    }
    .box * {
        box-sizing: border-box;
    }
    .box .header {
        width: 100%;
        height: 50px;
        display: flex;
        border-bottom: 1px solid #222;
    }
    .box .header span {
        transition: all 0.3s;
        background-color: #fff;
        line-height: 50px;
        text-align: center;
        cursor: pointer;
        font-size: 16px;
        border-right: 1px solid #000;
        flex: 1;
    }

    .box .header span.active,
    .box .header span:hover {
        background-color: #ccc;
    }

    .box .main {
        width: 100%;
    }

    .box .main div {
        width: 100%;
        text-align: center;
        font-size: 60px;
        font-weight: 800;
        display: none;
    }
    .box .main div.active {
        display: block;
    }
</style>

<body>
    <div class="box">
        <div class="header">
            <span class="active">tab1</span>
            <span>tab2</span>
            <span>tab3</span>
        </div>
        <div class="main">
            <div class="active">内容1</div>
            <div>内容2</div>
            <div>内容3</div>
        </div>
    </div>
</body>
```

### 4.1 案例 - 封装tab栏 - 抽离模型

```js
构造函数: 添加属性 => id, header, main, tab数组
原型对象: 公共方法 => 根据数据生成结构, 点击可以切换
实例对象: tab栏
```

### 4.2 案例 - 封装tab栏 - 添加公共属性

```js
将一些公共属性通过构造函数添加到实例对象里面去
id => 需要new的时候传递进去 代表唯一标识
header => 不需要传递 通过唯一标识找到里面的header模块
main  => 不需要传递 通过唯一标识找到里面的main
tab数组 => 需要传递
```

### 4.3 案例 - 封装tab栏 - 添加公共方法

```
init方法: 根据数据长度,动态添加元素到对应的header和main里面
addClick方法: 给对应的元素添加点击效果
```

## 经验值 

### 问题：对构造函数进行实例化有报错

* 描述：准备好构造函数，进行实例化操作获取对象，出现报错：

* 分析：

  * 控制台有报错的话，直接找到报错的位置进行分析；

  ```js
  let person_1 = New Star("zhangs", 18);
  ```

  * 报错分析：意外？哪里意外了？

  ```
  Unexpected identifier：意外的标识符
  ```

  * 我们说构造函数的函数名一般情况下大写，但是进行实例化的时候，其前面的关键字可不是大写；

* 解决：`new Star("zhangs", 18);`

* 总结：语法记忆一定要清晰，不要张冠李戴；



* 测试：下面实例化正确的是？

```js
function Star(name, age) {
    this.name = name;
    this.age = age;
}

A new Star(1,2);
B newStar(1,2);
C New Star(1,2);
D Star new(1,2);
```



### 问题：构造函数的原型对象上设置方法，实例化对象调用时报错；

* 描述：原型对象上设置方法sing，实例化p1调用时有报错找不到该函数；



* 分析：

  * 报错提醒在44行，查看44处代码：`p1.sing();`
  * p1只是个实例化对象，如果实例化对象报错找不到sing这个方法，那么意味着其构造函数的原型对象上该方法设置有问题
  * 打印`p1.__proto__`：发现其`__proto__`对象上此时是一个名为sing的函数方法；

  

  * `__proto__`和 `构造函数的prototype` 在内存上共用同一个堆地址，于是找到 `prototype`

  ```js
  Star.prototype = function sing() {
      console.log(1);
  }
  ```

  * prototype默认为构造函数的原型对象，是一个对象，上面的代码对这个属性名重新赋值为函数，所以会报错；

* 解决：prototype默认为构造函数的原型对象，原型对象上添加新的方法为：

```js
Star.prototype.sing = function() {
    console.log(1);
}
```

* 总结：各语法都有基本的规则和用法；同学们在使用的过程中应该记忆一些核心的规则；

* 测试：下面在构造函数**Star**的原型对象上进行设置方法，正确的是？

```js
A new Star.prototype.sing = function(){};
B Star.__proto__.sing = function(){};
C Star.prototype.star = function(){};
D Star.__prototype__.sing = function(){};
```

## 今日作业

```js
// 需求1: 尝试给现在系统的Array构造函数添加一个取最大值得函数, 确保下面代码正确
// 提示: 对象调用函数, 本身没有会去哪里找? 所以在调用之前可以在Array函数, 的什么位置上扩展添加方法呢?

/*这里应该补充什么代码呢?*/

/**********************/
Array.prototype.getMax = function () {
  this.arr = arr;
  Math.max(...this.arr);
};
let arr = [5, 2, 1, 10, 3, 4];
console.log(arr.getMax());


// 需求2: 给系统Date构造函数, 添加一个toMyString函数 - 调用函数, 得到今天日期YYYY年MM月DD日 HH:mm:ss 格式的时间
```



# Day02

## 昨日回顾

* 构造函数:
  * 本质: 函数
  * 目的: 定义模板, 批量创建对象
  * 使用: new 构造函数名() - 系统的Array/Date等都是构造函数~

* new关键字:
  * 新建对象, 赋予给构造函数里this的指向

* 原型prototype:
  * 构造函数的属性, 指向一个原型对象, 可以让对象实例共享方法

* `__proto__` 隐式原型
  * 对象的属性, 通过`__proto__ ` 可以访问到该对象的原型 

实例对象.`__proto__`  == 构造函数.prototype (相等)

* 原型链
  * 每个对象通过`__proto__` 属性, 进行逐级查找的关系过程, 叫做原型链

* this指向:

![image-20210220210918112](C:/Users/14517/Downloads/01-教学资料/01-教学资料/images/image-20210220210918112.png)

封装tab栏思路:

   	1. 先写静态的html+css
   	2. 把html变成动态JS创建, 数据自己放到数组中
   	3. 绑定切换功能的点击事件
   	4. 封装构造函数 (属性 和方法) - 调用创建, 传入需要的参数



## 作业讲解

```js
// 作业1: 再尝试给现在系统的Array构造函数添加一个取最大值得函数, 确保下面代码正确
// 提示: 对象调用函数, 本身没有会去哪里找? 所以在调用之前可以在Array函数, 的什么位置上扩展添加方法呢? 思考出来就能知道了.
// 给Array构造函数的prototype上,  添加一个getMax方法
Array.prototype.getMax = function(){
    let max = this[0]
    for (let i = 1; i < this.length; i++) {
      if (this[i] > max) {
        max = this[i]
      }
    }
    return max
}
let arr = [5, 2, 1, 10, 3, 4]
console.log(arr.getMax())

// 作业2: 给系统Date构造函数, 添加一个toMyString函数 - 
// 调用函数, 得到今天日期YYYY年MM月DD日 HH:mm:ss 格式的时间
Date.prototype.toMyString = function () {
  let year = this.getFullYear()
  let month = this.getMonth() + 1
  let day = this.getDate()
  let hour = this.getHours()
  let minutes = this.getMinutes()
  let seconds = this.getSeconds()

  month = month < 10 ? '0' + month : month
  day = day < 10 ? '0' + day : day
  hour = hour < 10 ? '0' + hour : hour
  minutes = minutes < 10 ? '0' + minutes : minutes
  seconds = seconds < 10 ? '0' + seconds : seconds

  return year + "年" + month + "月" + day + "日" + " " + [hour, minutes, seconds].join(":")
}
let date = new Date()
console.log(date.toMyString())
```



## 5. this 相关 - 补充(硬绑定)

| 方式     | 代码                         | this指向   | 备注                                        |
| -------- | ---------------------------- | ---------- | ------------------------------------------- |
| 默认绑定 | fn() / 定时器/ 计时器        | window     | 指向window                                  |
| 隐式绑定 | 事件处理函数 / 函数方法调用  | 调用者     | 调用者.函数名(), 指向调用者                 |
| new绑定  | new 构造函数()               | 新对象     | 指向这个新的实例对象                        |
| 硬绑定   | 使用call / apply / bind 方法 | 传入的参数 | call和apply马上执行函数, bind会返回一个函数 |

都是函数的方法, 可以指定this的指向

* call方法
* apply方法
* bind方法

```js
// 1. call方法, 调用函数的同时, 指定this指向
//    函数名.call(this指向, 参数1, 参数2, ...)
function Person (name, age) {
  this.name = name
  this.age = age
}
let obj = { desc: '不错' }
Person.call(obj, '小鹏', 18) // 硬绑定, 将函数执行的this指向obj (1) 立刻执行fn函数 (2) this指向obj
console.log(obj)

// 2. apply方法, 调用函数的同时, 指定this指向
//    函数名.apply(this指向, [参数1, 参数2, ...])
function Student (name, age) {
  this.name = name
  this.age = age
}
let stu = { className: '80期'}
Student.apply(stu, ['张三', 18]) // 执行Student函数, this指向stu, (注意传值, 必须使用数组格式)
console.log(stu)

// 3. bind方法, 不会立刻执行函数, 而是得到一个新的函数, 并且是绑定死了 this 指向
//    let newFn = fn.bind(this指向)
function Cat (name, age) {
  this.name = name
  this.age = age
}
let cat = { type: '加菲猫' }
let newFn = Cat.bind(cat)
newFn('小花', 2)
console.log(cat)

// 总结: 硬绑定 call apply bind
// (1) call apply bind 都可以替换修改函数this的指向 常用
// (2) call 和 apply 会立马调用函数, call参数需要一个个的传, apply传参必须是数组形式
// (3) bind 会得到一个新函数, 这个函数绑定死了 this 指向
```



## 6. 构造函数补充 - 继承

**为什么要学习继承 ?**

写的构造函数, 定义了一个类型 (人类), 万一项目非常大, 又有了细化的多个类型 (老师, 工人, 学生)

学习继承, 可以让多个构造函数之间建立关联, 便于管理和复用

**什么是继承 ?**

继承: 从别人哪里, 继承东西过来 (财产, 房产)

代码层面的继承: 继承一些属性和方法

### 6.1 继承 - 原型继承

分析需求:

​	人类, 属性: name, age

​	学生, 属性: name, age, className

​	工人, 属性: name, age, companyName

无论学生, 还是工人, => 都是人类, 所以人类原型上有的方法, 他们都应该要有

```js
// 1. 定义Person构造函数
function Person (name, age) {
  this.name = name
  this.age = age
}
Person.prototype.say = function () {
  console.log('人类会说话')
}

// 2. 定义Student构造函数
function Student (name, age, className) {
  this.name = name
  this.age = age
  this.className = className
}
Student.prototype.study = function() {
  console.log('学生在学习')
}

// 3. 原型继承: 利用原型链, 继承于父级构造函数, 继承原型上的方法
// 语法: 子构造函数.prototype = new 父构造函数()
Student.prototype = new Person()

let stu = new Student('张三', 18, '80期')
stu.say()
console.log(stu)
```



### 6.2 继承 - 组合继承方式

组合继承有时候也叫伪经典继承，指的是将原型链 和 借用构造函数 call 技术组合到一块，

从而发挥二者之长的一种继承模式，其背后的思路是使用原型链实现对原型属性和方法的继承 (主要是方法)，

而通过借用构造函数来实现对实例属性的继承。这样既通过在原型上定义方法实现了函数复用，又能保证每个实例都有它的自己的属性。

```js
// 1. 定义Person构造函数
function Person (name, age) {
  this.name = name
  this.age = age
}
Person.prototype.say = function () {
  console.log('人类会说话')
}

// 2. 定义Student构造函数
function Student (name, age, className) {
  Person.call(this, name, age) // 实现属性的继承
  this.className = className
}
Student.prototype.study = function() {
  console.log('学生在学习')
}

// 3. 原型继承: 利用原型链, 继承于父级构造函数, 继承原型上的方法
// 语法: 子构造函数.prototype = new 父构造函数()
Student.prototype = new Person()

let stu = new Student('张三', 18, '80期')
stu.say()
console.log(stu)

// 方法通过 原型继承
// 属性通过 父构造函数的.call(this, name, age)
```



### 6.3 继承 - 寄生组合继承方式

student实例上有 name age,  而原型 `__proto__`上不需要再有这些属性, 所以利用 Object.create 改装下

Object.create(参数对象),  Object.create 会创建一个新对象, 并且这个新对象的`__proto__` 会指向传入参数对象

```js
// 1. 定义Person构造函数
function Person (name, age) {
  this.name = name
  this.age = age
}
Person.prototype.say = function () {
  console.log('人类会说话')
}

// 2. 定义Student构造函数
function Student (name, age, className) {
  Person.call(this, name, age)
  this.className = className
}
Student.prototype.study = function() {
  console.log('学生在学习')
}

// 3. 原型继承: 利用原型链, 继承于父级构造函数, 继承原型上的方法
// 语法: 子构造函数.prototype = new 父构造函数()
Student.prototype = Object.create(Person.prototype)

let stu = new Student('张三', 18, '80期')
stu.say()
console.log(stu)


// 总结:
// Object.create() 以参数的对象, 作为新建对象的__proto__属性的值, 返回新建的对象
```



### 6.4 construtor属性 (了解)

* constructor 是什么?

  它是原型对象里的一个属性, 指向了构造函数方法


```js
function Person (name) {
  this.name = name
}
let p1 = new Person()
console.log(p1.__proto__.constructor === Person) // true
```



## 7. ES6 - 面向对象编程 - 类 

ES6 :  ECMAScript6, 第6代标准(ECMAScript 2015年发布的标准, 也叫ES2015)

ES5:   ECMAScript5, 第5代标准(2009年发布的标准)

注意, 新标准下, 旧标准中的语法也都会保留

> ### 面向过程开发
>
> **面向过程：POP(Process-oriented programming)**
>
> 面向过程就是分析出解决问题所需要的步骤，然后用函数和基础代码, 把这些步骤一步一步实现，
>
> 使用的时候再一个一个的依次调用就可以了
>
> ### 面向对象开发
>
> 面向对象是把事务分解成为一个个对象，然后由对象之间分工与合作。封装成类, 属性, 方法, 进行代码管理, 统一调度.
>
> ### 面向对象和过程区别
>
> 面向过程: 着重于步骤, 亲力亲为  (普通员工的思想)
>
> 面向对象: 着重于对象的分工, 进行拆分调度, 便于维护管理    (老板的思想)
>
> ### 类的概念
>
> 请把以下事物分类
>
> 一只叫大黄的狗, 老王的房子, 老刘的金毛, 小黑的劳斯莱斯车, 小李的别墅, 老北的阿拉斯加
>
> * 什么是类?
>
>   一组相同特征 和 行为的事物的抽象  (类似构造函数)
>
>   特征 -> 属性,  行为 -> 方法   
>
> * 类能干什么?
>
>   定义一套模板, 批量产生对象 (所以跟构造函数思路一样, 就是写的语法不同)

### 7.0 ES6类-创建 - class 

在ES5中, 我们使用的构造函数, 就是在模拟类的思想, 封装一个模板(类), 来批量生成对象

ES6中新增了关键字class, 来定义类, 但是class只是构造函数的语法糖, 底层还是构造函数

> 语法: class 大写开头类名 {}

````js
// 定义类 (类似于之前的构造函数)
// class 类名 {}
class Person {

}
// 创建一个实例对象
let p = new Person()
console.log(p)
````

### 7.1 ES6类-构造函数-constructor

```js
// 定义类
class Person {
  // 构造函数
  constructor (name, age) {
    this.name = name
    this.age = age
  }
  // 定义方法
  sayHi() {
    console.log('你好哇')
  }
}
// 实例化
let p = new Person('zs', 18)
console.log(p)

// 注意:
// 1. 实例属性, 定义在constructor中
// 2. class 是类不是对象, 添加类的方法, 直接写函数名() 即可, 不需要写function
//    每个函数之间, 也不需要逗号和分号
// 3. class中定义的函数, 会被自动挂载到原型对象上
```

### 7.2 ES6类-静态成员和实例成员 - static

**实例成员：由构造函数创建出来的对象能直接访问的属性和方法，包括：对象本身 以及原型中的所有的属性和方法。**

**静态成员：由构造函数直接访问到的属性和方法。**

```js
class Person {
  static aa = 123 // 静态成员
  constructor (name) {
    this.name = name // 实例成员
  }
  sayHello () { // 实例成员
    console.log('你好哇')
  }
  static fn () { // 静态成员
    console.log('静态成员测试')
  }
  static getRandomNum () {
    return Math.floor(Math.random() * 11)
  }
}
let p = new Person('zs')
// 实例成员的使用演示
console.log(p.name) 
p.sayHello()

// 静态成员的使用演示
console.log(Person.aa)
Person.fn()

// 只要被static修饰的属性或者方法, 就是静态成员
// 使用场景: 如果某个方法, 你觉得不需要实例对象来调用, 
// 就可以修饰成static, 用类直接调用 (比如: 一些工具方法 => 产生一个随机数)
```

### 7.3 ES6类-函数里this的值

```js
class Person {
  static aa = 123 // 静态成员
  constructor (name) {
    this.name = name // 实例成员
  }
  sayHello () { // 实例成员
    console.log('你好哇')
    console.log(this) // 指向实例对象
  }
  static getRandomNum () { // 静态成员
    return Math.floor(Math.random() * 11)
    console.log(this) // 指向类
  }
}
let p = new Person('zs')
console.log(p.name)
p.sayHello()

console.log(Person.aa)
console.log(Person.getRandomNum())
```

### 7.4 ES6类-继承 - extends

子类可以通过 extends 继承父类, 把属性和方法, 添加到子类的对象上, 底层还是寄生组合继承方式

* 属性继承原理, 使用 call() 来借调父类的构造函数
* 方法继承原理, 使用 prototype原型, 继承到父类的方法

```js
// 继承关键字 => extends
class Person {
  constructor (name, age) {
    this.name = name
    this.age = age
  }
  jump () {
    console.log('会跳')
  }
}
class Teacher extends Person {
  constructor (name, age, lesson) {
    super(name, age) // extends 中, 必须调用 super(), 会触发执行父类的构造函数
    this.lesson = lesson
    console.log('构造函数执行了')
  }
  sayHello () {
    console.log('会打招呼')
  }
}
let teacher1 = new Teacher('zs', 18, '体育')
console.log(teacher1)
```

### 7.5 ES6-类-子类 - super使用

需求: 子类中要调用到父类中的方法

* 构造函数中使用 super(), 会触发父级类的constructor执行
* 普通函数中使用 super.xxx(), 调用父级类里的函数执行
* ![image-20210221164205414](C:\Users\14517\AppData\Roaming\Typora\typora-user-images\image-20210221164205414.png)

### 7.6 案例-使用class封装tab案例

```js
class CreateTab {
  constructor(query, configObj) {
    this.el = document.querySelector(query)
    this.headerArr = configObj.headerArr
    this.mainArr = configObj.mainArr
    // 调用方法, 进行初始化
    this.init()
    this.addClick()
  }
  init() {
    // 基于 this.el, this.headerArr, this.mainArr 动态的创建结构
    let headerArr = this.headerArr
    let mainArr = this.mainArr
    // 动态创建元素
    let headerDiv = document.createElement('div')
    let mainDiv = document.createElement('div')
    // 添加元素
    this.el.appendChild(headerDiv)
    this.el.appendChild(mainDiv)
    // 配置类名
    headerDiv.className = 'header'
    mainDiv.className = 'main'

    // 生成内部的结构, 遍历headerArr, 动态生成tab标题
    for (let i = 0; i < headerArr.length; i++) {
      let theSpan = document.createElement('span')
      let theDiv = document.createElement('div')
      // 添加元素
      headerDiv.appendChild(theSpan)
      mainDiv.appendChild(theDiv)
      // 控制类名(默认让第一个激活)
      if (i === 0) {
        theSpan.className = 'active'
        theDiv.className = 'active'
      }
      // 控制内容
      theSpan.innerHTML = headerArr[i]
      theDiv.innerHTML = mainArr[i]
    }
  }
  addClick() {
    // 2. 注册点击事件 (给每一个span都要注册事件)
    let spansList = this.el.querySelectorAll('.header > span')
    let divList = this.el.querySelectorAll('.main > div')
    for (let i = 0; i < spansList.length; i++) {
      // 将下标, 记录到dom元素身上, 将来点击事件内部, 就可以通过 this.index 访问到下标
      // spansList[i].index = i
      // 给每个span都注册点击事件
      spansList[i].onclick = function () {
        // 切换 => 控制类名 (排他)
        for (let j = 0; j < spansList.length; j++) {
          spansList[j].className = ''
          divList[j].className = ''
        }
        this.className = 'active'
        divList[i].className = 'active'
      }
    }
  }
}

let tab1 = new CreateTab('#box1', {
  headerArr: ['动物', '明星', '热点'], 
  mainArr: ['内容1', '内容2', '内容3']
})

let tab2 = new CreateTab('#box2', {
  headerArr: ['tab1', 'tab2'],
  mainArr: ['内容1', '内容2']
})
```

## 8. ES5新增数组方法

| 对象调用的方法                                             | 作用      | 返回值   |
| ---------------------------------------------------------- | --------- | -------- |
| array.forEach(function(value, index, array){})             | 遍历      | 无       |
| array.map(function(value, index, array){})                 | 遍历&返回 | 新数组   |
| array.filter(function(value, index, array){ return 条件 }) | 过滤&返回 | 新数组   |
| array.reduce(function(sum, value, index, array) {}, 0)     | 遍历&累计 | 累计结果 |
| array.every(function(value, index, array){ return 条件})   | 遍历&判断 | 布尔值   |
| array.some(function(value, index, array){return 条件})     | 遍历&判断 | 布尔值   |

### 8.0 数组方法使用

```js
let arr = [1, 5, 7, 3, 10, 2, 4]
// 1. forEach可以用于遍历数组
arr.forEach(function(item, index) {
  console.log(item, index)
})

// 2. map() - 遍历 - 收集每次函数return的结果 - 返回全新数组
let resultArr = arr.map(function(item, index) {
  return item * item
})
console.log(resultArr)

// 3. filter() - 过滤 - 收集return true的结果, 返回一个新数组
let result2Arr = arr.filter(function(item, index) {
  return item > 5
})
console.log(result2Arr)

// 4. reduce 累计运算 - 需要返回累计结果
// arr.reduce(function(sum, item, index) { .. }, 0)
let result3 = arr.reduce(function(sum, item, index) {
  return sum + item
}, 0)
console.log(result3)

// 5. every 每个
// 作用: 遍历一个数组, 每个元素都会执行一次函数, 必须每次执行, 都返回true, 最终才会返回true
let flag = arr.every(function(item, index) {
  return item > 0
})
console.log(flag)

// 6. some 某个
// 作用: 遍历一个数组, 每个元素都会执行一次函数, 只要有一次执行, 返回true, 结果就是true
let flag2 = arr.some(function(item, index) {
  return item > 8
})
console.log(flag2)
```

### 8.1 案例

```js
给定一个数组
let list = [
  // wu: 武力    zhi:智力
  { id: 1, name: '张飞', wu: 97, zhi: 10 },
  { id: 2, name: '诸葛亮', wu: 55, zhi: 99 },
  { id: 3, name: '赵云', wu: 97, zhi: 66 },
  { id: 4, name: '周瑜', wu: 80, zhi: 98 },
  { id: 5, name: '吕布', wu: 100, zhi: 8 },
  { id: 6, name: '司马懿', wu: 30, zhi: 98 }
]
求数组中所有英雄的武力平均值
得到一个新数组，只保留英雄的名字， ['张飞', '诸葛亮', '赵云', '周瑜', '吕布', '司马懿']
得到一个新数组，新数组中只保留武力值超过90的英雄
判断数组中所有英雄的武力是否都超过60， 最终打印结果： 全是猛将 / 还有弱鸡
删除数组中所有智力低于60的英雄
```

## 经验值

### 问题：ES6类的继承，出现语法报错；

* 描述：使用ES6对其它构造函数进行继承时，语法报错如下：

![1597494688886](C:/Users/14517/Downloads/01-教学资料/01-教学资料/images/1597494688886.png)

* 分析：

```js
Must call super constructor in derived class before accessing 'this' or returning from derived constructor
在访问“ this”或从派生构造函数返回之前，必须在派生类中调用super 构造函数
```

* 解决：

```js
class Father {
    say() {
        return '我是爸爸';
    }
}
class Son extends Father {
    constructor(x, y) {
        super();
        this.x = x;
    }
}
```

* 总结：无论父亲上面有没有属性，只要在子类中构造器函数内部操作this，就需要super()



## 今日作业

使用class关键字, 自定义时间类MyDate, 继承自系统的时间类Date

**要求1:** 在自定义类MyDate里新增1个方法叫formatDate, 返回格式化的日期YYYY/MM/DD HH:mm:ss (要求, 在方法内, 请使用this, 思考this的指向)

**要求2**: 使用自定义类创建实例对象, 分别调用getFullYear()自带方法, 查看是否成功, 如果成功证明继承成功

**需求3**: 调用自己扩展的方法formatDate, 查看是否会返回当前系统的日期



# Day03

## 昨日回顾

构造函数 - 补充

1. 可以在构造函数的原型属性上, 扩展方法

2. this指向

   * 默认绑定  fn()   指向 window

   * 隐式绑定  xxx.fn()   指向调用者

   * new绑定  this 指向实例对象

     (1) 创建一个新对象

     (2) 将构造函数中的this指向新对象

     (3) 执行构造函数

     (4) 将新对象返回

   * 硬绑定 (bind / call / apply)  想让this指向谁, this就指向谁

     call apply: (1) 会立刻执行函数 (2) 会指定函数执行时的this

     ​	函数名.call(this指向, 参数1, 参数2)

     ​    函数名.apply(this指向, [参数1, 参数2])

     bind: (1) 执行bind时不会调用函数 (2) 拷贝一个一模一样的函数, 并且绑定死新函数的this指向

     ​    let newFn = fn.bind(this指向的内容)

     ​    newFn()

3. 原型继承: 利用原型链, 继承方法  (改造原型链)  (原型替换)

   子构造函数.prototype = new 父构造函数()

4. 组合继承方式: 原型链技术 + 借用构造函数 call 技术, 组合实现继承

   (1) 原型链技术, 继承方法

   子构造函数.prototype = new 父构造函数()

   (2) 借用父构造函数, 继承实例属性

   function Son (name, age, desc) {

   ​	Father.call(this, name, age)

   ​    this.desc = desc

   }

5. 寄生组合继承: Object.create(父类.prototype)

   子构造函数.prototype =  Object.create(父类.prototype)

6. constructor属性 - 原型对象的一个属性, 指向了对应的构造函数

   Person.prototype.constructor === Person

7. 面向过程编程: 着重于步骤, 亲力亲为

   拖地(今天)

8. 面向对象编程: 着重于分工, 统一调度 和 管理, => 封装类(属性+方法), 创建对象, 调用方法进行实现功能, 

   清洁工1.拖地(今天)

   清洁工2.拖地(明天)

9. ES6 - 类 (一系列具有相同特征和行为的事物的抽象)  创建对象的模板

   1.  创建类 => class 类名 {}
   2.  在 constructor函数中,  扩展实例的属性
   3.  静态成员 (被static修饰的属性和方法), 必须由类来调用, 实例成员, 由实例对象来调用
   4.  继承 => extends



## 作业讲解

使用class关键字, 自定义时间类MyDate, 继承自系统的时间类Date

**要求1:** 在自定义类MyDate里新增1个方法叫formatDate, 返回格式化的日期YYYY/MM/DD HH:mm:ss (要求, 在方法内, 请使用this, 思考this的指向)

**要求2**: 使用自定义类创建实例对象, 分别调用getFullYear()自带方法, 查看是否成功, 如果成功证明继承成功

**需求3**: 调用自己扩展的方法formatDate, 查看是否会返回当前系统的日期

```jsx
class MyDate extends Date {
  formatDate () {
    let year = this.getFullYear()
    let month = this.getMonth() + 1
    let day = this.getDate()
    let hour = this.getHours()
    let minutes = this.getMinutes()
    let seconds = this.getSeconds()
    // 补零操作
    month = month < 10 ? '0' + month : month
    day = day < 10 ? '0' + day : day
    hour = hour < 10 ? '0' + hour : hour
    minutes = minutes < 10 ? '0' + minutes : minutes
    seconds = seconds < 10 ? '0' + seconds : seconds
    return `${year}年${month}月${day}日 ${hour}:${minutes}:${seconds}`
  }
}
let date = new MyDate()
console.log(date.getFullYear())
console.log(date.formatDate())
```





## 8. ES5 / ES6 新增数组方法

### 8.0 方法讲解

- **ES5 新增数组方法**

| 对象调用的方法                                            | 作用                        | 返回值   |
| --------------------------------------------------------- | --------------------------- | -------- |
| array.forEach(function(item, index, array){})             | 遍历                        | 无       |
| array.map(function(item, index, array){})                 | 遍历&收集返回的项           | 新数组   |
| array.filter(function(item, index, array){ return 条件 }) | 过滤&保留return true的项    | 新数组   |
| array.reduce(function(sum, item, index, array) {}, 0)     | 遍历&累计求和               | 累计结果 |
| array.every(function(item, index, array){ return 条件})   | 遍历&判断是否都满足条件     | 布尔值   |
| array.some(function(item, index, array){return 条件})     | 遍历&判断是否有某个满足条件 | 布尔值   |
| Array.from(伪数组)                                        | 伪数组转真数组              | 真数组   |

- **ES6 新增数组方法**

| 对象调用的方法                                         | 作用     | 返回值               |
| ------------------------------------------------------ | -------- | -------------------- |
| array.find(function(item, index) { return 条件 })      | 遍历查找 | 找到的项 / undefined |
| array.findIndex(function(item, index) { return 条件 }) | 遍历查找 | 下标 / -1            |

```js
let arr = [1, 5, 7, 3, 10, 2, 4]
// 1. forEach可以用于遍历一个数组, 每个元素都会执行一次函数
// 2. item是形参, 如果是简单类型的形参的修改, 不会影响到原来值
arr.forEach(function(item, index) {
  console.log(item, index)
})

// 2. map() 映射, 遍历数组, 收集每次函数return的结果 - 返回全新数组
let resultArr = arr.map(function(item, index) {
  return item * item
})
console.log(resultArr)

// 3. filter() - 过滤 - 遍历数组, 收集return true的结果, 返回一个新数组
let result2Arr = arr.filter(function(item, index) {
  return item > 5
})
console.log(result2Arr)

// 4. reduce 累加运算 - 遍历数组,需要返回累加的值, 可以进行累加操作
// arr.reduce(function(sum, item, index) { .. }, 起始累加值)
let result3 = arr.reduce(function(sum, item, index) {
  return sum + item
}, 0)
console.log(result3)

// 5. every 每个
// 作用: 遍历一个数组, 每个元素都会执行一次函数, 必须每次执行都返回true, 最终才会返回true
let flag = arr.every(function(item, index) {
  return item > 0
})
console.log(flag)

// 6. some 某个
// 作用: 遍历一个数组, 每个元素都会执行一次函数, 只要有一次执行返回true, 结果就是true
let flag2 = arr.some(function(item, index) {
  return item > 8
})
console.log(flag2)

// 7. Array.from(伪数组) 作用: 将伪数组转换成真数组
let divs = document.querySelectorAll('div')
Array.from(divs).some(function (item, index) {
  if (item.className === 'active') {
    return true
  } else {
    return false
  }
})

// -----------------------------------------------------------
// 8. find 找第一个符合条件的项, 没找到会返回 undefined
let arr2 = [
  { name: 'zs', score: 100 },
  { name: 'ls', score: 99 },
  { name: 'zs', score: 100 }
]
let obj = arr2.find(function(item, index) {
  return item.score === 100
})
console.log(obj)

// 9. findIndex 找第一个符合条件项的下标, 没找到会返回 -1
let index = arr2.findIndex(function(item, index) {
  return item.score === 101
})
console.log(index)
```

### 8.1 案例

```js
// 给定一个数组
let list = [
  // wu: 武力    zhi:智力
  { id: 1, name: '张飞', wu: 97, zhi: 10 },
  { id: 2, name: '诸葛亮', wu: 55, zhi: 99 },
  { id: 3, name: '赵云', wu: 97, zhi: 66 },
  { id: 4, name: '周瑜', wu: 80, zhi: 98 },
  { id: 5, name: '吕布', wu: 100, zhi: 8 },
  { id: 6, name: '司马懿', wu: 30, zhi: 98 }
]
求数组中所有英雄的武力平均值
得到一个新数组，只保留英雄的名字， ['张飞', '诸葛亮', '赵云', '周瑜', '吕布', '司马懿']
得到一个新数组，新数组中只保留武力值超过90的英雄
判断数组中所有英雄的武力是否都超过60， 最终打印结果： 全是猛将 / 还有弱鸡
删除数组中所有智力低于60的英雄
查找 id 为 4 的英雄
找到 name 为 吕布的英雄的下标
```





## 9. 函数补充

### 9.1 递归函数

> 递归函数：函数内部, 自己调用自己, 就是递归

递归的要求：

1. 自己调用自己
2. 要有结束条件（出口）

递归函数主要是`化归思想` ,将一个复杂的问题简单化，主要用于解决数学中的一些问题居多。

- 把要解决的问题，归结为已经解决的问题上。
- 一定要考虑什么时候结束让函数结束，也就是停止递归（一定要有已知条件）

```jsx
// 递归函数: 在一个函数内部, 自己调用自己
// 递归的要求:
// 1. 自己调用自己
// 2. 一定要有结束条件 (否则会死递归)
let count = 0
function tellStory () {
  console.log('从前有座山')
  console.log('山里有座庙')
  console.log('庙里有个老和尚')
  console.log('老和尚再给小和尚讲故事')
  count++
  // 只要小于5遍, 就接着讲, 讲完到第五遍, 就不讲了
  if (count < 5) {
    tellStory()
  }
}
tellStory()
```

练习：

- 计算1-100之间所有数的和

```jsx
// 递归: 化归思想, 复杂问题简单化
// 求 1-100 的和
// getSum(100) = getSum(99) + 100
// getSum(99) = getSum(98) + 99
// ...
// getSum(2) = getSum(1) + 2
// getSum(1) = 1
function getSum (n) {
  if (n === 1) {
    return 1
  }
  return getSum(n - 1) + n
}
console.log(getSum(100))
```





### 9.2 回调函数 (认识专有名词)

一个函数被当成参数传递时, 将来被调用,  这个函数, 我们就称之为回调函数

```js
function my(fn){
    fn(); // 这里调用外部传入进来的函数 - 回调外部的函数体执行 - 这个动作就叫回调函数
}

my(function(){
    console.log("函数执行");
})
// 例如数组的所有方法(filter, map, forEach, every, some, reduce等), 
// 还有addEventListener, 还有计时器, 定时器 (都将函数当成参数传递, 传递的函数都可以称之为回调函数)
```



### 9.3 闭包 - 实现数据私有

**闭包语法: 内层函数, 引用外层函数上的变量, 就可以形成闭包** 

**闭包作用: 可以实现数据私有**   

(当然闭包以前还用于解决其他 js 的一些其他小bug, 但是由于新语法如 let 等的诞生, 就没有这些闭包使用的需求了, 所以记忆这个数据私有的需求即可)

前置理解:

​	全局变量: 声明在全局的, 全局范围都能访问到的

​	局部变量: 函数内声明的, 只有函数内能访问到

​	当函数内部执行完毕, 默认会销毁函数内所声明的局部变量

需求: 定义一个计数器方法, 每点击一次按钮, 就调用一次进行计数, 实现充币一元

```jsx
<button id="btn">充币一元</button>

<script>
  // 全局变量不安全, 很容易被别人修改掉
  let btn = document.querySelector('#btn')
  let money = 0 // 初始余额

  function countMoney () {
    money++
    console.log('已充值游戏币, 当前余额' + money + '元')
  }

  btn.onclick = function() {
    countMoney()
  }
</script>
```

缺点: money 是全局变量, 不安全, 很可能会被修改掉

利用闭包解决:

```jsx
<button id="btn">充币一元</button>

<script>
  let btn = document.querySelector('#btn')

  function countMoney () {
    let money = 0 // 初始余额
    function add() {
      money++
      console.log('已充值游戏币, 当前余额' + money + '元')
    }
    return add
  }
  let addFn = countMoney()

  btn.onclick = function() {
    addFn()
  }
</script>
```



## 10. 浅拷贝与深拷贝(了解)

> 深拷贝与浅拷贝一般是针对于复杂类型而言的。

推荐资源：[javaScript中浅拷贝和深拷贝的实现](https://github.com/wengjq/Blog/issues/3)

浅拷贝：将对象中的各个属性依次进行复制，浅拷贝只复制了一层对象的属性。如果对象属性中还有对象，那么赋值的仅仅是地址。还是会相互影响。

深拷贝：将对象中的各个属性一次进行复制，深拷贝会递归赋值所有层对象的属性。如果对象属性中还有对象，会继续拷贝，这样拷贝出来的对象完全独立。

- 浅拷贝的简易实现

```jsx
// 浅拷贝：将对象中的各个属性依次进行复制，浅拷贝只复制了一层对象的属性。
// 如果对象属性中还有对象，那么赋值的仅仅是地址。还是会相互影响。
let obj = {
  name: 'zs',
  age: 18
}
let obj2 = {}
for (let key in obj) {
  obj2[key] = obj[key]
}
console.log(obj2)
```

​	问题: 浅拷贝, 如果对象的属性中还有对象, 那么拷贝的仅仅是对象的地址, 将来两个对象间还是会互相影响

- 深拷贝的简易实现

```jsx
// 深拷贝：将对象中的各个属性一次进行复制，深拷贝会递归赋值所有层对象的属性。
// 如果对象属性中还有对象，会继续拷贝，这样拷贝出来的对象完全独立。
let xiaoming = {
  name: 'zs',
  age: 18,
  car: {
    name: '劳斯莱斯',
    money: 100
  }
}
function cloneObj (obj) {
  let temp = {}
  for (let k in obj) {
    // 如果对象的属性还是对象，递归复制
    // 如果对象的属性是简单类型，直接复制
    if (typeof obj[k] === 'object') {
      temp[k] = cloneObj(obj[k])
    } else {
      temp[k] = obj[k]
    }
  }
  return temp
}
let newObj = cloneObj(xiaoming)
newObj.car.money = 10
console.log(newObj.car.money)
```

**一般出在面试题中: 考察复杂类型赋值时, 其实赋值的是地址**

后面也有更简单的深拷贝方式, 如: JSON.stringify  和 JSON.parse 可以非常方便的实现深拷贝



## 11. 正则表达式

> 正则表达式：用于匹配字符串规律规则的表达式，在编程语言中有广泛的应用，经常用于表单校验，高级搜索等。

比如: 判断手机号是否符合11位规则等

### 11.0 创建正则表达式

构造函数的方式

```javascript
let regExp = new RegExp(/\d/);
```

正则字面量

```javascript
let regExp = /\d/;
```

正则的使用

```javascript
/\d/.test("aaa1"); // true
```



### 11.1 元字符

> 正则表达式由一些普通字符和元字符组成，普通字符包括大小写字母、数字等，而元字符则具有特殊的含义。

```jsx
// 正则表达式由一些普通字符和元字符组成，
// 普通字符包括大小写字母、数字等，而元字符则具有特殊的含义。
console.log(/a/.test('abc')) // true
console.log(/ac/.test('abc')) // false
```

#### (1) 常见基础元字符

![image-20210223011603200](C:/Users/14517/Downloads/day03/01-教学资料/images/image-20210223011603200.png)

```jsx
// 元字符, 有特殊含义的字符
// \d 数字, 0-9
console.log(/\d/.test('1da')) // true
console.log(/\d/.test('aba')) // false

// \D 非数字, 除了0-9
console.log(/\D/.test('1a1')) // true
console.log(/\D/.test('111')) // false

// \w 匹配单词字符, 字母数字下划线 范围 0-9 a-z A-Z _
console.log(/\w/.test('$_$')) // true
console.log(/\w/.test('$$')) // false
console.log(/\w/.test('123')) // true

// \W 匹配非单词字符
console.log(/W/.test('$$$')) // true
console.log(/W/.test('111')) // false

// . 匹配换行回车以外的任意字符 (没啥太大用, 但是要知道他是一个元字符)
console.log(/./.test('abc')) // true
console.log(/./.test('123')) // true
console.log(/./.test('\n')) // false
```



#### (2) 正则优先级

`|`表示或，优先级最低

`()`优先级最高，表示分组

```jsx
// 看字符串中, 有没有 a 或者 b
console.log(/a|b/.test('bac')) // true
console.log(/a|b/.test('ac')) // true
console.log(/a|b/.test('cd')) // false

// 匹配 ab 或 cd
console.log(/ab|cd/.test('abc')) // true
// 匹配 abd 或者 acd
console.log(/a(b|c)d/.test('abc')) // false
```

**猜一猜**

```jsx
console.log(/b|foot/.test('bbbbb'));

console.log(/(b|f)oot/.test('bbbbb'));
```



#### (3) 正则中括号语法 []

**需求: 匹配 a 或 b 或 c**

`[]`在正则表达式中表示一个字符的位置，[]里面写这个位置可以出现的字符。

```javascript
console.log(/[abc]/);//匹配a,b,c
```

`[^]`在中扩号中的^表示非的意思。

```javascript
//^表示该位置不可以出现的字符
console.log(/[^abc]/);//匹配除了a，b，c以外的其他字符
```

`[a-z]` `[1-9]`表示范围

```javascript
console.log(/[a-z]/.test("d"));//小写字母
console.log(/[A-Z]/.test("d"));//大写字母
console.log(/[0-9]/.test("8"));//数字
console.log(/[a-zA-Z0-9]/);//所有的小写字母和大写字母以及数字
```



#### (4) 正则边界语法 ^ $

> 我们前面学习的学习的正则,  只要有满足的条件的就会返回true，并不能做到精确的匹配。

^表示开头   请注意: ***[ ]里面的^表示取反***

$表示结尾

```javascript
console.log(/a/.test('abc')) // 不严格匹配, 只要有, 就是true

console.log(/^11/.test('11b')) // 必须以11开头, true
console.log(/11$/.test('11b')) // 必须以11结尾, false
console.log(/^11$/.test('11b')) // 必须是 11, false 严格匹配, 严格到字符数都是确定的
```

**小练习:**

```jsx
console.log(/^chuan/.test("dachuan"))
console.log(/^chuan/.test("chuang"))
console.log(/^chuan/.test("chuan"))

console.log(/chuan$/.test("chuan"))
console.log(/chuan$/.test("dachuan"))
console.log(/chuan$/.test("chuang"))

console.log(/^chuan$/.test("chuan"))
console.log(/^chuan$/.test("chuanchuan"))
```



#### (5) 正则量词语法

> 量词用来控制出现的次数，一般来说量词和边界会一起使用

1. `*`表示能够出现0次或者更多次，x>=0;
2. `+`表示能够出现1次或者多次，x>=1
3. `?`表示能够出现0次或者1次，x=0或者x=1
4. `{n}`表示能够出现n次
5. `{n,}`表示能够出现n次或者n次以上
6. `{n,m}`表示能够出现n-m次

```jsx
// a 可以出现, 0次或者1次或者多次
console.log(/^a*$/.test('')) // true
console.log(/^a*$/.test('a')) // true
console.log(/^a*$/.test('aa')) // true

// a 可以出现, 1次或者多次
console.log(/^a+$/.test('')) // false
console.log(/^a+$/.test('a')) // true
console.log(/^a+$/.test('aa')) // true

// a 可以出现, 0次或1次
console.log(/^a?$/.test('')) // true
console.log(/^a?$/.test('a')) // true
console.log(/^a?$/.test('aa')) // false

// 2个a
console.log(/^a{2}$/.test('aaa'))

// 2个a或者以上个a
console.log(/^a{2,}$/.test('aaa'))

// 2-4个a
console.log(/^a{2,4}$/.test('aaa'))
```



### 11.2 正则的使用

#### (1) 表单校验

1. 验证姓名

   - 只能是汉字
   - 长度2-6位之间
   - 汉字范围[\u4e00-\u9fa5]

   ```javascript
   let nameReg = /^[\u4e00-\u9fa5]{2,6}$/;
   ```

2. 验证QQ

   - 数字组成
   - 开头不能是0
   - 长度为5-11位

   ```javascript
   let qqReg = /^[1-9]\d{4,10}$/;
   ```

3. 验证手机

   - 以1开头
   - 第二位要求 3-9
   - 11位数字组成

   ```javascript
   let mobileReg = /^1[3-9]\d{9}$/;
   ```


4. 如若是, 邮箱等特别复杂的, 百度即可

#### (2) 正则替换

需求: 将字符串中所有的 "2b" 和 "2B" 替换为 "**"

```jsx
let str = '你是2b青年, 我就说了, 咋地! 2b, 2b, 2b, 你就是2B青年'
console.log(str.replace(/2b/gi, '**'))
```





## 今日作业

```js
// 思考下面这行代码, 请说出最后的结果 (考察, item是形参, 如果是简单类型的形参的修改, 不会影响到原来值)
let arr = [5, 6, 7, 8];
arr.forEach(function(item){
   item = item + 1;
})
console.log(arr);

let arr2 = [
    { name: 'zs', age: 18 },
    { name: 'zs', age: 20 },
]
arr2.forEach(function(item) {
    item.age = item.age + 1
})
console.log(arr2)
```



```jsx
// 给定一个数组
let studentList = [
  { id: 1, name: '小明', score: 97 },
  { id: 2, name: '小王', score: 55 },
  { id: 3, name: '小刘', score: 85 },
  { id: 4, name: '小刘', score: 80 }
]

1. 请用reduce, 求和后, 算出学生们的平均成绩
2. 请用some判断, 整个班级是否不及格的同学? (60分以下) 
3. 请用find找出, 整个班级中不及格的那一位同学
4. 请用filter筛选出及格的好学生们
```

下面这个 - 明天学完了做

```
请按要求写出, 匹配上海区域座机的正则: 
要求:
- 以021开头
- 第四位 "-"
- 再后面8位数字组成

例如: 021-xxxxxxxx
```



# Day04

## 昨日回顾

1. 新增数组的方法

- **ES5 新增数组方法**

| 对象调用的方法                                            | 作用                        | 返回值   |
| --------------------------------------------------------- | --------------------------- | -------- |
| array.forEach(function(item, index, array){})             | 遍历                        | 无       |
| array.map(function(item, index, array){})                 | 遍历&收集返回的项           | 新数组   |
| array.filter(function(item, index, array){ return 条件 }) | 过滤&保留return true的项    | 新数组   |
| array.reduce(function(sum, item, index, array) {}, 0)     | 遍历&累计求和               | 累计结果 |
| array.every(function(item, index, array){ return 条件})   | 遍历&判断是否都满足条件     | 布尔值   |
| array.some(function(item, index, array){return 条件})     | 遍历&判断是否有某个满足条件 | 布尔值   |
| Array.from(伪数组)                                        | 伪数组转真数组              | 真数组   |

- **ES6 新增数组方法**

| 对象调用的方法                                         | 作用     | 返回值               |
| ------------------------------------------------------ | -------- | -------------------- |
| array.find(function(item, index) { return 条件 })      | 遍历查找 | 找到的项 / undefined |
| array.findIndex(function(item, index) { return 条件 }) | 遍历查找 | 下标 / -1            |

2. 函数补充

   - 递归函数:  函数内部, 自己调用自己, 就是递归

   - 回调函数:  一个函数被当成参数传递时, 将来被调用,  这个函数, 我们就称之为回调函数

   - 闭包: 内层函数, 引用外层函数中的变量, 就可以形成闭包

     作用: 实现数据私有

3. 深拷贝 浅拷贝

4. 正则表达式

   - 正则表达式的创建: 构造函数 / 字面量

   - 元字符:  .  \d   \D   \w   \W

   - 中括号语法:  

     [abc]

     [a-z]

     [^a-z]

   - 正则边界语法

     ^ 以....开头

     $ 以....结尾



## 作业讲解

题目1:

```jsx
// 思考下面这行代码, 请说出最后的结果 (考察, item是形参, 如果是简单类型的形参的修改, 不会影响到原来值)
let arr = [5, 6, 7, 8];
arr.forEach(function(item){
   item = item + 1;
})
console.log(arr);

let arr2 = [
    { name: 'zs', age: 18 },
    { name: 'zs', age: 20 },
]
arr2.forEach(function(item) {
    item.age = item.age + 1
})
console.log(arr2)
```

题目2

```jsx
// 给定一个数组
let studentList = [
  { id: 1, name: '小明', score: 97 },
  { id: 2, name: '小王', score: 55 },
  { id: 3, name: '小刘', score: 85 },
  { id: 4, name: '小刘', score: 80 }
]

1. 请用reduce, 求和后, 算出学生们的平均成绩
2. 请用some判断, 整个班级是否不及格的同学? (60分以下) 
3. 请用find找出, 整个班级中不及格的那一位同学
4. 请用filter筛选出及格的好学生们
```



## 11. 正则表达式

> 正则表达式：用于匹配字符串规律规则的表达式，在编程语言中有广泛的应用，经常用于表单校验，高级搜索等。

比如: 判断手机号是否符合11位规则等

### 11.0 创建正则表达式

构造函数的方式

```javascript
let regExp = new RegExp(/\d/);
```

正则字面量

```javascript
let regExp = /\d/;
```

正则的使用

```javascript
/\d/.test("aaa1"); // true
```



### 11.1 元字符

> 正则表达式由一些普通字符和元字符组成，普通字符包括大小写字母、数字等，而元字符则具有特殊的含义。

```jsx
// 正则表达式由一些普通字符和元字符组成，
// 普通字符包括大小写字母、数字等，而元字符则具有特殊的含义。
console.log(/a/.test('abc')) // true
console.log(/ac/.test('abc')) // false
```

#### (1) 常见基础元字符

![image-20210223011603200](C:/Users/14517/Downloads/01-教学资料/01-教学资料/images/image-20210223011603200.png)

```jsx
// 元字符, 有特殊含义的字符
// \d 数字, 0-9
console.log(/\d/.test('1da')) // true
console.log(/\d/.test('aba')) // false

// \D 非数字, 除了0-9
console.log(/\D/.test('1a1')) // true
console.log(/\D/.test('111')) // false

// \w 匹配单词字符, 字母数字下划线 范围 0-9 a-z A-Z _
console.log(/\w/.test('$_$')) // true
console.log(/\w/.test('$$')) // false
console.log(/\w/.test('123')) // true

// \W 匹配非单词字符
console.log(/W/.test('$$$')) // true
console.log(/W/.test('111')) // false

// . 匹配换行回车以外的任意字符 (没啥太大用, 但是要知道他是一个元字符)
console.log(/./.test('abc')) // true
console.log(/./.test('123')) // true
console.log(/./.test('\n')) // false
```



#### (2) 正则优先级

`|`表示或，优先级最低

`()`优先级最高，表示分组

```jsx
// 看字符串中, 有没有 a 或者 b
console.log(/a|b/.test('bac')) // true
console.log(/a|b/.test('ac')) // true
console.log(/a|b/.test('cd')) // false

// 匹配 ab 或 cd
console.log(/ab|cd/.test('abc')) // true
// 匹配 abd 或者 acd
console.log(/a(b|c)d/.test('abc')) // false
```

**猜一猜**

```jsx
console.log(/b|foot/.test('bbbbb'));

console.log(/(b|f)oot/.test('bbbbb'));
```



#### (3) 正则中括号语法 []

**需求: 匹配 a 或 b 或 c**

`[]`在正则表达式中表示一个字符的位置，[]里面写这个位置可以出现的字符。

```javascript
console.log(/[abc]/);//匹配a,b,c
```

`[^]`在中扩号中的^表示非的意思。

```javascript
//^表示该位置不可以出现的字符
console.log(/[^abc]/);//匹配除了a，b，c以外的其他字符
```

`[a-z]` `[1-9]`表示范围

```javascript
console.log(/[a-z]/.test("d"));//小写字母
console.log(/[A-Z]/.test("d"));//大写字母
console.log(/[0-9]/.test("8"));//数字
console.log(/[a-zA-Z0-9]/);//所有的小写字母和大写字母以及数字
```



#### (4) 正则边界语法 ^ $

> 我们前面学习的学习的正则,  只要有满足的条件的就会返回true，并不能做到精确的匹配。

^表示开头   请注意: ***[ ]里面的^表示取反***

$表示结尾

```javascript
console.log(/a/.test('abc')) // 不严格匹配, 只要有, 就是true

console.log(/^11/.test('11b')) // 必须以11开头, true
console.log(/11$/.test('11b')) // 必须以11结尾, false
console.log(/^11$/.test('11b')) // 必须是 11, false 严格匹配, 严格到字符数都是确定的
```

**小练习:**

```jsx
console.log(/^chuan/.test("dachuan"))
console.log(/^chuan/.test("chuang"))
console.log(/^chuan/.test("chuan"))

console.log(/chuan$/.test("chuan"))
console.log(/chuan$/.test("dachuan"))
console.log(/chuan$/.test("chuang"))

console.log(/^chuan$/.test("chuan"))
console.log(/^chuan$/.test("chuanchuan"))
```



#### (5) 正则量词语法

> 量词用来控制出现的次数，一般来说量词和边界会一起使用

1. `*`表示能够出现0次或者更多次，x>=0;
2. `+`表示能够出现1次或者多次，x>=1
3. `?`表示能够出现0次或者1次，x=0或者x=1
4. `{n}`表示能够出现n次
5. `{n,}`表示能够出现n次或者n次以上
6. `{n,m}`表示能够出现n-m次

```jsx
// a 可以出现, 0次或者1次或者多次
console.log(/^a*$/.test('')) // true
console.log(/^a*$/.test('a')) // true
console.log(/^a*$/.test('aa')) // true

// a 可以出现, 1次或者多次
console.log(/^a+$/.test('')) // false
console.log(/^a+$/.test('a')) // true
console.log(/^a+$/.test('aa')) // true

// a 可以出现, 0次或1次
console.log(/^a?$/.test('')) // true
console.log(/^a?$/.test('a')) // true
console.log(/^a?$/.test('aa')) // false

// 2个a
console.log(/^a{2}$/.test('aaa'))

// 2个a或者以上个a
console.log(/^a{2,}$/.test('aaa'))

// 2-4个a
console.log(/^a{2,4}$/.test('aaa'))
```



### 11.2 正则的使用

#### (1) 表单校验

1. 验证姓名

   - 只能是汉字
   - 长度2-6位之间
   - 汉字范围[\u4e00-\u9fa5]

   ```javascript
   let nameReg = /^[\u4e00-\u9fa5]{2,6}$/
   ```

2. 验证QQ

   - 数字组成
   - 开头不能是0
   - 长度为5-11位

   ```javascript
   let qqReg = /^[1-9]\d{4,10}$/
   ```

3. 验证手机

   - 以1开头
   - 第二位要求 3-9
   - 11位数字组成

   ```javascript
   let mobileReg = /^1[3-9]\d{9}$/
   ```


4. 如若是, 邮箱等特别复杂的, 百度即可

#### (2) 正则替换

需求: 将字符串中所有的 "2b" 和 "2B" 替换为 "**"

//	g:全局

//	i:忽略大小写

```jsx
let str = '你是2b青年, 我就说了, 咋地! 2b, 2b, 2b, 你就是2B青年'
console.log(str.replace(/2b/gi, '**'))
```



## 12. 声明变量 let vs var

### 12.0 let - 块级作用域

> let声明的变量, 块级作用域 - 只能在大括号里使用
>
> var 没有块级作用域, 容易造成变量污染

```js
// 块级作用域: (if大括号/for大括号/while大括号/function大括号/独立的大括号), 
// 在大括号内, 用let声明的变量, 只能在大括号内使用
if (true) {
    let yourName = "老李头"
    console.log(yourName)
}
console.log(yourName)

// 好处: 避免全局变量污染
if (true) {
  var yourName = '老李头'
  console.log(yourName)
}
// 以前声明变量的方式var, 没有块级作用域, 在{}内定义的变量, 会污染全局
console.log(yourName)
```



### 12.2 let - 不挂载到window上

> let 不会挂载到 window 上,  避免污染window原有的全局变量
>
> 全局的 var 会被自动挂载到 window 上, 污染 window 的变量 (不合理)

```js
var a = 10
console.log(window.a)

let a = 10;
console.log(window.a)

// 总结: let声明的全局变量 - 不会挂载到window上(防止污染window原有的全局变量)
```



### 12.3 let - 先定义再使用

let 必须先定义再使用, 不然就会报错 (暂时性死区)

var 存在变量声明提升, **先使用后定义不报错!** 会打印 undefined (不合理)

```js
  console.log(a) // 报错
  let a = 10
```

```jsx
  console.log(a) // undefined
  var a = 10
```



### 12.4 let - 不可重复声明

let 同一个作用域下, 不能重复声明同一个名字的变量

```jsx
let a = 10
let a = 10 // 报错
```

var 就算同一个作用域下, 重复声明了相同的变量名, 也不报错 (不合理)

```jsx
var a = 10
var a = 10
```



## 13. 声明变量 const

### 13.0 const 基本说明

作用: 和let一样, 也是用于定义变量的

* 必须赋予初始值, 且以后无法修改, 所以也叫常量 (无法被修改的量)
* 其他特点和let相同
  * 块级作用域
  * 不挂载到 window 上
  * 必须先声明再使用 (没有变量声明提升)
  * 同一个作用域下, 不能重复声明同一个名字的变量

```js
const num = 100
num = 200 // 报错
console.log(num)

// 应用场景
// 数学, 圆的周长2πr, π的值是固定的
const PI = 3.1415926;
console.log(2 * PI * 10)
```

### 13.1 const - 注意事项

const 声明的变量, 如果存复杂类型,  存的是对象/数组的内存地址

只要地址不改变, 可以修改指向的对象或者数组的内容

```js
// const 声明的变量, 如果存复杂类型, 存的就是对象/数组的内存地址
const obj = {
  name: 'zs',
  age: 18
}
// obj = {} // 报错
obj.name = 'ls' // 正确
```



## 14. 解构赋值

概念:  ES6 允许 按照一个模式, 从目标结构 (数组/对象) 中提取值, 赋予给变量,  **解构的目的方便取值.**

目的: 优化取值的过程

### 14.0 数组的解构

数组的解构

* 完全解构
* 不完全解构赋值

```jsx
// 什么是解构赋值: ES6 允许 按照一个模式, 从数组/对象中提取值, 赋予给变量
// 数组的解构
let arr = ["春天", "夏天", "秋天", "冬天"]
// let winOne = arr[0]
// let winTwo = arr[1]
// let winThree = arr[2]
// let winFour = arr[3]

// 写起来很麻烦, 解构赋值怎么写呢?
// 模式(数组/对象), 两边的模式要对应, 数组的模式: 会按照下标
let [winOne, winTwo, winThree, winFour] = arr // 完全解构
console.log(winThree)

let [winOne, , winFour] = arr // 不完全解构
console.log(winOne, winFour)
```

### 14.1 对象的解构

对象的解构

- 基本解构
- 解构重命名

```js
// 例2: 对象解构举例
let obj = {
  name: 'zs',
  age: 18,
  desc: '不错'
}
// let name = obj.name
// let age = obj.age
// let desc = obj.desc
// 1. 基本解构
let { name, age, desc } = obj
console.log(name, age, desc)

// 2. 解构重命名
let { name: myname, age: myage, desc: mydesc } = obj
console.log(myname, myage, mydesc)

// 3. 解构默认值
let { name, age, desc, girl = '小花' } = obj
console.log(name, age, desc, girl)

// 4. 重命名+默认值
let { name, age, desc, girl: myGirl = '小花' } = obj
console.log(name, age, desc, myGirl)
```



## 15. 箭头函数

### 15.0 箭头函数 - 基础语法

作用: 简化匿名函数, 简化回调函数和普通函数的写法

语法: () => {}, 小括号内书写形参, 大括号内写函数体

使用:

1. 无参, 无返回

   ```jsx
   let fn = () => {
     console.log('我是函数体')
   }
   fn()
   ```

2. 无参, 有返回

   ```jsx
   let fn = () => {
     console.log('我是函数体')
     return '嘎嘎'
   }
   let result = fn()
   console.log(result) // 嘎嘎
   ```

3. 有参, 无返回值

   ```jsx
   let fn = (a, b) => {
     console.log(a + b)
   }
   fn(10, 20)
   ```

4. 有参, 有返回 

   ```jsx
   let fn = (a, b) => {
     return a + b
   }
   let result = fn(10, 20)
   console.log(result)
   ```

### 15.1 箭头函数 - 简化语法

简化:

1. 如果形参只写1个, 可以省略小括号
2. 方法体只有一行代码, 可以省略大括号, 省略大括号后, 默认返回此结果

```jsx
// 如果参数只有一个, 可以省略小括号
let fn = num => {
  console.log(num)
}
fn(100)

// 如果方法体, 只有一行代码, 可以省略大括号 (省略大括号后, 默认返回此结果)
let fn2 = num => num * 10
let result = fn2(5)
console.log(result) // 50
```



### 15.2 箭头函数 - this指向

* 箭头函数:   this指向的是外层作用域this的值

```js
let myObj = {
  a: function () {
    console.log(this)
  },
  b: () => {
    console.log(this)
  }
}
myObj.a() // myObj
myObj.b() // window
```

- **需求: 给按钮添加点击事件, 要求点击按钮, 一秒后, 修改按钮的背景颜色**

```jsx
let btn = document.querySelector('#btn')

btn.onclick = function() {
  setTimeout(() => {
    this.style.backgroundColor = 'skyblue'
  }, 1000)
}
```

小练习: 使用箭头函数, 改写练习题~



### 15.3 箭头 - 其他特征说明

其他特征(了解):

* 箭头函数不能用作构造函数, 不能和new配合使用
* 不能使用arguments



## 16. 剩余参数运算符rest

...剩余参数运算符 -> 函数形参上, 为了 接收所有的参数

- 一般配合箭头函数, 因为箭头函数内不能用arguments
- 必须出现在形参的最后面

```js
// ... 剩余参数运算符 => 用于函数形参上, 可以接收到剩余的参数
function youSum (a, ...args) {
  console.log(a, args)
}
youSum(5, 10)
youSum(5, 100, 200, 300)
youSum(5, 100, 200, 300, 400)

let mySum = (...args) => {
  console.log(args)
}
mySum(10, 20, 30, 40)
```



## 17. 展开运算符 ...

...展开运算符 => 用于数组 或 对象中, 可以将数组的项, 或者对象的键值对展开

```js
// 展开运算符 ...
// 1. 展开数组
let arr = [1, 3, 5, 7]
console.log(...arr) // 打印数组的每一项
console.log(arr[0], arr[1], arr[2], arr[3]) 

// 应用: 
// (1) 拼接两个数组
let arr1 = [1, 2, 3]
let arr2 = [4, 5, 6]
let newArr = [...arr1, ...arr2]
console.log(newArr)

// (2) 求数组最大值
let arr3 = [3, 100, 20]
let max = Math.max(...arr3)
console.log(max)


// 2. 展开对象
let obj1 = {
  name: 'zs',
  age: 18,
  house: '独栋大别墅'
}
let obj2 = {
  money: 100,
  car: '劳斯莱斯'
}
// 相当于快速浅拷贝
let obj3 = {
  ...obj1,
  ...obj2,
  name: 'ls'
}
```



## 18. 函数形参默认值

es6 允许我们在function / 箭头函数中, 对形参赋予默认值

形参的默认值----当不传入参数的时候(形参的值是undefined), 会被默认值覆盖掉

```js
// 默认值作用: 防止在不传递参数时, 有问题
// function fnOne(a, b){
//     console.log(a + b);
// }
// fnOne();

// 其实想在不传数字时, 默认是0 + 0, 但是现在是undefined相加(结果是NaN了)
function fnOne(a = 0, b = 0) {
    console.log(a + b);
}
fnOne();
fnOne(10, 20);

let fnTwo = (a = 0, b = 0) => {
    console.log(a + b);
}
fnTwo();
fnTwo(100, 200);
```



## 19. 对象键值对简化写法

1. 对象属性名, 属性值同名, 可以简写
2. 对象的方法,  "  : function " 可以省略

```js
let x = 10;
let y = 20;
let obj = {
  x, //x:x
  y, // y:y
  getPosition() {
    return obj.x
  }
 /*  
 getPosition function(){
 return obj.x
}
*/
}
```





## 20. ES6新增 数据类型

### 20.0 Set容器

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

```js
let set = new Set([1, 3, 3, 2, 3, 4, 1, 5])
console.log(Array.from(set)) // 数组去重 (伪数组转真数组)
```

- new Set(数组)
- set.add(value)
- set.delete(value)
- set.has(value) 查找  (返回true, 代表存在)
- set.size 容器的长度
- for...of 遍历 
- set.clear() 清空

场景: 去除数组中重复的元素

区别: 数组是有序可重复的,   Set是无序不重复的



### 20.1 Map容器

ES6 提供了新的数据结构 Map。他类似于对象, 是无序不重复的 key-value 的集合体

对象的键: 只能是字符串

Map容器的键: 可以是任意类型

```js
let map = new Map([['name', 'zs'], [25, 100], [document.body, 1]])
```

- Map()
- map.set(key, value)//添加
- map.get(key)
- map.delete(key)
- map.has(key) 查找 存在true
- map.size
- for...of遍历
- map.clear()

区别: Object对象的key只能是字符串, 而Map的key可以是任意类型

