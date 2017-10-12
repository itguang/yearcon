

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