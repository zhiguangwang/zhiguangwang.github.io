---
layout: post
title: 为什么你应该使用VPN
---

一段时间之前，我在 [AWS][1] 的东京机房上用 [PPTP][2] 搭建了一个VPN服务，供团队成员使用。每个入职的同事，都会得到一个账号。有的同事之前并没有用过VPN，就会问我这玩意是干嘛用的。由于 [每个人一生敲键盘的次数是有限的][3]，我想应该就这个问题写一篇blog了。

## 什么是VPN？

VPN即 [Virtual Private Network][4]，即多台计算机在公共互联网上相互建立网络连接，构成一个虚拟的局域网。

但如果从使用角度做一个简化，就是在你的上网设备C（PC、笔记本、智能手机或平板等等）和某个服务器A之间建立了一个网络连接，之后C的所有网络流量都先通过此连接转发给A，这样一来，对于你真正在访问的的服务器B看来，客户端就是服务器A，而不知道你的设备才是“幕后黑手”。

## 为什么要使用VPN？

其实 [这篇文章][5] 已经提供了详尽的解释，但我想针对日常使用做一个更易于理解的说明：

### 一、翻墙（GFW）

作为一个大陆的互联网用户，GFW（[Great FireWall][6]）是一个绕不过去的存在，虽然政府并不公开承认它。当你访问 [Facebook][7]（世界上最大的社交网络）、[Twitter][8]（世界上第二大的社交网络）、[Youtube][9]（世界上最大的在线视频服务）、[Blogger][10]（世界上最大的Blog托管服务）而发现“此页无法显示”；或是使用Google搜索时发现页面空白十几秒后才响应；或是当你搜索到了所需的资料后点击链接却打不开，99%的情况下都是GFW惹的祸。

VPN在传输网络数据时通常会将内容加密，这使得GFW无法获知你在访问什么服务，浏览什么内容。由于GFW判定是否阻断你的网络连接是依靠域名/IP黑名单检测、敏感词匹配等手段，此时在它看来，你只不过是向某个IP地址发送了一堆不知所云的任意数据。由于检测结果呈“阴性”，它只好采取默认行为，也就是“放行”。于是互联网变得畅通无阻，就好像呼吸PM2.5 < 10的空气一样。

### 二、更佳的个人网络安全

在用浏览器访问网络时，大部分的网站使用 [HTTP协议][11]，但它是明文传输的。另外有一些应用软件的私有协议，也使用了明文传输。这使得一些心怀叵测的人截获并查看你的上网内容成为可能。由于前述的VPN加密行为，不仅躲过GFW，也同时躲过了这些恶意行为。在不可信的网络环境下（例如公共场所的WiFi），上网前先连接到VPN，是一个良好的安全习惯。

### 三、使用某些区域受限的互联网服务

世界范围内的电商平台，例如 [Amazon][12]、[Kindle Store][13]、[iTunes Store][14]，都是按国家来分区提供服务的。由于某些原因，在中国区提供的内容相对来说是非常少的。例如iTunes Store在中国区不提供音乐、电影、电视剧的板块，书籍仅提供极少量过了版权保护期的名著类，App Store中的应用也偏少（当然这个是是开发者的行为了）。

如果你想要使用这些内容，得到相应的服务，就需要在这些平台注册非中国区的账号（大部分情况下是美国区）。而这些平台基本都会验证注册者的IP地址是否来自于该区域，于是你需要通过VPN来“伪装”，这样才能通过验证。

## 如何开始使用VPN服务？

### 一、购买现成的

由于GFW的存在导致了刚性需求，出现了很多提供收费VPN的服务商，例如 [NydusVPN][15]。我以前用过类似服务，优点就是省心，付钱就能开始用，价格公道，速度也比较有保障。缺点是由于使用人数多、网络流量大，域名或服务器IP容易被GFW盯上，结果就是加入到被墙的黑名单。因此每过一段时间服务商可能就会被迫修改服务地址，对用户来说这有点儿蛋疼。

### 二、自己动手，丰衣足食

动手能力强且有意愿的适合这种方式。

在VPS（[Virtual Private Server][16]）上搭建，例如这篇 [Linode的教程][17]。好处是性能有保障，开销明确易懂
在AWS的 [EC2][18] 上搭建，例如 [这篇教程][19]。好处是可以按使用情况付费，且新用户可以免费使用一年t1.micro实例，这对于运行VPN服务绰绰有余了。
推荐后者。

### 三、投奔一个能自己动手、丰衣足食的哥们

不解释。

[1]: http://aws.amazon.com/
[2]: http://en.wikipedia.org/wiki/Pptp
[3]: http://www.hanselman.com/blog/DoTheyDeserveTheGiftOfYourKeystrokes.aspx
[4]: http://zh.wikipedia.org/zh/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF
[5]: http://lifehacker.com/5940565/why-you-should-start-using-a-vpn-and-how-to-choose-the-best-one-for-your-needs
[6]: http://zh.wikipedia.org/wiki/%E9%98%B2%E7%81%AB%E9%95%BF%E5%9F%8E
[7]: https://www.facebook.com/
[8]: https://twitter.com/
[9]: http://www.youtube.com/
[10]: https://www.blogger.com/
[11]: http://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE
[12]: http://www.amazon.com/
[13]: http://www.amazon.com/Kindle-eBooks/b?ie=UTF8&node=154606011
[14]: http://www.apple.com/itunes/features/#store
[15]: http://www.nydus.biz/
[16]: http://en.wikipedia.org/wiki/Virtual_private_server
[17]: http://www.vpser.net/manage/linode-vps-pptp-vpn-howto.html
[18]: http://aws.amazon.com/ec2/
[19]: http://www.cnblogs.com/junqilian/archive/2013/01/09/2853224.html
