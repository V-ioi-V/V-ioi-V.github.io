---
title: cookie和session
tags: 计算机网络原理
category: CS大学生必备
date: 2020/03/24 15:34
---

<font size=4>

1、很久很久以前，Web 基本上就是文档的浏览而已， 既然是浏览，作为服务器， 不需要记录谁在某一段时间里都浏览了什么文档，每次请求都是一个新的HTTP协议， 就是请求加响应， 尤其是我不用记住是谁刚刚发了HTTP请求， 每个请求对我来说都是全新的。

2、但是随着交互式Web应用的兴起，像在线购物网站，需要登录的网站等等，马上就面临一个问题，那就是要管理会话，必须记住哪些人登录系统， 哪些人往自己的购物车中放商品， 也就是说我必须把每个人区分开，这就是一个不小的挑战，因为HTTP请求是无状态的，所以想出的办法就是给大家发一个会话标识(session id), 说白了就是一个随机的字串，每个人收到的都不一样， 每次大家向我发起HTTP请求的时候，把这个字符串给一并捎过来， 这样我就能区分开谁是谁了

<!--more-->

**为了解决客户端与服务端会话同步问题。**这便引出了下面几个概念：cookie、session。

## session

由于HTTP协议是**无状态的协议**，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这个机制就是Session。

典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。

这个Session是保存在服务端的，有一个唯一标识。在服务端保存Session的方法很多，内存、数据库、文件都有。集群的时候也要考虑Session的转移，在大型的网站，一般会有专门的Session服务器集群，用来保存用户会话，这个时候 Session信息都是放在**内存**的，使用一些缓存服务来放 Session。

## cookie

cookie就是服务器暂存放在你计算机上的一笔资料，好让服务器用来辨认你的计算机。当你在浏览网站的时候，Web服务器会先送一小小资料放在你的计算机上，cookie 会帮你在网站上所打的文字或是一些选择，都记录下来。当下次你再光临同一个网站，Web服务器会先看看有没有它上次留下的cookie资料，有的话，就会依据cookie里的内容来判断使用者，送出特定的网页内容给你。cookie的使用很普遍，许多提供个人化服务的网站，都是利用Cookie来辨认使用者，以方便送出使用者量身定做的内容，像是Web接口的免费e-mail网站，都要用到 cookie。

**思考一下服务端如何识别特定的客户？这个时候Cookie就登场了。**

 服务器第一次接收到请求时，开辟了一块Session空间（创建了Session对象），同时生成一个Session id，并通过响应头的Set-Cookie：“SESSIONID=XXXXXXX”命令，向客户端发送要求设置cookie的响应； 客户端收到响应后，在本机客户端设置了一个SESSIONID=XXXXXXX的cookie信息，该cookie的过期时间为浏览器会话结束；

接下来客户端每次向同一个网站发送请求时，请求头都会带上该cookie信息（包含Session id）； 然后，服务器通过读取请求头中的Cookie信息，获取名称为JSESSIONID的值，得到此次请求的Session id；

 注意：服务器只会在客户端第一次请求响应的时候，在响应头上添加Set-Cookie：“SESSIONID=XXXXXXX”信息，接下来在同一个会话的第二第三次响应头里，是不会添加Set- Cookie：“SESSIONID=XXXXXXX”信息的； 而客户端是会在每次请求头的cookie中带上SESSIONID信息；

Cookie其实还可以用在一些方便用户的场景下，设想你某次登陆过一个网站，下次登录的时候不想再次输入账号了，怎么办？这个信息可以写到Cookie里面，访问网站的时候，网站页面的脚本可以读取这个信息，就自动帮你把用户名给填了。

## 二者的区别
Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；
Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。

1. Session是存在服务器端的;而Cookie是存在客户端的
2. Session更不需要Cookie来支持和不会受浏览器端的设置影响，可记录每个访问者的信息，独立在服务器端，比Cookie安全
3. Session是存在内存中的，浏览器关闭它也就“死”了；Cookie是以文件方式存在的，可以修改其“存活”时间。

链接：https://www.zhihu.com/question/19786827/answer/28752144