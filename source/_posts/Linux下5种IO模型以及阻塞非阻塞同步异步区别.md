---
title: Linux下5种IO模型以及阻塞/非阻塞/同步/异步区别
tags: [计算机网络原理,操作系统]
category: CS大学生必备
date: 2020/03/19
---



# 1.引言

​         同步（synchronous） I/O和异步（asynchronous） I/O，阻塞（blocking） I/O和非阻塞（non-blocking）I/O分别是什么，到底有什么区别？这个问题其实不同的人给出的答案都可能不同，比如wiki就认为asynchronous I/O和non-blocking I/O是一个东西。这其实是因为不同的人的知识背景不同，并且在讨论这个问题的时候上下文(context)也不相同。所以，为了更好的回答这个问题，我先限定一下本文的上下文。

​        本文讨论的背景是Linux环境下的network I/O。

​        本文最重要的参考文献是Richard Stevens的“UNIX® Network Programming Volume 1, Third Edition: The Sockets Networking ”，6.2节“I/O Models ”，Stevens在这节中详细说明了各种I/O的特点和区别。Stevens的文风是有名的深入浅出，所以不用担心看不懂。本文中的流程图也是截取自参考文献。

# 2.Linux下的五种I/O模型

- 阻塞I/O（blocking I/O）
- 非阻塞I/O （nonblocking I/O）
- I/O复用(select 和poll) （I/O multiplexing）
- 信号驱动I/O （signal driven I/O (SIGIO)）
- 异步I/O （asynchronous I/O (the POSIX aio_functions)） 

Tip：前四种都是同步，只有最后一种才是异步I/O。

## 2.1 I/O发生时涉及的对象和阶段
对于一个network I/O (这里我们以read举例)，它会涉及到两个系统对象，一个是调用这个I/O的process (or thread)，另一个就是系统内核(kernel)。当一个read操作发生时，它会经历两个阶段：

-  1 等待数据准备 (Waiting for the data to be ready)

-  2 将数据从内核拷贝到进程中 (Copying the data from the kernel to the process)

记住这两点很重要，因为这些I/O Model的区别就是在两个阶段上各有不同的情况。

## 2.2 阻塞I/O模型(blocking I/O) 
<font color="red">在linux中，默认情况下所有的socket都是blocking</font>，一个典型的读操作流程大概是这样：



