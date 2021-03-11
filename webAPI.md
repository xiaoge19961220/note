- [ ] # WebApi基本概念


## 为什么要学习WebApi?

> 什么是webApi?

```html
就是JS提供的一套操作页面和浏览器的一套方法 
	
web: 网页 
api: 应用程序接口 (输入输出 => 参数和返回值 类似于push)
```

> 为什么学习webApi呢?

```html
之前学习JS都是学习语法, 没有使用JS作出真正好看的效果 (99乘法表)
学了webApi之后 => 可以操作网页上的html和css (轮播图, 下拉菜单等等用用户的交互效果)
```

> webApi的组成

```html
DOM和BOM

DOM => Document Object Model   操作页面的一套工具
BOM => Browser Object Model   操作浏览器的一套工具
```

# DOM-文档对象模型

## DOM基本概念

> DOM（Document Object Model）文档对象模型，是`W3C组织`推荐的一套用于处理`HTML`的标准`编程接口`。
>
> 说白了就是使用JS操作页面的一套工具, 关键是怎么操作的呢??

```
JS本质上并不能直接操作html, 而是浏览器会将网页当中的标签抽象成一个个的JS对象, 标签的一些属性和内容都可以在这个对象里面找到, 通过修改这个对象达到操作html的目的, 因为这个对象一旦修改,与之对应的标签也会修改
```

术语介绍

```js
document => 代表整个网页
element => 代表网页的一个个的标签  
节点node => 网页上所有的内容都可以称之为节点 (标签 属性 文本 注释)
```

## DOM初体验

> 需求:  通过JS将图片换掉

> 思路:  修改img的src的取值即可

```javascript
1. 先获取到对应的img对象 (在DOM中要操作一个dom对象  首先要获取到这个dom对象 类似于css选择器)
2. 修改该对象的src属性的取值即可
```

## querySelector介绍

> API介绍

```js
语法: document.getElementById(id名称)
querySelector（''）//可以添加class ，id ，标签选择器，伪元素等
querySelector //只对第一个有效
querySelectorAll //返回的是一个伪数组，必须要配合下标使用

作用: 通过id获取到对应的DOM对象
参数: id
返回值: 一个DOM对象
说明: 了解即可. 实际工作中很少使用
```

> 小知识点说明

```html
通过console.log()打印DOM对象 会以标签的形式展示出来
通过console.dir()打印DOM对象 会以对象的形式展示出来
html标签里面的属性在DOM对象里面都能找到
```

#  JS获取元素(DOM对象)的方式

> 使用id获取元素的方式过于单一, 所以JS引入了类似于css选择元素的方式 分为两种

```js
// 参数: css选择器  标签/类/id/后代/子代/属性选择器/结构伪类选择器
// 返回值: 满足条件的第一个元素  个数永远是一个 可以直接操作
document.querySelector('css选择器')
```

**特点**: 获取的结果永远只有一个值(第一个满足条件的值), 可以直接进行操作,弊端在于无法实现批量操作

> 但凡是涉及到批量操作的, 需要使用到下面这个API

```java
// 参数: css选择器   标签/类/id/后代/子代/属性选择器
// 返回值: 满足条件的伪数组集合 即使只有一个也是集合 必须通过下标操作
document.querySelectorAll('css选择器');
```

**特点**: 获取的结果一定是一个伪数组集合, 即使只有一个值也是数组集合. 所以不能直接操作,需要使用下标单独操作

> 小知识补充

```
body,html,head等特殊标签可以直接通过document获取到
```

# 注册事件

> 需求:  让用户 "点击" 一下按钮,然后完成换图片的交互效果 (点击按钮换属性)

```
当用户在某个元素上触发了某种行为之后在执行的事情
```

>  事件的组成

```js
事件源: 触发事件的元素 (按钮)
事件名称: 点击 (click)
事件处理函数: 触发事件要做的事情 (修改图片的属性)

基本语法:
事件源.on事件名 = 事件处理函数
// on... 在什么的时候  在点击的时候
```

> 基本思路

```
1. 获取事件源
2. 给事件源绑定点击事件
3. 在事件处理函数里面将图片的属性替换掉
```

> 细节说明

```txt
1. 事件处理函数不会主动执行,是当用户点击了才会执行,用户点击一次就会重新执行一次
2. JS解析器会将事件处理函数交给浏览器执行, 并且自己直接执行下面的代码 (后面的代码先执行)
```

> 练习

```html
<ul class="my-ul">
    <li>我是1</li>
    <li>我是2</li>
    <li>我是3</li>
    <li>我是4</li>
    <li>我是5</li>
    <li>我是6</li>
    <li>我是7</li>
    <li>我是8</li>
    <li>我是9</li>
    <li>我是10</li>
</ul>

练习1. 点击第一个li标签, 弹出你好
练习2. 点击一组li，点击哪个li都可以弹出你好
练习3. 给一组li添加点击事件，点击哪个li打印哪个li的索引值
```

# 元素属性操作

> 操作标签元素的属性,本质上就是操作DOM对象的属性, DOM对象的属性一旦发生变化, 会自动反应到标签上

```js
获取属性值: DOM对象.属性
设置属性值: DOM对象.属性 = 值
```

> 常用属性介绍

```js
href、title、src、className(标签class写法)、innerText 、innerHTML
```

> 注意点

