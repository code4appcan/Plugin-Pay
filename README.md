#AppCan应用开发之插件实践篇-支付插件
本节教程将介绍如何利用插件完成一个支付的页面。

页面效果图

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%871.png)

1、首先编写UI界面布局。新建一个appcan页面，命名为waitpay.html，同时IDE会自动根据waitpay.html页面生成一个浮动窗口waitpay_content.html页面。
waitpay.html主窗口头部header部分布局代码：

	    <div id="page_0" class="up ub ub-ver bc-bg" tabindex="0">
    <!--header开始-->
    <div id="header" class="uh ub bc-head-m">
        <div class="nav-btn blue" id="nav-left">
            <div class="fa fa-angle-left fa-2x"></div>
        </div>
        <h1 class="ut  bc-text ub-f1 ulev-3 ut-s tx-c" tabindex="0">待付款</h1>
        <div class="nav-btn blue" id="nav-right"></div>
    </div>
    <!--header结束--><!--content开始-->
    <div id="content" class="ub-f1 tx-l " style="background-color: #F2F2F2">
    </div>        
    <!--content结束-->
	</div>

效果如下：

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%878.png)

2、其次在waitpay.html页面js脚本区内编写javaScript代码，用于实现获取微信支付结果的回调函数。

	   uexWeiXin.cbStartPay = function(data) {
    if(JSON.parse(data).errCode==0){
        uexWindow.evaluatePopoverScript("myorder", "content", "show()");
        uexWindow.evaluatePopoverScript("paylist", "content", "show()");
        appcan.window.close(-1);
    }else if(JSON.parse(data).errCode==-2){
        uexWindow.toast("0", "5","取消支付", "2000");
    }else{
        uexWindow.toast("0", "5","支付失败", "2000");
	}

效果如下：

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%872.png)

然后接着在js脚本区域编写javaScript代码，用于实现当点击页面返回按钮时返回上一级页面并清除当前窗口的订单信息。代码如下：

	    appcan.button(".nav-btn", "btn-act", function() {
    		appcan.locStorage.remove("country");
    		appcan.locStorage.remove("orderId");
    		appcan.locStorage.remove('goodsArr');
    		appcan.locStorage.remove('length');
    		appcan.window.close(-1);
	})

效果如下：

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%873.png)

