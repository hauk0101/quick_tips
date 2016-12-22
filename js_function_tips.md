#常用Javascript方法汇总
######整理者：hauk0101
##说明
本文档中主要总结了日常开发中与Javascript相关的一些实现方法、遇到的常见问题及解决方法或思路、以及其他开发技巧等。
>1.关于微信分享，根据程序中产生的不同的结果，实现分享出不同的转发结果。

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

>2.由时间戳获取现实时间字符串方法。

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
		

	
	
