---
layout: page
title: Module H
---

## 简介

第42届世界技能大赛 网站设计 模块H 相关技术解析

## 评分指标
1. 为导航&幻灯片设计了全新的按钮，并且在hover/focus, click和normal三种状态下，按钮的观感会发生变化。  
	使用样式的[锚伪类](http://www.w3school.com.cn/css/css_pseudo_classes.asp)，或监听鼠标事件修改样式
2. ["Continue reading…"链接展示了三个栏目的完整文本(“About”, “41st Competition”, “40th Competition”)并伴随动画效果。](#continueReading) 
3. [通过点击导航按钮，页面会平缓地滚动到各自的栏目。](#navbtn)
4. 栏目的背景被很好地设计了且同网站的主题吻合
5. 幻灯上图片的效果能吸引用户的兴趣  
	使用样式、动画实现，这个就靠个人发挥了
6. 导航的观感有专业设计的色彩和拓扑结构
7. ["Hand Symbol"通过动画到达3个栏目并且遵循提供的路径。](#symbol)

<a name="continueReading"></a>

## Continue reading…
Continue reading…"链接展示了三个栏目的完整文本(“About”, “41st Competition”, “40th Competition”)并伴随动画效果。

方法一：通过修改隐藏内容的高度，再配合样式实现动画效果。

	var hidtext = _;
	hidtext.style.display = 'block';
	var h = hidtext.offsetHeight;
	hidtext.style.height = '0px';
	var timer =setTimeout(function(){
		hidtext.style.height = h + 'px';
		clearTimeout(timer);
	},100);

方法二：通过增量修改高度的办法，不断地修改高度，实现动画效果。

    var hidtext = _;
    var height = hidtext.offsetHeight, start = 0, step = Math.max(height / 100, 15);
    hidtext.style.height = 0;
    var timer = setInterval(function() {
        if (start < height) {
            hidtext.style.height = Math.min((start += step), height) + "px";
        } else
            clearInterval(timer);
    }, 1);

<a name="navbtn"></a>

## 通过点击导航按钮，页面会平缓地滚动到各自的栏目

1. 获取某个对象在页面的坐标  
通过元素的"offsetTop"属性可以获取对象相对于版面或由"offsetTop"属性指定的父坐标的计算顶端位置   
如：

		var offsetTop = document.getElementById("id").offsetTop;

2. 获得滚动条当前滚动的高度

		var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;

3. 平缓地控制滚动条

	    var start = document.documentElement.scrollTop || document.body.scrollTop;
        var target = document.getElementById("id").offsetTop;
        var direct = target > start ? 1 : -1;
        var step = Math.max(Math.abs((target - start) / 100), 10) * direct;
        //这里需要使用全局的定时器，在执行新的滚动动画时，应该停止以前的滚动效果
        timer && clearInterval(timer);
        timer = setInterval(function() {
            var cur = document.documentElement.scrollTop || document.body.scrollTop;
            if ((cur - target) * direct >= 0)
                clearInterval(timer);
            else
                scrollTo(0, start += step);
        }, 1);

<a name="symbol"></a>

## "Hand Symbol"通过动画到达3个栏目并且遵循提供的路径

1. 注册监听事件，在页面滚动的时候做些什么

	    $(window).addEventListener('scroll', function(e) {
            //do something
		});

2. 未完待续。。。