3、接下来在waitpay_content.html浮动窗口编写UI布局代码：

	    <div class="ub ub-f1 ub-ver" style="background-color: #FFFFFF;line-height: 3em;margin-bottom: .5em;">
	<div class="ub" style="height:3em;border-bottom: 1px solid #DBDBDB;padding:0 1em;color: #7A7A7A">
	订单号：<span id="orderNo"></span>
	</div>
	<div class="ub" style="height:3em;padding:0 1em;color: #FF3B77">
	等待买家付款
	</div>
	</div>
	<div class="ub ub-f1" style="padding: 0.8em;background-color: #FFFFFF;margin-bottom: .5em;">
	<div class="ub ub-ver ub-f1" >
	<div class="" style="color:#3D3D3D;line-height: 2em;margin:0 1em;">
    收货人：<span id="contact"></span>
    <div style="float: right" id="tel"></div>
	</div>
	<div class="ub ulev-1" style="color:#808080;line-height: 1.4em;margin-top: .5em;padding: 0 1em;">
    <img class="" src="../image/Pin-Assistor.png" style="width:1.2em"/>
    <div style="width:85%;margin: 0 1em;">
        收货地址：<span id="address"></span>
    </div>
	</div>
	</div>
	</div>
	<div class="ub ub-ver" style="padding: 0.8em;background-color: #FFFFFF;margin-bottom: .5em;">
	<ul id="List">

	</ul>
	</div>
	<div class="ub ub-f1" style="padding: 0.8em;background-color: #FFFFFF;margin-bottom: 1px;">
	<div class="ub ub-f1" style="line-height: 2em;">
	<div class="" style="color:#3D3D3D;margin: 0 .5em;">
	    配送
	</div>
	<img class="" src="../image/Question2.png" style="width:1.5em;margin-top: .2em;" id="peisong"/>
	<div style="color:#808080;margin-left: 30%;">
	    卖家回国后发货，到付
	</div>
	</div>
	</div>
	<div class="ub ub-f1 isover" style="padding: 0.8em;background-color: #FFFFFF;margin-bottom: 1px;">
	<div class="ub ub-f1" style="line-height: 2em;" id="couponlist">
	<div class="ub ub-f1" style="color:#3D3D3D;margin: 0 .5em;">
	    优惠券
	</div>
	<div style="color:#808080;margin-right: 1em;" id="coupon">
	    满99元可以使用10元优惠券
	</div>
	<div class="fa fa-angle-right fa-2x" style="font-size: 1.5em;margin-right: 0.5em;color:#808080"></div>

	</div>
	</div>
	<div class="ub ub-f1 isover" style="padding: 0.8em;background-color: #FFFFFF;margin-bottom:1px;">
	<div class="ub ub-f1" style="line-height: 2em;">
	<div class="ub ub-f1" style="color:#3D3D3D;margin: 0 .5em;">
	    积分
	</div>
	<div style="color:#808080;margin-right: 1em;" id="points"></div>
	<div class="ub ub-ac ub-pc" style="margin-right: .5em;">
	    <div id="choose1" class="ub ub-ac false" style="width: 2em;height: 1.2em;border-radius: 1em;">
	        <div class="cbgb" style="width: 1.1em;height: 1.2em;border-radius: 1em;"></div>
	    </div>
	</div>
	</div>
	</div>
	<div class="ub ub-f1 isover" style="padding: 0.8em;background-color: #FFFFFF;margin-bottom: 1px;">
	<div class="ub ub-f1" style="line-height: 2em;">
	<div class="ub ub-f1" style="color:#3D3D3D;margin: 0 .5em;">
	    余额
	</div>
	<div style="color:#808080;margin-right: 1em;" id="balance"></div>
	<div class="ub ub-ac ub-pc" style="margin-right: .5em;">
	    <div id="choose2" class="ub ub-ac false" style="width: 2em;height: 1.2em;border-radius: 1em;">
	        <div class="cbgb" style="width: 1.1em;height: 1.2em;border-radius: 1em;"></div>
	    </div>
	</div>
	</div>
	</div>
	<div class="ub ub-f1 ub-pc ub-ac ub-ver isover" id="pricepart" style="margin:1em;">
	<div class="ub ub-ac ub-pc" style="width: 15em" id="test">
	<div id="mustpay">
	    应付
	</div>
	<div style="color:#FF0D57;" id="totalPrice">
	    ￥0
	</div>
	&nbsp;&nbsp;&nbsp;&nbsp;
	<div style="font-size: .8em;width:9em">
	    已优惠<span style="color:#FF0D57;" id="cheap">￥0</span>
	</div>
	</div>
	</div>
	<div class="ub ub-f1 ub-ver isover" id="paymode" style="background: #FFFFFF;">
	<div class="ub ub-ac ub-pc" style="width: 100%;line-height: 3em;color: #999999">
	支付方式
	</div>
	<div class="ub ub-f1" style="padding: .5em;">
	<ul style="width:100%;padding-bottom: .5em;">
    <li style='width:31%;display: inline-block;margin-left: 1.5%;text-align: center'  onclick="Alipay();">
        <img src="../image/alipay.png" style="height:2.2em"/>
        <div class="ub ub-ac ub-pc" style="width:100%;line-height: 2.4em;">
            支付宝
        </div>
    </li>
    <li style='width:31%;display: inline-block;margin-left: 1.5%;text-align: center' onclick="weixinpay();">
        <img src="../image/weixin.png" style="height:2.8em"/>
        <div class="ub ub-ac ub-pc" style="width:100%;line-height: 2.4em;">
            微信支付
        </div>
    </li>
	   </ul>
	</div>
	</div>
	<div class="ub ub-ac ub-pc uhide" id="cancleorder" style="height: 4em;padding:.5em;background-color: #F2F2F2">
	<div class="ub ub-ac ub-pc ulev-3" id="cancle" style="width:9em;height: 2.2em;border-radius:.2em;color:#00C1F9;background-color:#FFFFFF;border: 1px solid #00C1F9;">
	取消定单
	</div>
	</div>

效果如下：

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%874.png)

4、随后在waitpay_content.html页面js脚本区内编写javaScript代码，用于实现当前页面的支付宝支付功能和当前页面的微信支付功能，以及当前页面的数据获取功能。

(1) 关于支付宝支付功能的使用请参考appcan官网uexAliPay插件的相关说明。
http://newdocx.appcan.cn/newdocx/docx?type=1385_975

(2)关于微信支付功能的使用请参考appcan官网uexWeiXin插件的相关说明。
http://newdocx.appcan.cn/newdocx/docx?type=1020_975

