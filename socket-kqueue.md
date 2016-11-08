title: python异步io，kqueue模式
date: 2016-06-25 00:19
Tags: socket,tcp,select,kqueue
category: python
---
本实验实现了一个server同时响应多个client的字符回显。

<font color='ff8000'>Server端代码kqueue-socket.py</font>
```python
#!/usr/bin/env python
import socket, select

HOST = 'localhost'
PORT = 5000
ADDR = (HOST, PORT)


serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serversocket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
serversocket.bind(ADDR)
serversocket.listen(1)
serversocket.setblocking(0)

kqueue = select.kqueue()
kevents = [select.kevent(serversocket.fileno(), filter=select.KQ_FILTER_READ, flags=select.KQ_EV_ADD|select.KQ_EV_ENABLE), ]

try:
    connections = {}; adds={}
    while True:
        events = kqueue.control(kevents, 1)
        if events:
            for event in events:
                if event.ident == serversocket.fileno():
                    connection, address = serversocket.accept()
                    print "...connect from ", address
                    connection.setblocking(0)
                    connections[connection.fileno()] = connection
                    adds[connection.fileno()]=address
                    kevents.append(select.kevent(connections[connection.fileno()].fileno(), select.KQ_FILTER_READ, select.KQ_EV_ADD, udata=connection.fileno()))
                elif event.udata in connections:
                    print event.udata
                    data = connections[event.udata].recv(1024)
                    if data.startswith('\n'):
                        kevents.remove(select.kevent(connections[event.udata].fileno(), select.KQ_FILTER_READ, select.KQ_EV_ADD, udata=event.udata))
                        print "...close connection from", adds[event.udata]
                        del connections[event.udata]
                        del adds[event.udata]
                    else:
                        connections[event.udata].send(data)
finally:
    kqueue.close()
    serversocket.close()
```

<font color='ff8000'>Client端代码tsTclnt.py</font>
```python
#!/usr/bin/env python
from socket import *

HOST='localhost'
PORT=5000
BUFSIZ = 1024
ADDR = (HOST, PORT)

tcpCliSock = socket(AF_INET, SOCK_STREAM)
tcpCliSock.connect(ADDR)

while True:
    data = raw_input('> ')
    if not data:
        tcpCliSock.send('\n')
        break
    tcpCliSock.send(data)
    data = tcpCliSock.recv(BUFSIZ)
    if not data:
        break
    print data

tcpCliSock.close()
```


<img src="http://ww1.sinaimg.cn/large/8011a96ejw1f56rizyx18j21kw0zkacy.jpg" alt="server-client启动视图">

如上图，先启动server，再启动三个client。启动后提示信息如下图：

<img src="http://ww1.sinaimg.cn/large/8011a96ejw1f56rj0njhzj21kw0zkq6m.jpg" alt="启动视图">

0窗口是server，1、2、3窗口分别是三个client。

***在client3输入：***
>&gt; aevaerva
>
>aevaerva
>
>&gt; 

***在client2输入：***
>&gt; aef
>
>aef
>
>&gt;

***在client1输入：***
>&gt; dafd
>
>dafd
>
>&gt; fav
>
>fav
>
>&gt; 
>

对应上面三个client的输入，server打印的信息是:
> 9
>
> 8
>
> 7
>
> 7
>

<font color='red'>说明：9是client3与server连接的socket fd，8是client2与server连接的socket fd，7是client1连接的socket fd。</font>

对client1、client2、client3依次直接回车，client关闭。server响应client关闭请求（字符'\n'代表关闭请求）关闭socket连接。server端打印的信息可以看出先后关闭了三个socket连接。

下图是整个交互过程的打印信息：
<img src="http://ww3.sinaimg.cn/large/8011a96ejw1f56rj1b43rj21kw0zkjwj.jpg" alt="交互界面">


PS:图片中界面多窗口terminal是tmux，推荐大家使用！