```js
1. class是ES6的一个关键字,所以早期为了避免冲突, 使用className代替class
2. 修改元素的内容有两个属性可以完成
	1.innerHTML
	2.innerText	
共同点: 都可以修改内容
区别: 当内容含有标签的时候
 innerText: 不会去识别内部的标签, 获取:只会获取内容 设置:不会解析标签,将标签原样显示在页面上
 innerHTML: 会去识别内容里面的标签, 获取,获取标签 + 内容 设置: 会将标签解析显示在页面上

实际工作中对于从用户那边得到的数据都使用innerText, 因为更加安全, 对于innerHTML, 如果能够明确数据的来源,使用较为方便.
```

> 练习

```js
练习1. 点击按钮, 给div赋予一句话(我爱web)
练习2. 点击p标签, 修改p标签里的内容
练习3. 点击按钮, 修改img标签上的图片
练习4. 点击按钮, 修改div的class值, class(将矩形变成圆形)
```

# 综合案例

## 美女相册案例

```js
思路：
	1. 获取标签
	2. 批量注册点击事件
	3. 设计图片数组
	4. 图片数组的下标和点击的元素的下标吻合进行关联
```

## 美女画廊

```js
思路:
	1. 获取标签
	2. 批量注册点击事件
	3. 设计图片和文本数据数组
	4. 使用点击的下标和数组的下标吻合进行关联
```

# 表单属性

> 表单的常用属性: `type`,` value`,`disabled`, `checked`,`selected`

```js
如type,value这些属性与正常的标签属性操作没有什么不同, 都是可以通过点语法直接获取或者赋值的.
但是disabled, checked, selected属性稍微有些不同, 这些属性在html上只要添加了就有效果  没添加就没有效果类似于这样的属性 那么在JS中的控制使用布尔类型控制  true则表示设置了  false则表示取消了	
```

> 练习

```js
练习1. 点击按钮, 来控制一个多选框, 选中和未选中切换
练习2. 有3个按钮, 上面分别有名字, 点击以后, 把名字赋予到一个输入框中显示
练习3. 在输入框输入你的名字, 然后点击按钮, 弹出你输入的名字
```

## 综合练习

### 表单全选

```
需求1: 点击全选按钮 让tbody的input和全选按钮的checked一样
```

```js
需求2: 给tbody的input批量绑定点击事件,当里面所有的input的checked结果都是true的情况下,让全选按钮的checked结果也是true 否则就是false
//给全选按钮一个触发动作。
//每一个按钮分别给个触发动作。并进行便利判断是否有false存在，并利用return结束下面的操作
```

###  购物车

```js
需求: 点击+, 增加数量, 点击-, 减少数量, 当数量为1时, 禁用-号
++的同时接触禁用，--的时候判断《=1，当唯1的时候启用禁止-号
```



# 样式操作（style属性）

> 标签不仅可以通过class属性操作样式，还可以通过style属性操作样式。同样的DOM对象可以通过className操作样式，也可以通过style属性操作样式。

```
className操作css的弊端
1. className可能会带来权重问题
2. 会使js和css产生耦合
3. className只能实现静态css设置,当有逻辑的时候,css可能需要动态计算出来(比如: 随机设置颜色)
```

> 需求: 使用JS来实体化一个div

> style操作属性的特点说明

```javascript
1. 每一个DOM对象都有一个style属性,style属性的值是一个对象，里面存储了所有行内样式对应的键值对。
2. 如果样式的名字带了-，比如background-color,到了style对象中，变成了驼峰命名法，backgroundColor（因为-在js中-会被解析成减号）
3. style属性只能获取和设置行内样式，在类样式中定义的样式通过style获取不到。
```

> style属性对比className操作属性的使用差异

```
如果是静态的css,并且条数较多的时候, 推荐使用className操作
如果是动态的css,则必须使用style设置
```

**当逻辑比较简单的时候两者都可以 但逻辑复杂的时候推荐使用style(运算)控制**

【案例 - 开关灯效果】

```
思路:
1: 找对象
2: 给button注册点击事件,
3: 修改body的背景颜色
	- 进行判断 使用变量表示开关的状态,一定记得在分支里面将状态切换 (flag = !flag)
```

# tab切换

> 案例需求: tab切换 (需求拆解)

```
需求1: 点击谁  谁自己变化  其他人不变化
需求2: 点击谁  控制与之对应的元素变化 
```

## 排他思想

> 排他思想：干掉所有人，复活我自己

> 需求：点击谁 谁自己变红 同时其他人不变红

```
点击的时候自身变红之外,其他人需要还原,筛选其他人有点麻烦,所以最快捷的方法是
找到所有人,并且将所有人都干掉
然后复活自己就OK了

干掉所有人: 遍历所有的按钮,将所有按钮的红色去掉
```

**排他思想是一个新套路, 当出现 “其他元素整体状态A 当前操作的元素状态为B”的时候就可以使用这个套路**

[使用排他思想完成tab切换]

## tab栏

```
需求1: 点击li的时候,让点击的li有对应的css变化,其他的li还原成正常css效果
思路1: 点击li元素让对应的li元素添加active这个类 其他li去掉active这个类
需求2: 点击li的时候, 让对应的main元素显示, 让其他的mian元素隐藏
思路2: 点击对应的元素找到这个元素对应的下标 通过下标在找到对应的main 给这个main添加selected类名, 其他的main移出selected类名

小知识点: classList对象操作类名
	classList.add(类名) => 添加类名
	classList.remove(类名) => 移出类名
```