(3) 首先定义从支付宝官网申请得到的签约商户信息，其次是定义当前页面的币种、汇率、积分以及买家账户余额的信息。具体代码如下：

	    var minusMy = appcan.locStorage.getVal("minusMy");
		var partner = "208845648165561";
		var seller = "48652321@qq.com";
		var rsaPrivate= "MIICdwIBADANBgkn4E3TszcjB+Kf7CGVQ/nsvyywjA+i+0vmaftUzoOdIcWnI8UEr9I=";
		var rsaPublic = "MIGfMA0GCSqGSIb3DQEBAQUAVsW8Ol75p6/B5KsiNG9zpgmLCUYuLkxpLQIDAQAB";
		var notifyUrl;
		var orderId = appcan.locStorage.getVal("orderId");
		var userId = appcan.locStorage.getVal("userId");
		console.log(orderId);
		var country = appcan.locStorage.getVal("country");
		var countryf = areaList[country].name;
		var back = appcan.locStorage.getVal("back");
		var fetchgoSeller;
		//卖家id;
		var originAmount = 0;
		//订单初始金额，外币
		var priceCode;
		//币种
		var payRate;
		//汇率
		var userCoupon;
		//var points=appcan.locStorage.getVal("integar");//获取到的用户积分
		var points;
		//积分
		var rates = Number(0.01);
		//积分兑换率
		var totalPrice = 0;
		var cheap;
		//积分可以兑换的金额
		var couponPrice = 0;
		var num;
		var last = 0;
		var balance;
		//余额
		var lastCheap = 0;
		var showBalance = 0;
		//最后用户使用的余额

(4)随后在页面预加载函数内编写查询当用登录用户积分和余额的代码，支付宝支付状态的监听函数以及微信支付状态和生成预支付订单的回调函数，具体代码如下：

	    appcan.ready(function() {
		if (minusMy <= 0) {
		$(".isover").remove();
		$("#cancleorder").removeClass("uhide");
		} else {
		/*查询用户的积分和余额*/
		appcan.request.ajax({
    url : api + '/api/user/me/' + userId,
    type : 'get',
    dataType : 'json',
    success : function(data) {
        balance = Math.floor(data.data.user.money);
        //查询到用户的余额
        $("#balance").html(balance + "元");
        point = Number(data.data.user.score);
        points = point - point % 100;
        //查询到用户的积分
        cheap = points * rates;
        /*单个订单只允许使用1000积分*/
        if (points < 1000) {
            $("#points").html(points + "积分抵扣" + cheap + "元");
        } else {
            points = 1000;
            cheap = 10;
            $("#points").html(points + "积分抵扣" + cheap + "元");
        }
    },
    error : function(errMessage) {
    }
	});
	}
	show();
	uexAliPay.onStatus = function(status, des) {
	if (status == 0) {
    uexWindow.evaluatePopoverScript("myorder", "content", "show()");
    uexWindow.evaluatePopoverScript("paylist", "content", "show()");
    uexWindow.evaluateScript('waitpay', '0', 'appcan.window.close(-1)');
	} else if (status == 4) {
    uexWindow.toast("0", "5", "取消支付", "2000");
	} else {
    uexWindow.toast("0", "5", "支付失败", "2000");
	}
	}
	uexWeiXin.cbStartPay = function(data) {
	if (JSON.parse(data).errCode == 0) {
    uexWindow.evaluatePopoverScript("myorder", "content", "show()");
    uexWindow.evaluatePopoverScript("paylist", "content", "show()");
    appcan.window.close(-1);
	} else if (JSON.parse(data).errCode == -2) {
    uexWindow.toast("0", "5", "取消支付", "2000");
	} else {
    uexWindow.toast("0", "5", "支付失败", "2000");
	}
	}
	//微信授权回调
	uexWeiXin.cbRegisterApp = function(opCode, dataType, data) {
	if (data == 0) {
    uexWeiXin.isSupportPay();
	}
	};
	uexWeiXin.cbIsSupportPay = function(opCode, dataType, data) {
	if (data == 0) {
    getPrepayId();
	}
	}
	uexWeiXin.cbGetPrepayId = function(data) {
	var pay = JSON.parse(data);
	var date = new Date();
	var timestamp = date.getTime().toString().substring(0, 10);
	if (JSON.parse(data).result_code == "SUCCESS") {
    var json = {
        appid : "wxf14d58cec986585b", //(必选)微信分配的AppID
        partnerid : "1234567890", //(必选)微信支付分配的商户号
        prepayid : "wx201506031538433160984eee0861221810", //(必选)微信返回的支付交易会话ID
        "package" : "Sign=WXPay", //(必选)暂填写固定值Sign=WXPay
        noncestr :"weradfdgdvccfexs", //(必选)随机字符串
        timestamp : "1412000000", //(必选)时间戳
        sign :"8FC5935C38628F44B924C838D760E33E"//(必选)签名}
    }
	var strrrr="appid=wxf14d58cec986585b&noncestr="+weradfdgdvccfexs+ "&package=Sign=WXPay&partnerid=1234567890&prepayid="+wx201506031538433160984eee0861221810+ "&timestamp=" + 1412000000 + "&key=0e857460d1b309130b9b1d2530ac094d";
    json.sign = hex_md5(strrrr).toUpperCase();
    uexWeiXin.startPay(JSON.stringify(json));
	}
	}
	});

