---
title: jekyll howto 添加百度地图
layout: post

tags:
- blogging
- programming
- javascript
- baidu
- map

categories:
- blogging

---

## jekyll howto: 添加百度地图

> 这篇文档记录我在给Blog添加百度支持时，是如何完成的。以及涉及的对
  Jekyll的理解。

### 起因

昨天在测试Blog的时候，写了一篇旅行的日志，见[2014国庆秦岭草甸](/2014/10/travel-qinlingcaodian/)。 在写的时候，就想能不能在日志中添加一个地图。以方便的标记出来秦岭草甸的具体位置。这样，如果有朋友看到了，想去这个地方就很方便的可以看到位置。而不是再google/baidu了。

有了这个想法后，就开始到处找怎么样把地图添加进来。 当然由于在朝内，所以首先想到的还是百度地图。在网上查找了一下百度地图API后，发现有一个API可以直接在页面中嵌入一个小区域。然后把需要的位置显示在里面。很是方便，于是研究了一下这个API的用法。具体的API介绍和使用见后面的内容。

有了地图后发现，如果只是在这篇日志中添加一个地图倒也方便。但如果以后还有类似的需要，那不是很麻烦？每次都得写一堆类似的代码嵌入在日志中。有没有可能，直接将地图做为一个扩展放到jekyll模板中。这样在以后的日志中，如果需要增加地图，只需要设置好对应的地理位置坐标就行。

### 设想&设计

想法是开发jekyll的一个扩展。在写日志的时候，只需要设置‘启用地图’和‘位置坐标’两个要素。地图就可以正确的显示在页面上。而在写新日志的时候，其它什么多余的代码都不需要。这多好啊！

为了实现这个想法，得解决以个问题：

- 如何设置‘启用地图’和‘位置坐标’？
- 如何在jekyll各模板和页面中传递变量？
- 百度地图API是个独立的页面，怎么添加到日志模板中？

#### jekyll变量和传值

经过研究jekyll的运行机会，发现jekyll早已经有了相应的解决方案。简单点来说，jekyll中有多个内置变量空间。如**site, post**。具体点：

- **site** 变量定义在_config.yml文件中。代表了这个站点中的全局变量。它可以在所有的页面和位置中获取得使用。
- **post** 变量的作用域为单个的日志页面。这也就是在写日志是，在每一个页面最前面的yaml格式的页面说明中的内容。

