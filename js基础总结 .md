### JavaScript

##  概念

> JavaScript是一种运行在客户端（浏览器）编程语言
>

客户端：**客户端**是相对于服务器而言的，在这里理解为浏览器

浏览器就是一个客户端软件，浏览器从服务器上将资源（html,css,js,图片等）请求下来 并且在本地利用浏览器去解析这些资源
服务器本质上也是一台电脑。用来接收客户端发过来的请求，并处理请求。同时存储数据 读取数据等操作

**总结： Js是运行在用户电脑的一门编程语言**

###  作用
1、网页特效 (监听用户的一些行为让网页作出对应的反馈)
2、表单验证 (针对表单数据的合法性进行判断)
3、数据交互 (获取后台的数据, 渲染到前端)
4、服务端的JS (node.js)
5、命令行工具 (node.js)
6、app
7、游戏开发

总结：JS无所不能，我们学习的方向主要针对的是web页面

### 组成
#### ECMAscript：

 JavaScript的核心，描述了语言的基本语法和数据类型，ECMAScript是一套标准，定义了一种语言的标准与具体实现无关

####  DOM：

​	（document object model 文档对象模型）一套操作页面元素的API

#### BOM：

​	（browser object model  浏览器对象模型）一套操作浏览器功能的API

##  JS的基础

​	初体验

​		运行在客户端- Js是运行在用户电脑的一门编程语言

​		书写环境 :（这对标签可以写在网页的任何位置，一般习惯放在body结束标签的前面）

```html

<script>
    // 弹出框
	alert('Hello world');
</script>

说明:
	1. JS的注释 => // 	快捷键 => ctrl + /
	2. 每写完一句话,使用分号结束(非必须,后期也可使用无分号的规范)
	3. 所有的语法符号都是英文状态,中文状态会报错
```
> 外链式引入

```html
<script src="路径"></script>
```
**注意点:  引用外部js文件的script标签中不可以写JavaScript代码**

**注意点： 尽量将JS脚本放到页面的最底部**

**输出语法**

> 对外输出对应的内容

```js
// 弹出框功能
alert('Hello world');
// 向控制台输出日志
console.log('我是出现在控制台 ，一般用于后期调试代码');
// 对话框功能
prompt('Hello, where are you from?');
// 确认框功能
confirm('Are you sure?');
// 向页面输入对应的值
document.write('你好呀');	
```
###  变量

> 概念：一块被命名的运行存储空间

> 运行存储空间：电脑应用程序在运行的时候开辟的内存空间

简单理解：容器
命名：为了在众多容器中找到对应的容器
容器里面装了什么东西 这个变量就代表什么东西

```js
// 先声明  
let a;
// 后赋值
a = 1;
console.log(a);      

// 声明并赋值 使用最多
let a = 1;
console.log(a);

// 同时声明多个变量 并单独赋值
 let a, b, c, d, e;
 a = 1;
 b = 2;
 c = 3;
 d = 4;
 e = 5;
 console.log(a, b, c, d, e);

// 同时声明多个变量并且直接赋值
let  a = 1,
     b = 2,
     c = 3;

// 不声明直接赋值  不要这么使用 会带来作用域问题
// a = 1;
// console.log(a);

// 不声明不赋值 直接使用   直接报错
// console.log(a);
```
####   变量的规则和规范

+ 规则 不遵守会报错
  - 由字母、数字、下划线、$符号组成，不能以数字开头
  - 不能是关键字和保留字，例如：var for while
    - 关键字：对于JS来说有特殊意义的字符 [查询保留字和关键字]https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords
    - 保留字：现在没有意义 但是保留在那边 以后可能会有意义的字符
  - 严格区分大小写
+ 规范 尽量遵守
  - 变量名必须有意义
  - 遵守驼峰式命名法 首字母小写，后面单词首字母大写 例如：realName ,lastName
+ 交互两个变量的值
  - 借助于另外一个容器
  - 运算（了解即可 基本不用）
```js
	let a = 10;
    let b = 20;

    a = a + b;
    b = a - b;
    a = a - b;
```
> var 声明变量(了解)

```js
var 声明变量不够严谨, 可以先声明 在使用. 同时还可以重复声明
let 声明变量严谨, 必须先声明 在使用, 并且不能重复声明
```

###  数据类型

> 在程序设计中，数据类型被定义为数据的种类，也就是说一系列可能值以及基于这些值的基本操作。
好处：更加充分和高效的利用内存和使用数据

六大数据类型分类 (ES6之前)

基本数据类型（简单数据类型）

​	number 数值型

​	string 字符串

​	boolean 布尔型

​	undefined 未定义

​	null 空引用

引用数据类型（复杂数据类型）

​	object  对象

​	function  函数 

​	array    数组

###   number数据类型

​	所有的数字(不管整数和小数)都是Number数据类型, 多用于计数和数学运算

​	利用typeof运算符可以返回当前数据的数据类型（只针对简单数据类型）

###  string数据类型
​	所有添加了引号的数据都是字符串数据类型 单双引号都可

​	使用场景: 一般用于表示文本数据,描述文案等, 使用的**最为广泛**的一种数据类型

**注意：**单双引号都可以  但是同样的引号不要出现嵌套 如果嵌套 那么 一个单引号 一个双引号  成对

###  布尔数据类型
​	布尔类型只有两个值 `true`或者`false` 一般用于**条件的判断**

###  undefined null 
​	undefined类型只有一个值,就是它本身. **表示未定义** 变量只声明没有赋值的时候浏览器默认会赋值一个`undefined`
null类型也只有一个值,就是它本身,**表示空*****期待**未来这里是一个对象或者还不明确是什么数据类型的时候使用`null`

- [null返回的是一个Object的原因]https://www.zhihu.com/question/21691758

###  数据类型之间的相互转换
​	将数据转换成数值型

​		**parseInt()** => 把字符串转换成整数, 遇到小数点或不能转换的则停止,将前面转换好的输出  ps:常用来提取数字

​		**parseFloat()** => 把字符串转换成小数, 遇到不能转换的停止,将前面转换好的输出

​		**Number** => 在转换的过程中,只要遇到有不能转换的直接输出成NaN

​		**NaN: is not a number** (不是一个有效的数字) 不是一个新的数据类型,是Number类型的一个特殊值

```
在不明确数据是否能够转换成功的时候,使用Number转换,更加安全, 如果明确数据是什么样子,主要针对数据做一些处理的时候,可以使用parse系列 比如 '100px' 转换成100
```

​		将数据转换成字符串

​			**变量.toString()** 将数据转换成字符串

​			**String()** 将数据转换成字符串 有些数据不能用toString 比如 undefined null

​			**任何数据与字符串相加减结果一定是字符串**

​			**任何数据与NaN相加减结果一定是NaN**

​			**NaN与字符串相加减结果一定是字符串(NaN)**

​			将数据转换成布尔类型

​			使用 boolean() 转换

​				true falase

​				0，null，undefined，'',  NaN

​		**黑科技**		

