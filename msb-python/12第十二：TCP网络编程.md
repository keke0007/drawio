# TCP网络编程

## 一、什么是TCP协议

TCP( Transmission control protocol )即传输控制协议，是一种面向连接、可靠的数据传输协议，它是为了在不可靠的互联网上提供可靠的端到端字节流而专门设计的一个传输协议。

* **面向连接** ：数据传输之前客户端和服务器端必须建立连接
* **可靠的** ：数据传输是有序的 要对数据进行校验

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1662183929016/325adaf8197244c3bfc8ebb4a4b539e8.png)

## 二、TCP三次握手

为了保证客户端和服务器端的可靠连接，TCP建立连接时**必须**要进行三次会话，也叫TCP三次握手，**进行三次握手的目的是为了确认双方的接收能力和发送能力是否正常。**

举个例子：
公安局长王哥 和 陈某打电话，准备自首劝说通话。

公安局：你好！陈某，听得到吗？（一次会话）
陈某：听到了，王哥，你能听到吗 （二次会话）
公安局：听到了，你过来自首吧 （开始会话）（三次会话）

通过这个例子我们可以知道三次会话的目的就是为了确保双方的连接正常，同理，TCP三次握手也是这个过程，下面用图文形式来解释一下TCP三次握手。

![](https://img-blog.csdnimg.cn/7bfb98fdba4740d2a5afaaef8cf9ab98.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzU2NjQ5NTU3,size_16,color_FFFFFF,t_70)

第一次握手 TCP客户进程也是先创建传输控制块TCB，然后向服务器发出连接请求报文，这是报文首部中的同部位SYN=1，同时选择一个初始序列号 seq=x ，此时，TCP客户端进程进入了 SYN-SENT 同步已发送状态

第二次握手 TCP服务器收到请求报文后，如果同意连接，则会向客户端发出确认报文。确认报文中应该 ACK=1，SYN=1，确认号是ack=x+1，同时也要为自己初始化一个序列号 seq=y，此时，TCP服务器进程进入了 SYN-RCVD 同步收到状态.

第三次握手 TCP客户端收到确认后，还要向服务器给出确认。确认报文的ACK=1，ack=y+1，自己的序列号seq=x+1，此时，TCP连接建立，客户端进入ESTABLISHED已建立连接状态 触发三次握手

**三次握手主要作用：防止已经失效的连接请求报文突然又传送到了服务器，从而产生错误**

第一次握手： 客户端向服务器端发送报文
证明客户端的发送能力正常
第二次握手：服务器端接收到报文并向客户端发送报文
证明服务器端的接收能力、发送能力正常
第三次握手：客户端向服务器发送报文
证明客户端的接收能力正常

## 三、TCP四次挥手

建立TCP连接需要三次握手，终止TCP连接需要四次挥手。

举个例子

张三和李四的对话

张三：好的，那我先走了
李四：好的，那你走吧
李四：那我也走了？
张三：好的，你走吧

![](https://img-blog.csdnimg.cn/3ca4f59c07ed4c83b9ca7a16ba8344f7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzU2NjQ5NTU3,size_16,color_FFFFFF,t_70)

第一次挥手 客户端发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），此时，客户端进入FIN-WAIT-1（终止等待1）状态

第二次挥手 服务器端接收到连接释放报文后，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT 关闭等待状态

第三次挥手 客户端接收到服务器端的确认请求后，客户端就会进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文，服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。

第四次挥手 客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态，但此时TCP连接还未终止，必须要经过2MSL后（最长报文寿命），当客户端撤销相应的TCB后，客户端才会进入CLOSED关闭状态，服务器端接收到确认报文后，会立即进入CLOSED关闭状态，到这里TCP连接就断开了，四次挥手完成

**为什么客户端要等待2MSL？**
主要原因是为了保证客户端发送那个的第一个ACK报文能到到服务器，因为这个ACK报文可能丢失，并且2MSL是任何报文在网络上存在的最长时间，超过这个时间报文将被丢弃，这样新的连接中不会出现旧连接的请求报文。

## 四、TCP实现一对一的简单聊天

服务端代码

增加：客户端想要离开，必须发给服务器一个'bye' , 服务器发给客户端一个'^^^'。 这个时候就真正close连接。

```python
# coding=utf-8
# 文件名：qq_server.py

from socket import *

# 创建socket
tcp_server = socket(AF_INET, SOCK_STREAM)

# 绑定本地信息
host_port = ('', 12345)
tcp_server.bind(host_port)

# 使用socket创建的套接字默认的属性是主动的，使用listen将其变为被动的，这样就可以接收别人的链接了
tcp_server.listen(5)

while True:

    # 如果有新的客户端来链接服务器，那么就产生一个信心的套接字专门为这个客户端服务器
    # newSocket用来为这个客户端服务
    # tcpSerSocket就可以省下来专门等待其他新客户端的链接
    newSocket, host_port = tcp_server.accept()

    while True:

        # 接收对方发送过来的数据，最大接收1024个字节
        recvData = newSocket.recv(1024)

        # 如果接收的数据的长度为0，则意味着客户端关闭了链接
        if len(recvData) > 0:
            print('recv:', recvData)
        else:
            break

        # 发送一些数据到客户端
        sendData = input("send:")
        newSocket.send(sendData.encode('utf8'))

    # 关闭为这个客户端服务的套接字，只要关闭了，就意味着为不能再为这个客户端服务了，如果还需要服务，只能再次重新连接
    newSocket.close()

# 关闭监听套接字，只要这个套接字关闭了，就意味着整个程序不能再接收任何新的客户端的连接
tcp_server.close()

```

客户端代码：

```python
# coding=utf-8
# 文件名：qq_server.py


from socket import *

# 创建socket
tcp_client = socket(AF_INET, SOCK_STREAM)

# 链接服务器
host_port = ('127.0.0.1', 12345)
tcp_client.connect(host_port)

while True:

    # 提示用户输入数据
    sendData = input("send：")

    if len(sendData) > 0:
        tcp_client.send(sendData.encode('utf8'))
    else:
        break

    # 接收对方发送过来的数据，最大接收1024个字节
    recvData = tcp_client.recv(1024)
    print('recv:', recvData.decode('uft8'))

# 关闭套接字
tcp_client.close()

```