![1.jpg](https://i.loli.net/2020/03/19/oICR2gEYXu6mdTS.jpg)

​       当用户进程调用了recvfrom(**接受服务器的数据**)这个系统调用，kernel（内核）就开始了I/O的第一个阶段：准备数据。对于network io来说，很多时候数据在一开始还没有到达（比如，还没有收到一个完整的UDP包），这个时候kernel就要等待足够的数据到来。而在用户进程这边，整个进程会被阻塞。当kernel一直等到数据准备好了，它就会将数据从kernel系统缓冲区中拷贝到用户内存，然后kernel返回结果，用户进程才解除block的状态，重新运行起来。所以，<font color="red">blocking IO的特点就是在I/O执行的两个阶段都被block了。</font>

当使用socket()函数和WSASocket()函数创建套接字时，默认的套接字都是阻塞的。这意味着当调用Windows Sockets API不能立即完成时，线程处于等待状态，直到操作完成。

并不是所有Windows Sockets API以阻塞套接字为参数调用都会发生阻塞。例如，以阻塞模式的套接字为参数调用bind()、listen()函数时，函数会立即返回。将可能阻塞套接字的Windows Sockets API调用分为以下四种:

- 输入操作： recv()、recvfrom()、WSARecv()和WSARecvfrom()函数。以阻塞套接字为参数调用该函数接收数据。如果此时套接字缓冲区内没有数据可读，则调用线程在数据到来前一直睡眠。
- 输出操作： send()、sendto()、WSASend()和WSASendto()函数。以阻塞套接字为参数调用该函数发送数据。如果套接字缓冲区没有可用空间，线程会一直睡眠，直到有空间。
- 接受连接：accept()和WSAAcept()函数。以阻塞套接字为参数调用该函数，等待接受对方的连接请求。如果此时没有连接请求，线程就会进入睡眠状态。
- 外出连接：connect()和WSAConnect()函数。对于TCP连接，客户端以阻塞套接字为参数，调用该函数向服务器发起连接。该函数在收到服务器的应答前，不会返回。这意味着TCP连接总会等待至少到服务器的一次往返时间。

使用阻塞模式的套接字，开发网络程序比较简单，容易实现。当希望能够立即发送和接收数据，且处理的套接字数量比较少的情况下，使用阻塞模式来开发网络程序比较合适。

阻塞模式套接字的不足表现为，在大量建立好的套接字线程之间进行通信时比较困难。当使用“生产者-消费者”模型开发网络程序时，为每个套接字都分别分配一个读线程、一个处理数据线程和一个用于同步的事件，那么这样无疑加大系统的开销。其最大的缺点是当希望同时处理大量套接字时，将无从下手，其扩展性很差。

 

## 2.3 非阻塞I/O模型（non-blocking IO）
linux下，可以通过设置socket使其变为non-blocking。当对一个non-blocking socket执行读操作时，流程是这个样子：

![2.jpg](https://i.loli.net/2020/03/19/efuBkNrtwJCbUh9.jpg)

从图中可以看出，当用户进程发出read操作时，如果kernel中的数据还没有准备好，那么它并不会block用户进程，而是立刻返回一个error。从用户进程角度讲 ，它发起一个read操作后，并不需要等待，而是马上就得到了一个结果。用户进程判断结果是一个error时，它就知道数据还没有准备好，于是它可以再次发送read操作。一旦kernel中的数据准备好了，并且又再次收到了用户进程的system call，那么它马上就将数据拷贝到了用户内存，然后返回。所以，<font color="red">用户进程其实是需要不断的主动询问kernel数据好了没有。</font>

我们把一个SOCKET接口设置为非阻塞就是告诉内核，当所请求的I/O操作无法完成时，不要将进程睡眠，而是返回一个错误。这样我们的I/O操作函数将不断的测试数据是否已经准备好，如果没有准备好，继续测试，直到数据准备好为止。在这个不断测试的过程中，会大量的占用CPU的时间。

当使用socket()函数和WSASocket()函数创建套接字时，默认都是阻塞的。在创建套接字之后，通过调用ioctlsocket()函数，将该套接字设置为非阻塞模式。Linux下的函数是:fcntl()。.

套接字设置为非阻塞模式后，在调用Windows Sockets API函数时，调用函数会立即返回。大多数情况下，这些函数调用都会调用“失败”，并返回WSAEWOULDBLOCK错误代码。说明请求的操作在调用期间内没有时间完成。通常，应用程序需要重复调用该函数，直到获得成功返回代码。

需要说明的是并非所有的Windows Sockets API在非阻塞模式下调用，都会返回WSAEWOULDBLOCK错误。例如，以非阻塞模式的套接字为参数调用bind()函数时，就不会返回该错误代码。当然，在调用WSAStartup()函数时更不会返回该错误代码，因为该函数是应用程序第一调用的函数，当然不会返回这样的错误代码。

要将套接字设置为非阻塞模式，除了使用ioctlsocket()函数之外，还可以使用WSAAsyncselect()和WSAEventselect()函数。当调用该函数时，套接字会自动地设置为非阻塞方式。

由于使用非阻塞套接字在调用函数时，会经常返回WSAEWOULDBLOCK错误。所以在任何时候，都应仔细检查返回代码并作好对“失败”的准备。应用程序连续不断地调用这个函数，直到它返回成功指示为止。上面的程序清单中，在While循环体内不断地调用recv()函数，以读入1024个字节的数据。这种做法很浪费系统资源。

 

## 2.4 I/O复用模型（I/O multiplexing）
I/O multiplexing这个词可能有点陌生，但是如果我说select，poll、epoll，大概就都能明白了。有些地方也称这种I/O方式为event driven I/O，也是实际中使用最多的一种I/O模型。我们都知道，select/epoll的好处就在于单个process就可以同时处理多个网络连接的I/O。它的基本原理就是select/epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。它的流程如图：

![3.jpg](https://i.loli.net/2020/03/19/KSbgFmGp5Vuzl9Z.jpg)

当用户进程调用了select，那么整个进程会被block，而同时，kernel会“监视”所有select负责的socket，当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从kernel拷贝到用户进程。

这个图和blocking I/O的图其实并没有太大的不同，事实上，还更差一些。因为这里需要使用两个system call (select 和 recvfrom)，而blocking I/O只调用了一个system call (recvfrom)。但是，用select的优势在于它可以同时处理多个connection。（多说一句。所以，如果处理的连接数不是很高的话，使用select/epoll的web server不一定比使用multi-threading + blocking I/O的web server性能更好，可能延迟还更大。**select/epoll的优势并不是对于单个连接能处理得更快，而是在于能处理更多的连接。**）

在I/O multiplexing Model中，实际中，对于每一个socket，一般都设置成为non-blocking，但是，如上图所示，整个用户的process其实是一直被block的。只不过process是被select这个函数block，而不是被socket I/O给block。

 

## 2.5 信号驱动I/O模型（Signal-driven I/O）
首先我们允许套接口进行信号驱动I/O,并安装一个信号处理函数，进程继续运行并不阻塞。当数据准备好时，进程会收到一个SIGIO信号，可以在信号处理函数中调用I/O操作函数处理数据。(signal driven I/O在实际中并不常用)



![4.jpg](https://i.loli.net/2020/03/19/m3DFIPx6afpQYwX.jpg) 

## 2.6 异步I/O模型（Asynchronous I/O）
linux下的asynchronous IO其实用得很少。先看一下它的流程：

![5.jpg](https://i.loli.net/2020/03/19/kSUzWwI49M8ErfF.jpg)

用户进程发起read操作之后，立刻就可以开始去做其它的事。而另一方面，从kernel的角度，当它受到一个asynchronous read之后，首先它会立刻返回，所以不会对用户进程产生任何block。然后，kernel会等待数据准备完成，然后将数据拷贝到用户内存，当这一切都完成之后，kernel会给用户进程发送一个signal，告诉它read操作完成了。

## 2.7 5种I/O模型比较

![6.jpg](https://i.loli.net/2020/03/19/agyWxDRcri8lmNb.jpg)

经过上面的介绍，会发现non-blocking I/O和asynchronous I/O的区别还是很明显的。在non-blocking I/O中，虽然进程大部分时间都不会被block，但是它仍然要求进程去主动的check，并且当数据准备完成以后，也需要进程主动的再次调用recvfrom来将数据拷贝到用户内存。而asynchronous I/O则完全不同。它就像是用户进程将整个I/O操作交给了他人（kernel）完成，然后他人做完后发信号通知。在此期间，用户进程不需要去检查I/O操作的状态，也不需要主动的去拷贝数据。

 # 3.结语
到目前为止，已经将四个IO Model都介绍完了。现在回过头来回答最初的那几个问题：

- blocking和non-blocking的区别在哪？？

- synchronous IO和asynchronous IO的区别在哪？？

先回答最简单的这个：blocking VS non-blocking。前面的介绍中其实已经很明确的说明了这两者的区别。调用blocking IO会一直block住对应的进程直到操作完成，而non-blocking IO在kernel还准备数据的情况下会立刻返回。

再说明synchronous IO和asynchronous IO的区别之前，需要先给出两者的定义。Stevens给出的定义（其实是POSIX的定义）是这样子的：

A synchronous I/O operation causes the requesting process to be blocked until that I/O operation completes;
An asynchronous I/O operation does not cause the requesting process to be blocked; 

两者的区别就在于synchronous I/O做”I/O operation”的时候会将process阻塞。按照这个定义，之前所述的blocking I/O，non-blocking I/O，I/O multiplexing，Signal-driven I/O都属于synchronous I/O。有人可能会说，non-blocking I/O并没有被block啊。这里有个非常“狡猾”的地方，定义中所指的”I/O operation”是指真实的I/O操作，就是例子中的recvfrom这个system call。non-blocking I/O在执行recvfrom这个system call的时候，如果kernel的数据没有准备好，这时候不会block进程。但是，当kernel中数据准备好的时候，recvfrom会将数据从kernel拷贝到用户内存中，这个时候进程是被block了，在这段时间内，进程是被block的。而asynchronous I/O则不一样，当进程发起I/O 操作之后，就直接返回再也不理睬了，直到kernel发送一个信号，告诉进程说I/O完成。在这整个过程中，进程完全没有被block。



原文链接：https://blog.csdn.net/yxtxiaotian/article/details/84068839