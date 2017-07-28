# 常用Javascript方法汇总
###### 整理者：hauk0101
## 说明

本文档中主要总结了日常开发中与Javascript相关的一些实现方法、遇到的常见问题及解决方法或思路、以及其他开发技巧等。

## 目录

1. [微信动态生成分享话术](#weixindongtaishengchengfenxianghuashu)
2. [时间戳格式化](#shijianchuohuoqushijian)
3. [jQuery中ajax向服务器发送/请求数据](#jq_ajax_get_post_data)
4. [URL字符串编码/解码](#url_string_format)
5. [jQuery对html元素循环添加事件](#jq_add_events)
6. [手机类型判断](#手机类型判断)
7. [返回字符串长度](#返回字符串长度)
8. [获取url中的参数](#获取url中的参数)
9. [js事件绑定器（兼容低版本IE）](#js事件绑定器)
10. [js移除事件器（兼容低版本IE）](#js移除事件器)
11. [获取当前浏览器JS的版本](#获取JS版本)

<h5 id="weixindongtaishengchengfenxianghuashu">微信动态生成分享话术</h5>

> 关于微信分享，根据程序中产生的不同的结果，实现分享出不同的转发结果。

解决方法：可参考如下示例。
	
	/**
	 * 微信分享接口
	 * @param {Object} title
	 * @param {Object} desc
	 * @param {Object} link
	 * @param {Object} imgUrl
	 */
	function shareConfig(title, desc,shareURL,imgURL) {
		if(!window.wx) return;	
	
		wx.onMenuShareTimeline({
			    title: title, // 分享标题
			    link: shareURL, // 分享链接
			    imgUrl: imgURL, // 分享图标
			    success: function () { 
			        // 用户确认分享后执行的回调函数
			    },
			    cancel: function () { 
			        
			    }
			});
			wx.onMenuShareAppMessage({
			    title:title, // 分享标题
			    desc: desc, // 分享描述
			    link: shareURL, // 分享链接
			    imgUrl: imgURL, // 分享图标
			    type:'link',
			    success: function () { 
			        // 用户确认分享后执行的回调函数
			    },
			    cancel: function () { 
			        // 用户取消分享后执行的回调函数
			    }
			});
			
			//包括但不限于以上两种分享接口
	}
	
	//程序逻辑，并出现需要的分享数据,作为示例
	var title = getTitle();
	var desc = getDesc();
	var shareURL = getShareURL();
	var imgURL = getImgURL();
	//设置分享结果，并不局限于只在初始化微信分享时设置相关的分享内容，可以在任意处设置需要分享的标题、内容、URL、图片等等。
	shareConfig(title,desc,shareURL,imgURL);

_tip:使用此方法设置分享内容时，务必保证已经实现微信分享接口所需要的其他步骤，此处并不介绍如何正常设置微信分享内容的相关方法。_

<h5 id="shijianchuohuoqushijian">时间戳格式化</h5>

>由时间戳获取现实时间字符串方法。

解决方法如下：

	/**
	 * 由时间戳获取现实时间字符串方法，截止到分钟数，秒数可对应添加
	 * @param {Object} time 时间戳
	 * @param {Object} type 需要返回的字符串格式
	 * 						type == 0 返回 x月x日
	 * 						type == 1 返回 x时x分
	 * 						其他值默认返回 x年x月x日x时x分
	 */
	function GetTimeString(time,type){
		var date = new Date(parseInt(time) * 1000);
		var year = date.getFullYear();
		var mon = date.getMonth() + 1;
		var day = date.getDate();
		var hours = date.getHours();
		var min = date.getMinutes();
		//如果对应的值不满10，则在前方添加0
		mon = mon >= 10 ? mon : "0" + mon;	
		day = day >= 10 ? day : "0" + day;
		hours = hours >= 10 ? hours : "0" + hours;
		min = min >= 10?min:"0"+min;
		
		//type == 0，返回月日
		if(type == 0){
			return mon + "-" + day;
		}//type == 1,返回时分
		if(type == 1){
			return hours + ":" + min;
		}//返回年-月-日 时：分
		else{
			return year + "-" + mon + "-" + day + "&nbsp;" + hours +":"+min;
		}
	}
		

<h5 id="jq_ajax_get_post_data">使用jQuery中ajax向服务器发送/请求数据</h5>

解决方法如下：

	/**
	 * 向服务器发送/获取数据
	 */
	function getDataByServer(){
		//服务器相关接口地址
		var url = "";
		//向服务器发送的数据
		var data = {};
		$.ajax({
			url:url,
			type:'GET',	
			data:data,
			dataType:'json',
			success:function(json){			
				//TO DO...
			},
			error:function(json){
				console.log("error:","获取数据失败！");
			}
		});
	}	

* 方法中，type可以是“GET”或者“POST”。
	* 其中“GET”是将发送的数据通过拼接在请求地址后，以达到前端与服务器端数据交互的目的。
	* “POST”的请求发送到服务器的对象，是键值对字符串或对象。
	* 其实际原理是xhr的send方法，而“GET”类型并不存在send方法。
* data为需要向服务器发送的数据。
	_将自动转换为请求字符串格式。GET 请求中将附加在 URL 后。查看 processData 选项说明以禁止此自动转换。必须为 Key/Value 格式。如果为数组，jQuery 将自动为不同值对应同一个名称。如 {foo:["bar1", "bar2"]} 转换为 '&foo=bar1&foo=bar2'。_
* dataType为服务器返回的数据类型。
	* "xml": 返回 XML 文档，可用 jQuery 处理。
	* "html": 返回纯文本 HTML 信息；包含的 script 标签会在插入 dom 时执行。
	* "script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了 "cache" 参数。注意：在远程请求时(不在同一个域下)，所有 POST 请求都将转为 GET 请求。（因为将使用 DOM 的 script标签来加载）
	* "json": 返回 JSON 数据 。
	* "jsonp": JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。
	* "text": 返回纯文本字符串
* success、error分别表示ajax的响应服务器的成功和错误对应事件。

<h5 id="url_string_format">URL字符串编码/解码</h5>

> javascript对URL字符串进行编码/解码。

只有字母和数字[0-9a-zA-Z]、一些特殊符号“$-_.+!*'(),”[不包括双引号]、以及某些保留字，才可以不经过编码直接用于URL。

关于URL编码，在js中有三个相关的函数，分别是escape(),encodeURI(),encodeURIComponent().

* escape()编码方法，对应的解码方法为unescape().
	* eascape方法： 返回一个可在所有计算机上读取的编码String对象。
	* 不会被此方法编码的字符： * @ - _ + . / 
	* 实际上，escape()不能直接用于URL编码，它的真正作用是返回一个字符的Unicode编码值。比如“春节”的返回结果 是%u6625%u8282，也就是说在Unicode字符集中，“春”是第6625个（十六进制）字符，“节”是第8282个（十六进制）字符。
	* 它的具体规则是，除了ASCII字母、数字、标点符号“@ * _ + - . /”以外，对其他所有字符进行编码。在\u0000到\u00ff之间的符号被转成%xx的形式，其余符号被转成%uxxxx的形式。
	* 无论网页的原始编码是什么，一旦被Javascript编码，就都变为unicode字符。也就是说，Javascipt函数的输入和输出，默认都是Unicode字符。
* encodeURI()编码方法，对应的解码方法为decodeURI().
	* encodeURI 方法：返回编码为有效的统一资源标识符 (URI) 的字符串。
	* 不会被此方法编码的字符：! @ # $ & * ( ) = : / ; ? + '
	* encodeURI()是Javascript中真正用来对URL编码的函数。
	* 它着眼于对整个URL进行编码，因此除了常见的符号以外，对其他一些在网址中有特殊含义的符号"; / ? : @ & = + $ , #"共11个 ，也不进行编码。编码后，它输出符号的utf-8形式，并且在每个字节前加上%。
* encodeURIComponent()编码方法，对应的解码方法为decodeURIComponent().
	* 与encodeURI()的区别是，它用于对URL的组成部分进行个别编码，而不用于对整个URL进行编码。
	* 因此，“; / ? : @ & = + $ , #”，这些在encodeURI()中不被编码的符号，在encodeURIComponent()中统统会被编码。至于具体的编码方法，两者是一样。

参考示例如下：

	(function(){
		var url = "http://www.google.com?a='这里是中文'&b=2";	
		//三种编码
		console.log("escape:" + escape(url));
		console.log("encodeURI:" + encodeURI(url));
		console.log("encodeURIComponent:" + encodeURIComponent(url));
	})();
	//输出如下：
	escape:http%3A//www.google.com%3Fa%3D%27%u8FD9%u91CC%u662F%u4E2D%u6587%27%26b%3D2
	encodeURI:http://www.google.com?a='%E8%BF%99%E9%87%8C%E6%98%AF%E4%B8%AD%E6%96%87'&b=2
	encodeURIComponent:http%3A%2F%2Fwww.google.com%3Fa%3D'%E8%BF%99%E9%87%8C%E6%98%AF%E4%B8%AD%E6%96%87'%26b%3D2

_此模块参考如下网站：<br>
[W3School.com-escape()函数](http://www.w3school.com.cn/jsref/jsref_escape.asp)<br>
[W3School.com-encodeURI()函数](http://www.w3school.com.cn/jsref/jsref_encodeuri.asp)<br>
[W3School.com-encodeURIComponent()函数](http://www.w3school.com.cn/jsref/jsref_encodeURIComponent.asp)<br>
[博客园-JS对URL字符串进行编码/解码分析](http://www.cnblogs.com/liuhongfeng/p/5012570.html?utm_source=tuicool&utm_medium=referral)<br>_

<h5 id="jq_add_events">使用jQuery对html元素循环添加事件</h5>

解决方法如下：
	
	<!-- 以下内容可直接在浏览器中运行，在调试窗口console下可查看输出结果 -->
	<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>利用jQuery循环添加点击事件</title>
		</head>
		<body>
			<div class="demo-click">点我试试 1</div>
			<div class="demo-click">点我试试 2</div>
			<div class="demo-click">点我试试 3</div>
			<div class="demo-click">点我试试 4</div>
			<div class="demo-click">点我试试 5</div>
		</body>
		 <script type="text/javascript" src="http://libs.baidu.com/jquery/1.9.1/jquery.min.js"></script>
		 <script type="text/javascript">
		 	//获取类名为demo-click的所有元素
		 	var objects = $(".demo-click");
		 	var target;
		 	for(var i = 0; i < objects.length; i++){
		 		target = objects[i];
		 		if(target){
		 			//注意target和$(target)的区别，其实就是dom原生对象和jQuery对象的区别
		 			$(target).bind("click",{index:i},clickHandler);
		 		}
		 	}
		 	//元素点击事件处理函数
		 	function clickHandler(evt){
		 		//通过evt.data.index获得对应的元素索引
		 		console.log(evt.data.index);
		 	}
		 </script>
	</html>
	
<h5 id="手机类型判断">手机类型判断</h5>

```
var BrowserInfo = {
	userAgent:navigator.userAgent.toLowerCase(),
	isAndroid:Boolean(navigator.userAgent.match(/android/ig)),
	isIphone:Boolean(navigator.userAgent.match(/iphone|ipod/ig)),
	isIpad:Boolean(navigator.userAgent.match(/ipad/ig)),
	isWeiXin:Boolean(navigator.userAgent.match(/MicroMessage/ig))
}

```

<h5 id="返回字符串长度">返回字符串长度,汉字计数为2</h5>

```
function strLength(str){
	var _total = 0;
	for(var i = 0; i < str.length;i++){
		if(str.charCodeAt(i) > 255){
			_total += 2; //大于255时，视为汉字
		}else{
			_total ++;
		}	
	}
	return _total;
}

```

<h5 id="获取url中的参数">获取url中的参数</h5>

```
function GetQueryStringRegExp(name,url){
	var reg = new RegExp("(^|\?|&)" + name + "=([^&]*)(\s|&|$)", "i");
	if(reg.test(url)){
		return decodeURIComponent(RegExp.$2.replace(/+/g, " "));
	}
	return "";
}

```
<h5 id="js事件绑定器">js事件绑定器（兼容低版本IE）</h5>

```
function eventBind(obj, eventType, callBack) {
  if (obj.addEventListener) {
      obj.addEventListener(eventType, callBack, false);
  }
  else if (window.attachEvent) {
      obj.attachEvent('on' + eventType, callBack);
  }
  else {
      obj['on' + eventType] = callBack;
  }
};
eventBind(document, 'click', bodyClick);
```

<h5 id="js移除事件器">js移除事件器（兼容低版本IE）</h5>

```
this.moveBind = function (objId, eventType, callBack) {
var obj = document.getElementById(objId);
	if (obj.removeEventListener) {
	   obj.removeEventListener(eventType, callBack, false);
	}
	else if (window.detachEvent) {
	   obj.detachEvent('on' + eventType, callBack);
	}
	else {
	   obj['on' + eventType] = null;
	}
}　
```


<h5 id="获取JS版本">获取当前浏览器JS的版本</h5>

```
function getjsversion(){
var n = navigator;
var u = n.userAgent;
var apn = n.appName;
var v = n.appVersion;
var ie = v.indexOf('MSIE ');
if (ie > 0){
  apv = parseInt(i = v.substring(ie + 5));
  if (apv > 3) {
      apv = parseFloat(i);
  }
} else {
  apv = parseFloat(v);
}
var isie = (apn == 'Microsoft Internet Explorer');
var ismac = (u.indexOf('Mac') >= 0);
var javascriptVersion = "1.0";
if (String && String.prototype) {
  javascriptVersion = '1.1';
  if (javascriptVersion.match) {
      javascriptVersion = '1.2';
      var tm = new Date;
      if (tm.setUTCDate) {
          javascriptVersion = '1.3';
          if (isie && ismac && apv >= 5) javascriptVersion = '1.4';
          var pn = 0;
          if (pn.toPrecision) {
              javascriptVersion = '1.5';
              a = new Array;
              if (a.forEach) {
                  javascriptVersion = '1.6';
                  i = 0;
                  o = new Object;
                  tcf = new Function('o', 'var e,i=0;try{i=new Iterator(o)}catch(e){}return i');
                  i = tcf(o);
                  if (i && i.next) {
                      javascriptVersion = '1.7';
                  }
              }
          }
      }
  }
}
return javascriptVersion;
}　　
```

<>