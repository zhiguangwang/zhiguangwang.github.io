---
layout: post
title: 使用Lantern翻墙
---

一直都在用自建的VPN翻墙。从年初开始不仅Gmail全面被墙，连VPN也时不时地被干扰，工作也受到影响，真是有够烦的。

前几天在Google对比各种软件VPN的方案，无意间读到 [这篇访谈][1]，从而发现了这个翻墙利器：[Lantern][2]。

Lantern是一个是基于 [P2P][3] 的HTTP Proxy。它由非盈利组织 [Brave New Software][4] 开发的桌面应用，专门设计用来供人们突破对互联网审查与封锁（Censorship）。

## Lantern vs VPN

和VPN相比，两者有以下区别：

- Lantern的使用极其简单，是It just works理念的典范。只需下载客户端并运行即可，全程不超过30秒。而VPN的配置对于普通大众来说还是有一定复杂度的。
- Lantern使用起来更稳定。这不仅因为它基于P2P技术，还因为它在设计时就不像VPN那样有明显的特征，数据经过Lantern的代理结点时，对于墙来说就像是普通的SSL流量，这使得极难被封锁。
- VPN会修改操作系统的路由表，因而可以转发本机所有流量（所有网络协议），而Lantern是一个HTTP Proxy，只会影响浏览器中的流量。
- Lantern利用 [Proxy auto-config][5] 机制，能够动态地做到到只代理被墙的站点（也可以设置为Proxy all traffic），对于本机可访问的站点（例如百度）采取直连的策略，从而达到整体上更快的访问速度，这一点比VPN更有优势。
- 对于被访问的互联网服务来说，使用VPN时源IP为VPN服务器的IP（固定），使用Lantern时源IP为墙外的某个Proxy Node的IP（不固定）。
- Lantern目前仅能用于桌面设备（Windows、Mac OS、Linux），VPN在Mobile设备（iOS、Android）上也有很好的支持。

如果翻墙时的需求仅是为了在浏览器中访问Web站点，那么Lantern堪称最佳工具，没有之一。

## 获取Lantern

如你所料，Lantern的 [官方网站][6] 是被墙了的。客户端安装文件放在AWS S3上，利用 [Collateral Freedom][7] 曾一度免于被墙，结果今年开始GFW直接把S3这种互联网基础设施级别的服务也完全封锁了，真是丧心病狂。

好在Lantern开发团队把编译出的客户端同步发布在GitHub上了，点击链接选择相应的桌面平台即可下载：

<p class="message">
<a href="https://github.com/getlantern/lantern/releases/tag/latest" target="_blank">https://github.com/getlantern/lantern/releases/tag/latest</a>
</p>

## Lantern相关的资源整理

- [Official Website][2]
- [GitHub Project][9]
- [Adam Fisk on Evading Internet Censorship in China (The New York Times)][1]
- [Lantern: internet freedom by design: Adam Fisk at TEDxHoboken (YouTube)][10]
- [Adam Fisk: “Lantern” \| Talks at Google (YouTube)][11]
- [Lantern and WebRTC: An Interview With Adam Fisk][12]

[1]: http://sinosphere.blogs.nytimes.com/2015/03/30/q-and-a-adam-fisk-on-evading-internet-censorship-in-china/
[2]: https://getlantern.org/
[3]: https://en.wikipedia.org/wiki/Peer-to-peer
[4]: http://bravenewsoftware.org/
[5]: https://en.wikipedia.org/wiki/Proxy_auto-config
[6]: https://getlantern.org/
[7]: https://en.greatfire.org/blog/2014/jan/collateral-freedom-faq
[8]: https://github.com/getlantern/lantern/releases/tag/latest
[9]: https://github.com/getlantern/lantern/
[10]: https://www.youtube.com/watch?v=PpwSJDdo6Vs
[11]: https://www.youtube.com/watch?v=a5F70OjAov8
[12]: https://bloggeek.me/lantern-webrtc-interview/