(5)编写微信授权函数和微信生成预支付订单函数，当点击微信支付Logo时打开支付界面。具体代码如下：

	    function weixinpay() {
		uexWeiXin.registerApp('wxd930ea5d5a258f4f');
		}
		function getPrepayId() {
		var money;
		var userCouponId;
		if ($("#choose1").hasClass("true")) {
		} else {
    points = 0;
	}
	if ($("#choose2").hasClass("true")) {
    money = showBalance;
	} else {
    money = 0;
	}
	if ($('#coupon').html() == '满99元可以使用10元优惠券') {
    userCouponId = 'X';
	} else {
    userCouponId = userCoupon;
	}
	var date = new Date();
	var timestamp = date.getTime().toString().substring(0, 10);
	originAmount = money + last + couponPrice;
	alert("money:" + money + "last:" + last + "couponPrice:" + couponPrice);
	alert(originAmount);
	var param1 = {
    appid : "wxd930ea5d5a258f4f",
    mch_id : "1234567890",
    nonce_str : "weradfdgdvccfexs1",
    body : "海外购",
    detail : "detail",
    attach : orderId + "_" + originAmount + "_" + payRate + "_" + priceCode + "_" + money + "_" + points + "_" + userCouponId + "_" + "1",
    out_trade_no : timestamp,
    fee_type : "CNY",
    total_fee : 1, //last*100
    spbill_create_ip : "127.0.0.1",
    notify_url : api + "/api/trans/wxpay",
    trade_type : "APP",
    sign : "8FC5935C38628F44B924C838D760E33E"
		};
		Var stringSign="appid=wxd930ea5d5a258f4f&attach="+param1.attach+"&body="+param1.body+ "&detail="+param1.detail+"&fee_type=CNY&mch_id=1234567890&nonce_str=weradfdgdvccfexs1&notify_url="+param1.notify_url+"&out_trade_no="+param1.out_trade_no+&spbill_create_ip=127.0.0.1&total_fee=" + param1.total_fee + "&trade_type=APP&key=0e857460d1b309130b9b1d2530ac094d";
		var md = hex_md5(stringSign).toUpperCase();
		alert(param1.attach);
		param1.sign = md;
		var data1 = JSON.stringify(param1);
		uexWeiXin.getPrepayId(data1);
		}

效果如下：

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%875.png)

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%876.png)

(6)编写支付宝设置商户信息函数以及支付功能函数，当点击支付宝Logo时打开支付界面。具体代码如下：

	   function setInfo() {
		var money = 0;
	if ($("#choose2").hasClass("true")) {
    money = Number(showBalance);
	}
	if ($('#coupon').html() == '满99元可以使用10元优惠券') {
    var userCouponId = '';
	} else {
    var userCouponId = userCoupon;
	}
		originAmount = money + last + couponPrice;
		alert("money:" + money + "last:" + last + "couponPrice:" + couponPrice);
		alert(originAmount);
		notifyUrl = api + "/api/trans/alipay?orderId=" + orderId + "&originAmount=" + originAmount + "&priceCode=" + priceCode + "&rate=" + payRate + "&score=" + points + "&money=" + money + "&userCouponId=" + userCouponId + "&action=1" + "&total_fee=" + last;
		alert(notifyUrl);
		uexAliPay.setPayInfo(partner, seller, rsaPrivate, rsaPublic, notifyUrl);
		}
		function Alipay() {
		setInfo();
		var subject = "海外购" + num;
		var body = "订单内容";
		var fee = 0.01;
		uexAliPay.pay(num, subject, body, fee);
		}

效果如下：

![](https://raw.githubusercontent.com/code4appcan/Plugin-Pay/master/picture/%E5%9B%BE%E7%89%877.png)