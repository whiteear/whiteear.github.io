---
title: howto install scala
layout: post
tags:
- programming
- scala

---

scala is java based functional programming language. this blog introduce you `how to install scala`. it's simple and strangforward.

1. install JDK

make sure that java environment has been properly installed. you can check it with following commands

{% highlight yaml %}

# java -version
java version "1.7.0_75"
OpenJDK Runtime Environment (rhel-2.5.4.0.el6_6-x86_64 u75-b13)
OpenJDK 64-Bit Server VM (build 24.75-b04, mixed mode)

{% endhighlight %}

if java not installed. on centos, you can install it with 

{% highlight yaml %}

# yum install java-1.7.0-openjdk-devel.x86_64

{% endhighlight %}

2. download the scala binary tarball

scala can be downloaded from official website http://www.scala-lang.org/download/

choose your favorate version and download it. like:

{% highlight yaml %}

# wget http://downloads.typesafe.com/scala/2.11.6/scala-2.11.6.tgz

{% endhighlight %}

3. install it

{% highlight yaml %}

# tar zxf scala-2.11.6.tar.tgz
# mv scala-2.11.6 /usr/lib
# ln -f -s /usr/lib/scala-2.11.6 /usr/lib/scala
# export PATH=$PATH:/usr/lib/scala/bin

{% endhighlight %}

for quick access, please add following commands into your bash startup scripts.

{% highlight yaml %}

# vim ~/.bashrc
...
export PATH=$PATH:/usr/lib/scala/bin
...

{% endhighlight %}

