---
layout: page
title: Module H
---

## 简介

第42届世界技能大赛 网站设计 模块H 相关技术解析

## 评分指标
* 为导航&幻灯片设计了全新的按钮，并且在hover/focus, click和normal三种状态下，按钮的观感会发生变化。
* ["Continue reading…"链接展示了三个栏目的完整文本(“About”, “41st Competition”, “40th Competition”)并伴随动画效果。](#continueReading) 
* [通过点击导航按钮，页面会平缓地滚动到各自的栏目。](#navbtn)

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
这里需要掌握以下知识点

1. 获取某个对象在页面的坐标  
通过元素的『offsetTop』属性可以获取对象相对于版面或由『offsetTop』属性指定的父坐标的计算顶端位置   
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