​		当+号的左边没有数据,只有右边有数据的时候, 这个+号就会被解析成正号,正号后面一定是Number 所以JS引擎自动转换 (隐式转换)

**问题**:	

​		**任何数据只需要跟字符串相加最终结果一定是字符串**

​		**数字的计算会出现精度丢失的问题 =>  解决: 扩大到整数**

#####  连接符

​	**模板字符串**

​			es6: `${变量名}`


###   操作符（运算符）

​	用来操作数据的符号，一般用于运算

####  算术运算符

```js
+ - * / %(取余数)
```

####   一元运算符

​	一元运算符：只有一个操作数的运算符

​	**5 + 6  两个操作数的运算符 二元运算符**

​	**++  自身加1**

​	**-- 自身减1**



前置++     先加在赋值

```js
var num1 = 5;
++ num1; 

var num2 = 6;
console.log(num1 + ++ num2);
```

后置++   先赋值在加

```js
var num1 = 5;
num1 ++;    
var num2 = 6 
console.log(num1 + num2 ++);
```

```
++a + a++

模板 1+ 2

++在前模板前部分变 +1 

++在后模板后部分不变

2+2
```



####   逻辑运算符(布尔运算符)

```js
&& 与 两个操作数同时为true，结果为true，否则都是false
|| 或 两个操作数有一个为true，结果为true，否则为false
!  非  取反

若是两个数进行逻辑运算,为真则返回第二个值
console.log(123& & 456); //返回456
```

####  逻辑中断-短路运算

 --- 如果左侧可以判断结果时,就不在运行右侧运算



逻辑与 &&  



表达式1 && 表达式2

如果第一个表达式的值为真,则返回表达式2

如果第一个表达式的值为假,则返回表达式1

如果有空的或者否定的为假 其余的是真的 0 ' ' null undefined NaN

 

逻辑或 || 



如果表达式1 结果为真 则返回表达式一

如果表达式1 结果为假 则返回表达式二 



####  关系运算符(比较运算符)

```
<
>  
>=  大于或等于 
<=  小于或等于
==  判断符号左右的取值是否相等 (只比较数值不比较类型)
!=  符号左右的取值不等于(只比较数值不比较类型)
=== 判断符号左右的取值是否相等 (比较数值,比较类型,优先比较类型)
!== 符号左右的取值完全不等于(比较数值,比较类型,优先比较类型)
```

```js
==与===的区别：==只进行值得比较，===类型和值同时相等，则相等

var result = '55' == 55;  	// true
var result = '55' === 55; 	// false 值相等，类型不相等
var result = 55 === 55; 	// true
```

```
特殊情况:
如果是数值和其他值的比较 ,则其他值会自动转换成数字去比较
console.log(5 > '6') // false
涉及到NaN都是false
console.log('' > '6') // false
如果是字符串与字符串比较 则用每一个字符的ASCII码去比较
console.log('a' > 'b') // false
如果是布尔值参与比较 布尔值会转成数字0和1
console.log(true > false) // true
```

####  赋值运算符

```js
=   直接赋值   
+=  加一个数后再赋值
-=  减一个数后再赋值
*=  乘后再赋值 
/=  除后再赋值
%=  取模后再赋值

例如：
var num = 0;
num += 5;	//相当于  num = num + 5;
```

####  运算符的优先级

```js
优先级从高到底
	1. ()  优先级最高
	2. 一元运算符  ++   --   !
	3. 算数运算符  先*  /  %   后 +   -
	4. 关系运算符  >   >=   <   <=`
	5. 相等运算符   ==   !=    ===    !==
	6. 逻辑运算符 先 与&&   后或||
	7. 赋值运算符
	8. 逗号运算符
```

###  流程控制

####  程序的三种基本结构

#####  顺序结构

 从上到下执行的代码就是顺序结构

程序默认就是由上   到下顺序执行的

##### 分支结构	

根据不同的情况，执行对应代码

##### 循环结构

循环结构：重复做一件事情

####  分支结构

##### if语句

```js
//代码的执行
// 一条语句
if (/* 条件表达式 */) {
  // 执行语句
}

//------------------------------------------------//
// 两天语句
if (/* 条件表达式 */){
  // 成立执行语句
} else {
  // 否则执行语句
}
 
//-----------------------------------------------//
// 多条语句
if (/* 条件1 */){
  // 成立执行语句
} else if (/* 条件2 */){
  // 成立执行语句
} else if (/* 条件3 */){
  // 成立执行语句
} else {
  // 最后默认执行语句
}
```

```js
//可以返回一个结果
三元运算符: 条件 ? 表达式一(true) : 表达式二(false)
也可 console.log( 条件 ? 表达式一(true) : 表达式二(false))
求三个数的最大值:
let num1 = +prompt("请输入第一个数");
let num2 = +prompt("请输入第二个数");
let num3 = +prompt("请输入第三个数");
let max = num1 > num2 ? num1 :num2;
max = max > num3 ? max : num3;
console.log(max);
//判断输入的值做判断
//因为NaN不等于任何值,所以无法判断NaN的存在
//检测输入的是不是NaN可以用isNaN进行判断 ,结果为true则是NaN,反之不是
isNan(a);
```



##### switch case语句

```js
switch (expression) {
  case 常量1:
    语句;
    break;
  case 常量2:
    语句;
    break;
  case 常量3:
    语句;
    break;
  …
  case 常量n:
    语句;
    break;
  default:
    语句;
    break;
}


