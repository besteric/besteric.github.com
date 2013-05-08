---
author: admin
comments: true
date: 2011-07-24 17:31:40
layout: post
slug: something-about-work
title: Something about Work
wordpress_id: 826
categories:
- 蛋疼的事
---

最近负责淘宝首页的一些需求，开始写一些简单的Javascript，主要是基于淘宝的开源前端框架Kissy


简单的显示逻辑


[![](http://www.besteric.com/wp-content/uploads/2011/07/ALD_Logic.png)](http://www.besteric.com/wp-content/uploads/2011/07/ALD_Logic.png)

`
/**
 * 阿拉丁系统定投广告
 *
 * @creator heisan@taobao.com
 * @depends ks-core
 *
1 从首页Cookie取得tracknick，传给后台回调取得JSON格式的数据
2 读取淘宝商城品牌墙的标签，如果存在data-ald属性的话将JSON数据填充（前提条件不为空）
 **/

FP.add('ald',
function(S) {
	S.namespace('Ald');

	S.Ald.init = function() {

		var NICK = decode(S.Cookie.get("_nk_")) || decode(S.Cookie.get("tracknick")),
		tms_product = S.query('a', S.get('div.hot-banner-wrapper')),
		needald = false,
		tms_title = [],
		new_ald = [];

		for (var i = 0; i < tms_product.length; i++) {

			if (tms_product[i].getAttribute('data-ald') == 'yes') {
				needald = true;
			} else {
				//将现有的TMS指定显示的商品Title保存起来
				tms_title.push(tms_product[i].title);
			}
		}
		// 如果Cookie里面存在用户信息则继续执行
		if (!NICK || !needald)
		 return;

		S.getScript('http://ald.taobao.com/home/recommendStrValueJson.htm?appID=94&num;=1&varName;=aldata&key;=' + encodeURI(NICK),
		function() {
			if (typeof aldata === 'undefined' || aldata.length != 6)
			 return;

			var POSITION_ID = ["15004843018100849003", "1500484303e36c07f894", "1500484304dcc54f6ca9", "1500484305c313771183", "1500484306ae48c2cc1e", "15004843075b08638541"];

			//删除阿拉丁与TMS指定显示的重复数据,构建新的数组
			for (var i = 0; i < aldata.length; i++) {
				if (!S.inArray(aldata[i].title, tms_title)) {
					new_ald.push(aldata[i]);

				}
			}

			for (var i = 0, j = tms_product.length; i < j; i++) {
				var item = tms_product[i];
				if (item.getAttribute('data-ald') == 'yes') {

					//拼接recommend_id与TMS位置埋点信息
					new_ald[0].url = "http://ju.atpanel.com/?url=" + new_ald[0].url + "&recommendId;=" + new_ald[0].biParam + "?ad_id=&am;_id=&cm;_id=&pm;_id=" + POSITION_ID[i];

					var img = S.DOM.get('img', item);
					//替换超链接的标题，图片链接，跳转链接
					item.title = new_ald[0].title;
					img.src = new_ald[0].img;
					item.href = new_ald[0].url;
					//填充完毕删除该元素
					new_ald.splice(0, 1);
				}
			}

		});
		// unicode解码
		function decode(str) {
			if (str) {
				return str.replace(/\\u([0-9a-f]{4,4})/ig,
				function(t, m1) {
					return String.fromCharCode(parseInt(m1, 16));
				});
			} else {
				//str为空字符串
				return null;
			}
		};

	};

});

`