# 更多事件

## 鼠标和表单事件

```js
onclick：点击事件

onmouseenter：鼠标移入事件
onmouseleave：鼠标移出事件

// 针对表单
onfocus：获得焦点
onblur：失去焦点

// 针对表单
onchange：内容发生改变(失去焦点时)=>只针对变化后的值(对比)
oninput: 当value值被修改的时候触发 (IE不支持)=>实时检测
```

> 案例: 搜索提示列表

```
思路: 
	1. 获取焦点, 显示提示列表
	2. 失去焦点, 隐藏提示列表
```

> 案例: 京东导航栏

```
需求: 鼠标"移入"左侧导航, 显示"相应"右侧导航, 而且鼠标也能在右侧分类中滑动, 当离开父级标签区域后, 导航高亮消失,分类隐藏

思路: 鼠标移入左侧导航, 排他思想, 只设置当前li高亮和索引对应div右侧出现, 移出父级区域时, 把导航和div回归初始状态

易错点: 直接给所有的左侧导航li注册鼠标移出事件,这样会导致鼠标移入到右侧分类的时候会直接隐藏 => 解决: 给整个大slide注册鼠标离开事件来达到隐藏效果
```

> 同意协议

```
需求: 当多选框的状态发生改变, 结果为勾选的时候, 按钮是可以交互的,不勾选,按钮不可以交互

思路: 给多选框注册change事件,获取checked状态,根据checked状态来设置disabled属性

细节说明: 当前案例 - 引入了css插件(表单插件)-别人写好的css代码 -
		 运行- 检查- 选中要查看的标签 - 分析生效的class类名
```

> 密码验证

```
需求: 输入内容符合要求的时候,边框变成绿色, 否则变成红色

思路: 注册input事件, 检测value的取值是否符合要求.

小细节: 边框有默认的选中样式,所以需要去掉,否则看不到边框效果
```



# 经验值

## 问题：元素样式属性操作问题

- 问题：给元素添加样式属性，没有效果！

![1597052033443](C:/Users/14517/Downloads/day02/01-教学资料/笔记/images/1597052033443.png)

- 分析：
  - 1.先看控制台是否有报错；
  - 2.此处没有报错，但是没有把红色添加到原生上；此时就要回到添加背景色的样式设置处，观察样式设置是否有问题；

- 解决：因为是添加样式，我们需要用style属性操作。所以，给元素添加上style【div.style.background = 'red'】
- 总结：原生操作样式，必须在其style上进行CSS属性的操作！需要记忆；

### 问题：设置样式值的问题

* 描述：样式属性值用变量接受，设置给DOM节点时没有设置成功；

![1597053689520](C:/Users/14517/Downloads/day02/01-教学资料/笔记/images/1597053689520.png)

* 分析：
  * 代码没有报错时，找到没有出现效果地方代码，该处找位置移动；检查页面元素：
  * 如果设置成功，元素会有行内样式显示的；说明代码在设置上存在问题；

![1597055759842](C:/Users/14517/Downloads/day02/01-教学资料/笔记/images/1597055759842.png)

* 解决：

  * 该处变量接受数据，设置时需要用字符串和变量形成拼接；
  * `div.style.transform = "translateY(" + num + "px)";`

* 总结：

  * 只要放在字符串里面的字母，就是字符串，不再是变量；
  * 如果是变量和其他类似的数据组合在一起，需要我们使用拼接；






# 节点操作

> 操作节点的本质就是对标签元素的增,删,改,查等操作.

```jsx
<div id="myDiv">
    <a href="#" id="myA">我是a标签</a>
    <p id="myP">我是第一个p标签</p>
    <img id="myImg" src="./images/a.jpg" alt="" />
    <!-- 我是注释 -->
</div>
```

## 节点查找 (找亲戚)

```javascript
parentNode:父节点
```

### 孩子节点

返回的是一个数组

```javascript
// childNodes:获取所有的孩子节点（包括了元素节点和其他很多类型的节点，基本不常用）
// chileren[0] //第一个子节点 
// chileren[children.length-1] //最后一个节点

// children:获取所有的子元素
// firstElementChild //第一个子元素
// lastElementChild //最后一个子元素

// 现在孩子节点的查找也可以使用结构伪类选择器去代替
```

### 兄弟节点

```javascript
//1. nextSibling 下一个兄弟节点//废弃
//2. previousSibling 上一个兄弟节点//废弃
//3. nextElementSibling 下一个兄弟元素
//4. previousElementSibling 上一个兄弟元素
```

**总结: 操作节点本质上也是获取对应的元素,在有明确的亲戚关系(当前的事件源和目标对象)的元素里面使用就会更方便**

## 节点的说明 (了解)

> 页面上的任何内容都可以称之为节点, `标签,文字,注释,标签属性`  等等.....   

![1592278401003](C:/Users/14517/Downloads/day02/01-教学资料/节点笔记/imgs/note.md)

> 每一个节点身上都有三个属性`nodeType`(节点类型) `nodeName`(节点名称)` nodeValue`(节点内容)这三个基本属性