// break可以省略，如果省略，代码会继续执行下一个case //
// switch 语句在比较值时使用的是全等操作符, 因此不会发生类型转换（例如，字符串'10' 不等于数值 10）// 
```

##### 布尔类型的隐式转换

流程控制语句会把后面的值隐式转换成布尔类型

```
转换为true   非空字符串  非0数字  true 任何对象
转换成false  空字符串  0  false  null  undefined
```

####  循环结构

在javascript中，循环语句有三种，while、do..while、for循环。

##### while语句

```js
// 当循环条件为true时，执行循环体，
// 当循环条件为false时，结束循环。
while (循环条件) {
  //循环体
}
```

##### do...while语句

do..while循环和while循环非常像，二者经常可以相互替代，但是do..while的特点是不管条件成不成立，都会执行一次。

```js
do {
  // 循环体;
} while (循环条件);
```

##### for语句

​		while和do...while一般用来解决无法确认次数的循环。for循环一般在循环次数确定的时候比较方便

for循环语法：

```js
// for循环的表达式之间用的是;号分隔的，千万不要写成,
for (初始化表达式1; 判断表达式2; 自增表达式3) {
  // 循环体4
}
// 循环输出10次
0
for (i=0;i<10;i++){
    conselo.log('1');
}
```

执行顺序：1243  ----  243   -----243(直到循环条件变成false)

1. 初始化表达式
2. 判断表达式
3. 自增表达式
4. 循环体

### continue和break



> break:立即跳出整个循环，即**循环结束**，开始执行循环后面的内容（直接跳到大括号）-- 用于结果已经达到了,后续的循环不需要的时候可以使用
>
> continue:立即跳出当前循环，继续下一次循环（跳到i++的地方）--  排除或者跳过的时候

### 调试

- 过去调试JavaScript的方式
  - alert()
  - console.log()
- 断点调试

>断点调试是指自己在程序的某一行设置一个断点，调试时，程序运行到这一行就会停住，然后你可以一步一步往下调试，调试过程中可以看各个变量当前的值，出错的话，调试到出错的代码行即显示错误，停下。

- 调试步骤

  ```
  浏览器中按F12-->sources-->找到需要调试的文件-->在程序的某一行设置断点
  ```

- 调试中的相关操作

  ```
  tips: 监视变量，不要监视表达式，因为监视了表达式，那么这个表达式也会执行。
  
  1. 代码调试的能力非常重要，只有学会了代码调试，才能学会自己解决bug的能力。初学者不要觉得调试代码麻烦就不去调试，知识点花点功夫肯定学的会，但是代码调试这个东西，自己不去练，永远都学不会。
  2. 今天学的代码调试非常的简单，只要求同学们记住代码调试的这几个按钮的作用即可，后面还会学到很多的代码调试技巧。
  Watch: 监视，通过watch可以监视变量的值的变化，非常的常用。
  F10: 程序单步执行，让程序一行一行的执行，这个时候，观察watch中变量的值的变化。
  F8：跳到下一个断点处，如果后面没有断点了，则程序执行结束。
  ```

  tips: ***监视变量，不要监视表达式，因为监视了表达式，那么这个表达式也会执行。***

  1. 代码调试的能力非常重要，只有学会了代码调试，才能学会自己解决bug的能力。初学者不要觉得调试代码麻烦就不去调试，知识点花点功夫肯定学的会，但是代码调试这个东西，自己不去练，永远都学不会。
  2. 今天学的代码调试非常的简单，只要求同学们记住代码调试的这几个按钮的作用即可，后面还会学到很多的代码调试技巧。

## 数组

### 为什么要学习数组

> 之前学习的数据类型，只能存储一个值(比如：Number/String。我们想存储班级中所有学生的姓名，此时该如何存储？

###  数组的概念

> 所谓数组，就是将多个元素（通常是同一类型）按一定顺序排列放到一个集合中，那么这个集合我们就称之为数组。

### 数组的定义

> 数组是一个有序的列表，可以在数组中存放任意的数据，并且数组的长度可以动态的调整。

通过数组字面量创建数组

```
// 创建一个空数组
var arr1 = []; 
// 创建一个包含3个数值的数组，多个数组项以逗号隔开
var arr2 = [1, 3, 4]; 
// 创建一个包含2个字符串的数组
var arr3 = ['a', 'c']; 

// 可以通过数组的length属性获取数组的长度
console.log(arr3.length);
// 可以设置length属性改变数组中元素的个数
arr3.length = 0;
```

### 获取数组元素

数组的取值

```js
// 格式：数组名[下标]	下标又称索引
// 功能：获取数组对应下标的那个值，如果下标不存在，则返回undefined。
var arr = ['red', 'green', 'blue'];
arr[0];	// red
arr[2]; // blue
arr[3]; // undefined  这个数组的最大下标为2,因此返回undefined
```

### 遍历数组

> 遍历：遍及所有，对数组的每一个元素都访问一次就叫遍历。

数组遍历的基本语法：

```js
for(var i = 0; i < arr.length; i++) {
	// 数组遍历的固定结构
	console.log(arr[i]);
	
}
```

翻转

```js
let arr=[1,2,3,4,5]；
let arr1=[];
for(let i=arr.length-1;i>=0;i--){
console.log(arr[i]);
// 将反转后的加入新数组
arr1.push(arr[i]);
}
console.log(arr);
console.log(arr1);
```



### 数组中-增删改查-元素

数组的赋值

```js
// 修改 格式：数组名[下标/索引] = 值;
// 查询 格式：数组名[下标/索引];
// 删除 格式：数组名.splice(下标,删除个数 )
// 新增 格式：数组名[数组名.length] = ' ' ; 不常用
// 新增 格式：数组名.push(增加的数据);          
// 如果下标有对应的值，会把原来的值覆盖，如果下标不存在，会给数组新增一个元素。
var arr = ["red", "green", "blue"];
// 把red替换成了yellow
arr[arr.length] = "yellow";
// 给数组新增加了一个pink的值
arr[3] = "pink";
```

## 函数

### 为什么要有函数

> 如果要在多个地方求1-100之间所有数的和，应该怎么做？

### 什么是函数

>把一段相对独立的具有特定功能的代码块封装起来，形成一个独立实体，就是函数，起个名字（函数名），在后续开发中可以**反复调用**
>
>函数的作用就是封装一段代码，将来可以重复使用

### 函数的定义

- 函数声明

```javascript
function 函数名 () {
  // 函数体
}
```

- 函数表达式

```javascript
var fn = function() {
  // 函数体
}
```

- 特点：

  函数声明的时候，函数体并不会执行，只要当函数被调用的时候才会执行。
  函数一般都用来干一件事情，需用使用动词+名词，表示做一件事情 `tellStory` `sayHello`等

### 函数的调用

**与循环的不同**：

**1.循环是一次性重复对应的次数**

**2.函数可以根据场景的不同去复用**

- 调用函数的语法：

```javascript
//函数名();
```

特点：

​		声明一次

​		可多次调用

函数体只有在调用的时候才会执行，调用需要()进行调用。
可以调用多次(重复使用)

代码示例：

```javascript
// 声明函数
function sayHi() {
  console.log("吃了没？");
}
// 调用函数
sayHi();

// 求1-100之间所有数的和
function getSum() {
  var sum = 0;
  for (var  i = 0; i < 100; i++) {
    sum += i;
  }
  console.log(sum);
}
// 调用
getSum();
```

### 函数的参数

- 为什么要有参数

```javascript
function getSum() {
  var sum = 0;
  for (var i = 1; i <= 100; i++) {
    sum += i;
  }
  console.log();
}

// 虽然上面代码可以重复调用，但是只能计算1-100之间的值
// 如果想要计算n-m之间所有数的和，应该怎么办呢？
```

- 语法：

```javascript
// 函数内部是一个封闭的环境，可以通过参数的方式，把外部的值传递给函数内部
// 带参数的函数声明
function 函数名(形参1, 形参2, 形参...){
  // 函数体
}

