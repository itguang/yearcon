
# [evalJS()方法](httpwww.html5plus.orgdoczh_cnwebview.html#plus.webview.WebviewObject.evalJS)


在Webview窗口中执行JS脚本



```js

<!DOCTYPE html>
<html>
	<head>
	<meta charset="utf-8">
	<title>Webview Example</title>
	<script type="text/javascript">
		var ws=null,embed=null;
		// H5 plus事件处理
		function plusReady(){
			ws=plus.webview.currentWebview();
			embed=plus.webview.create('http://m.weibo.cn/u/3196963860', '', {top:'46px',bottom:'0px'});
			//将另一个Webview窗口作为子窗口添加到当前Webview窗口中，添加后其所有权归父Webview窗口，当父窗口关闭时子窗口自动关闭。
			ws.append( embed );
		}
		if(window.plus){
			plusReady();
		}else{
			document.addEventListener('plusready', plusReady, false);
		}
		// 在Webview窗口中执行JS脚本
		function evalJS() {
			embed.evalJS('alert("evalJS: "+location.href);');
		}
	</script>
	</head>
	<body>
		在Webview窗口中执行JS脚本
		<button onclick="evalJS()">evalJS</button>
	</body>
</html>
```


# plusReady()最佳实践


```js
// H5 plus事件处理
		function plusReady(){
			ws=plus.webview.currentWebview();
			embed=plus.webview.create('http://m.weibo.cn/u/3196963860', '', {top:'46px',bottom:'0px'});
			ws.append( embed );
		}
		if(window.plus){
			plusReady();
		}else{
			document.addEventListener('plusready', plusReady, false);
		}


```