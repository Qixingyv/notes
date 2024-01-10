# 1 CSS简介

CSS：层叠样式表，Cascading Style Sheets的简称。又名CSS样式表或级联样式表。一种标记语言

作用：让HTML去做结构的呈现，样式交给CSS。样式与结构相分离

## 1.1 语法规范

```css
h1 {
    color: red;
    font-size: 25px;
}
```

1. 选择器：用于指定css样式的标签，h1就是选择器
2. 大括号内就是具体的样式，样式为属性值
3. 属性和属性值用`:`分割
4. 多个属性对用`;`分割

## 1.2 CSS导入方式

css一共有3种导入方式

### 1.1 内联样式

在标签内部使用style属性，属性值是css属性键值对

```html
<div style="color: red">Hello CSS~</div>
```

> 给方式只能作用在这一个标签上，如果其他的标签也想使用同样的样式，那就需要在其他标签上写上相同的样式。复用性太差。不推荐

### 1.2 内部样式

定义<style>标签，在标签内部定义css样式，<style标签在<head>中定义

```html
<style type="text/css">
	div{
		color: red;
    }
</style>
```

### 1.3 外部样式

编写单独的css文件，在html文件中定义link标签，引入外部的css文件

```css
<!--定义css文件-->
div{
	color: red;
}
```

```html
<link rel="stylesheet"  href="demo.css">	<!--在html文件中导入css文件-->
```

> <link>标签要定义在<head>标签中，这种方式可以在多个页面进行复用。其他的页面想使用同样的样式，只需要使用 `link` 标签引入该css文件。

# 2 CSS选择器

## 2.1 基础选择器

### 2.1.1 元素选择器

```css
元素名称 {
    color: red;
}
```

利用html标签作为选择器

### 2.1.2 类选择器

差异化选择不同的标签，单独选一个或者几个标签。使用类选择器。同时，可以在一个标签内部指定多个类名，该标签可以共享多个属性

```css
.class属性值 {
    color: red;
}
```

例子：

```html
<html>
    <head>
        <style>
            .red {
                color: green;
            }
            .fonts {
                font-size: 20px;
            }
        </style>
    </head>
    
    <body>
        <div class="red fonts">
            
        </div>
    </body>
</html>
```

### 2.1.3 id选择器

id选择器只能在html中使用一次

```css
#id属性值 {
    color: red;
}
```

> 优先级：id选择器>class选择器>元素选择器

### 2.1.4 通配符选择器

作用：整个文档将全部适配该选择器中的样式

```css
* {
	font-size: 16px;
}
```

## 2.2 复合选择器

# 3 CSS样式

## 3.1 字体属性

css字体属性用于定义字体，大小，粗细和文字样式

### 3.1.1 字体系列

```css
p {
    font-family: "Microsoft Yahei"; 
}
```

1. 各种字体必须使用逗号隔开
2. 多个单词构成的字体加引号
3. 尽量使用系统自带字体，保证可以在用户的浏览器上显示
4. 最常用字体: `Microsoft YaHei`,`tahoma,arial`,`Hiragino Sans GB`

### 3.1.2 字体大小

```css
p {
    font-size: 20px;
}
```

1. 编写网页时，要给所有文字一个明确的字体大小，不要使用默认
2. 最常用是16px

### 3.1.3 字体粗细

```css
p {
    font-weight: blod;
}
```

| 属性值  | 描述                       |
| ------- | -------------------------- |
| normal  | 默认值（不加粗）           |
| bold    | 定义粗体（加粗）           |
| 100~900 | 400等于normal，700等于bold |

1. 通过该属性，可以让标题等标签不加粗
2. 用数字表示粗细

### 3.1.4 文字样式

```css
p {
    font-style: normal;
}
```

| 属性值 | 作用                       |
| ------ | -------------------------- |
| normal | 默认值，浏览器显示标准字体 |
| italic | 浏览器显示倾斜字体         |

### 3.1.5 复合属性

```css
body {
    font: font-style font-weight font-size/line-height font-family;
}
```

1. 不能更换上述书写顺序
2. 可以简写，必须保留font-size和font-family

## 3.2 文本属性

### 3.2.1 文本颜色

```css
div {
    color: red;
}
```

| 表示方式     | 属性值                         |
| ------------ | ------------------------------ |
| 预定义颜色值 | red,green,blue等               |
| 十六进制     | #FF0000                        |
| RGB代码      | rgb(255,0,0)或rgb(100%, 0%,0%) |

### 3.2.2 对齐方式

```css
div {
    text-align: center;
}
```

| 属性值 | 解释     |
| ------ | -------- |
| left   | 左对齐   |
| right  | 右对齐   |
| center | 居中对齐 |

