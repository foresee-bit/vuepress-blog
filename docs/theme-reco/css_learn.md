# CSS学习笔记

CSS 层叠样式表 : 是一种标记语言

![image-20220509155144554](css_learn.assets/image-20220509155144554.png)

## 选择器

选择器分为==基础选择器==和==标签选择器==两大类。

### 基础选择器

基础选择器是由单个选择器组成的。

#### 标签选择器

标签选择器是指将HTML中的标签作为选择器的名称进行选择。

优点：能够快速为页面中**同类型**的标签统一设置样式。

缺点：不能设计差异化样式，只能选择全部的当前标签。

#### 类选择器

如果想要差异化选择不同的标签，单独选一个或者几个标签，可以使用类选择器。

结构需要使用class属性来调用class类。

```css
    <style>
        .red {
            color: blue;
            font-size: 26px;
        }
    </style>
	<body>
		<li class="red">冰雨</li>
	</body>
```

##### 多类名

1. 我们可以给一个标签指定多个类名，从而达到更多的选择目的。这些类名都可以选出这个标签。简单理解就是一个标签有多个名字。

```html
    <style>
        .red {
            color: red;
        }
        .font35 {
            font-size: 35px;
        }
    </style>
<div class="red font35">刘德华</div>
```

2. 多类名开发中使用场景
    1. 可以把一些标签元素相同的样式放到一个类里。
    2. 这些标签都可以调用这个公共的类，然后再调用自己独有的类。

#### id选择器

id选择器可以为标有特定id的HTML元素指定特定的样式。

HTML元素以id属性来设置id选择器，css中id选择器以"#"来定义。

```html
    <style>
        #pink {
            color: pink;
        }
    </style>
<div id="pink">迈克尔杰克逊</div>
```

注意：样式#定义，结构id调用，只能==调用一次==，别人切勿调用。

##### id选择器和类选择器的区别

id选择器和类选择器最大的不同是在**使用次数**上。

#### 通配符选择器

在css中，通配符选择器使用 "*"定义，它表示选取页面中所有元素（标签）。

```html
* {
	margin:0;
	padding:0;
  }
```

![image-20220509163633224](css_learn.assets/image-20220509163633224.png)

### 复合选择器

复合选择器是由两个或多个基础选择器，通过不同的方式组合而成的。

常用的复合选择器包括：==后代选择器==、==子选择器==、==并集选择器==、==伪类选择器==等

#### 后代选择器

又称 包含选择器

元素1  元素2  {样式声明}

```html
ol li {
	color: pink;
}
```

元素2可以是儿子也可以是孙子，只要是元素1的后代即可。

#### 子选择器

子元素选择器只能选择作为某元素的最近一级子元素。简单理解就是选择亲儿子。

语法： 元素1  > 元素2 {样式声明}



## 字体属性

CSS Fonts（字体）属性用于定义字体系列、大小、粗细和文字样式（如斜体）。

### 字体

font-family

```html
body {
	font-family: 'Microsoft YaHei',tahoma,arial;
}
```

各种字体之间必须使用英文状态下的逗号隔开。

### 字体大小

font-size 属性定义字体大小。

```html
p {
	font-size: 14px;
}
```

标题标签比较特殊，需要特别指定大小。

### 字体粗细

font-weight           700为加粗，和 bold一个效果。 400为正常。

```html
.bold {
	font-weight: bold;
}
```

### 文字样式

font-style  设置文本的风格，斜体。

```html
p {
	font-style: normal; //正常
	font-style: italic; //斜体
}
```

html中的 (em, i)标签也可以表示倾斜。

### 字体复合属性

```body
body {
	font: font-style font-weight font-size/line-height font-family
}
```

前面两个属性可以省略，但必须保留font-size和font-family属性，否则font属性将不起作用。

![image-20220509185304262](css_learn.assets/image-20220509185304262.png)

## CSS文本属性



#### CSS Text（文本）

属性可以定义文本的外观，比如文本的颜色、对齐文本、装饰文本、文本缩进、行间距等。

#### 颜色 color

#### text-align 对齐文本     

本质是让盒子里面的文字水平居中对齐。

#### text-decoration 装饰文本

一般用  text-decoration: none;   将超链接里的下划线取消。

或者 ==underline== 可以添加下划线。

```html
a {
	text-decoration: none;
}
```

#### text-indent 首行文本缩进   

10px  2em  

==em==:  是一个相对单位，就是当前元素(font-size) 1个文字的大小。

#### line-height 设置行高

可以控制文字行与行之间的距离。

```html
p {
	line-height: 26px;
}
```

![image-20220509194022959](css_learn.assets/image-20220509194022959.png)

测量行高：从上一行的最下沿到下一行的最下沿。

![image-20220509194526220](css_learn.assets/image-20220509194526220.png)

## CSS的引入方式

### 1. 行内样式表

### 2. 内部样式表

### 3. 外部样式表

在html页面中，使用<link>标签引入css文件

```html
<link rel="stylesheet" href="css文件路径">
```



