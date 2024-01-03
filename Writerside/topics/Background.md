# Background

## body背景色

### 代码：



```html
<html>
<head>
<meta charset="utf-8"> 
<style>
body
{
	background-color:#128ce9;
}
</style>
</head>

<body>

<h1>这是H1</h1>
<p>这是P</p>

</body>
</html>
```

### 效果：
![image.png](../images/front/css/Background/26dce47a-f119-4b46-abeb-905f01db9dce.png)


## 标签背景色


### 代码：



```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<style>
h1
{
	background-color:yellow;
}
p
{
	background-color:pink;
}
div
{
	background-color:orange;
}
</style>
</head>

<body>

<h1>这是h1</h1>
<div>
这是p的上面
<p>这是p</p>
这是p的下面
</div>

</body>
</html>
```

### 效果：
![image.png](../images/front/css/Background/94c2538c-6af2-4152-a5f6-ac266d130729.png)


## 背景图


### 代码：



```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<style>
body 
{
	background-image:url('/Users/xxx/Downloads/back.jpeg');
	background-color:blue;
}
</style>
</head>

<body>
<h1>Hello World!</h1>
</body>

</html>
```

### 效果：
![image.png](../images/front/css/Background/f15d7532-3c4a-4a79-a444-d6a93baad117.png)


## repeat-x


图片水平平铺

### 代码：



```html
background-repeat:repeat-x;
```

### 效果：
![image.png](../images/front/css/Background/8ebd65d3-afb5-4ccb-931b-3c2b726d8297.png)


## repeat-y


图片垂直平铺

### 代码：



```html
background-repeat:repeat-y;
```

### 效果：
![image.png](../images/front/css/Background/438013ce-fdd0-44e9-966e-e8ca2fd3d09a.png)



## no-repeat

图片不平铺

### 代码：



```html
background-repeat:no-repeat;
```

### 效果：
![image.png](../images/front/css/Background/2579f288-43a1-4820-953e-59685c261c65.png)



## position


定位

### 代码：
```html
background-position:right top;
```

### 效果：
![image.png](../images/front/css/Background/e0ae1954-0228-45cf-b1d3-f9a4f2661d02.png)