```jsx
// 元素节点: nodeType为1
// 属性节点: nodeType为2
// 文本节点: nodeType为3  文本节点包含文字 换行 空格等

let childNodesList = document.getElementById("myDiv").childNodes;
for (var i = 0; i < childNodesList.length; i++) {
    let theNode = childNodesList[i];
    console.log(`----${i}---`);
    console.log('节点类型:' + theNode.nodeType);
    console.log('节点名称:' + theNode.nodeName);
    console.log('节点取值:' + theNode.nodeValue);
}

实际开发中, 一般都是获取元素节点,属性节点和文本节点一般用不到
```

> 案例 [点击关闭二维码]
>
> 案例 [仿新浪下拉菜单]

## 创建节点(元素)

> 在实际工作当中, 很多节点都是通过JS动态创建出来的 比如分页, 下拉加载更多

```jsx
let node = document.createElement('标签名称')

tips: 创建出来的节点是在内存中存在的.所以,只有有创建节点,一定就有添加节点的操作
```

## 添加节点

### appendChild

=>内容后面添加

语法：parent.appendChild(child)

parent: 调用者，父节点来调用

child:需要添加的那个孩子。

作用：把child添加到parent的孩子的最后面。

ps:如果添加的是页面中本来就存在的元素，是一个剪切的效果，原来的就不在了。

```javascript
let demo = document.getElementById("demo");
let box = document.getElementById("box");
box.appendChild(demo);
```

### insertBefore

=>内容前面添加

语法：parent.insertBefore(child, refChild);

parent:必须要父节点来调用

child：需要添加的那个节点

refChild:添加到哪一个节点的前面。

如果refch ild元素找不到,默认会以appendchild添加

```javascript
var ul = document.getElementById("list");
var li = document.createElement("li");
li.innerHTML = "飞飞";
//就是添加到子节点的最前面。
ul.insertBefore(li, ul.children[0]);
```

## 删除节点

语法：parent.removeChild(基于当前元素去找到要删除的元素);

功能：由父盒子调用，删除里面的一个子元素。

删全部:parent.innerHtml=' ';



## 克隆节点

语法：let newNode = 目标节点.cloneNode(true)

功能：在内存中克隆一份节点

参数：deep

- false：默认值：是浅复制，只会复制标签，节点本身，不会复制节点的孩子。
- true: 深度复制，会复制标签，还会复制标签的所有内容 --- **常用**

> 1. 克隆出来的节点跟原来的节点没有关系了，修改了也不会相互影响。
> 2. 如果克隆的节点带了id，我们需要给id重新设置一个值，不让id冲突





# 事件的高级运用

>  目标

```
1. 能够写出元素注册事件的两种方式
2. 能够说出删除事件的两种方式
3. 能够说出DOM事件流的三个阶段
4. 能够利用事件对象完成跟随鼠标案例
5. 能够阻止事件冒泡
6. 能够说出常见的鼠标和键盘事件
```

## 单击

```js
ipt.onclick=function(){}

```

## 双击

```js
ipt.ondblclick=function(){}
```



# 事件对象

## 事件对象的概述

> 在触发某个事件的时候，都会产生一个事件对象Event，这个对象中包含所有与事件相关的一些信息，包括触发事件的元素，事件的类型以及其他与事件相关的信息。

鼠标事件触发时，事件对象中会包含鼠标的位置信息。

键盘事件触发时，事件对象中会包含按下的键相关的信息。

```javascript
每一个事件在触发时，都会产生一个事件对象。
你见或者不见，我就在那里，不悲不喜。
你爱或者不爱，爱就在那里，不增不减。
你用或者不用，我都会给你，不离不弃。 
```

> 既然事件对象中存储了这么多的信息，我们首先需要做的就是获取到这个事件对象。

获取事件对象非常的简单，只需要在注册事件的时候，指定一个形参即可。这个形参就是我们想要获取到的事件对象。

```javascript
btn.onclick = function(event){
    //event就是事件对象，里面包含了事件触发时的一些信息。
	console.log(event);
}
```

## 事件对象的常用属性

> 事件对象中有很多很多的属性，但是很多属性并不常用。我们经常用到的是***鼠标位置信息*** 和***键盘码***  相关的信息。

> https://developer.mozilla.org/zh-CN/docs/Web/API/Event

> 记录了鼠标位置信息的相关属性

```javascript
//获取初始位置
let startX = this.offsetLeft;
let startY = this.offsetTop;
//鼠标落下位置
let X = e.pageX;
let Y = e.pageY;
//移动后的鼠标位置
let mvX = e.pageX;
let mvY = e.pageY
//鼠标移动距离
div.style.left = mvX - X 
div.style.top = mvY - Y 
//实际物体移动距离
div.style.left = mvX - X + startX + 'px'
div.style.top = mvY - Y + startY + 'px'
//
screenX与screenY：光标相对于屏幕左上角的水平位置与垂直位置。
clientX与clientY：光 标相对于可视区左上角的水平位置和垂直位置。
pageX与pageY：光标相对于网页（文档document）左上角的水平位置与垂直位置（推荐使用）
```

> 键盘事件

```js
keydown => 键盘按下
keyup => 键盘抬起
//定义键盘点击
set.onkeydown=function(e){
}
set.onkeyup=function(e){
}
键盘事件一般是给表单或者document注册
给表单注册: 在表单里面输入的时候触发
给document注册: 只要页面获取焦点的时候, 按键盘都会触发

注意点:
	keydown:会在值落到表单之前触发, 所以获取的结果是上一次的结果
	keyup:会在值落到表单之后触发, 所有获取的是当前这一次的结果
```