// 带参数的函数调用
函数名(实参1, 实参2, 实参3);
```

- 形参和实参

  > 1. 形式参数：在声明一个函数的时候，为了函数的功能更加灵活，有些值是固定不了的，对于这些固定不了的值。我们可以给函数设置参数。这个参数没有具体的值，**仅仅起到一个占位置的作用**，我们通常称之为形式参数，也叫形参。
  > 2. 实际参数：如果函数在声明时，设置了形参，那么在函数调用的时候就需要传入对应的参数，我们把传入的参数叫做实际参数，也叫实参。
  > 3. **实参传递给形参的过程叫传参**

```javascript
let x = 5, y = 6;
fn(x,y); 
function fn(a, b) {
  console.log(a + b);
}
//x,y实参，有具体的值。函数执行的时候会把x,y复制一份给函数内部的a和b，函数内部的值是复制的新值，无法修改外部的x,y
```

### 案例

### 函数的返回值

>当函数执行完的时候，并不是所有时候都要把结果打印。我们期望函数给我一些反馈（比如计算的结果返回进行后续的运算），这个时候可以让函数返回一些东西。也就是返回值。函数通过return返回一个返回值

返回值语法：

```javascript
//声明一个带返回值的函数
function 函数名(形参1, 形参2, 形参...){
  //函数体
  return 返回值;
}

//可以通过变量来接收这个返回值
var 变量 = 函数名(实参1, 实参2, 实参3);
```

函数的调用结果就是返回值，因此我们可以直接对函数调用结果进行操作。

返回值详解：
    如果函数没有显示的使用 return语句 ，那么函数有默认的返回值：undefined
    如果函数使用 return语句，那么跟再return后面的值，就成了函数的返回值
    如果函数使用 return语句，但是return后面没有任何值，那么函数的返回值也是：undefined
    函数使用**return**语句后，这个函数会在执行完 return 语句之后停止并立即退出，也就是说**return**后面的所有其他代码都**不会再执行。**

**函数里面的循环可以使用return**

**循环的返回是break;**    

```js
//推荐的做法是要么让函数始终都返回一个值，要么永远都不要返回值。
```

### 案例

- 求阶乘
- 求1!+2!+3!+....+n!
- 求一组数中的最大值
- 求一组数中的最小值

### arguments的使用

> JavaScript中，arguments对象是比较特别的一个对象，实际上是当前函数的一个内置属性。也就是说所有函数都内置了一个arguments对象，arguments对象中存储了传递的所有的实参。arguments是一个伪数组，因此及可以进行遍历

- 案例 

```javascript
//求任意个数的最大值
//求任意个数的和
```

### 案例

```javascript
//求斐波那契数列Fibonacci中的第n个数是多少？      1 1 2 3 5 8 13 21...
//翻转数组，返回一个新数组
//对数组排序，从小到大
//输入一个年份，判断是否是闰年[闰年：能被4整数并且不能被100整数，或者能被400整数]
//输入某年某月某日，判断这一天是这一年的第几天？
```

## 函数其它

### 匿名函数

> 匿名函数：没有名字的函数

匿名函数如何使用：

```js
//将匿名函数赋值给一个变量，这样就可以通过变量进行调用 --表达式
//匿名函数自调用
```

关于自执行函数（匿名函数自调用）的作用：防止全局变量污染。

匿名函数自执行不能用来复用,而是用来提供作用域的

### 自调用函数

>匿名函数不能通过直接调用来执行，因此可以通过匿名函数的自调用的方式来执行

```javascript
(function () {
  alert(123);
})();
```

### 函数是一种数据类型

```javascript
function fn() {}
console.log(typeof fn);
```

- 函数作为参数

因为函数也是一种类型，可以把函数作为两一个函数的参数，在两一个函数中调用

- 函数做为返回值

因为函数是一种类型，所以可以把函数可以作为返回值从函数内部返回，这种用法在后面很常见。

```javascript
function fn(b) {
  var a = 10;
  return function () {
    alert(a+b);
  }
}
fn(15)();
```

### 代码规范

```js
//1.命名规范	
//2.变量规范   
//3.注释规范
//4.空格规范
//5.换行规范
var name = 'zs';
	var arr = [1, 2, 3, 4];
	if (a > b) {
      
	}
	for(var i = 0; i < 10; i++) {
      
	}
	function fn() {
      
	}
```

### 回调函数

可以作为一个返回值返回 => 高阶函数(回调函数)

```js
function fn(a){
a();
}
fn(function (){})
```



### 闭包 

可以作为一个返回值 => 高阶函数(闭包)

```js
function fn(){

return function(){
    console.log(1111)
}

}
let a = fn();
a();
```



## 作用域

作用域：变量可以起作用的范围

### 全局变量和局部变量

- 全局变量

  在任何地方都可以访问到的变量就是全局变量，对应全局作用域

- 局部变量

  只在固定的代码片段内可访问到的变量，最常见的例如函数内部。对应局部作用域(函数作用域)
  
  - 再能访问到的情况下   	**先局部** 		**在全局** 

```js
//不使用var声明的变量是全局变量，不推荐使用。
//变量退出作用域之后会销毁，全局变量关闭网页或浏览器才会销毁
```

### 块级作用域

任何一对花括号（｛和｝）中的语句集都属于一个块，在这之中定义的所有变量在代码块外都是不可见的，我们称之为块级作用域。
**在es5之前没有块级作用域的的概念,只有函数作用域**，现阶段可以认为JavaScript没有块级作用域

### 词法作用域

变量的作用域是在定义时决定而不是执行时决定，也就是说词法作用域取决于源码，通过静态分析就能确定，因此词法作用域也叫做静态作用域。

**在 js 中词法作用域规则:**

- 函数允许访问函数外的数据.
- 整个代码结构中只有函数可以限定作用域.
- 作用域规则首先使用提升规则分析
- 如果当前作用规则中有名字了, 就不考虑外面的名字

```javascript
var num = 123;
function foo() {
  console.log( num );
}
foo();

if ( false ) {
    var num = 123;
}

console.log( num ); // undefiend
```

### 作用域链

	只有函数可以制造作用域结构， 那么只要是代码，就至少有一个作用域, 即全局作用域。凡是代码中有函数，那么这个函数就构成另一个作用域。如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域。
	
	将这样的所有的作用域列出来，可以有一个结构: 函数内指向函数外的链式结构。就称作作用域链。

```javascript
// 案例1：
function f1() {
    function f2() {
    }
}

