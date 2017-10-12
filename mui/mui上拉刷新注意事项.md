

## 上拉刷新


```js


				function pullupRefresh() {
					console.log("上拉刷新开始...当前页" + page);
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(); //参数为true代表没有更多数据了。
					initdata(page++,25);
					//					initdata(2);

					console.log("上拉刷新结束...");

				}
```



```js
<!DOCTYPE html>
<html>

	<head>
		<meta charset="utf-8">
		<title>Hello MUI</title>
		<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,user-scalable=no">
		<meta name="apple-mobile-web-app-capable" content="yes">
		<meta name="apple-mobile-web-app-status-bar-style" content="black">

		<!--标准mui.css-->
		<link rel="stylesheet" href="../../css/mui.min.css">
		<!--App自定义的css-->
		<!--<link rel="stylesheet" type="text/css" href="../css/app.css" />-->
		<style>
			.mui-card .mui-control-content {
				padding: 10px;
			}
			
			.mui-control-content {}
			
			.fdiv {
				float: left;
			}
		</style>
	</head>

	<body>
		<header class="mui-bar mui-bar-nav">
			<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<h1 class="mui-title">会员回访</h1>
		</header>
		<div class="mui-content">
			<div style="padding: 10px 10px;">
				<div id="segmentedControl" class="mui-segmented-control">
					<a class="mui-control-item mui-active" href="#item1">未处理</a>
					<a class="mui-control-item" href="#item2">已处理</a>
				</div>
			</div>
			<div id="pullrefresh" class="mui-content mui-scroll-wrapper">
				<div id="item1" class="mui-control-content mui-active">
					<ul class="mui-table-view deal-yes">
						<li class="mui-table-view-cell mui-media vipdata-yes">
							<a href="javascript:;">
								<div id="" class="fdiv" style="width: 15%;">
									<img onclick="imgclick()" class="mui-media-object mui-pull-left" src="../../images/custom.jpg">
								</div>

								<div class="fdiv" style="width: 65%;">
									<p style="height: 50%;"><span class="vip-name" style="color: #000000;font-weight:bold;">小明 </span>
										<span class="integral" style="margin-right: 10px;">积分:100</span>
										<span onclick="call()" id="call" class="mui-icon mui-icon mui-icon-phone call" style="color: #28A745;"></span>
									</p>

									<p class="cardno" style="height: 50%;" class='mui-ellipsis'>卡号:12121212312312</p>
								</div>
								<div class="fdiv" style="width: 20%;">
									<!--<p ><button style="font-size: 10px;padding-bottom: 0.5px;margin-bottom:2px ;" type="button" class="mui-btn mui-btn-outlined">打电话</button></p>-->
									<button class="deal" type="button" class="mui-btn mui-btn-outlined">已处理</button>
								</div>
							</a>
						</li>

					</ul>
				</div>
				<!--已处理-->
				<div id="item2" class="mui-control-content">
					<ul class="mui-table-view deal-no">
						<li class="mui-table-view-cell mui-media vipdata-no">
							<a href="javascript:;">
								<div id="" class="fdiv" style="width: 15%;">
									<img onclick="imgclick()" class="mui-media-object mui-pull-left" src="../../images/custom.jpg">
								</div>

								<div class="fdiv" style="width: 65%;">
									<p style="height: 50%;"><span class="vipname" style="color: #000000;font-weight:bold;">小明 </span>
										<span class="integral" style="margin-right: 10px;">积分:100</span>
										<!--<span id="call" class="mui-icon mui-icon mui-icon-phone call" style="color: #28A745;"></span>-->
									</p>

									<p class="cardno" style="height: 50%;" class='mui-ellipsis'>卡号:12121212312312</p>
								</div>

							</a>
						</li>

					</ul>
				</div>

			</div>

			<script src="../../js/mui.min.js"></script>
			<script src="../../js/h.min.js"></script>
			<script>
				mui.init({
					swipeBack: true, //启用右滑关闭功能
					pullRefresh: {
						container: '#pullrefresh',
						up: {
							auto: true, //自动上拉刷新一次
							contentrefresh: '正在加载...',
							callback: pullupRefresh
						}
					}
				});

				mui.plusReady(function() {
					console.log("plus api 加载完毕!");

					//initdata(1); //初始化 页面数据,第一页

					//addlistener(); //添加监听事件

				});

				var page = 1;

				function pullupRefresh() {
					console.log("上拉刷新开始...当前页" + page);
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(); //参数为true代表没有更多数据了。
					initdata(page++,25);
					//					initdata(2);

					console.log("上拉刷新结束...");

				}

				mui.ready(function() {
					console.log("DOM构建 完毕!");
					h(".vipdata-yes").hide(); //隐藏未处理模版
					h(".vipdata-no").hide(); //隐藏已处理模版

				});

				function initdata(page,size) {

					//未处理
					console.log("未处理...........:");
					getdata(page, size, 0, 0, function(data) {
						//服务器返回响应，根据响应结果，分析是否登录成功；
						if(data.status != "1") {
							console.log("网络请求页面数据---失败!");
							mui.alert(data.info);
							return;
						}
						if(data.status == "1") {
							console.log("网络请求页面数据---成功!");
							console.log("未处理数据大小:" + data.listhrvip.length);
							if(data.listhrvip.length == 0) {
								console.log("没有更多数据了!");
								mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
								return;
							}
							//添加未处理会员列表
							adddatano(data);
						}
					});

					//已处理
					console.log("已处理...........");
					getdata(page, size, 0, 1, function(data) {
						//服务器返回响应，根据响应结果，分析是否登录成功；
						if(data.status != "1") {
							console.log("网络请求页面数据---失败!");
							mui.alert(data.info);
							return;
						}
						if(data.status == "1") {
							console.log("网络请求页面数据---成功!");
							console.log("已处理数据大小:" + data.listhrvip.length);
							if(data.listhrvip.length == 0) {
								console.log("没有更多数据了!");
								mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
								return;
							}
							//添加已处理会员列表
							adddatayes(data);
						}
					});

				}

				function adddatayes(data) {

					var vipdatas = new Array();
					vipdatas = data.listhrvip;
					for(var i = 0; i < vipdatas.length; i++) {
						var yesli = h(".vipdata-yes").clone();
						yesli.show();
						yesli.find(".vip-name").html(vipdatas[i].vipname); //会员名字
						yesli.find(".integral").html("积分:" + vipdatas[i].integral); //会员积分
						yesli.find(".cardno").html("卡号:" + vipdatas[i].cardno); //会员卡号
						yesli.appendTo(".deal-yes");
					}

				}

				function adddatano(data) {
					var vipdatas = new Array();
					vipdatas = data.listhrvip;
					for(var i = 0; i < vipdatas.length; i++) {
						var noli = h(".vipdata-no").clone();
						noli.show();
						noli.find(".vipname").html(vipdatas[i].vipname); //会员名称
						noli.find(".integral").html("积分:" + vipdatas[i].integral); //会员积分
						noli.find(".cardno").html("卡号:" + vipdatas[i].cardno); //会员卡号
						noli.appendTo(".deal-no");
					}

				}

				function getdata(page, size, type, status, callback) {
					console.log("网络请求 页面数据---开始");
					mui.post('http://61.153.61.50:18024/req/HandReport.ashx', {
						act: "usertasklist",
						username: plus.storage.getItem('TOKEN_USER'),
						key: plus.storage.getItem('TOKEN_LOGIN'),
						page: page,
						size: size,
						searchkey: "",
						type: type,
						status: status

					}, callback, 'json');

				}

				//点击头像跳转到详情页
				function imgclick() {
					mui.openWindow({
						url: "../customs.html"
					});
				}

				function call() {
					alert("打电话");
				}
			</script>

	</body>

</html>
```