> 记录了键盘码的属性

```javascript
点击键盘:onkeyup / onkeydown
获取键盘码: 在键盘事件中，通过e.keyCode可以获取到按下的键盘码
```

【微博评论-添加回车发送功能】

```
area.onkeyup=function(){
if(e.keyCode===13){
 send.onclick();
}
}
```

【案例:让小天使飞】

```
 let img = document.querySelector('img');
      document.onmousemove = function (e) {
        // console.log(e.pageX, e.pageY);
        // 将鼠标落点同步给图片的left和top值
        img.style.left = e.pageX + 'px';
        img.style.top = e.pageY + 'px';
      };
```



## 拖拽特效

移除事件(解绑事件)：

```javascript
box.onclick = null;	，
```

```
思路：
	1. 分解拖拽的过程, 搭建基本的拖拽事件框架
		// 注意点1: onmousemove一定要在mousedown之后触发
      	// 注意点2: 在鼠标抬起需要将mousemove解绑
      	// 注意点3: 将mousemove事件注册给document 如果注册给box在移动过快的情况容易移动到box外面去
	2. 在mousedown的时候,获取鼠标落点和当前盒子的位置
	3. 在mousemove的时候重新获取鼠标落点, 使用mousemove的落点减去mousedown的落点得到两者之间移动的距离, 在加上盒子之前的位置
```

【可拖拽的盒子.html】



## 节流阀思想

> 控制事件的触发,让事件函数内部的代码有限制的去执行, 一般用在重复不断触发的事件里面  => mousemove

```
// 节流阀:  一个阀门
// 1. 首先需要考虑什么事件需要使用节流阀  
// 2. 确定阀门最开始是关着还是开着 
// 3. 在合适的时候将阀门打开
// 4. 在合适的时候将阀门关闭 
```

# 事件流

## 事件冒泡

> 当一个元素的事件被触发时，同样的事件将会在该元素的所有祖先元素中依次被触发。这一过程被称为事件冒泡。

说白了就是：当我们触发了子元素的某个事件后，父元素对应的事件也会触发。 ![bubble](C:/Users/14517/Desktop/day03/01-教学资料/事件的高级运用/images/bubble.png)

**注意** : 只要存在嵌套关系的元素,就会有事件冒泡.跟有没有绑定该事件无关



## 事件委托

自己不再执行,委托给别人执行 

// 获取点击 对象 e.target 

## 事件捕获（了解）

> 事件捕获是火狐浏览器提出来的，IE678不支持事件捕获（基本上，我们都是用事件冒泡）
> 事件的处理将从DOM层次的根开始，而不是从触发事件的目标元素开始，事件被从目标元素的所有祖先元素依次往下传递



> 注意： 事件捕获必须通过事件监听绑定，并且将第三个参数设置为true，因为传统事件绑定是采用冒泡的机制

 ![capture](C:/Users/14517/Desktop/day03/01-教学资料/事件的高级运用/images/capture.png)



## 事件流-事件的三个阶段 

1. 事件的捕获阶段
2. 事件的目标阶段（触发自己的事件）
3. 事件的冒泡阶段

事件有三个阶段，首先发生的是捕获阶段，然后是目标阶段，最后才是冒泡阶段，让`addEventLinstener`第三个参数为true时，表示该事件在捕获阶段发生，如果第三个参数为false，表示该事件在冒泡阶段发生。

**细节:** on注册的事件只会在冒泡阶段触发

# 事件补充：

## 事件冒泡的好处

> 当多个DOM对象需要绑定同一个事件的时候, 我们不需要单独给每一个元素绑定事件,我们只需要给其父元素绑定事件即可 利用冒泡的原理触发该事件， 我们称之为事件委派

```
在事件委派中,事件源是恒定的(父元素), 我们可以通过事件对象 e.target去获取当前触发的那个元素
```

## 阻止事件冒泡

> 通过e.stopPropagation()方法可以阻止事件冒泡

```
1. 先要明确那一块区域不能冒泡
2. 需要阻止什么事件传递就给这块区域的最大盒子注册该事件
3. 在事件处理函数里面接受事件对象, 并添加上e.stopPropagation()
最大盒子.onclick=function(e){
e.stopPropagation()
}
```

【案例：登录框】

## 阻止浏览器默认行为

> 阻止浏览器默认行为除了使用 `
>
> 还可以通过 e.preventDefault() 可以阻止浏览器的默认行为

```js
submit.onclick = function (e) {
      // 阻止默认行为
      e.preventDefault();
      if (ipt.value === '10') {
        // 提交
        form.submit();
      }
    };