var num = 456;
function f3() {
    function f4() {    
    }
}
```



```javascript
// 案例2
function f1() {
    var num = 123;
    function f2() {
        console.log( num );
    }
    f2();
}
var num = 456;
f1();
```



## 预解析

> JavaScript代码的执行是由浏览器中的JavaScript解析器来执行的。JavaScript解析器执行JavaScript代码的时候，分为两个过程：预解析过程和代码执行过程

预解析过程：

1. 把变量的声明提升到当前作用域的最前面，只会提升声明，不会提升赋值。
2. 把函数的声明提升到当前作用域的最前面，只会提升声明，不会提升调用。
3. 先提升var，在提升function



JavaScript的执行过程

```javascript
var a = 25;
function abc (){
  alert(a);//undefined
  var a = 10;
}
abc();
// 如果变量和函数同名的话，函数优先
console.log(a);
function a() {
  console.log('aaaaa');
}
var a = 1;
console.log(a);
```



### 全局解析规则

### 函数内部解析规则

### 变量提升

- 变量提升

  定义变量的时候，变量的声明会被提升到作用域的最上面，变量的赋值不会提升。

- 函数提升

  JavaScript解析器首先会把当前作用域的函数声明提前到整个作用域的最前面

```javascript
// 1、-----------------------------------
var num = 10;
fun();
function fun() {
  console.log(num);
  var num = 20;
}
//2、-----------------------------------
var a = 18;
f1();
function f1() {
  var b = 9;
  console.log(a);
  console.log(b);
  var a = '123';
}
// 3、-----------------------------------
f1();
console.log(c);
console.log(b);
console.log(a);
function f1() {
  var a = b = c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
```

## 对象

### 为什么要有对象

```javascript
function printPerson(name, age, sex....) {
}
// 函数的参数如果特别多的话，可以使用对象简化
function printPerson(person) {
  console.log(person.name);
  ……
}
```

### 什么是对象

```
现实生活中：万物皆对象，对象是一个具体的事物，一个具体的事物就会有行为和特征。
举例： 一部车，一个手机
车是一类事物，门口停的那辆车才是对象
	特征：红色、四个轮子
	行为：驾驶、刹车
```

### JavaScript中的对象

```js
JavaScript中的对象其实就是生活中对象的一个抽象
JavaScript的对象是无序属性的集合。
	其属性可以包含基本值、对象或函数。对象就是一组没有顺序的值。我们可以把JavaScript中的对象想象成键值对，其中值可以是数据和函数。
对象的行为和特征
	静态属性 --- 具体的值
	动态行为 --- 匿名函数
```

+ 事物的特征在对象中用属性来表示。

+ 事物的行为在对象中用方法来表示。

  ### 属性和方法

  ```
  如果一个变量属于一个对象所有，那么该变量就可以称之为该对象的一个属性，属性一般是名词，用来描述事物的特征
  如果一个函数属于一个对象所有，那么该函数就可以称之为该对象的一个方法，方法是动词，描述事物的行为和功能
  ```

  ### 

### **增删改查**

查 : 对象名.属性名

改: 对象名.属性名=新值

增: 对象名.新属性名=新值

删: delete.对象名.属性名

### 对象字面量(单个对象)

> 字面量：11 'abc'  true  [] {}等 

```javascript
var o = {
  name: 'zs,
  age: 18,
  sex: true,
  sayHi: function () {
    console.log(this.name);
  }
};
```

思考：

```javascript
//如何把学生对象、老师对象、英雄对象改写成字面量的方式
```

### 对象创建方式

- 对象字面量

```javascript
var o = {
  name: 'zs',
  age: 18,
  sex: true,
  sayHi: function () {
    console.log(this.name);
  }
};   
```

### new Object()创建对象 

```javascript
var person = new Object();
  person.name = 'lisi';
  person.age = 35;
  person.job = 'actor';
  person.sayHi = function(){
  console.log('Hello,everyBody');
}
```

#### 工厂函数创建对象 -- 多个对象/批量

```javascript
function createPerson(name, age, job) {
  var person = new Object();
  person.name = name;
  person.age = age;
  person.job = job;
  person.sayHi = function(){
    console.log('Hello,everyBody');
  }
  return person;
}
var p1 = createPerson('张三', 22, 'actor');
```

#### 自定义构造函数创建对象和new运算

```javascript
function Person(name,age,job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayHi = function(){
  	console.log('Hello,everyBody');
  }
}
var p1 = new Person('张三', 22, 'actor');
```

#### 工厂函数创建对象与自定义构造函数创建对象的区别

1. 代码量更少 => 不需要手动声明和返回
2. 有明确的对象类型 => 类型的名称是构造函数的名称 => 可以实现方法共享(js)

系统内置构造函数

1. let obj=new Objec();

2. let arr=new Array();



#### []语法操作对象的属性

.语法操作属性名=> 对象名.属性名

```
console.log(obj.name)
```

[ ]语法操作对象的属性名

```
console.log(obj['name'])
```

不同点

[]语法可识别变量

obj[变量]

obj['属性名']



### new关键字

> 构造函数 ，是一种特殊的函数。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。

1. **构造函数用于创建一类对象，首字母要大写。**
2. 构造函数要和new一起使用才有意义。
3. 函数的调用模式

**new在执行时会做四件事情**

1. new会在内存中创建一个新的空对象
2. new 会让this指向这个新的对象
3. 执行构造函数  目的：给这个新对象加属性和方法
4. new会返回这个新对象

### this详解

	JavaScript中的this指向问题，有时候会让人难以捉摸，随着学习的深入，我们可以逐渐了解
	现在我们需要掌握函数内部的this几个特点
		1. 函数在定义的时候this是不确定的，只有在调用的时候才可以确定
		2. 一般函数直接执行，内部this指向全局window
		3. 函数作为一个对象的方法，被该对象所调用，那么this指向的是该对象
		4. 构造函数中的this其实是一个隐式对象，类似一个初始化的模型，所有方法和属性都挂载到了这个隐式对象身上，后续通过new关键字来调用，从而实现实例化

## 对象的使用

### 遍历对象的属性

> 通过for..in语法可以遍历一个对象

```javascript
for(var 变量名 in 对象名) {
  console.log(对象名[变量名]);
}
```



### 引用数据类型和值类型

值类型: Number /String /Bloolean/ Null /Undefine

引用数据类型: Object /Function/ Array

值类型在变量中存放的是值本身

```
let a=18 
let b= 10;
b=a;

console.log(a); //18
```

引用数据类型在变量在中存放的引用地址

```
let a={name:'zs',age=18} 
let b= a;
b.name='ls';
console.log(a.name); //ls
```

变量到变量的赋值 => 将变量里面的内容地址拷贝一份给另外一个变量,如果变量更改,则改的是地址



```javascript
// 下面代码输出的结果
function Person(name,age,salary) {
  this.name = name;
  this.age = age;
  this.salary = salary;
}
function f1(person) {
  person.name = "ls";
  person = new Person("aa",18,10);
}

var p = new Person("zs",18,1000);
console.log(p.name);
f1(p);
console.log(p.name);
```

### 简单类型和复杂类型的区别

>基本类型又叫做值类型，复杂类型又叫做引用类型
>
>值类型：简单数据类型，基本数据类型，在存储时，变量中存储的是值本身，因此叫做值类型。
>
>引用类型：复杂数据类型，在存储是，变量中存储的仅仅是地址（引用），因此叫做引用数据类型。

- 堆和栈

  ```
  堆栈空间分配区别：
  　　1、栈（操作系统）：由操作系统自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈；
  　　2、堆（操作系统）： 存储复杂类型(对象)，一般由程序员分配释放， 若程序员不释放，由垃圾回收机制回收，分配方式倒是类似于链表。
  ```

- 注意：JavaScript中没有堆和栈的概念，此处我们用堆和栈来讲解，目的方便理解和方便以后的学习。



## 内置对象

JavaScript中的对象分为3种：内置对象、浏览器对象、自定义对象

JavaScript 提供多个内置对象：Math/Array/Number/String/Boolean...

对象只是带有**属性**和**方法**的特殊数据类型。

学习一个内置对象的使用，只要学会其常用的成员的使用（通过查文档学习）

可以通过MDN/W3C来查询

内置对象的方法很多，我们只需要知道内置对象提供的常用方法，使用的时候查询文档。

### [MDN]()

Mozilla 开发者网络（MDN）提供有关开放网络技术（Open Web）的信息，包括 HTML、CSS 和万维网及 HTML5 应用的 API。

- [MDN](https://developer.mozilla.org/zh-CN/)
- 通过查询MDN学习Math对象的random()方法的使用


### 如何学习一个方法？

1. 方法的功能
2. 参数的意义和**类型**
3. 返回值意义和**类型**
4. demo进行测试

### Math对象

Math对象不是构造函数，它具有数学常数和函数的属性和方法，都是以静态成员的方式提供

跟数学相关的运算来找





Math中的成员（求绝对值，取整）

[Math](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)

演示：Math.PI、Math.random()、Math.floor()/Math.ceil()、Math.round()、Math.abs()	、Math.max()

```javascript
Math.PI						// 圆周率
Math.random()				// 生成随机数
Math.floor()/Math.ceil()	 // 向下取整/向上取整
Math.round()				// 取整，四舍五入
Math.abs()					// 绝对值
Math.max()/Math.min()		 // 求最大和最小值
Math.sin()/Math.cos()		 // 正弦/余弦
Math.power()/Math.sqrt()	 // 求指数次幂/求平方根
```

#### 案例

用floor需要+1

- ​	0-n 之间的随机数:

  Math.floor(Math.random()*(n+1));

- ​	m-n之间的随机数:

  Math.floor(Math.random()*(n-m+1))+m;

- 求10-20之间的随机数

  Math.round(Math.random() * 10)+10;

- 随机生成颜色RGB

  a = Math.round(Math.random() * 20);

  r = Math.round(Math.random() * 256);

  g = Math.round(Math.random() * 256);

  b = Math.round(Math.random() * 256);

  console.log(`color:rgb(${r},${b},${g})`);

- 模拟实现max()/min()

### Date对象

创建 `Date` 实例用来处理日期和时间。Date 对象基于1970年1月1日（世界标准时间）起的毫秒数。



~~~javascript
// 获取当前时间，UTC世界时间，距1970年1月1日（世界标准时间）起的毫秒数
var now = new Date();
console.log(now.valueOf());	// 获取距1970年1月1日（世界标准时间）起的毫秒数

Date构造函数的参数
1. 毫秒数 1498099000356		new Date(1498099000356)
2. 日期格式字符串  '2015-5-1'	 new Date('2015-5-1')
3. 年、月、日……				  new Date(2015, 4, 1)   // 月份从0开始
~~~

- 获取日期的毫秒形式

```javascript
var now = new Date();
// valueOf用于获取对象的原始值
console.log(date.valueOf())	

// HTML5中提供的方法，有兼容性问题
var now = Date.now();	

// 不支持HTML5的浏览器，可以用下面这种方式
var now = + new Date();			// 调用 Date对象的valueOf() 
```

- 日期格式化方法

```javascript
toString()		// 转换成字符串
valueOf()		// 获取毫秒值
// 下面格式化日期的方法，在不同浏览器可能表现不一致，一般不用
toDateString()
toTimeString()
toLocaleDateString()
toLocaleTimeString()


```

- 获取日期指定部分

```javascript
getTime()  	  // 返回毫秒数和valueOf()结果一样，valueOf()内部调用的getTime()
getMilliseconds() 
getSeconds()  // 返回0-59
getMinutes()  // 返回0-59
getHours()    // 返回0-23
getDay()      // 返回星期几 0周日   6周6
getDate()     // 返回当前月的第几天
getMonth()    // 返回月份，***从0开始***
getFullYear() //返回4位的年份  如 2016
```

#### 封装日期:

```js
function forMaterDate() {
  let d = new Date();
  let year = d.getFullYear();
  let month = d.getMonth() + 1; //月份从0开始算起所以要加一
  let day = d.getDate();
  let hours = d.getHours();
  let min = d.getMinutes();
  let sec = d.getSeconds();

  return `${addZero(year)}-${addZero(month)}-${addZero(day)} ${addZero(
    hours
  )}:${addZero(min)}:${addZero(sec)}`;
}
//补零
function addZero(n) {
  return n < 10 ? "0" + n : n;
}
console.log(forMaterDate());
```

时间戳:  作用=>倒计时

隐式转换成数字就是时间戳

let d= +new Date();

可用于计算时间差



#### 案例

- 写一个函数，格式化日期对象，返回yyyy-MM-dd HH:mm:ss的形式

```javascript
function formatDate(d) {
  //如果date不是日期对象，返回
  if (!date instanceof Date) {
    return;
  }
  var year = d.getFullYear(),
      month = d.getMonth() + 1, 
      date = d.getDate(), 
      hour = d.getHours(), 
      minute = d.getMinutes(), 
      second = d.getSeconds();
  month = month < 10 ? '0' + month : month;
  date = date < 10 ? '0' + date : date;
  hour = hour < 10 ? '0' + hour : hour;
  minute = minute < 10 ? '0' + minute:minute;
  second = second < 10 ? '0' + second:second;
  return year + '-' + month + '-' + date + ' ' + hour + ':' + minute + ':' + second;
}
```

- 计算时间差，返回相差的天/时/分/秒

```javascript
function getInterval(start, end) {
  var day, hour, minute, second, interval;
  interval = end - start;
  interval /= 1000;
  day = Math.round(interval / 60 /60 / 24);
  hour = Math.round(interval / 60 /60 % 24);
  minute = Math.round(interval / 60 % 60);
  second = Math.round(interval % 60);
  return {
    day: day,
    hour: hour,
    minute: minute,
    second: second
  }
}
```

### Array对象

创建数组对象的两种方式

字面量方式

new Array()

```javascript
// 创建了一个空数组
var arr = new Array();
// 创建了一个数组，里面存放了3个字符串
var arr = new Array('zs', 'ls', 'ww');
// 创建了一个数组，里面存放了4个数字
var arr = new Array(1, 2, 3, 4);


// 2. 使用字面量创建数组对象
var arr = [1, 2, 3];

// 获取数组中元素的个数
console.log(arr.length);
```

检测一个对象是否是数组

instanceof

Array.isArray()     HTML5中提供的方法，有兼容性问题

函数的参数，如果要求是一个数组的话，可以用这种方式来进行判断

toString()/valueOf()

toString()		把数组转换成字符串，逗号分隔每一项

valueOf()         返回数组对象本身

数组常用方法

演示：push()、shift()、unshift()、reverse()、sort()、splice()、indexOf()

##### **尾部的增加: 数组.push();**

   参数: 需要追加的内容 可以是一个或者多个

   返回值: 新的数组长度

```js
let arr = ['a', 'b', 'c'];
let count = arr.push('d', 'e');
console.log(arr);
console.log(count);
```

##### **尾部的删除: 数组.pop()**

没有参数

返回值: 被删除的那个数

```js
  let arr = ['a', 'b', 'c'];
    let result = arr.pop();
    console.log(arr);
    console.log(result);
```

#####  **头部的增加: 数组.unshift()**

  参数: 需要增加的数据 可以是一个或者多个

  返回值: 新的数组长度

```js
 let arr = ['a', 'b', 'c'];
    let count = arr.unshift('e');
    console.log(arr);
    console.log(count);
```

#####  **头部的删除**: 数组.shift()

  没有参数

  返回值: 被删除的那个元素

```js
 let arr = ['a', 'b', 'c'];
   let result = arr.shift();
   console.log(arr);
   console.log(result);
```

#####  数组.**join**(连接符)

  作用: 将数组按照特定的连接符链接成字符串

  参数: 连接符

  返回值: 连接好的字符串

```js
 let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
    let str = arr.join('❤');
    console.log(str);
```

需求: 不需要任何的连接符连接

```js
 let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
    let str = arr.join();
    console.log(str);// 如果不传递 默认是逗号链接 =>  赵云,马超,刘备,关羽,张飞
    
```

可以传递一个空字符串

```js
  let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
 let str = arr.join('');
    console.log(str);
```

##### **翻转:数组.reverse**()

```js
 arr.reverse();
 console.log(arr);
```

##### **查找数组元素indexOf**

indexOf方法用来查找数组中某个元素第一次出现的位置，如果找不到，返回-1



   语法: array.indexOf(search, [fromIndex]);

   参数1: search: 需要检索的数据

   参数2: [fromIndex]: 起始的下标 默认从0开始

   返回值: 对应的下标或者 -1

```js
 let arr = ['a', 'b', 'c', 'b', 'd', 'e', 'f', 'ade'];
      console.log(arr.indexOf('a')); // 0
      console.log(arr.indexOf('d')); // 4
      console.log(arr.indexOf('h')); // -1
      console.log(arr.indexOf('ad')); // -1
      console.log(arr.indexOf('b')); // 1
      console.log(arr.indexOf('b', 2)); // 3
```

  lastIndexOf()从后面开始查找数组中元素出现位置,如果找不到，返回-1

   array.lastIndexOf(search, [fromIndex]);

   记住一点: indexOf 从左往右 lastIndexOf => 从右往左

```js
 let arr = ['a', 'b', 'c', 'b', 'd', 'e', 'f', 'ade'];
      console.log(arr.lastIndexOf('a')); // 0
      console.log(arr.lastIndexOf('d')); // 4
      console.log(arr.lastIndexOf('h')); // -1
      console.log(arr.lastIndexOf('ad')); // -1
      console.log(arr.lastIndexOf('b')); // 3
      console.log(arr.lastIndexOf('b', 2)); // 1
```

##### 数组的拼接:concat

作用: 将多个小数组拼接成一个大数组: 数组1.concat(数组2,[...])

   参数: 需要链接的数组

   返回值: 链接好的新数组 不会影响原数组

```js
   let arr = [1, 2, 3];
    let arr2 = [4, 5, 6];
    let newArr = arr.concat(arr2);
    console.log(newArr);
```

数组的提取/截取:

##### 数组名.slice()

  作用: 从一个大数组当中提取出一个数组片段: 数组名.slice()

  参数:

  \1. 不传参数 => 复制一个新数组

  \2. 传递一个参数 => 数组名.slice(起始的下标) => 从这个下标一直提取到结束

  \3. 传递两个参数 => 数组名.slice(起始的下标, 结束的下标) =>从起始下标到结束下标, 不包括结束下标

  返回值: 新的数组 不会影响原数组

```js
let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
    let arr2 = arr.slice();
    console.log(arr2); // ["赵云", "马超", "刘备", "关羽", "张飞"]
    arr[0] = '哈哈';
    console.log(arr2);
```

传递一个参数 => 数组名.slice(起始的下标) => 从这个下标一直提取到结束

```js
let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
    let newArr = arr.slice(2);
    console.log(newArr); // ['刘备', '关羽', '张飞']
```

传递两个参数 => 数组名.slice(起始的下标, 结束的下标) =>从起始下标到结束下标, 不包括结束下标

```js
  let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
    let newArr = arr.slice(1, 4);
    console.log(newArr);
```

##### 数组.splice()

splice: 可以实现数组的任意位置的 删, 增, 改..

   语法: 数组.splice(起始的下标, 删除的个数, 替换的内容)

   返回值: 由被删除的元素组成的数组

删除

```js
let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
    let result = arr.splice(2, 1);
    console.log(arr);
    console.log(result);
```

增加

```js
let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
      let result = arr.splice(2, 0, '飞飞', '岐哥');
      console.log(arr);
```

更改

```js
  let arr = ['赵云', '马超', '刘备', '关羽', '张飞'];
      let result = arr.splice(2, 1, '飞飞');
      console.log(arr);
      console.log(result);
```

##### 数组.sort()

 因为sort底层是将数组的数据转换成字符串比较, 字符串比较的ascill编码 并且是按位比较 所以a - z 的字符串排序可以直接使用

1. 不传递参数: A-Z

   2. 传递参数 => 函数

​    return a - b; 从小到大

​    return b - a; 从大到小

```js
从小到大
    arr.sort(function (a, b) {
      return a - b;
    });

    从大到小
    arr.sort(function (a, b) {
      return b - a;
    });
    console.log(arr);
```



##### 清空数组

```javascript
// 方式1 推荐 
arr = [];
// 方式2 
arr.length = 0;
// 方式3
arr.splice(0, arr.length);
```

##### 去重--数组

```js
let newArr = [];
        for (i = 0; i < arr.length; i++) {
            if (newArr.indexOf(arr[i]) === -1) {
                newArr.push(arr[i]);
            }
        }

        console.log(newArr);
```

##### 替换--字符串

```js
while (true) {
            if (str.indexOf('o') === -1) {
                break;
            }
            str = str.replace('o', '!')
        }
        console.log(str);
```

##### 去空格--字符串

```js
let str = " dsa  sdas dsa dsad sa sa  ";
        while (true) {
            if (str.indexOf(' ') === -1) {
                break;
            }
            str = str.replace(' ', '')
        }
        console.log(str);
```



### 基本包装类型

为了方便操作基本数据类型，JavaScript还提供了三个特殊的引用类型：

String/Number/Boolean

简单数据类型是没有方法的

只有复杂数据类型才有属性和方法

```javascript
发生了三件事情
   1. 把简单类型转换成复杂类型：let s = new Number(num);
   2. 调用包装类型的toString方法：let result = s.toString();
   3. 销毁刚刚创建的复杂类型
```



### String对象--字符串

```js
1. 字符串可以遍历
2. 字符串不可以通过下标单独修改, 只能整体重新赋值
3. 字符串方法的共性: 如果返回的是字符串 一定是一个新字符串
4. 字符串不是一个数组, 所以不能使用数组的方法
```
**字符串对象的常用方法**

字符串所有的方法，都不会修改字符串本身(字符串是不可变的)，操作完成会返回一个新的字符串

#### indexof--字符串

indexOf:获取某个字符串第一次出现的位置，如果没有，返回-1

lastIndexOf:从后面开始查找第一次出现的位置。如果没有，返回-1

```javascript
let str = 'abcdefgc';
      console.log(str.indexOf('c')); // 2
      console.log(str.indexOf('h')); // -1
      console.log(str.indexOf('cd')); // 2
      console.log(str.indexOf('cde')); // 2
      console.log(str.indexOf('cdf')); // -1
      console.log(str.indexOf('c', 3)); // 7
```

####  trim() --字符串

去掉字符串左右的空白 中间的空格是保留的

一般用于将从用户那边得到的数据过滤一下

```js
   let str = ' ab c d efg ';
      // 一定记住要将新的字符串重新还给str
      str = str.trim();
      console.log(str);
```

大小写转换

####  toUpperCase--字符串

：全部转换成大写字母

####    toLowerCase--字符串

：全部转换成小写字母

```js
 let str = 'i hate you';

   str = str.toUpperCase();

   let str2 = 'I LOVE YOU';

   str2 = str2.toLowerCase();

   console.log(str);

   console.log(str2);

```

字符串拼接

   可以用concat，用法与数组一样，但是字符串拼串我们一般都用+

   字符串截取的方法有很多，记得越多，越混乱，因此就记好用的就行

#### slice --字符串

从start开始，end结束，并且取不到end。

####    substring --字符串

从start开始，end结束，并且取不到end

####    substr --字符串

从start开始，截取length个字符。

```js
 let str = 'abcdefg';
    console.log(str.slice(0, 2)); // ab
    console.log(str.slice(2)); // cdefg
    console.log(str.substring(0, 2)); // ab
    console.log(str.substring(2)); // cdefg
    console.log(str.substr(2, 2)); // cd
```

替换

#### searchValue--字符串

#### 一个需要替换出现时

   参数：searchValue:需要替换的值  replaceValue:用来替换的值

```js
   let str = '你怎么这么垃圾';

   str = str.replace('垃圾', '❤❤');

   console.log(str);
```

#### 多个需要替换出现时 

```js
 let str = '你怎么这么垃圾, 垃圾中的战斗机, 垃圾垃圾垃圾';
    // 思路
    // 1. 使用while循环 str = str.replace('垃圾', '❤❤');
    // 2. 当字符串当中没有垃圾了之后跳出循环
 
 while (true) {
      // 字符串当中没有垃圾了之后跳出循环
      if (str.indexOf('垃圾') === -1) {
        break;
      }
      str = str.replace('垃圾', '❤❤');
    }
    console.log(str);
```

####  split--字符串

String.split(切割符) 将字符串按照特定的分割符分割成数组

```js
let str = 'shen-yan-fei';
    let arr = str.split('');
    console.log(arr);
```

#### 驼峰式命名-首字母大写

需求:制作一个函数, 接受任意的字符串,将其转换成驼峰式

```js
  function changeName(str) {
        // 将字符串按照特定的分割符分割成数组
        let arr = str.split('-');
        // 遍历数组 直接从下标为1开始即可
        for (let i = 1; i < arr.length; i++) {
          // 获取当前这一项的第一个字符将其转换成大写 并且将这一项后面的所有的字符重新拼接一下赋值给当前这一项
          // 原因: 字符串不可通过下标单独修改 只能整体重新赋值
          arr[i] = arr[i][0].toUpperCase() + arr[i].slice(1);
        }
        // 将数组链接成一个字符串
        return arr.join('');
      }

      console.log(changeName('shen-yan-fei'));
      console.log(changeName('di-li-re-ba'));
      console.log(changeName('ma-er-zha-ha'));
```



#### 冒泡排序

```js
let a=[2,4,6,1,3,5,8,9,7,10];
coumt=0
for(i=1;i<= a.length-1;i++){
count++;
for(j=1;j<=a.length-j-i-1;i++){
if(a[j]<a[j+1]){
stmp=a[j+1];
a[j+1]=a[j];
a[j]=stmp;
}

}

}

```



## 总结

```js
数组的首尾增删操作
      push,pop,shift,unshift
      增加的方法: 参数: 需要增加的元素,可以是一个或者多个,返回最新的数组长度
      删除的方法: 返回值: 被删除的元素

      数组的转换 => 将数组转换成字符串 =>arr.join()
      参数: 连接符 => 特殊情况(=> 不需要连接符 => '')
      返回值: 连接好的字符串

      数组的翻转 => reverse() => 操作的是原数组

      indexOf => 查找数组当中是否有存在的某个数据,如果有,返回这个数据在数组的下标 没有返回-1
      参数: 查找的数据, 操作的起始下标(可选)
      返回值: 下标或者-1

      lastIndexOf => 从右往左查找
      数组的下标是不变的

      concat => 将多个数组拼接成一个数组
      参数: 数组, 可以是一个或者多个
      返回值: 链接好的新数组
      注意:返回的是新数组, 对于原数组没有影响

      slice => 从数组当中提取出对应的子数组
      参数:
      1. 不传递参数 => 直接复制一个数组出来
      2. 只传递一个参数 => 起始的下标 => 从这个下标开始, 一直截取到最后,包括最后
      3. 传递两个参数 => 起始下标, 结束下标 => 从这个下标开始, 一直到结束下标,不包含结束下标
      返回值: 提取出来的新数组
      注意:返回的是新数组, 对于原数组没有影响

      splice => 可以实现数组任意位置的删除,增加,替换
      参数: 操作的起始下标, 删除的个数, 替换的数据(可以是一个或者多个)
      返回值: 删除元素对应的数组

      -----------------------------------------------------------------------------

      基本包装类型 => 将简单数据类型临时包装成复杂数据类型 => 钢铁侠铠甲
      new Number() new String()  new Boolean

      String包装类型
      1. 字符串可以理解为一个字符伪数组, 可以遍历,可以通过下标访问
      2. 字符串不能通过下标单独修改,除非整体重新赋值
      3. 字符串是一个伪数组,不能使用数组的方法
      4. 字符串的方法如果返回的是字符串, 返回的一定是一个新字符串

      indexOf
      lastIndexOf
      trim
      toUpperCase()
      toLowerCase()
      slice
      substring
      substr
      replace
      split

      排序
      sort排序
      1. 不传递参数: 使用字符串的方式排序
      2. 传递一个函数
         2.1 => return a - b; 从小到大
         2.2 => return b - a; 从大到小
```

# 经典面试题

 复杂数据类型比较的是内存地址

```js
console.log([] == []); // false
    console.log({} == {}); // false
    let temp = [];
    // 将temp的地址复制给temp1
    let temp1 = temp;
    console.log(temp == temp1); // true
```

