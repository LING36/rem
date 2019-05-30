使用rem 解决自适应问题
====
一、兼容性。
----

目前，IE9+，Firefox、Chrome、Safari、Opera 的主流版本都支持了rem（大胆用吧，目前几乎所有手机浏览器都支持rem）

二、什么是rem。
----

rem是相对于根元素html字体大小来计算的，即( 1rem = html字体大小 )

三、使用rem布局有什么优点。
---

他的强大可以让你不在考虑不同尺寸屏幕的手机，和制作PC端一样的写法，只需要设置好参数，就可以为所欲为了。

四、你可能会疑惑，但只要你看了这段JS后你会明白的，看不懂的小伙伴们，看了第五点的介绍你就会明白了。
------

font函数，rem解决自适应问题   100px/100=1rem


`function font(designWidth, maxWidth) {
	var doc = document, //Document 对象 -浏览器的 HTML 文档
		win = window, //Window 对象表示浏览器中打开的窗口
		docEl = doc.documentElement, //documentElement 是整个节点树的根节点root，即<html> 标签
		remStyle = document.createElement("style"),  //页面的style
		tid;
		
	function refreshRem() {
		var width = docEl.getBoundingClientRect().width;//方法返回元素的大小及其相对于视口的位置。()
		maxWidth = maxWidth || 540;
		width > maxWidth && (width = maxWidth);
		var rem = width * 100 / designWidth;
		remStyle.innerHTML = 'html{font-size:' + rem + 'px;}';
		console.log(width);
	}

	if(docEl.firstElementChild) {
		docEl.firstElementChild.appendChild(remStyle);
	} else {
		var wrap = doc.createElement("div");
		wrap.appendChild(remStyle);
		doc.write(wrap.innerHTML);
		wrap = null;
	}
	refreshRem();

	win.addEventListener("resize", function() {
		clearTimeout(tid);
		tid = setTimeout(refreshRem, 300);
	}, false);

	win.addEventListener("pageshow", function(e) {
		if(e.persisted) {
			clearTimeout(tid);
			tid = setTimeout(refreshRem, 300);
		}
	}, false);

	if(doc.readyState === "complete") {
		doc.body.style.fontSize = "16px";
	} else {
		doc.addEventListener("DOMContentLoaded", function(e) {
			doc.body.style.fontSize = "16px";
		}, false);
	}
};`


五、给大家介绍下如何使用上面这段js和这段代码的意义。
----

1）用法很简单，只需要在html文件head最上面加入视口代码

`<meta name="viewport" content="width=device-width, initial-scale=1.0">`

2）然后新建一个js文件夹，将我们的佛祖和动态计算html字体大小的js代码放进去，然后在视口下面引入，注意，引入的script标签需要加上 async="async"，不然页面在加载时布局会出现短时间的错乱

3)使用过程中，这段js代码有两个参数可以传入，这两个参数就是你的设计师给你的设计稿宽度，填上就可以了，

4)使用很简单，设计稿宽度除以100使用即可（例如：设计稿宽度300px = 3rem，采用的是除100来换算，从而使在使用过程中更方便，无需使用rem单位换算工具）

5)rem不仅仅可以用在移动端布局，还可以用在PC端上，

例如：设计师给了你内容宽度1200px以上的设计搞要你做自适应时，你完全可以不用担心，只需要量一下设计稿宽度，修改我们js的两个参数修改为设计稿内容宽度即可。（提个醒，记得加上视口）

6)这段js主要是动态修改hrml字体大小，从而做到rem单位不动的情况下，自动适应所有手机端屏幕大小，想一下1rem在不同宽度手机上的值都是不一样的，是不是很完美呢。

7)引用了这一段js的脚本文件script标签 记得加上   async="async"  属性  不然会导致进入页面时，页面出现短时间错乱问题

六、在用rem的时候可能会遇到点小坑，下面小编给大家列出来几个经典的。
----
1)border:0.01rem solid #ccc;  边框的0.01rem在手机上会看不见，所以边框的0.01rem建议使用1px替代。

2)background-size使用rem无效，建议：修改背景图大小不要卡死，使用雪碧图的就会遇到这种问题了，建议使用直接的图片比较方便，用定位的方式或者使用百分比去控制，比如background-size: 100% auto;等
<br>
原文链接  https://blog.csdn.net/likeYou1207/article/details/80772466