```



# offset系列

### offsetHeight与offsetWidth

> offsetHeight与offsetWidth

```javascript
1. 获取的是元素真实的高度和宽度
2. 获取到的是数值类型，方便计算
3. offsetHeight与offsetWidth是只读属性，不能设置。
```

```javascript
获取宽度和高度offsetWidth与offsetHeight
```

### offsetParent

> parentNode和offsetParent

```javascript
1. parentNode始终是父元素
2. offsetParent是离当前元素最近的定位元素(absolute、relative)，如果没有，那就找body
```

### offsetLeft与offsetTop

> offsetLeft: 自身左侧到offsetParent左侧的距离；
>
> offsetTop:自身顶部到offsetParent顶部的距离；

```javascript
1.	元素自身与offsetParent真实的距离
2.	获取到的是数值类型，方便计算
3.	只读属性，只能获取，不能设置
```

 

# 放大镜效果（案例）

> 放大镜在开发中是一个很常见的特效，但是所有的放大镜的实现效果都是一样。

【案例-07-放大镜特效】

实现思路：

```javascript
1. 给box注册onmouseover事件，让big和mask显示
2. 给box注册onmouseout事件，让big和mask隐藏
3. 给box注册onmousemove事件，获取鼠标在box中的位置，让mask跟着移动
鼠标落点减去大盒子的具体位置-小盒子的一半
4. 限定mask的移动范围
5. 根据比例让bigImg跟着移动

// 比例问题:
// 张三和李四比赛吃包子  张三吃10个包子, 李四可以吃20个 
// 请问: 张三吃到第4个包子的时候, 李四应该吃到第几个包子
```

# 注册事件的新方式（事件监听）

> 传统事件绑定： 只能注册一个，如果注册了多个，会出现覆盖问题。
>
> on事件的方式只能在冒泡阶段触发
>
> 事件监听 
>
> DOM对象.addEventListener(事件类型,事件处理函数,是否使用捕获机制)

addEventListener与removeEventListener

> 现代浏览器支持的注册事件的新方式，这种方式注册的事件不会出现覆盖问题。

addEventListener的语法

```javascript
//第一个参数：事件的类型：click mouseover
//第二个参数：函数，监听者，每次点击，这个函数就执行。
//第三个参数：是否使用捕获，默认为false，表示冒泡
addEventListener(type, 函数名字, useCapture);
```

**tips：如果想要让你注册的事件能够移除，不能使用匿名函数。** 

```javascript
function fn1() {
    alert("hehe");
}
//如果想让注册的事件能移除，不能用匿名函数。
box.addEventListener("click", fn1, false);
```

removeEventListen的语法

```javascript
//第一个参数：参数类型
//第二个参数：要移除的那个函数
//第三个参数：false 可省略
removeEventListener(type, fn1, false);	
```

# 



# BOM

> BOM（Browser Object Model）：浏览器对象模型，提供了一套操作浏览器功能的工具。

 ![1592535955541](D:/KingPan/04WebAPi/day04/day04/01-教学资料/BOM/images/1592535955541.png)



BOM包含的内容很多，但是很多东西都不太常用，在BOM中需要大家掌握的就一个东西，那就是***定时器*** 。

## window对象

```
1. window对象是一个全局对象，也可以说是JavaScript中的顶级对象
2. 像document、alert()、console.log()这些都是window的属性，基本BOM的属性和方法都是window的。
3. 所有定义在全局作用域中的变量、函数都会变成window对象的属性和方法
4. window对象下的属性和方法调用的时候可以省略window
```

### window.onload（掌握）

> window.onload事件会在窗体加载完成后执行，通常我们称之为入口函数。

```javascript
window.onload = function(){
	//里面的代码会在窗体加载完成后执行。
	//窗体加载完成包括文档树的加载、还有图片、文件的加载完成。
}
```

思考：一个页面能不能写两个window.onload?

## 延时器与定时器

### setTimeout

> 延迟一段时间之后再去执行对应的函数，只会执行一次

>  设置延时器

```javascript
// 语法：setTimeOut(callback, time);
// 参数1：回调函数，时间到了就会执行。
// 参数2：延时的时间, 以毫秒计数
// 返回：定时器的id，用于清除
// 示例：
let timerId = setTimeOut(function(){
	//1秒后将执行的代码。
}, 1000);
// 注意: 每一个定时器调用都会产生一个不同的定时器id
```

> 清除延时器

```javascript
// 语法：clearTimeOut(timerId)
// 参数：定时器id
// 示例：
clearTimeOut(timerId); // 清除上面定义的定时器
```

### setInterval

> 每间隔一段时间就去执行对应的函数

```
对比延时器: 最大的不同在执行次数上不同  
延时器: 一次  
定时器: 一直执行 除非清除该定时器
```

>  设置定时器

```javascript
// 语法：let intervalID = setInterval(func, delay);
// 参数1：重复执行的函数
// 参数2：每次间隔的毫秒数
// 返回：定时器的id，用于清除
// 示例：
let timer = setInterval(function(){
	//重复执行的代码。
}, 1000);
```

>  清除定时器

```javascript
//语法：clearInterval(intervalID)
//参数：定时器id
//示例：
clearInterval(timer);//清除上面定义的定时器
```

> 暴力解决开启定时器问题

```
当用户暴力点击的时候,会出现开启多个定时器的问题!!

解决: 
	方式1: 触发了定时器之后, 立即禁用对应的触发按钮
	方式2: 在每一次开启定时器之前, 将上一次的定时器清除掉即可	 (注意: 装定时器的变量需要是全局变量) 
