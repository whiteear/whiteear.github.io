---
title: 测试redcarpet2 markdown 引擎
layout: post
tags:
- blogging
- programming
categories:
- blogging

---

python block:

{% highlight python %}
def foo():
    print 'aaa'

def foob():
    print 'line2'

{% endhighlight %}

html block:

{% highlight html linos %}
<body>
   <ul>list sample
      <li>first
      <li>second
   </ul>
</body>
{% endhighlight %}

shell block:

{% highlight bash linos %}
# ls -al
-rw-------   1 root root      1487 9月  26 14:19 anaconda-ks.cfg
-rw-------   1 root root      7068 10月  8 17:49 .bash_history
-rw-r--r--   1 root root        18 9月  26 14:18 .bash_logout
-rw-r--r--   1 root root       355 9月  28 15:39 .bash_profile
-rw-r--r--   1 root root       176 9月  26 14:18 .bashrc
-rw-r--r--   1 root root       100 9月  26 14:18 .cshrc
{% endhighlight %}

