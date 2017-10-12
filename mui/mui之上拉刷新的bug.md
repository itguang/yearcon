# mui之上拉刷新的bug


关于上拉刷新的官方说明:http://dev.dcloud.net.cn/mui/pullup/


## bug描述:

首先,先上代码:

```js

<!DOCTYPE html>
<html>

	<head>
		<meta charset="utf-8">
		<title>Hello MUI</title>
		<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,user-scalable=no">
		<meta name="apple-mobile-web-app-capable" content="yes">
		<meta name="apple-mobile-web-app-status-bar-style" content="black">
		<link rel="stylesheet" href="../css/mui.min.css">
		<link rel="stylesheet" href="../css/bootstrap.min.css">

		<style>
			html,
			body {
				background-color: #efeff4;
			}
			
			.title {
				margin: 20px 15px 10px;
				color: #6d6d72;
				font-size: 15px;
			}
			
			.progress-red {
				width: 100%;
				height: 25px;
				background-color: #BD2130;
			}
			
			.progress-yellow {
				width: 0%;
				height: 25px;
				background-color: #E0A800;
			}
		</style>
	</head>

	<body>
		<header class="mui-bar mui-bar-nav">
			<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<h1 class="mui-title">销售业绩汇总查询结果</h1>
		</header>

		<!--内容主体-->

		<!--下拉刷新容器-->
		<div id="pullrefresh" class="mui-content mui-scroll-wrapper">
			<div class="mui-scroll">
				<!--数据列表-->
				<table class="table">
					<!--<caption>响应式表格布局</caption>-->
					<thead>
						<tr>
							<th>类品</th>
							<th>季节</th>
							<th>成交数量</th>
							<th>成交金额</th>

						</tr>
					</thead>
					<tbody class="my_table_body">
						<tr class="my_tr">

						</tr>

					</tbody>
				</table>
			</div>

		</div>
	</body>
	<script src="../js/mui.min.js"></script>

	<script>
		mui.init({
			swipeBack: false,
			pullRefresh: {
				container: '#pullrefresh',
				up: {
					auto: true, //可选,默认false.首次加载自动上拉刷新一次
					contentrefresh: "正在加载...", //可选，正在加载状态时，上拉加载控件上显示的标题内容
					callback: pullupRefresh
				}
			}
		});
		
		

		var begindate;
		var enddate;
		mui.plusReady(function() {
			console.log("plusAPI加载完毕1!");
			var data = plus.webview.currentWebview(); //可以得到上个页面携带过来的数据

			begindate = data.startdate;
			enddate = data.enddate;

		});

		/**
		 * 上拉加载具体业务实现
		 */
		var page = 1;

		function pullupRefresh() {
			console.log("上拉刷新请求数据开始!");
			mui('#pullrefresh').pullRefresh().endPullupToRefresh(); //参数为true代表没有更多数据了。
			getdata(page++, 25, begindate, enddate);
			console.log("开始结束时间:!" + begindate + "---" + enddate);
			console.log("本次上拉刷新请求数据结束!");

		}

		function getdata(page,
			size,
			begindate,
			enddate) {
			var wd = plus.nativeUI.showWaiting();
			console.log("开始网络请求");
			mui.post(plus.storage.getItem("url"), {
				act: "salesreport",
				username: plus.storage.getItem('TOKEN_USER'),
				key: plus.storage.getItem('TOKEN_LOGIN'),
				page: page,
				size: size,
				v_begindate: begindate,
				v_endate: enddate
			}, function(data) {
				
				
				console.log("请求成功!");
				console.log("响应状态" + data.status);
				wd.close();
				if(data.status != "1") {
					mui.alert(data.info);
					console.log("请求失败!");
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
					return;
				}

				if(data.lists.length == 0) {
					mui.toast("没有数据了!");
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
					return;
				}
				if(data.status == 1) {
					var table = document.body.querySelector('.my_table_body');
					for(var i = 0; i < data.lists.length; i++) {
						var tr = document.createElement("tr");
						tr.innerHTML = '<td> ' + data.lists[i].attribname + '</td>' + '<td> ' + data.lists[i].attribname1 + '</td>' + '<td> ' + data.lists[i].qty + '</td>' + '<td> ' + data.lists[i].amt + '</td>';
					
						table.appendChild(tr);
						
					}
				}
				if(data.lists.length < size) {

					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
					return;
				}
			}, 'json');
		}
	</script>

</html>

```

这些代码,看似没有任何问题,并且在android上完美运行,但是ios上运行却无法显示数据,页面已知显示正在加载,由于没有mac,我是使用.ipa的包安装测试的,这样一来无法进行ios下真机运行调试,费了好大劲,才发现了bug所在:

原因: plus 环境在ios下未准备完成导致上拉刷新无效.


## 解决方案:

代码如下:


```js

<!DOCTYPE html>
<html>

	<head>
		<meta charset="utf-8">
		<title>Hello MUI</title>
		<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,user-scalable=no">
		<meta name="apple-mobile-web-app-capable" content="yes">
		<meta name="apple-mobile-web-app-status-bar-style" content="black">
		<link rel="stylesheet" href="../css/mui.min.css">
		<link rel="stylesheet" href="../css/bootstrap.min.css">

		<style>
			html,
			body {
				background-color: #efeff4;
			}
			
			.title {
				margin: 20px 15px 10px;
				color: #6d6d72;
				font-size: 15px;
			}
			
			.progress-red {
				width: 100%;
				height: 25px;
				background-color: #BD2130;
			}
			
			.progress-yellow {
				width: 0%;
				height: 25px;
				background-color: #E0A800;
			}
		</style>
	</head>

	<body>
		<header class="mui-bar mui-bar-nav">
			<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<h1 class="mui-title">销售业绩汇总查询结果</h1>
		</header>

		<!--内容主体-->

		<!--下拉刷新容器-->
		<div id="pullrefresh" class="mui-content mui-scroll-wrapper">
			<div class="mui-scroll">
				<!--数据列表-->
				<table class="table">
					<!--<caption>响应式表格布局</caption>-->
					<thead>
						<tr>
							<th>类品</th>
							<th>季节</th>
							<th>成交数量</th>
							<th>成交金额</th>

						</tr>
					</thead>
					<tbody class="my_table_body">
						<tr class="my_tr">

						</tr>

					</tbody>
				</table>
			</div>

		</div>
	</body>
	<script src="../js/mui.min.js"></script>

	<script>
		mui.init({
			swipeBack: false,
			pullRefresh: {
				container: '#pullrefresh',
				up: {
//					auto: true, //可选,默认false.首次加载自动上拉刷新一次
					contentrefresh: "正在加载...", //可选，正在加载状态时，上拉加载控件上显示的标题内容
					callback: pullupRefresh
				}
			}
		});
		
		if(mui.os.plus) {
			mui.plusReady(function() {
				setTimeout(function() {
					console.log("plusAPI加载完毕2!");
					mui('#pullrefresh').pullRefresh().pullupLoading();//这句可以自动执行上拉刷新,有了这句就不要 auto:true

				}, 1000);

			});
		} else {
			mui.ready(function() {
				console.log("muiDOM加载完毕!");
				mui('#pullrefresh').pullRefresh().pullupLoading();//这句可以自动执行上拉刷新,有了这句就不要 auto:true
			});
		}

		var begindate;
		var enddate;
		mui.plusReady(function() {
			console.log("plusAPI加载完毕1!");
			var data = plus.webview.currentWebview(); //可以得到上个页面携带过来的数据

			begindate = data.startdate;
			enddate = data.enddate;

		});

		/**
		 * 上拉加载具体业务实现
		 */
		var page = 1;

		function pullupRefresh() {
			console.log("上拉刷新请求数据开始!");
			mui('#pullrefresh').pullRefresh().endPullupToRefresh(); //参数为true代表没有更多数据了。
			getdata(page++, 25, begindate, enddate);
			console.log("开始结束时间:!" + begindate + "---" + enddate);
			console.log("本次上拉刷新请求数据结束!");

		}

		function getdata(page,
			size,
			begindate,
			enddate) {
			var wd = plus.nativeUI.showWaiting();
			console.log("开始网络请求");
			mui.post(plus.storage.getItem("url"), {
				act: "salesreport",
				username: plus.storage.getItem('TOKEN_USER'),
				key: plus.storage.getItem('TOKEN_LOGIN'),
				page: page,
				size: size,
				v_begindate: begindate,
				v_endate: enddate
			}, function(data) {
				
				console.log("请求成功!");
				console.log("响应状态" + data.status);
				wd.close();
				if(data.status != "1") {
					mui.alert(data.info);
					console.log("请求失败!");
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
					return;
				}

				if(data.lists.length == 0) {
					mui.toast("没有数据了!");
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
					return;
				}
				if(data.status == 1) {
					var table = document.body.querySelector('.my_table_body');
					for(var i = 0; i < data.lists.length; i++) {
						var tr = document.createElement("tr");
						tr.innerHTML = '<td> ' + data.lists[i].attribname + '</td>' + '<td> ' + data.lists[i].attribname1 + '</td>' + '<td> ' + data.lists[i].qty + '</td>' + '<td> ' + data.lists[i].amt + '</td>';
						//							table.insertBefore(tr, table.lastChild);
						table.appendChild(tr);
						//mui('.mui-scroll-wrapper').scroll();
					}
				}
				if(data.lists.length < size) {

					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
					return;
				}
			}, 'json');
		}
	</script>

</html>

```

只需要加上这几句代码:


```js

if(mui.os.plus) {
			mui.plusReady(function() {
				setTimeout(function() {
					console.log("plusAPI加载完毕2!");
					mui('#pullrefresh').pullRefresh().pullupLoading();//这句可以自动执行上拉刷新,有了这句就不要 auto:true

				}, 1000);

			});
		} else {
			mui.ready(function() {
				console.log("muiDOM加载完毕!");
				mui('#pullrefresh').pullRefresh().pullupLoading();//这句可以自动执行上拉刷新,有了这句就不要 auto:true
			});
		}
```

意思是:等到plus 环境准备好,再进行上拉刷新,`mui('#pullrefresh').pullRefresh().pullupLoading();`,这句代码可以自动执行上拉刷新,所以需要注意的是,再上拉刷新的参数配置中,去掉自动刷新的代码 `auto:true`.


