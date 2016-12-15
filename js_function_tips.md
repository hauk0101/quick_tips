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
	
