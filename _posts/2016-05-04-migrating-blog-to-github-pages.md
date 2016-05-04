---
layout: post
title: 将Blog迁移到Github Pages
---

本站最早是放在 [Linode][1] 上，因为价格便宜量又足。后来工作中开始使用 [AWS][2]，出于对它的喜爱，迁移到了EC2上。两者都是用的 Wordpress + Apache 来建站，后来甚至弄了 [自己的SSL证书][3]。

自己建站固然好处多多，拥有完全的控制权，但需要维护的东西也多，不仅要维护虚拟机（例如我就从AWS收到过EC2将要retire的邮件，需要在若干天内将网站迁移），还要考虑数据的备份。

再有就是Wordpress的写作体验一直不能令我满意，不能很好地支持Markdown，整个内容管理系统也太过臃肿，写作过程经常分心。

五一假期仔细研究了一下 [Github Pages][4]，发现它完全能满足我对写作的期望：

- 支持用 [Markdown][5] 写作，更干净更专注于内容本身
- 使用Git做版本管理，无需费心考虑数据备份问题，也方便版本随时回滚
- 支持从Git仓库自动编译并部署整个网站，Add/Commit/Push即可，几乎没有维护成本
- 能够自己完全定制网站的几乎所有细节
- 可以通过CNAME来使用自定义域名
- 完全免费

花了一天的时间搭建环境、学习相关的技术，并将旧文章迁移了过来：

- 静态网站生成采用 [Jekyll][6]
- 显示主题基于 [Lanyon][7]，做了一些修改
- 评论系统使用 [Disqus][8]
- 网站源码放在 [Github][9]。

Github Pages固然好用，但也有它的局限：

- 自动编译与部署不支持非Ruby的静态网站生成工具，例如 [Hexo][10]，这种只能自己编译，还要维护两个分支，比较繁琐
- 自定义域名目前不支持上传SSL证书，因而无法支持https

这些问题 [Gitlab Pages][11] 都解决了，希望Github在未来也能迎头赶上。

[1]: https://www.linode.com
[2]: https://aws.amazon.com
[3]: /2016/03/21/lets-encrypt/
[4]: https://pages.github.com
[5]: https://daringfireball.net/projects/markdown/
[6]: http://jekyllrb.com/
[7]: http://jekyllthemes.io/theme/15489219/lanyon
[8]: https://disqus.com/
[9]: https://github.com/zhiguangwang/zhiguangwang.github.io
[10]: https://hexo.io/
[11]: https://pages.gitlab.io/
