# margin与padding的区别

## margin&padding

### margin

margin是外边距，是元素与周边其他元素的边距。


### padding {id="padding_1"}

padding是内边距，是元素内部文本与元素边框的边距。

这么说，好像不够直观，来看几段代码和几个截图就明白了。

## margin代码 {id="margin_1"}

### 代码：
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<style>
p
{
	background-color:yellow;
}
p.margin
{
	margin-top:100px;
	margin-bottom:30px;
	margin-right:50px;
	margin-left:20px;
}
body
{
	background-color:blue;
}
</style>
</head>

<body>
<p>没有margin的P标签1</p>
<p class="margin">有margin的P标签</p>
<p>没有margin的P标签2</p>
</body>

</html>
```

### 截图：
![image_1.png](14d60819-031b-45bc-9d9c-6bbf0b7e4b6d.png)


## 总结：
整体`body`背景是蓝色。
没有设置margin的p标签就在最上面，按默认长度和间距存在。
设置了margin的p标签，他与上下p标签的间距是按照设定的`px`来的。
我的`margin-top`是`100px`，所以，我跟`没有margin的P标签1`标签的距离特别远。
我的`margin-bottom`是`30px`，所以，我跟`没有margin的P标签2`标签的距离很近
我的`margin-left`是`20px`，所以，背景色黄色跟左侧的距离稍短。
我的`margin-right`是`50px`，所以，背景色黄色跟右侧的距离比左侧稍长。


## padding代码

### 代码：
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<style>
p
{
	background-color:yellow;
}
p.padding
{
	padding-top:25px;
	padding-bottom:100px;
	padding-right:450px;
	padding-left:450px;
}
	body{
	background-color:blue;
	}
</style>
</head>

<body>
<p>没有padding的p标签1</p>
<p class="padding">有padding的p标签</p>
<p>没有padding的p标签2</p>	
</body>

</html>
```


### 截图：
![padding代码效果](e1ad7c10-d1c5-4bf0-8a1c-97a6a0102dea.png)

## 总结：

整体`body`背景是蓝色。
没有设置padding的p标签就在最上面，按默认存在。
设置了padding的p标签，他的字与标签自身边缘的间距是按照设定的`px`来的。
我的`padding-top`是`25px`，所以，我的字离边缘很近。
我的`padding-bottom`是`100px`，所以，我的字离边缘有点距离
我的`padding-left`跟`padding-right`都是`450px`，所以，导致了字会折叠，因为限制了字的左右距离。