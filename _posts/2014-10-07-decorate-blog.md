---
title: 美化、修订、更新本Blogging系统记录
layout: post
tags:
- life
- programming
- tweak
- blogging

---

> 这篇博客主要记录我自身对Jekyll的学习和记录。包括对Jekyll系统的认识、本Blog模板[JIM](http://blog.sevenche.com/about/)的修改。如果你对Jekyll以及这个模板有兴趣，请评论或者邮件我。

-------
### 2014/10/07 更新


- 添加[life](/life)和[programming](/programming)两个页面。
  在Jekyll中，大部分的页面都是以POST（博客）来出现的。而每一个页面则是独立的，可以有不同的风格、不同的布局。在Jekyll原生的模板中，页面是放置在_pages目录中的。但JIM模板里是没有_pages目录的。页面是完全由用户来控制的。在_layout/default.html中，定义了页面的显示。如下图：
![pages](/media/system/pages.png)

  代码见_layout/default.html
{% highlight html %}
  <body>
    <div id="container">
      <div id="main" role="main">
        <header>
        <div class="buttons">
          <a href="/" class="ss-icon" title="Go to homepage">home</a>
          <a href="/life" class="ss-icon" title="The Living Life">love</a>
          <a href="/programming" class="ss-icon" title="Programming">desktop</a>
          <a href="/categories" class="ss-icon" title="Category" >folder</a>
          <a href="/tags" class="ss-icon" title="Tag Cloud" >tags</a>
          <a href="/guestbook" class="ss-icon" title="Guest Book" >talk</a>
          <a href="/about" class="ss-icon" title="About" >user</a>
          <a href="/feed" class="ss-icon" title="Subscribe by RSS" >rss</a>
        </div>
        <h1>{{ page.title }}</h1>
        </header>
        <hr>
        <article class="content">
        {{ content }}
        </article>
      </div>
{% endhighlight %}

  看到了没有？添加一个超连接即可。同时需要创建相应的目录，以存储链接对应的页面内容。

  在本次修改中。我添加了两个页面。分别为life和programming。用来记录日常的生活和在程序设计实现方面的经验和想法。我不打算对每一个POST都重新做。所以，在这两个页面中，要展示的主要的在这两类大主题下的博客内容。所以在life和programming中，我分别针对Tag进行了过滤。所有包括了life tag的POST都会自动显示在life页面上（programming类似）。理论上，所有的POST在主页面、categories、tag页面都可以找到。

  **图标**

  注意，在页面列表上面都有一个图标显示。这个东西让我搞了好半天。事实上，我也不懂HTML和CSS。所以就一步一步找这些图标在那儿。后来还是在CSS中有了些发现。它是定义在ss-standard.css中的。但是没有直接使用图片。每一个图标都有一个名子。图标和名子是怎么对应的也不清楚。我是在网上搜了ss-standard后，看显示的图片蒙出来图标名的。比如love对应的就是那个heart。在上同的示例图片中，就是那个“心”的符号 :-)

- 代码块 (code block)

  代码块高亮在Blog中是必须的。但在我使用马克飞象和标准的markdown语法时，代码块是通过如下的符号实现的 
  ![](/media/system/code_block_md.png)

  来实现的。但这个语法在Jekyll中，貌似不工作。查了一个Jekyll的语法。只能使用如下的语法：
  ![](/media/system/code_block_jekyll.png)

  **备注**

  到目前为止，我还不知道怎么直接写代码来输出这些特殊的字符。所以只能截图说明了。:-(


