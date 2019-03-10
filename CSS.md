# CSS中遇到的问题

## 盒模型
标准模式 盒子模型的width/height = content + padding + border + margin

IE模式  盒子模型的width/height = content + margin ，其中content包含上面的content + padding + border

两种模式通过html5 第一行的doctype区分

两种盒模型也可以通过box-size切换：content-box就是标准盒模型 border-box就是IE盒模型 inherit是从父div继承

## 行内元素与块级元素
- 行内元素，在文档流中水平排列，不可以设置宽高，宽高为元素内容的宽高，但可以设置行高line-height，不可以设置padding和margin（只在ie6中）的上下边距。其中不能包含块级元素。<span>

- 块级元素，在文档流中垂直排列，可以设置宽高，宽高为元素内容的宽高加，可以设置padding和margin的上下边距。其中可以包含行内元素。<div>

- 行内块级元素，在文档流中水平排列，可以设置宽高，宽高为元素内容的宽高加，可以设置padding和margin的上下边距。其中可以包含行内元素和块级元素。display:inline-block

行内元素可以和块级元素通过display相互转换。

## css选择器
- 类选择器
- id选择器
- 元素选择器
- 通用选择器
- 伪类选择器
- 属性选择器

内联样式>内部样式>外部样式  ！important权值最高 
- 内联样式表的权值最高 1000。
- ID 选择器的权值为 100。
- Class 类选择器的权值为 10。
- HTML 标签（类型）选择器的权值为 1。

## 块级元素并排
- float方法 漂浮的元素脱离了文档流，可能带来布局上的混乱（不推荐）。
- table布局 （各种各样的问题，不推荐）
- flex弹性盒模型
- grid栅格布局

## flex弹性盒模型
![flex](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
```css
.class{
  display:flex/*指定元素为flex元素*/
  display:inline-flex/*指定行内元素为flex元素*/
}
```
```css
.box {
  flex-direction: row | row-reverse | column | column-reverse; \*控制主轴方向*\
  flex-wrap: nowrap | wrap | wrap-reverse; \*控制元素是否换行以及换行方向*\
  flex-flow: <flex-direction> || <flex-wrap>; \*此属性是以上两个属性的简写*\
  justify-content: flex-start | flex-end | center | space-between | space-around; \*定义子元素在主轴上的对齐方式*\
  align-items: flex-start | flex-end | center | baseline | stretch; \*定义元素在交叉轴上的对齐方式*\
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;\*定义多根轴线的对齐方式*\
}
```
