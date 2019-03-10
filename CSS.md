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
