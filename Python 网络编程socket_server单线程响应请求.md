title: Python 网络编程socket_server单线程响应请求
date: 2012-12-05 23:45
Tags: socket,单线程,server,network
category: python
---
刚开始接触python网络编程，使用socket编写一个简单的单线程server。socket模块提供了一个工厂函数，也被称为socket，开发者可以调用函数以生成一个套接字对象S。要想执行网络层操作，可以调用S上的方法。在客户程序中，可以调用`S.connect`连接到一个服务器。在服务器程序中，可以调用`S.bind`和`S.listen`等待客户程序的连接。在客户程序请求连接时，服务器程序可以调用`S.accept`接受请求，该方法将返回连接到客户程序的另一个套接字对象S1.在有了一个连接的套接字对象之后，就可以调用该对象的send方法传输数据，调用该对象的recv方法接收数据了。关于socket模块提供的函数和socket对象提供的方法请读者参阅python相关的技术书籍（例如《python技术手册》）。

分三步：

- 创建模块对象
- 创建模块对象的属性
- 调用模块对象的方法完成模块所想要实现的功能

源代码如下：
```python

#import socket module
from socket import *                  

serverSocket = socket(AF_INET, SOCK_STREAM) 
 
serverSocket.bind(('',8000))     

#监听该套接字的连接尝试，5代表允许的最多maxpending个排队的连接尝试                            
serverSocket.listen(5)                                          
print 'The server socket is ready...'
while True:
    #Establish the connection
    print 'Ready to serve...'

    connectionSocket, addr = serverSocket.accept()   
    try:
        message = connectionSocket.recv(8192)  
        
        filename = message.split()[1]   
        f = open(filename[1:])                                    
        outputdata = f.readlines(-1)
        for i in range(0, len(outputdata)):
            connectionSocket.send(outputdata[i])   
        connectionSocket.close()
    
    except IOError:                                                   
        #Send response message for file not found
        connectionSocket.send('404 Not found')
        #Close client socket
        connectionSocket.close()
serverSocket.close()

```
在源码文件所在的目录下建一个`hello.html`文件(里面写上任意文字，比如我写的`Hello world!`)。在其他任意一台主机上的`web browser`地址栏输入：`http:\\serverHost:8000\hello.html`回车即可看到你在`html`文件所写的文字（`serverHost`是指你运行server的主机IP）。也可以使用本机运行server脚本，在本机的browser上访问server，把`serverHost`换成`127.0.0.1`或者`localhost`就可以了。
