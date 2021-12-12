---
title: CSS基础
categories: CSS基础
tags:
  - 基础
  - CSS
---

主要用于记录 CSS 的基本用法

## 选择器

```
全部选择    *
标签选择器  ex：h1
类选择器    .类名  注： 类名选择器可有多个，以空格划分
ID选择器    #ID  ID名唯一
```

### 伪类选择器

```
默认状态       link
已点击的       visited
鼠标在上面     hover
点击 时        active
聚焦           focus
```

### 选择器权重

不同的选择器拥有不同的权重，当冲突时决定当前的样式

```
通配符           权重0
标签，伪类选择器  权重1
类选择器         权重10
ID选择器         权重100
继承             无权重 < 0

```

## 文本

```
字体          font-family
字种          font-weight: normal|bold|100
字号          font-size： 100%（百分比是对比于父元素来说）|20px
颜色          color：#dddd
行高          line-height： 1.5em
换行          word-wrap: break-word(在长单词或 URL 地址内部进行换行)|normal(只在允许的断字点换行)
字体风格      font-style: italic|normal|oblique|inherit
大小写设置    text-transform： capitalize(以大写开头)|uppercase|lowercase
下划线        text-decoration： none|underline(下划线)|overline(上划线)|line-through(删除线)|blink(闪烁文本)
文本阴影      text-shadow：颜色 左右偏移 上下偏移  模糊度
                          red 5px  5px  3px
溢出          overflow： visible|hidden|scroll|auto
文本溢出      text-overflow: clip(修剪文本)|ellipsis(显示省略符号来代表被修剪的文本)|string(使用给定的字符串来代表被修剪的文本);
文本缩进      text-indent: 2em
文本对齐      text-align： center|right|left
垂直对齐方式   vertical-align：top|middle|bottom|length   相对于父元素来说
```

## 盒子模型

CSS 都假定每个元素都会生成一个或多个的矩形框，称之为元素框，元素框中心是内容区域，四周有可选的外边距，内边距，边框和轮廓。

<!-- 内外边距会加宽块元素的体型   -->

块元素之间的边框会取较大的那个

```
边框           border：边框类型  边框宽度  边框颜色
外边距         margin： 上 右 下  左
              居中  margin  0 auto
内边距         padding：上 右 下 左
块级框         box-sizing：content-box|border-box|padding-box
               content-box： 默认，块长宽度只是内容的长宽度，设边距会加大长宽度
               border-box：设置后，块长宽度是包含边距的
               padding-box: winth和height属性包括padding的大小，不包括border和margin
圆角           border-radius：4px(也可以设置单个角的弧度)
轮廓线         outline：边框类型  边框宽度  边框颜色
               outline不会影响其他元素
元素模式       display：block|inline|inline-block|none
显示隐藏       visibility：hidden(不会丢失空间)
内容溢出       overflow：visible|hidden|scroll|auto
尺寸控制       max-width，min-width，min-height，max-height(最小最大长宽)
自动撑满       width/height：fill-availabel
根据内容自适应尺寸  width/height：fit-content，max-content，min-content
```

## 背景配置

### 基本配置

```
背景颜色   background-color: red
背景图片   background-image: url()
背景重复   background-repeat: no-repeat|space(平均分配)
背景固定   background-attachment: fixed(固定)|scroll
背景位置   background-position: center|left|100px
背景尺寸   background-size：宽+高(数值或百分比)|cover(覆盖背景板，可能丢失)|contain(保证图片显示，可能留白)
```

### 盒子阴影

```
box-shadow: 左右偏移量  上下偏移量 （模糊度）  颜色
box-shadow: 10px 10px 10px blue
```

### 颜色渐变

```
线性渐变      backgroud:  linear-gradient((方向),color1,color2,color3)
方向          90deg | to left top
径向渐变      backgroud： radial-gredient((方向)),color1,color2,color3
方向          100px  100px | to left top
重复颜色渐变   backgroud: repeating-line-gredient((方向)，color1，（color1开始位置），color2，（），color3，（）)
              background：repeating-line-gredient
```

## 浮动与定位

### 浮动

- 浮动会使该元素**脱离文档流**对后续元素造成影响
- 浮动的元素一般会放在一个父元素中
- 使用浮动的元素被认为是一个块级元素，但需要高度由此被父元素**感知**，(或者清除浮动)

```
float: left
```

**清除浮动**

```
clear: left|right|both
```

利用伪类元素 ::after 可以清除浮动 content:""

## 定位

相对定位 相对于当前位置的相对位置

```
position:relative
top
bottom
left/right
```

绝对定位 参照具有定位属性且不是 static 的最近容纳块
脱离了文件流

```
position:absolute
```

## 弹性盒模型

弹性盒容器属性

```
弹性盒模型           display: flex|inline-flex
元素方向             flex-direction: column|column-revese|row-reverse|row
弹性元素溢出(换行)   flex-wrap: wrap|wrap-reverse
主轴排列方式            justify-content：center|space—between|space-evenly|space-around|flex-start|flex-end
交叉轴排列              align-item:center|space—between|space-evenly|space-around|flex-start|flex-end


```

弹性盒元素属性

```
单个元素交叉轴排列       align-self：align-item:center|space—between|space-evenly|space-around|flex-start|flex-end
平均分配                flex-grow: 0|1|2|.....(对元素剩余空间按比例进行的分配)
缩小比例                flex-shrink：0|1|2|3....（对元素进行比例缩小）
元素排序                order：0|1|2|3|....

```

## 栅格系统

设置容器后设置行列来进行实现页面的排列

```
display: grid|inline-grid
划行          grid-template-rows： 100px 100px 100px（三行）|33% 33% 33%
划列          grid-template-columns：100px  100px  100px（三列）|33% 33% 33%
划分也可以重复划分  ex： grid-template-rows：repeat(5,20%) (分五份，每份占比20%) 
                                           repeat(auto-fill,100px)(自动填充，每份100px)
                                           repeat(3,1fr)(平均分三份，每份占比为1)
```

<!-- 获取属性，attr() -->
<!-- https://www.bilibili.com/video/BV1tJ411Y7fB -->
<!-- 134-->

```

```
