# [在Webview窗口中添加子窗口](http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewObject.append)

```js

<!DOCTYPE html>
<html>
	<head>
	<meta charset="utf-8">
	<title>Webview Example</title>
	<script type="text/javascript">
var embed=null;
// H5 plus事件处理
function plusReady(){
	embed=plus.webview.create('http://m.weibo.cn/u/3196963860', '',{top:'46px',bottom:'0px'});
	plus.webview.currentWebview().append(embed);
}
if(window.plus){
	plusReady();
}else{
	document.addEventListener('plusready', plusReady, false);
}
	</script>
	</head>
	<body>
		在Webview窗口中添加子窗口
		<button onclick="plus.webview.currentWebview().close();">Back</button>
	</body>
</html>
```



# show()

显示已创建或隐藏的Webview窗口，需先获取窗口对象或窗口id，并可指定显示窗口的动画及动画持续时间。

```js


<!DOCTYPE html>
<html>
	<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
	<title>Webview Example</title>
	<script type="text/javascript">
// H5 plus事件处理
function plusReady(){
}
if(window.plus){
	plusReady();
}else{
	document.addEventListener('plusready', plusReady, false);
}

// 创建并显示新窗口
function create(){
	var w = plus.webview.create('http://m.weibo.cn/u/3196963860');
	plus.webview.show(w); // 显示窗口
}
	</script>
	</head>
	<body>
		显示Webview窗口<br/>
		<button onclick="create()">Create</button>
	</body>
</html>

```

# open() 创建并打开Webview窗口

```js
创建并打开Webview窗口

<!DOCTYPE html>
<html>
	<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
	<title>Webview Example</title>
	<script type="text/javascript">
// H5 plus事件处理
function plusReady(){
}
if(window.plus){
	plusReady();
}else{
	document.addEventListener('plusready', plusReady, false);
}

// 创建并显示新窗口
function openWebview(){
	var w = plus.webview.open('http://m.weibo.cn/u/3196963860');
}
	</script>
	</head>
	<body>
		打开Webview窗口<br/>
		<button onclick="openWebview()">Open</button>
	</body>
</html>
		

```
