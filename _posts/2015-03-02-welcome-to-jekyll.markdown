---
layout: post
title:  Vagrant 'got minus one from a read call'
summary: Solving ORACLE listener problem
date:   2015-03-02 19:19:58
categories: ORACLE
---

When you Vagrant-box hasn't shutdown correctly, which means the ORACLE drivers are still running.
The listener cannot shutdown correctly. When doing a 'vagrant up' and connecting to the ORACLE database you receive a 'got minus one from a read call' the following steps can resolve this problem.

### The Solution

First connect through SSH to your virtual environment (vagrant).

Check if a listener is still active.

{% highlight PowerShell %}
    ps -ef | grep tnslsnr
{% endhighlight %}