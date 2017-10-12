

## 模版

```html

	<body>
		<header class="mui-bar mui-bar-nav">
			<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<!--<a class=" mui-icon mui-icon-email mui-pull-right" onclick="sendMessage()"></a>-->

			<h1 id="title" class="mui-title">会员回访</h1>

		</header>
		

			<div id="pullrefresh" class="mui-content mui-scroll-wrapper">
				<div class="mui-scroll">
					<div class="mui-content">
						<!--你的上拉刷新内容-->
					</div>
				</div>
			<div>
```


## 示例:


```html
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
			
			.call {
				margin-right: 10px;
			}
		</style>
	</head>

	<body>
		<header class="mui-bar mui-bar-nav">
			<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<!--<a class=" mui-icon mui-icon-email mui-pull-right" onclick="sendMessage()"></a>-->

			<h1 id="title" class="mui-title">会员回访</h1>

		</header>
		

			<div id="pullrefresh" class="mui-content mui-scroll-wrapper">
				<div class="mui-scroll">
					<div class="mui-content">
					<div style="padding: 10px 10px; ">
						<div id="segmentedControl" class="mui-segmented-control">
							<a class="no mui-control-item mui-active" href="#item1">未</a>
							<a class="yes mui-control-item" href="#item2">已</a>
						</div>
					</div>
					<!--未处理-->
					<div id="item1" class="mui-control-content mui-active">
						<ul class="mui-table-view deal-no">
							<li class="mui-table-view-cell mui-media vipdata-no">

								<div id="" class="fdiv" style="width: 15%;">
									<img class="photo mui-media-object mui-pull-left" src="../../images/custom.jpg">
									<span class="vipid" style="display:none"></span>
								</div>

								<div class="fdiv" style="width: 65%;">
									<p style="height: 50%;">
										<span class="vipname" style="color: #000000;font-weight:bold;">小明 </span>
										<span class="integral" style="margin-right: 10px;">积分:100</span>
										<span class=" mui-icon mui-icon mui-icon-phone call" style="color: #28A745;"></span>
										<span class=" mui-icon mui-icon-email msg" style="color: #28A745;"></span>
									</p>

									<p class="cardtype" style="height: 50%;" class='mui-ellipsis'>卡号:12121212312312</p>
								</div>
								<div class="fdiv" style="width: 20%;">
									<button type="button" class="deal mui-btn mui-btn-outlined">处理</button>
								</div>

							</li>

						</ul>
					</div>
					<!--已处理-->
					<div id="item2" class="mui-control-content">
						<ul id="ul2" class="mui-table-view deal-yes">
							<li class="mui-table-view-cell mui-media vipdata-yes">

								<div id="" class="fdiv" style="width: 15%;">
									<img class="photo mui-media-object mui-pull-left" src="../../images/custom.jpg">
									<span class="vipid" style="display:none"></span>
								</div>

								<div class="fdiv" style="width: 65%;">
									<p style="height: 50%;"><span class="vipname" style="color: #000000;font-weight:bold;">小明 </span>
										<span class="integral" style="margin-right: 10px;">积分:100</span>
									</p>

									<p class="cardtype" style="height: 50%;" class='mui-ellipsis'>卡号:12121212312312</p>
								</div>

							</li>

						</ul>
					</div>
				</div>
			</div>

		</div>

		<script src="../../js/mui.min.js"></script>
		<script src="../../js/h.min.js"></script>
		<script>
			var globalphone;

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

			});

			var page = 1;
			var hasMore_no = false;
			var hasMore_yes = false;

			function pullupRefresh() {
				var wd = plus.nativeUI.showWaiting();
				console.log("上拉刷新开始...当前页" + page);
				if(hasMore_no && hasMore_yes) {
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(true); //参数为true代表没有更多数据了。
				} else {
					mui('#pullrefresh').pullRefresh().endPullupToRefresh(false); //参数为true代表没有更多数据了。
				}

				var data = plus.webview.currentWebview();
				var type = data.type;
				var title = data.title;
				h("#title").html(title);

				if(type <= 2) {
					//已处理.未处理
					h(".no").html("未处理");
					h(".yes").html("已处理");
					h(".deal").html("处理");

				}
				if(type > 2) {
					//已唤醒,未唤醒
					h(".no").html("未唤醒");
					h(".yes").html("已唤醒");
					h(".deal").html("唤醒");

				}

				initdata(page++, 25, data.type);
				wd.close();
			}

			mui.ready(function() {
				console.log("DOM构建 完毕!");
				h(".vipdata-yes").hide(); //隐藏未处理模版
				h(".vipdata-no").hide(); //隐藏已处理模版

				Date.prototype.Format = function(fmt) {
					var o = {
						"M+": this.getMonth() + 1, //月份 
						"d+": this.getDate(), //日 
						"h+": this.getHours(), //小时 
						"m+": this.getMinutes(), //分 
						"s+": this.getSeconds(), //秒 
						"q+": Math.floor((this.getMonth() + 3) / 3), //季度 
						"S": this.getMilliseconds() //毫秒 
					};
					if(/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
					for(var k in o)
						if(new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
					return fmt;
				}
			});

			//------------------------------------android监听通话状态----------------------------------------------------
			/**
			 * 自定义一个全局函数,赋值函数给全局变量  Native
			 */
			var Native = (function($) {
				var native = {};
				var receiver, main, context, TelephonyManager;
				//添加监听事件的函数,此函数可接收一个回调函数
				native.listenTelPhone = function(callback) {

						$.plusReady(function() {
							context = plus.android.importClass('android.content.Context'); //上下文
							TelephonyManager = plus.android.importClass('android.telephony.TelephonyManager'); //通话管理
							main = plus.android.runtimeMainActivity(); //获取activity
							receiver = plus.android.implements('io.dcloud.feature.internal.reflect.BroadcastReceiver', {
								onReceive: doReceive //实现onReceiver回调函数
							});
							var IntentFilter = plus.android.importClass('android.content.IntentFilter');
							var Intent = plus.android.importClass('android.content.Intent');
							var filter = new IntentFilter();

							filter.addAction(TelephonyManager.ACTION_PHONE_STATE_CHANGED); //监听电话状态
							main.registerReceiver(receiver, filter); //注册监听
						});

						function doReceive(context, intent) {
							plus.android.importClass(intent);
							var phoneNumber = intent.getStringExtra(TelephonyManager.EXTRA_INCOMING_NUMBER),
								telephony = context.getSystemService(context.TELEPHONY_SERVICE),
								state = telephony.getCallState();
							switch(state) {
								case TelephonyManager.CALL_STATE_RINGING:
									callback && callback(1, phoneNumber);
									window.console.log("[Broadcast]等待接电话=" + phoneNumber);
									break;
								case TelephonyManager.CALL_STATE_IDLE:
									callback && callback(0, phoneNumber);
									window.console.log("[Broadcast]电话挂断=" + phoneNumber);
									break;
								case TelephonyManager.CALL_STATE_OFFHOOK:
									callback && callback(2, phoneNumber);
									window.console.log("[Broadcast]通话中=" + phoneNumber);
									break;
							}
						}
					},
					//取消监听事件的函数
					native.removeListenTelPhone = function() {
						if(receiver) {
							main = plus.android.runtimeMainActivity(); //获取activity
							main.unregisterReceiver(receiver); //删除监听
							console.log("取消android通话监听事件");
							receiver = null;
						}
					}
				return native;
			}(mui));

			//androidRecord();

			function postData(url, data, callback, waitingDialog) {
				mui.ajax(url, {
					data: data,
					dataType: 'json',
					crossDomain: true,
					type: 'post',
					timeout: 60000,
					success: callback,
					error: function(xhr, type, errorThrown) {
						waitingDialog.close();
						mui.alert("<网络连接失败，请重新尝试一下>", "错误", "OK", null);
					}
				});
			};

			//page当前页,size页大小,type类型(参考接口)
			function initdata(page, size, type) {

				//未处理
				console.log("未处理...........:");
				getdata(page, size, type, 0, function(data) {

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
							hasMore_no = true;
							//								mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
							return;
						}
						//添加未处理会员列表
						adddatano(data);
					}
				});

				//已处理
				console.log("已处理...........");
				getdata(page, size, type, 1, function(data) {
					//服务器返回响应，根据响应结果，分析是否登录成功；
					if(data.status != "1") {
						console.log("网络请求页面数据---失败!");
						//							mui.alert(data.info);
						return;
					}
					if(data.status == "1") {
						console.log("网络请求页面数据---成功!");
						console.log("已处理数据大小:" + data.listhrvip.length);
						if(data.listhrvip.length == 0) {
							console.log("没有更多数据了!");
							hasMore_yes = true;
							//								mui('#pullrefresh').pullRefresh().endPullupToRefresh(true);
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
					yesli.find(".vipid").html(vipdatas[i].vipid); //会员id
					yesli.find(".vipname").html(vipdatas[i].vipname); //会员名字
					yesli.find(".integral").html("积分:" + vipdatas[i].integral); //会员积分
					yesli.find(".cardtype").html("卡类型:" + vipdatas[i].typename); //会员卡号

					yesli.appendTo(".deal-yes");
				}

			}

			function adddatano(data) {
				var vipdatas = new Array();
				vipdatas = data.listhrvip;
				for(var i = 0; i < vipdatas.length; i++) {
					var noli = h(".vipdata-no").clone();
					noli.show();
					noli.find(".vipid").html(vipdatas[i].vipid); //会员id
					noli.find(".vipname").html(vipdatas[i].vipname); //会员名称
					//						console.log("会员名:---------------" + vipdatas[i].vipname);
					noli.find(".integral").html("积分:" + vipdatas[i].integral); //会员积分
					noli.find(".cardtype").html("卡类型:" + vipdatas[i].typename); //会员卡号

					noli.appendTo(".deal-no");
				}

			}

			function getdata(page, size, type, status, callback) {
				console.log("网络请求 页面数据---开始");
				mui.post(plus.storage.getItem("url"), {
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

			//点击头像进入详情页面
			mui("#pullrefresh").on('tap', 'img', function() {
				console.log(h(this).next().html());
				var wd = plus.nativeUI.showWaiting();
				var vipid = h(this).next().html();
				var data = {
					"act": "getvipdetail ",
					"username": plus.storage.getItem('TOKEN_USER'),
					"key": plus.storage.getItem('TOKEN_LOGIN'),
					"vipid": vipid
				};
				mui.post(plus.storage.getItem("url"), data, function(data) {
					wd.close();
					if(data.status != "1") {
						mui.alert(data.info);
						return;
					}
					mui.openWindow({
						url: '../customs.html',
						extras: data

					});
				}, 'json');

			});
			//群发短信事件
			function sendMessage() {
				//					alert("发短信");
				mui.openWindow({
					url: 'message.html',

				});
			}

			//打电话
			mui("#item1").on('tap', '.mui-icon-phone', function() {
				androidRecord();
				call(this);
			});

			function call(t) {
				console.log(h(t).parent().parent().parent().find(".vipid").html()); //会员id

				var wd = plus.nativeUI.showWaiting();
				var vipid = h(t).parent().parent().parent().find(".vipid").html();
				var data = {
					"act": "getvipdetail",
					"username": plus.storage.getItem('TOKEN_USER'),
					"key": plus.storage.getItem('TOKEN_LOGIN'),
					"vipid": vipid
				};
				mui.post(plus.storage.getItem("url"), data, function(data) {
					wd.close();
					if(data.status != "1") {
						mui.alert(data.info);
						return;
					}
					console.log("电话号码:" + data.mobil);
					var btnArray = ['拨打', '取消'];

					var phone = data.mobil;
					globalphone = phone;
					mui.confirm('是否拨打' + phone + '?', '提示', btnArray, function(e) {
						//拨打
						if(e.index == 0) {
							if(mui.os.ios) {
								//添加ios通话记录监听事件
								createCall(phone);
							}
							//	android通话

							plus.device.dial(phone, false);

						} else {
							//取消
							if(mui.os.android) {
								Native.removeListenTelPhone(); //取消通话监听事件
								console.log("取消监听事件...");
							}

						}
					});

				}, 'json');
			}
			/**
			 *记录android通话 
			 */
			function androidRecord() {

				var sTime, endTime;
				mui.os.android && Native.listenTelPhone(function(code, number) {
					if(code === 2) { //接通
						sTime = new Date();
					}
					if(code === 0) { //挂断
						endTime = new Date();
						var secn = parseInt((endTime.getTime() - sTime.getTime()) / 1000);
						//							alert(secn)
						console.log("通话时间间隔secn:" + secn);
						if(secn > 3) {
							var wd = plus.nativeUI.showWaiting();
							var vipdata = plus.webview.currentWebview();
							var data = {
								"act": "addstorecall",
								"username": plus.storage.getItem('TOKEN_USER'),
								"key": plus.storage.getItem('TOKEN_LOGIN'),
								"addjson": JSON.stringify([{
									"phone": globalphone,
									"phonetimes": secn,
									"phoneaddtime": sTime.Format("yyyy-MM-dd hh:mm:ss"),
									"phonetype": "呼出"
								}])
							};
							console.log(JSON.stringify(data));
							console.log("电话号码" + globalphone);

							postData(plus.storage.getItem("url"),
								data,
								function(data) {
									wd.close()
									if(data.status != "1") {
										console.log("网络请求失败!" + data.info);
										mui.alert(data.info);
										return;
									}

									console.log("成功添加通话记录！");
									Native.removeListenTelPhone(); //取消通话监听事件
								},
								wd
							);
						}
					}
				});

			};

			//------------------ios通话监听--------------------------------------------
			/**
			 * ios通话记录监听	
			 * @param {Object} phone 电话号码
			 */
			function createCall(phone) {
				var starttime;
				var endtime;

				mui.plusReady(function() {
					document.addEventListener("pause", function() {
						starttime = new Date();
						console.log(starttime.getTime() / 1000 + " 应用从前台切换到后台");
						mui.toast(" 应用从前台切换到后台");
					}, false);
					document.addEventListener("resume", function() {
						endtime = new Date();
						console.log(endtime.getTime() / 1000 + " 应用从后台切换到前台");

						mui.toast("应用从后台切换到前台");

						addIOSPhoneData(starttime, endtime);

					}, false);

				});

			}
			/**
			 * 添加ios通话记录
			 * @param {Object} starttime 开始监听时间
			 * @param {Object} endtime   结束监听时间
			 */
			function addIOSPhoneData(starttime, endtime) {
				//					document.removeEventListener("pause", true);
				//					document.removeEventListener("resume", true);
				//					console.log("取消事件...");
				// 对Date的扩展，将 Date 转化为指定格式的String
				// 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符， 
				// 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字) 
				// 例子： 
				// (new Date()).Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423 
				// (new Date()).Format("yyyy-M-d h:m:s.S")      ==> 2006-7-2 8:9:4.18 
				Date.prototype.Format = function(fmt) { //author: meizz 
					var o = {
						"M+": this.getMonth() + 1, //月份 
						"d+": this.getDate(), //日 
						"h+": this.getHours(), //小时 
						"m+": this.getMinutes(), //分 
						"s+": this.getSeconds(), //秒 
						"q+": Math.floor((this.getMonth() + 3) / 3), //季度 
						"S": this.getMilliseconds() //毫秒 
					};
					if(/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
					for(var k in o)
						if(new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
					return fmt;
				}
				var end = endtime.getTime() / 1000;
				var start = starttime.getTime() / 1000;
				var secn = end - start;

				console.log("时间差:" + Math.floor(secn));
				var phoneaddtime = starttime.Format("yyyy-MM-dd hh:mm:ss");
				var data = {
					"act": "addstorecall",
					"username": plus.storage.getItem('TOKEN_USER'),
					"key": plus.storage.getItem('TOKEN_LOGIN'),
					"addjson": JSON.stringify([{
						"phone": globalphone,
						"phonetimes": Math.floor(secn),
						"phoneaddtime": phoneaddtime,
						"phonetype": "呼出"
					}])
				};
				mui.post(plus.storage.getItem("url"), data, function(data) {

					if(data.status != "1") {
						mui.alert(data.info);

						return;
					}
					mui.toast("成功添加本次通话记录!");

				}, 'json');

			}

			//发短信事件
			mui("#item1").on('tap', '.mui-icon-email', function() {
				send(this);
			});
			/**
			 * 发短信事件
			 * @param {Object} t
			 */
			function send(t) {

				console.log("vipid=" + h(t).parent().parent().parent().find(".vipid").html()); //会员id

				var wd = plus.nativeUI.showWaiting();
				var vipid = h(t).parent().parent().parent().find(".vipid").html();
				var data = {
					"act": "getvipdetail ",
					"username": plus.storage.getItem('TOKEN_USER'),
					"key": plus.storage.getItem('TOKEN_LOGIN'),
					"vipid": vipid
				};
				mui.post(plus.storage.getItem("url"), data, function(data) {
					wd.close();
					if(data.status != "1") {
						mui.alert(data.info);
						return;
					}
					console.log("电话号码:" + data.mobil);

					var vipid = h(t).parent().parent().parent().find(".vipid").html();
					var phoneNumber = data.mobil;
					//						console.log("通过id获取会员详情:"+JSON.stringify(data));
					var vipname = data.vipname;

					mui.openWindow({
						url: 'one-message.html',
						extras: {
							"vipid": vipid,
							"phoneNumber": phoneNumber,
							"vipname": vipname
						}

					});

				}, 'json');
			}

			//已处理事件
			mui("#item1").on('tap', '.deal', function() {
				deal(this);
			});

			/**
			 * 已处理事件	
			 * @param {Object} t
			 */
			function deal(t) {
				console.log("要处理的vipid:" + h(t).parent().parent().find(".vipid").html());
				console.log("要处理的vipname:" + h(t).parent().parent().find(".vipname").html());

				var wd = plus.nativeUI.showWaiting();
				var vipid = h(t).parent().parent().find(".vipid").html();
				var name = h(t).parent().parent().find(".vipname").html();
				var data = {
					"act": "updateuserstatus ",
					"username": plus.storage.getItem('TOKEN_USER'),
					"key": plus.storage.getItem('TOKEN_LOGIN'),
					"vipid": vipid,
					"name": name
				};
				mui.post(plus.storage.getItem("url"), data, function(data) {
					wd.close();
					if(data.status != "1") {
						mui.alert(data.info);
						return;
					}
					var noli = h(t).parent().parent().clone(); //必须先克隆一个出来才能追加
					noli.find(".call").hide(); //隐藏电话图标
					noli.find(".deal").hide(); //隐藏已处理按钮
					noli.find('.msg').hide(); //隐藏短信图标
					noli.prependTo(".deal-yes"); //追加到已处理列表头部

					h(t).parent().parent().hide(); //如果点击 已处理 按钮,则隐藏

					mui.toast(data.info); //最后弹出 toast 提示
				}, 'json');
			}
		</script>

	</body>

</html>

```