具体jekyll变更的说明和使用见：[jekyll build-in variables](http://havee.me/internet/2013-11/jekyll-liquid-designers.html)

post变量作用域作用在每一个日志页面上。正符合我们在单独页面中设置地图的需要。**就是你了**

**标记格式设计**

{% highlight yaml %}

---
title: the title of your post
layout: post

ditu:
  x : the x dimension of the map position.
  y : the y dimension of the map position.
---

{% endhighlight %}

如果用户设置了(x,y)坐标，则就在页面中显示地图。并且在地图中显示(x,y)对应的位置。获取(x,y)坐标，则需要使用百度地图API进行查询。具体查询方法，使用后面引用中的百度地图API，一看就明白了。

#### 百度地图API

如下图所示，很容易就可以得到地图上的某个位置对应的地图页面。最后点击“生成代码”，就可以得到对应的HTML内容。
![](/media/2014/baidu_api.png)

问题是获取的HTML代码对应的是一个完整的页面。如果把它嵌入到日志页面中呢？基本上来说有两种方法：

- 把整个页面完整的放置到服务器上。并在日志页面内，使用iframe将这个HTML嵌入。
- 把HTML的内容分割，把有用的内容抽取出来。在日志页面重新组合。

对比一下这两个方法。方法1简单方便。但有个最大的问题就是这个页面是针对某个具体位置坐标的。无法复用。每次嵌入地图都得生成对应的页面代码并保存HTML。还得写代码来加入到日志中。这与我们前面设想的方案并不一致。

方法2比较理解，但能实现嘛？

#### 抽取地图API

下面就是百度生成的地图页面代码。分析可以发现，在header部分，最主要的就是引用的Baidu 地图API的javascript文件。在body部分，就是生成一个id为dituCentent的div标签。而最后的javascript也就是调用api并生成具体的地图图象。同时，createMap函数中有明确的两个位置坐标。

所以，只要我们合理的把这三部分融入到日志页面中就可以解决问题。而且由于createMap是可以传入值的。结合页面变更post，就可以实现动态的传递位置信息而动态的生成地图。

一切都噢了，耶!!!

{% highlight html %}
{% raw %}

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
<meta name="keywords" content="百度地图,百度地图API，百度地图自定义工具，百度地图所见即所得工具" />
<meta name="description" content="百度地图API自定义地图，帮助用户在可视化操作下生成百度地图" />
<title>百度地图API自定义地图</title>
<!--引用百度地图API-->
<style type="text/css">
    html,body{margin:0;padding:0;}
    .iw_poi_title {color:#CC5522;font-size:14px;font-weight:bold;overflow:hidden;padding-right:13px;white-space:nowrap}
    .iw_poi_content {font:12px arial,sans-serif;overflow:visible;padding-top:4px;white-space:-moz-pre-wrap;word-wrap:break-word}
</style>
<script type="text/javascript" src="http://api.map.baidu.com/api?key=&v=1.1&services=true"></script>
</head>

<body>
  <!--百度地图容器-->
  <div style="width:697px;height:550px;border:#ccc solid 1px;" id="dituContent"></div>
</body>
<script type="text/javascript">
    //创建和初始化地图函数：
    function initMap(){
        createMap();//创建地图
        setMapEvent();//设置地图事件
        addMapControl();//向地图添加控件
    }
    
    //创建地图函数：
    function createMap(){
        var map = new BMap.Map("dituContent");//在百度地图容器中创建一个地图
        var point = new BMap.Point(116.765028,40.495063);//定义一个中心点坐标
        map.centerAndZoom(point,9);//设定地图的中心点和坐标并将地图显示在地图容器中
        window.map = map;//将map变量存储在全局
    }
    
    //地图事件设置函数：
    function setMapEvent(){
        map.enableDragging();//启用地图拖拽事件，默认启用(可不写)
        map.enableScrollWheelZoom();//启用地图滚轮放大缩小
        map.enableDoubleClickZoom();//启用鼠标双击放大，默认启用(可不写)
        map.enableKeyboard();//启用键盘上下左右键移动地图
    }
    
    //地图控件添加函数：
    function addMapControl(){
        //向地图中添加缩放控件
	var ctrl_nav = new BMap.NavigationControl({anchor:BMAP_ANCHOR_TOP_LEFT,type:BMAP_NAVIGATION_CONTROL_LARGE});
	map.addControl(ctrl_nav);
        //向地图中添加缩略图控件
	var ctrl_ove = new BMap.OverviewMapControl({anchor:BMAP_ANCHOR_BOTTOM_RIGHT,isOpen:1});
	map.addControl(ctrl_ove);
        //向地图中添加比例尺控件
	var ctrl_sca = new BMap.ScaleControl({anchor:BMAP_ANCHOR_BOTTOM_LEFT});
	map.addControl(ctrl_sca);
    }
    
    
    initMap();//创建和初始化地图
</script>
</html>

{% endraw %}
{% endhighlight %}

### 实现

由于我们要在日志中完成地图的显示，所以需要修改_layout/default.html这个文件。这是定义所有页面的基础layout模板。在post.html的header中，添加百度地图API的引用。也就是如下内容：

{% highlight html %}
<script type="text/javascript" src="http://api.map.baidu.com/api?key=&v=1.1&services=true"></script>
{% endhighlight %}

然后，我们将上面代码中的script部分添加到一个独立的可引用文件includes/ditu.md。并修改它的内容如下：

**注意**其中两个变量的使用page.ditu.x, page.ditu.y。这样原来固定的地图函数就变成动态的了。再结合日志关的变量定义，就可以动态的生成地图了。

{% highlight html %}
{% raw %}

<script type="text/javascript">
    //创建和初始化地图函数：
    function initMap(){
        createMap();//创建地图
        setMapEvent();//设置地图事件
        addMapControl();//向地图添加控件
    }
    
    //创建地图函数：
    function createMap(){
        var map = new BMap.Map("dituContent");//在百度地图容器中创建一个地图
        var point = new BMap.Point({{page.ditu.x}}, {{page.ditu.y}});//定义一个中心点坐标
        map.centerAndZoom(point,17);//设定地图的中心点和坐标并将地图显示在地图容器中
        window.map = map;//将map变量存储在全局
    }
    
    //地图事件设置函数：
    function setMapEvent(){
        map.enableDragging();//启用地图拖拽事件，默认启用(可不写)
        map.enableScrollWheelZoom();//启用地图滚轮放大缩小
        map.enableDoubleClickZoom();//启用鼠标双击放大，默认启用(可不写)
        map.enableKeyboard();//启用键盘上下左右键移动地图
    }
    
    //地图控件添加函数：
    function addMapControl(){
        //向地图中添加缩放控件
	var ctrl_nav = new BMap.NavigationControl({anchor:BMAP_ANCHOR_TOP_LEFT,type:BMAP_NAVIGATION_CONTROL_LARGE});
	map.addControl(ctrl_nav);
                //向地图中添加比例尺控件
	var ctrl_sca = new BMap.ScaleControl({anchor:BMAP_ANCHOR_BOTTOM_LEFT});
	map.addControl(ctrl_sca);
    }
        
   initMap();//创建和初始化地图
</script>

{% endraw %}
{% endhighlight %}

最后一步，在每一篇日志的内容中，在需要添加日志的地方，添加如下的代码：

{% highlight html %}
<div style="width:100%;height:550px;border:#ccc solid 1px;" id="dituContent"></div>
{% endhighlight %}

最最后，在_layout/post.html文件中，添加地图的引用即可。这里我特点对地图进行了一个判读。没有地图的日志就不会再引用ditu.md了。可以加速页面。

![](/media/2014/baidu_ditu_inc.png)


### 资源引用

- [jekyll build-in variables](http://havee.me/internet/2013-11/jekyll-liquid-designers.html)
- [baidu map API](http://api.map.baidu.com/lbsapi/creatmap/index.html)

<全文完>
