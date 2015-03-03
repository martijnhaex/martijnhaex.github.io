---
layout: post
title:  Got minus one from a read call
summary: Solving ORACLE listener problem
date:   2015-03-02 19:19:58
tags:
- ORACLE
- Vagrant
---

This problem occurred during the development project when our Vagrant box (running an ORACLE database) was suspended or shutdown correctly.

When doing a 'up' of your Vagrant box it looks to be working, no explicit errors on startup. So you start developing. After a while every Java developer needs to contact the database. But then you receive a somewhat cryptic exception like:
![ORA exception](/public/images/posts/got_minus_one_from_a_read_call.png)

### Four easy steps for a quick fix

####Step 1
Connect through SSH to your virtual environment (Vagrant).

####Step 2
Check if a listener is still active, if a listener is still active kill the process. (When this problem occurs there should be no active listeners.)

{% highlight PowerShell %}
    > ps -ef | grep tnslsnr
> kill <processID>
{% endhighlight %}

####Step 3
Verify the content of listener.ora file.

{% highlight PowerShell %}
    > cd $ORACLE_HOME/network/admin
> vim listener.ora
{% endhighlight %}

listener.ora isn't allowing the restart of the ORACLE listener, due to misconfiguration of the HOST. Change `HOST = oracle` to `HOST = localhost`.

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

####Step 4
Restart the listeners.

Try to reconnect and query your database.

{% highlight PowerShell %}
    > $ORACLE_HOME/bin/lsnrctl start
{% endhighlight %}

It can be that you need to do a 'suspend' and a 'up' of your Vagrant machine.