```

### 案例

【倒计时】

```
 let aa = setInterval(function () {
                //倒计时实现
                time--;
                //判断是否1
                if (time <= 0) {
                    //清除倒计时
                    clearInterval(aa);
                    //解除禁止点击
                    button.disabled = false;
                    //返回
                    return;
                }
```

【短信验证码案例.html】

```js
  <button>点击发送短信</button>
  <script>
let button = document.querySelector("button");
        button.onclick = function () {
            //点击后禁止点击
            button.disabled = true;
            //定义一个倒计时
            let time = 4;
            //定义一个变量接收解除ID
            let aa = setInterval(function () {
                //倒计时实现
                time--;
                //判断是否1
                if (time <= 0) {
                    //清除倒计时
                    clearInterval(aa);
                    //解除禁止点击
                    button.disabled = false;
                    //重置文字
                    button.innerText = '点击发送短信';
                    //返回
                    return;
                }
                button.innerText = `${time}s后再次发送`;
            }, 1000);
        }
         </script>
```



## location对象

> location对象也是window的一个属性，location其实对应的就是浏览器中的地址栏。

### 常用属性和方法

```javascript
location.href => 可以设置和获取页面的地址
location.reload() => 重载页面
```

【案例：定时跳转.html】

## navigator对象

> window.navigator的一些属性可以获取客户端的一些信息

```javascript
//navigator.userAgent：浏览器版本
```

判断设备

```
if（navigator.userAgent indexOf（mobile === -1））{
不是移动端
}else{
移动端
}
```



## history对象

history对象表示页面的历史

```javascript
//后退：
history.back();
history.go(-1);
//前进：
history.forward();
history.go(1);
```

# 动画 

### 原理

> 每一次基于当前的位置在往前移动一定的距离即可

```
步骤:
1. 获取当前盒子的位置
2. 基于这个位置在增加一个距离(步长)
3. 使用定时器不断执行这个步骤
4. 添加一个结束的条件 清除定时器
```

### 简单封装

```
1. 多个元素需要移动?
2. 移动的目标位置不一致?
3. 移动的速度不一致?

提取出来作为参数
```

### 反向移动

> 右移: 基于当前位置加步长   >= 目标位置
>
> 左移: 基本当前位置减步长   <= 目标位置

```
在最开始的判断: 如果目标位置 > 初始位置, 那么就是加步长 否则就是减步长
结束的条件需要判断: 将目标位置和当前位置做差, 如果差值的绝对值小于步长的绝对值,代表快要到达终点咯
```

### 解决定时器的BUG

> 1. 当点击右边移动的时候,在没有到达目标的时候点击往左边移动, 会出现抽搐的BUG
> 2. 当连续点击的时候,会出现加速行为

```
BUG的根源来源于定时器多次触发, 没有及时清除,所有出现了多个定时器干扰的问题
解决: 每一次开定时器的时候,清除上一次定时器即可 
```

> 解决过程

```
1. 使用局部变量存timeId => 每一次局部变量会被重新初始化,无法存储上一次的timeId
2. 使用全局变量存timerId => 可以保存上一次的timerId,但是无法解决多个元素同时移动的问题(下一个对象会将上一个对象的定时器给清除咯,所以导致只有最有一个对象会移动)
3. +
```

### 添加回调函数

> 当动画执行结束之后需要做的事情 由于每一次调用之后做的事情不一样,所以可以将这个事情作为一个函数传递进去,在动画结束之后调用这个函数即可

> 回调函数: 把A函数当做参数传递给B函数, 在B函数内部调用A函数, 一般用于当某件事情执行完毕之后再去执行该函数

```
事件处理函数就是一个典型的回调函数. 将函数交给浏览器,当用户触发了之后浏览器在去调用这个函数
```

# 轮播图

> 是一种常见的网页特效, 主要目的是练习逻辑思维和Api的使用

## 静态布局

> 轮播图的布局核心是一个很长的盒子里面放了很多的浮动li, 利用Js动态修改长盒子的left值实现轮播效果

```html
<div class="box">
    <!-- 放图片的 -->
    <ul>
        <li>
            <a href="#"><img src="./images/1.jpg" alt="" /></a>
        </li>
        <li>
            <a href="#"><img src="./images/2.jpg" alt="" /></a>
        </li>
        <li>
            <a href="#"><img src="./images/3.jpg" alt="" /></a>
        </li>
        <li>
            <a href="#"><img src="./images/4.jpg" alt="" /></a>
        </li>
        <li>
            <a href="#"><img src="./images/5.jpg" alt="" /></a>
        </li>
    </ul>
	<!-- 箭头 -->
    <span class="arrow arrow-l">&lt;</span>
    <span class="arrow arrow-r">&gt;</span>

    <!-- 放小圆点 -->
    <ol>
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ol>
</div>
```

## 添加左右箭头hover效果

```css
.arrow {
    width: 50px;
    height: 100px;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    font-family: '宋体';
    text-align: center;
    line-height: 100px;
    font-size: 24px;
    cursor: pointer;
    display: none;
}

.box:hover .arrow {
    display: block;
}

.arrow-l {
    left: 0;
}

.arrow-r {
    right: 0;
}
```

## 添加小圆点效果

> 需求: 点击哪个小圆点,  实现基本的轮播效果

```
轮播图的核心: 改变ul的left取值
基本算法: ul的left取值 = 点击的小圆点下标 * 一张图的距离 (注意:因为ul是往左边移动, 所以left是负数)
```

> 思路

```
1. 遍历小圆点
2. 批量绑定点击事件
3. 利用基本算法改变ul的left取值
4. 引入animate.js 以动画的形式移动
5. 给小圆点实现排他
```

## 动态生成小圆点

> 需求: 小圆点可以自动根据图片的张数自动生成,这样后期动态概念图片的张数也不会干扰到小圆点

```
1. 获取所有的图片li元素
2. 遍历所有的图片li元素
3. 每遍历一个就创建一个小圆点
4. 将小圆点追加到ol里面去
5. 在遍历的时候判断,如果是第一个就自动添加一个active类

注意点: 动态添加小圆点的代码需要放到小圆点点击效果前面, 因为只有先有小圆点,才能获取小圆点
```

## 右侧按钮的点击效果

> 需求: 点击右侧按钮实现无缝轮播, 因为是一个按钮触发,所以没有办法存下标.就使用变量模拟

```
1. 设置一个变量,代表下标
2. 每点击一次,该变量自增
3. 实现无缝轮播
```

> 需求: 无缝轮播

```
无缝轮播其实是一个障眼法, 是将第一张图片拷贝一份到最后一张作为临时工,当用户点击到咯最后一张临时工的之后,下一次点击直接使用style的形式跳到第一张图片(由于第一张和最后一张是完全一致的图片,所以用户看不出来)

1. 克隆第一个li 追加到ul的最后面
2. 在自增"前面进行判断",如果超出了临时工的下标,重置下标为0,并以style的形式回到0

注意点：
'	1. 极值判断放在自增的前面, 是为,了方便无缝处理
	2. 如果获取图片的是动态集合(推荐使用静态集合),那么动态创建小圆点的代码需要放到克隆的前面去,在克隆之前创建
```

> 需求: 同步小圆点

```
点击的时候,让小圆点也跟随变化

1. 设置一个全局变量控制小圆点的下标
2. 点击的时候自增
3. 进行极值判断, 如果超出最大下标,回到0即可
4. 进行排他

小圆点的个数是正常的, 没有所谓的临时工,按照正常的逻辑即可
```

> 需求:  解决BUG. 

```
当点击小圆点看到第三张图片, 在点击右箭头看下一张, 会出现不统一的情况. 原因是点击小圆点的下标是自定义的index下标属性,而点击右箭头则是全局变量的下标,两个下标不统一

解决: 在点击小圆点的时候, 那当前事件源的index下标赋值给全局变量的下标
```

## 左侧按钮的点击效果

```
原理和右侧按钮一致. 
```

## 自动轮播

```
1. 添加一个定时器
2. 在定时器里面调用click事件
3. 在鼠标移入的清除定时器, 移出的时候重启定时器
```

## 解决暴力点击

```
动画的执行是需要时间的, 而如果快速点击导致短时间触发了很多动画,导致体验不太好.所以可以使用节流阀来限制事件的执行

节流阀:  一个阀门
1. 首先需要考虑什么事件需要使用节流阀  => click
2. 确定阀门最开始是关着还是开着  => 开着
3. 在合适的时候将阀门关闭 => 点击了之后立马关闭
4. 在合适的时候将阀门开启 => 动画执行结束之后开启
```

# 三大家族

> 在网页当中, 有三大家族 分别是offset家族. scroll家族, client家族

## offset家族

```
offsetWidth		=> 元素的实际宽度
offsetHeight	=> 元素的实际高度
offsetLeft		=> 元素左侧距离offsetParent元素左侧的距离
offsetTop 		=> 元素顶部距离offsetParent元素懂不的距离
offsetParent	=> 最近的定位父元素

特点: 都是只读属性,只能获取,不能设置 是网页特效中使用的最多的一类
```

## scroll 家族

```
scrollWidth 	=> 文字区域 + padding区域
scrollHeight 	=> 文字区域 + padding区域
scrollTop 		=> 水平滚动的大小
scrollLeft		=> 垂直滚动的大小

特点: scroll家族使用的不多, scrollTop使用的相对较多一些

常用事件: onscroll事件 => 滚动条滚动的时候出  scrollTop和scroll事件经常配合一起使用, 可以实现在滚动的时候实时获取滚动的距离
```

> 获取页面级别的scrollTop的大小

```
// 固定写法
window.onscroll = function(){
	var _scrollTop =
          window.pageYOffset ||
          document.documentElement.scrollTop ||
          document.body.scrollTop;
   	console.log(_scrollTop)
}
```

## client家族

```
clientWidth		=>可视区域的宽
clientHeight 	=>可视区域的高
clientLeft 		=> 边框的宽度
clientTop		=> 边框的高度

说明: 基本不用, 了解即可
```







# 面试习题

## 事件完整流程

先true在false

事件流的三个阶段

## 事件冒泡

> 当一个元素的事件被触发时，同样的事件将会在该元素的所有祖先元素中依次被触发。这一过程被称为事件冒泡。

说白了就是：当我们触发了子元素的某个事件后，父元素对应的事件也会触发

## 函数节流

解决暴力点击速度异常

```js
right.onclick = function () {
            //  节流判断
            if (flag) {
                pIndex++;
                if (pIndex >= lis.length) {
                    pIndex = 0;
                }
                move(ul, -(pIndex * box.offsetWidth), 80, function () {
                    //解开节流
                    flag = true;

                })

                for (let pIndex = 0; pIndex < point.length; pIndex++) {
                    point[pIndex].classList.remove('active');

                }
                point[pIndex].classList.add('active');
            }
            //开启节流
            flag = false;
        }
```

