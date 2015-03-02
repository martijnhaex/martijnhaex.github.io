---
layout: post
title:  Vagrant 'got minus one from a read call'
summary: Solving ORACLE listener problem
date:   2015-03-02 19:19:58
tags: ORACLE, Vagrant
---

When your Vagrant-box hasn't shutdown correctly, which means the ORACLE listeners were still running at that moment.
The listeners couldn't shutdown correctly and release the resources. When doing a 'vagrant up' and querying to the ORACLE database you receive a 'got minus one from a read call' the following steps can resolve this problem.

### The Solution

First connect through SSH to your virtual environment (vagrant).

Check if a listener is still active, if a listener is still active kill the process. (When this problem occurs there should be no active listeners.)

{% highlight PowerShell %}
    > ps -ef | grep tnslsnr
> kill <processID>
{% endhighlight %}

After killing the listeners, the next step is verifying the content of listener.ora file. Open the listener.ora file:

{% highlight PowerShell %}
    > cd $ORACLE_HOME/network/admin
> vim listener.ora
{% endhighlight %}

listener.ora isn't allowing the restart of the ORACLE listener, due to misconfiguration of the HOST. Change **HOST = oracle** to **HOST = localhost**.

{% highlight PowerShell %}
    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
          (ADDRESS = (PROTOCOL = TCP)(HOST = oracle)(PORT = 1521))
        )
      )

ADR_BASE_LISTENER = /oracle/app/oracle
{% endhighlight %}

Restart the listeners with the following command:

{% highlight PowerShell %}
    > $ORACLE_HOME/bin/lsnrctl start
{% endhighlight %}