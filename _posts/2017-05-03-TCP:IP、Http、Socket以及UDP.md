---
layout: post
title:  TCP/IP、Http、Socket以及UDP
date:   2017-05-03 17:42:12 +0800
categories: 技术
tag: Object-C
---


* TOC
{:toc}


--------------------

一、好文学习（感谢大神分享）
=======

- [[面试∙网络] TCP/IP（一）：数据链路层](http://www.jianshu.com/p/f16d7f3c8d5f)

- [[面试∙网络] TCP/IP（二）：IP协议](http://www.jianshu.com/p/9cb5cf1864da)

- [[面试∙网络] TCP/IP（三）：IP协议相关技术](http://www.jianshu.com/p/f0d5a8ee9f17)

- [[面试∙网络] TCP/IP（四）：TCP 与 UDP 协议简介](http://www.jianshu.com/p/dc456cf57e06)

- [[面试∙网络] TCP/IP（五）：TCP 协议详解](http://www.jianshu.com/p/d9edbba4035b)

- [[面试∙网络] TCP/IP（六）：HTTP 与 HTTPS 简介](http://www.jianshu.com/p/6e7521041e92)

- [九个问题从入门到熟悉HTTPS](http://www.jianshu.com/p/072a657337ae)

二、概念介绍
====

**网络由下往上分为：物理层、数据链路层、网络层、传输层、会话层、表示层和应用层。**

　　`IP协议`：网络层；

　　`TCP协议`：传输层；

　　`HTTP协议`：应用层；

　　`Socket`：对TCP/IP协议的封装和应用(程序员层面上)。

四者从本质上来说没有可比性。也可以说，TPC/IP协议是传输层协议，主要解决数据如何在网络中传输；而HTTP是应用层协议，主要解决如何包装数据。

关于TCP/IP和HTTP协议的关系，网络有一段比较容易理解的介绍：

>“我们在传输数据时，可以只使用(传输层)TCP/IP协议，但是那样的话，如果没有应用层，便无法识别数据内容。

>如果想要使传输的数据有意义，则必须使用到应用层协议。

应用层协议有很多，比如HTTP、FTP、TELNET等，也可以自己定义应用层协议。

WEB使用HTTP协议作应用层协议，以封装HTTP文本信息，然后使用TCP/IP做传输层协议将它发到网络上。”

而我们平时说的最多的socket是什么呢，**实际上socket是对TCP/IP协议的封装，Socket本身并不是协议，而是一个调用接口(API)。**通过Socket，我们才能使用TCP/IP协议。实际上，Socket跟TCP/IP协议没有必然的联系：Socket编程接口在设计的时候，就希望也能适应其他的网络协议。所以说，Socket的出现只是使得程序员更方便地使用TCP/IP协议栈而已，是对TCP/IP协议的抽象，从而形成了我们知道的一些最基本的函数接口，比如create、listen、connect、accept、send、read和write等等。

　　网络有一段关于socket和TCP/IP协议关系的说法比较容易理解：
>“TCP/IP只是一个协议栈，就像操作系统的运行机制一样，必须要具体实现，同时还要提供对外的操作接口。

>这个就像操作系统会提供标准的编程接口，比如win32编程接口一样，TCP/IP也要提供可供程序员做网络开发所用的接口，这就是Socket编程接口。”

　　关于TCP/IP协议的相关只是，用博大精深来讲我想也不为过，单单查一下网上关于此类只是的资料和书籍文献的数量就知道，今天就先总结一些基于基于TCP/IP协议的应用和编程接口的知识，也就是刚才说了很多的HTTP和Socket。

　　CSDN上有个比较形象的描述：HTTP是轿车，提供了封装或者显示数据的具体形式；Socket是发动机，提供了网络通信的能力。

　　实际上，传输层的TCP是基于网络层的IP协议的，而应用层的HTTP协议又是基于传输层的TCP协议的，而Socket本身不算是协议，就像上面所说，它只是提供了一个针对TCP或者UDP编程的接口。

　　下面是一些经常在笔试或者面试中碰到的重要的概念，特在此做摘抄和总结。

2.1 什么是TCP连接的三次握手
------------

　　第一次握手：客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认;

　　第二次握手：服务器收到syn包，必须确认客户的SYN(ack=j+1)，同时自己也发送一个SYN包(syn=k)，即SYN+ACK包，此时服务器进入SYN_RECV状态;

　　第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

　　握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。

　　理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。

　　断开连接时服务器和客户端均可以主动发起断开TCP连接的请求，断开过程需要经过“四次握手”(过程就不细写了，就是服务器和客户端交互，最终确定断开)

2.2 利用Socket建立网络连接的步骤
----------

　　建立Socket连接至少需要一对套接字，其中一个运行于客户端，称为ClientSocket ，另一个运行于服务器端，称为ServerSocket 。

　　套接字之间的连接过程分为三个步骤：服务器监听，客户端请求，连接确认。

　　1、服务器监听：服务器端套接字并不定位具体的客户端套接字，而是处于等待连接的状态，实时监控网络状态，等待客户端的连接请求。

　　2、客户端请求：指客户端的套接字提出连接请求，要连接的目标是服务器端的套接字。

　　为此，客户端的套接字必须首先描述它要连接的服务器的套接字，指出服务器端套接字的地址和端口号，然后就向服务器端套接字提出连接请求。

　　3、连接确认：当服务器端套接字监听到或者说接收到客户端套接字的连接请求时，就响应客户端套接字的请求，建立一个新的线程，把服务器端套接字的描述发给客户端，一旦客户端确认了此描述，双方就正式建立连接。

　　而服务器端套接字继续处于监听状态，继续接收其他客户端套接字的连接请求。

2.3 HTTP链接的特点
---------

　　HTTP协议即超文本传送协议(Hypertext Transfer Protocol )，是Web联网的基础，也是手机联网常用的协议之一，HTTP协议是建立在TCP协议之上的一种应用。

　　HTTP连接最显著的特点是客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。从建立连接到关闭连接的过程称为“一次连接”。

2.4 TCP和UDP的区别
-------

　　1、TCP是面向链接的，虽然说网络的不安全不稳定特性决定了多少次握手都不能保证连接的可靠性，但TCP的三次握手在最低限度上(实际上也很大程度上保证了)保证了连接的可靠性;

　　而UDP不是面向连接的，UDP传送数据前并不与对方建立连接，对接收到的数据也不发送确认信号，发送端不知道数据是否会正确接收，当然也不用重发，所以说UDP是无连接的、不可靠的一种数据传输协议。

　　2、也正由于1所说的特点，使得UDP的开销更小数据传输速率更高，因为不必进行收发数据的确认，所以UDP的实时性更好。

　　知道了TCP和UDP的区别，就不难理解为何采用TCP传输协议的MSN比采用UDP的QQ传输文件慢了，但并不能说QQ的通信是不安全的，

　　因为程序员可以手动对UDP的数据收发进行验证，比如发送方对每个数据包进行编号然后由接收方进行验证啊什么的，

　　即使是这样，UDP因为在底层协议的封装上没有采用类似TCP的“三次握手”而实现了TCP所无法达到的传输效率。

本人觉得TCP就像是 打电话，一再确认，三次握手-----（才会开始发送数据，安全，效率低），而UDP就像是发短信，不管在不在，数据都会发送过去---（实时，高效，不安全）。
　　
2.5 TCP长连接与短连接的区别
---------

1. TCP连接

当网络通信时采用TCP协议时，在真正的读写操作之前，server与client之间必须建立一个连接，当读写操作完成后，双方不再需要这个连接时它们可以释放这个连接，连接的建立是需要三次握手的，而释放则需要4次握手，所以说每个连接的建立都是需要资源消耗和时间消耗的

经典的三次握手示意图：

![]({{ '/source/TCP、IP、Http、Socket以及UDP/01.png'}})

经典的四次握手关闭图：

![]({{ '/source/TCP、IP、Http、Socket以及UDP/02.png'}})

2. TCP短连接

我们模拟一下TCP短连接的情况，client向server发起连接请求，server接到请求，然后双方建立连接。client向server发送消息，server回应client，然后一次读写就完成了，这时候双方任何一个都可以发起close操作，不过一般都是client先发起close操作。为什么呢，一般的server不会回复完client后立即关闭连接的，当然不排除有特殊的情况。从上面的描述看，短连接一般只会在client/server间传递一次读写操作

短连接的优点是：管理起来比较简单，存在的连接都是有用的连接，不需要额外的控制手段

3.TCP长连接

接下来我们再模拟一下长连接的情况，client向server发起连接，server接受client连接，双方建立连接。Client与server完成一次读写之后，它们之间的连接并不会主动关闭，后续的读写操作会继续使用这个连接。

首先说一下TCP/IP详解上讲到的TCP保活功能，保活功能主要为服务器应用提供，服务器应用希望知道客户主机是否崩溃，从而可以代表客户使用资源。如果客户已经消失，使得服务器上保留一个半开放的连接，而服务器又在等待来自客户端的数据，则服务器将应远等待客户端的数据，保活功能就是试图在服务器端检测到这种半开放的连接。

如果一个给定的连接在两小时内没有任何的动作，则服务器就向客户发一个探测报文段，客户主机必须处于以下4个状态之一：

客户主机依然正常运行，并从服务器可达。客户的TCP响应正常，而服务器也知道对方是正常的，服务器在两小时后将保活定时器复位。
客户主机已经崩溃，并且关闭或者正在重新启动。在任何一种情况下，客户的TCP都没有响应。服务端将不能收到对探测的响应，并在75秒后超时。服务器总共发送10个这样的探测 ，每个间隔75秒。如果服务器没有收到一个响应，它就认为客户主机已经关闭并终止连接。
客户主机崩溃并已经重新启动。服务器将收到一个对其保活探测的响应，这个响应是一个复位，使得服务器终止这个连接。
客户机正常运行，但是服务器不可达，这种情况与2类似，TCP能发现的就是没有收到探查的响应。
从上面可以看出，TCP保活功能主要为探测长连接的存活状况，不过这里存在一个问题，存活功能的探测周期太长，还有就是它只是探测TCP连接的存活，属于比较斯文的做法，遇到恶意的连接时，保活功能就不够使了。

在长连接的应用场景下，client端一般不会主动关闭它们之间的连接，Client与server之间的连接如果一直不关闭的话，会存在一个问题，随着客户端连接越来越多，server早晚有扛不住的时候，这时候server端需要采取一些策略，如关闭一些长时间没有读写事件发生的连接，这样可以避免一些恶意连接导致server端服务受损；如果条件再允许就可以以客户端机器为颗粒度，限制每个客户端的最大长连接数，这样可以完全避免某个蛋疼的客户端连累后端服务。

长连接和短连接的产生在于client和server采取的关闭策略，具体的应用场景采用具体的策略，没有十全十美的选择，只有合适的选择。

参考：[TCP长连接与短连接的区别](http://www.cnblogs.com/beifei/archive/2011/06/26/2090611.html)


三、Socket使用
======

socket（套接字）是通信的基石，是支持TCP/IP协议的网络通信的基本操作单元，包含进行网络通信必须的五种信息：连接使用的协议，本地主机的IP地址，本地进程的协议端口，远地主机的IP地址，远地进程的协议端口。

多个TCP连接或多个应用程序进程可能需要通过同一个TCP协议端口传输数据。为了区别不同的应用程序进程和连接，计算机操作系统为应用程序与TCP/IP协议交互提供了套接字(Socket)接口。应用层可以和传输层通过Socket接口，区分来自不同应用程序进程或网络连接的通信，实现数据传输的并发服务。

建立Socket连接至少需要一对套接字，其中一个运行于客户端，称为ClientSocket，另一个运行于服务器端，称为ServerSocket。套接字之间的连接过程分为三个步骤：服务器监听，客户端请求，连接确认。

Socket可以支持不同的传输层协议（TCP或UDP），当使用TCP协议进行连接时，该Socket连接就是一个TCP连接,UDP连接同理。

![]({{ '/source/TCP、IP、Http、Socket以及UDP/03.png'}})


socket使用的库函数

1. 创建套接字

``` bash

Socket(af,type,protocol)//建立地址和套接字的联系
bind(sockid, local addr, addrlen)//服务器端侦听客户端的请求
listen( Sockid ,quenlen)//建立服务器/客户端的连接 (面向连接TCP）

```

2. 客户端请求连接

``` bash

Connect(sockid, destaddr, addrlen)//服务器端等待从编号为Sockid的Socket上接收客户连接请求
newsockid=accept(Sockid，Clientaddr, paddrlen)//发送/接收数据

```

3. 面向连接：

``` bash

send(sockid, buff, bufflen)
recv()

```

4. 面向无连接：

``` bash

sendto(sockid,buff,…,addrlen)
recvfrom()

```

5. 释放套接字

``` bash

close(socked)

```

在iOS中以NSStream(流)来发送和接收数据,可以设置流的代理，对流状态的变化做出相应的动作(连接建立，接收到数据，连接关闭）。


`NSStream`：数据流的父类，用于定义抽象特性，例如：打开、关闭代理，NSStream继承自CFStream(CoreFoundation)。

`NSInputStream`：NSStream的子类，用于读取输入。

`NSOutputStream`：NSSTream的子类，用于写输出。
