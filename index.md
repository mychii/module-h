---
layout: page
title: Module H
---

## 简介

第42届世界技能大赛 网站设计 模块H 相关技术解析

## 评分指标
1. 为导航&幻灯片设计了全新的按钮，并且在hover/focus, click和normal三种状态下，按钮的观感会发生变化。  
	使用样式的[锚伪类](http://www.w3school.com.cn/css/css_pseudo_classes.asp)，或监听鼠标事件修改样式。
2. ["Continue reading…"链接展示了三个栏目的完整文本(“About”, “41st Competition”, “40th Competition”)并伴随动画效果。](#continueReading) 
3. [通过点击导航按钮，页面会平缓地滚动到各自的栏目。](#navbtn)
4. 栏目的背景被很好地设计了且同网站的主题吻合。
5. 幻灯上图片的效果能吸引用户的兴趣。
6. 导航的观感有专业设计的色彩和拓扑结构。
7. ["Hand Symbol"通过动画到达3个栏目并且遵循提供的路径。](#symbol)
8. 结果列表的视图结构需相应改变，以便用一个全新的视角来展现信息。  
	建议使用js去分析并提取数据，然后生成新界面。
9. 背景图片按照要求添加到了栏目中。
10. 幻灯片框架部分的设计提升了用户体验。
11. 结果栏目的布局样式既有功能性又有良好的可读性。
12. [应用在背景上之移动效果的质量能提升用户体验。](#background)  
13. 通过点击4个导航按钮，能显示合适的栏目。  
	同3
14. [幻灯页自动循环播放切换，间隔5秒](#autoplay)
15. [在模板内点击按钮就能去到前一张或者下一张幻灯](#prev_and_next)
16. 当滚动页面的时候，栏目背景的位置会发生改变。  
	同12
17. 当搜索文本框被focus时，"Hand Symbol"将被移动到"Results"栏目标题的左侧，并伴随一个动画效果。
18. 在结果栏目中，为列表中的每一个选手都显示所有要求的信息（如括号中所列出的）。
19. 通过输入姓名，技能名或者国家名等短语，结果列表中的数据会被立即进行过滤。  
	通过对元素的查找，然后控制元素的可见性。
20. 用户可以通过点击列表中的name, country, skill name 或者 medal title来更改搜索关键词，并且搜索结果应当同时被改变。  
	绑定点击事件，将相应的值写到搜索框中，然后使用19的搜索功能。
21. [通过按住SHIFT并点击关键字，就能在文本框内输入带+号的关键字，文本本身不会高亮并且立即返回正确的信息。](#shift)
22. 如果对应关键词没有任何搜索结果，则显示一条消息而不是空结果列表。
23. [当在搜索文本框中输入文字时，“自动完成”列表框将会被显示。并且在选择一个备选关键词选项后，都能根据大小写选项的状态来显示搜索结果。](#datalist)
24. 当世界技能组织的logo “Hand Symbol” 被点击时，页面会平缓回滚到顶部。
25. [在重新打开浏览器后，通过"CTRL+S"操作被保存了的搜索关键词以及结果列表会被显示。](#save)

<a name="continueReading"></a>

## Continue reading…
Continue reading…"链接展示了三个栏目的完整文本(“About”, “41st Competition”, “40th Competition”)并伴随动画效果。

方法一：通过修改隐藏内容的高度，再配合样式实现动画效果。

	var hidetext = _;
	hidetext.style.display = 'block';
	var h = hidetext.offsetHeight;
	hidetext.style.height = '0px';
	var timer =setTimeout(function(){
		hidetext.style.height = h + 'px';
		clearTimeout(timer);
	},100);

方法二：通过增量修改高度的办法，不断地修改高度，实现动画效果。

    var hidetext = _;
    var height = hidetext.offsetHeight, start = 0, step = Math.max(height / 100, 15);
    hidetext.style.height = 0;
    var timer = setInterval(function() {
        if (start < height) {
            hidetext.style.height = Math.min((start += step), height) + "px";
        } else
            clearInterval(timer);
    }, 1);

<a name="navbtn"></a>

## 通过点击导航按钮，页面会平缓地滚动到各自的栏目

1. 获取某个对象在页面的坐标。  
通过元素的"offsetTop"属性可以获取对象相对于版面或由"offsetTop"属性指定的父坐标的计算顶端位置。   
如：

		var offsetTop = document.getElementById("id").offsetTop;

2. 获得滚动条当前滚动的高度。

		var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;

3. 平缓地控制滚动条。

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

1. 注册监听事件，在页面滚动的时候做些什么。

	    $(window).addEventListener('scroll', function(e) {
            //do something
		});

2. 根据滚动条的高度调整"Hand Symbol"的位置。

        //定义不同章节的坐标
        var sections = {"#top": {"left": 55, "top": 0}, '#about': {"left": 870, "top": 80},
            '#competition41': {"left": 20, "top": 40}, '#competition40': {"left": 860, "top": 40}};
        var windowScrollTop = $(window).scrollTop();
        for (var id in sections) {
            var section = $(id), sectionTop = section.offset().top + sections[id].top, sectionHeight = section.height();
			 if(windowScrollTop < sectionTop || windowScrollTop < 50 && id == "#top"){
				 $("#handSymbol").css("top", sectionTop + "px").css("left", sections[id].left + "px");
				 break;
			 }
        }

3. 添加样式，使得改变top和left时，有动画效果。

<a name="background"></a>

## 应用在背景上之移动效果的质量能提升用户体验。
在页面滚动的时候，修改背景图的位置，使得背景有动画效果。

	$('#id').css("backgroundPosition", "0 -38px");

<a name="autoplay"></a>

## 幻灯页自动循环播放切换，间隔5秒

	function autoplay() {
        next();//此处省略了切换图片的方法
        timer = setTimeout(autoplay, 5000);
    }

<a name="prev_and_next"></a>

## 在模板内点击按钮就能去到前一张或者下一张幻灯

	function next(direction) {
		//这里timer使用的是全局变量，方便点击左右按钮时清除自动播放的定时器
        clearInterval(timer);
        direction = direction || 1;
        var nextli = (index + direction + 3) / 3;//假设图片总数为3，加3后再模3是为了解决越界问题
        $(".ani li").hide().eq(nextli).show();
    };

<a name="shift"></a>

## SHIFT
通过按住SHIFT并点击关键字，就能在文本框内输入带+号的关键字，文本本身不会高亮并且立即返回正确的信息。

在绑定点击事件的代码中，可以通过检查event的shiftKey属性，得知是否按住了SHIFT键

 	$(".sth").click(function (e){
       var val = this.innerHTML, iptVal = $("#ipt").val();
	   if(e.shiftKey && iptVal != "") val = iptVal + "+" + val;
       $("#ipt").val(val);
       search();//此处省略了查询方法
	});

<a name="datalist"></a>

## datalist
当在搜索文本框中输入文字时，“自动完成”列表框将会被显示。并且在选择一个备选关键词选项后，都能根据大小写选项的状态来显示搜索结果。

建议使用datalist完成这个功能，<a href="http://www.w3school.com.cn/html5/html5_datalist.asp" target="_blank">参考资料</a>

因为datalist不支持限制数量的功能，所以需要实时解析输入框的内容，生成相应的option。

<a name="datalist"></a>

## CTRL+S
在重新打开浏览器后，通过"CTRL+S"操作被保存了的搜索关键词以及结果列表会被显示。

	ipt.addEventListener("onkeypress", function(e){
		if( (e.charCode == 115 || e.charCode == 83) && e.ctrlKey){
			if(ipt.value.trim() == ''){
				localStorage.schText = null;
			}else{
				localStorage['schText'] = ipt.value.trim();//保存搜索的关键字
			}
			var state = ipt_chk.checked ? 'yes' : 'no';
			local('ipt_chk_val',state);//保存复选框状态
			e.preventDefault();//取消事件的默认动作
			return false;
		}
	});