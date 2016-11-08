title: 网络socket cs最小通信系统
date: 2016-06-21 21:05
Tags: socket,tcp
category: python
---
创建一个能接受客户端消息，在消息前加一个时间戳后返回的TCP服务器。

<font color='ff8000'>tsTclnt.py</font>
```python
#!/usr/bin/env python

from socket import *
from time import ctime

HOST=''
PORT=21567
BUFSIZ = 1024
ADDR = (HOST, PORT)

tcpSerSock = socket(AF_INET, SOCK_STREAM)
tcpSerSock.bind(ADDR)
tcpSerSock.listen(5)

while True:
    print 'waiting for connection...'
    tcpCliSock, addr = tcpSerSock.accept()
    print '...conneted from:', addr

    while True:
        data = tcpCliSock.recv(BUFSIZ)
        if not data:
            break
        tcpCliSock.send('[%s] %s' % (ctime(), data))
        
    tcpCliSock.close()
tcpSerSock.close()
```
---
创建一个TCP客户端。

<font color='ff8000'>tsTclnt.py</font>
```python
#!/usr/bin/env python
from socket import *

HOST='localhost'
PORT=21567
BUFSIZ = 1024
ADDR = (HOST, PORT)

tcpCliSock = socket(AF_INET, SOCK_STREAM)
tcpCliSock.connect(ADDR)

while True:
    data = raw_input('> ')
    if not data:
        break
    tcpCliSock.send(data)
    data = tcpCliSock.recv(BUFSIZ)

    if not data:
        break
    print data

tcpCliSock.close()
```

先运行服务器，后开客户端。
下面就是客户端的输入和输出，不输入数据直接按回车键就可以退出程序：
<pre><code>
$ tsTclnt.py
&gt; hi
[Tue Jun 21 21:22:42 2016] hi
&gt; 南二舍   
[Tue Jun 21 21:23:21 2016] 南二舍
&gt;
$
</code></pre>

服务器的输出主要用于调试目的：
<pre><code>
$ tsTserv.py
waiting for connection...
...conneted from: ('127.0.0.1', 50272)
waiting for connection...''
</pre></code>
当有客户端连接上来的时候，会显示一个“...connect from...”信息。
