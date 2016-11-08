title: Linux 安装 Telnet
date: 2014-08-31
Tags: linux, telnet
Category: dev-tools
---
linux默认是不安装telnet的，默认使用ssh服务。 
首先，查看一下有没有安装telnet及相关的服务软件：

####第一步： 
<pre><code>
:rpm -qa |grep telnet 
:rpm -qa |grep xinetd
</code></pre>

如果不存在就使用如下命令安装：

<pre><code>
:yum install xinetd telnet-server telnet-client(这个可选，从本机使用telnet登陆其他主机)
</code></pre>

####第二步： 
安装好之后，需要修改一下telnet的设置： 
<pre><code>
:vim /etc/xinetd.d/telnet
</code></pre>

>修改使能选项： 
disable = yes 改为 no 
保存退出。

启动xinetd服务 
<pre><code>
:service xinetd start
</code></pre>

若启动成功，即可从其他主机使用telnet登陆该机。

PS：若要用root身份登陆，需要修改安全策略： 
<pre><code>
:vim /etc/securetty
</code></pre>
添加两行：
>pts/0<br>
pts/1 

保存退出，重启xinetd服务。 
<pre><code>
:service xinetd restart
</code></pre>

尝试以root身份登陆。
