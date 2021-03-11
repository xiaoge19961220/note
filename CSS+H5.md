## 复合选择器总结



| 选择器         | 作用                     | 特征                 | 使用情况 | 隔开符号及用法                          |
| -------------- | ------------------------ | -------------------- | -------- | --------------------------------------- |
| 后代选择器     | 用来选择元素后代         | 是选择所有的子孙后代 | 较多     | 符号是**空格** .nav a                   |
| 子代选择器     | 选择 最近一级元素        | 只选亲儿子           | 较少     | 符号是**>**   .nav>p                    |
| 交集选择器     | 选择两个标签交集的部分   | 既是 又是            | 较少     | **没有符号**  p.one                     |
| 并集选择器     | 选择某些相同样式的选择器 | 可以用于集体声明     | 较多     | 符号是**逗号** .nav, .header            |
| 链接伪类选择器 | 给链接更改状态           |                      | 较多     | 重点记住 a{} 和 a:hover  实际开发的写法 |

## 块级元素(block-level)

```
常见的块元素有<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等，其中<div>标签是最典型的块元素。
```

  (1）比较霸道，自己独占一行

（2）高度，宽度、外边距以及内边距都可以控制。

（3）宽度默认是容器（父级宽度）的100%

（4）是一个容器及盒子，里面可以放行内或者块级元素。

PS:

1.只有 文字才 能组成段落  因此 p  里面不能放块级元素，特别是 p 不能放div 

2.同理还有这些标签h1,h2,h3,h4,h5,h6,dt，他们都是文字类块级标签，里面不能放其他块级元素。

## 行内元素(inline-level)

```
常见的行内元素有<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>等，其中<span>标签最典型的行内元素。有的地方也成内联元素
```

## 表格的边框

```
通过表格的cellspacing="0",将单元格与单元格之间的距离设置为0，但是两个单元格之间的边框会出现重叠，从而使边框变粗
通过:table{ border-collapse:collapse; }  去除
```

## 盒子阴影(CSS3)

```
box-shadow:水平阴影 垂直阴影 模糊距离（虚实）  阴影尺寸（影子大小）  阴影颜色  内/外阴影；
```

## after伪元素清除浮动

```
 .clearfix:after {  content: ""; display: block; height: 0; clear: both; visibility: hidden;  }   

 .clearfix {*zoom: 1;}   /* IE6、7 专有 */
```



## 双伪元素清除浮动

```
.clearfix:before,.clearfix:after { 
  content:"";
  display:table; 
}
.clearfix:after {
 clear:both;
}
.clearfix {
  *zoom:1;
}
```

## 定位模式 (position)

| 值         |     语义     |
| ---------- | :----------: |
| `static`   | **静态**定位 |
| `relative` | **相对**定位 |
| `absolute` | **绝对**定位 |
| `fixed`    | **固定**定位 |

使用

| 位模式           | 是否脱标占有位置     | 移动位置基准           | 模式转换（行内块） | 使用情况                 |
| ---------------- | -------------------- | :--------------------- | ------------------ | ------------------------ |
| 静态static       | 不脱标，正常模式     | 正常模式               | 不能               | 几乎不用                 |
| 相对定位relative | 不脱标，占有位置     | 相对自身位置移动       | 不能               | 基本单独使用             |
| 绝对定位absolute | 完全脱标，不占有位置 | 相对于定位父级移动位置 | 能                 | 要和定位父级元素搭配使用 |
| 固定定位fixed    | 完全脱标，不占有位置 | 相对于浏览器移动位置   | 能                 | 单独使用，不需要父级     |

PS:

1. **边偏移**需要和**定位模式**联合使用，**单独使用无效**；
2. `top` 和 `bottom` 不要同时使用；
3. `left` 和 `right` 不要同时使用。

## display 显示

不保留原来的位置

```
display: none 隐藏对象

display：block 除了转换为块级元素之外，同时还有显示元素的意思。
```

## visibility 可见性

保留原先的位置

```
visibility：visible ; 　对象可视

visibility：hidden; 　  对象隐藏
```

## overflow 溢出

| 属性值      | 描述                                       |
| ----------- | ------------------------------------------ |
| **visible** | 不剪切内容也不添加滚动条                   |
| **hidden**  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| **scroll**  | 不管超出内容否，总是显示滚动条             |
| **auto**    | 超出自动显示滚动条，不超出不显示滚动条     |

## 显示与隐藏总结

| 属性           | 区别                   | 用途                                                         |
| -------------- | ---------------------- | ------------------------------------------------------------ |
| **display**    | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
| **visibility** | 隐藏对象，保留位置     | 使用较少                                                     |
| **overflow**   | 只是隐藏超出大小的部分 | 1. 可以清除浮动 2. 保证盒子里面的内容不会超出该盒子范围      |

## 鼠标样式cursor

```
  属性值        	描述    
  default    	小白  默认
  pointer    	小手    
  move       	移动    
  text       	文本    
  not-allowed	禁止    
```

## 轮廓线 outline

outline: 0;   或者  outline: none;

## 防止拖拽文本域resize

```
<textarea  style="resize: none;"></textarea>
```

## 户界面样式总结

| 属性         | 用途                 | 用途                                                         |
| ------------ | -------------------- | ------------------------------------------------------------ |
| **鼠标样式** | 更改鼠标样式cursor   | 样式很多，重点记住 pointer                                   |
| **轮廓线**   | 表单默认outline      | outline 轮廓线，我们一般直接去掉，border是边框，我们会经常用 |
| 防止拖拽     | 主要针对文本域resize | 防止用户随意拖拽文本域，造成页面布局混乱，我们resize:none    |

## vertical-align 垂直对齐

- 有宽度的块级元素居中对齐，是margin: 0 auto;
- 让文字居中对齐，是 text-align: center;

```
vertical-align : 
baseline 图片与文字底部对齐
top     图片与文字顶部对齐
middle  图片与文字居中对齐
bottom 
```

## 去除图片底侧空白缝隙

```
给img vertical-align:middle | top| bottom等等。  让图片不要和基线对齐。
```

## 溢出的文字省略号显示

```
 /*1. 先强制一行内显示文本*/
      white-space: nowrap;
  /*2. 超出的部分隐藏*/
      overflow: hidden;
  /*3. 文字用省略号替代超出的部分*/
      text-overflow: ellipsis;
```

## H5标签

- `header`   ---  头部标签
- `nav`        ---  导航标签
- `article` ---   内容标签
- `section` ---   块级标签
- `aside`     ---   侧边栏标签
- `footer`   ---   尾部标签

## 音频标签

- 音频  -- `audio`
- 视频  -- `video`

## input标签

```
email
url
date
time
month
week
number
tel
search
color
```

## 

## `2D` 转换之 `translate`

- x 就是 x 轴上水平移动

- y 就是 y 轴上水平移动

  ```
  transform: translate(x, y)
  transform: translateX(n)
  transfrom: translateY(n)
  ```

  PS:

  - `2D` 的移动主要是指 水平、垂直方向上的移动
  - `translate` 最大的优点就是不影响其他元素的位置
  - `translate` 中的100%单位，是相对于本身的宽度和高度来进行计算的
  - 行内标签没有效果

## `2D` 转换之 `scale`

用来控制元素的放大与缩小

```
transform: scale(x, y)
```

PS:

- 意，x 与 y 之间使用逗号进行分隔
- `transform: scale(1, 1)`: 宽高都放大一倍，相当于没有放大
- `transform: scale(2, 2)`: 宽和高都放大了二倍
- `transform: scale(2)`: 如果只写了一个参数，第二个参数就和第一个参数一致
- `transform:scale(0.5, 0.5)`: 缩小
- `scale` 最大的优势：可以设置转换中心点缩放，默认以中心点缩放，而且不影响其他盒子

## 动画

```
@keyframes 动画名称 {
    0% {
        width: 100px;
    }
    100% {
        width: 200px
    }
}
```

属性

```
div {

  /* 动画名称 */
  animation-name: move;
  /* 动画花费时长 */
  animation-duration: 2s;
  /* 动画速度曲线 */
  animation-timing-function: ease-in-out;
  linera 匀速
  ease 先低速在加快最后低速
  ease-in动画以低速开始
  ease-out动画以低速结束
  ease-in-out动画以低速开始和结束
  steps()指定间隔数量-步长
  /* 动画等待多长时间执行 */
  animation-delay: 2s;
  /* 规定动画播放次数 infinite: 无限循环 */
  animation-iteration-count: infinite;
  /* 是否逆行播放 */
  animation-direction: alternate;
  /* 动画结束之后的状态 */
  animation-fill-mode: forwards;
}

div:hover {
  /* 规定动画是否暂停或者播放 */
  animation-play-state: paused;
}

```



