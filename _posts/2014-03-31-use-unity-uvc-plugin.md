---
layout: post
title: 让Unity集成SVN功能？试试UVC插件
---

Unity项目中的版本管理是个痛点。Team License内建的Asset Server是个很初级的系统，不靠谱。常见做法是使用SVN（Subversion）做版本管理系统，这导致了两个蛋疼的地方：

1. Unity Editor没有内建SVN功能，因此既不能看到工程中文件的版本状态，也不能做版本管理操作。必须额外使用SVN的命令行或GUI工具，来回切换的心力成本高。

2. 使用像SVN这样的第三方版本管理系统需要打开.meta文件选项，这引入了额外的复杂度。meta文件很容易被漏提交，导致新增Asset之间缺失引用关系；更麻烦的是在改名或移动Asset时，不能直接在Unity Editor中进行，必须用外部SVN工具来做，且必须同时改名或移动对应的meta文件，一旦遗漏，再进入Unity时系统便会认为该Asset缺失meta文件，便会生成新的（内部包含了新的GUID），结果Asset已有的引用关系丢失，从而运行出错。

我想，要解决这些问题，就需要想办法在Unity Editor中集成SVN功能。既然没有内建，就可以通过编写插件的方式来扩展。为了不重新发明轮子，在Asset Store找找先。结果就发现了这个 [UVC（Unity Version Control）][1]，而且还是个免费且开源的项目噢。

## 安装

这个插件**需要Unity为最新的4.3.4f1**。安装步骤和其他Unity插件没有差别，Import即可。

插件用纯C#实现，为了提高性能，开发者并没有将源码发布在unitypackage中，而是预先编译成了dll。它并非是重新发明了一遍SVN客户端，而是需要依赖于SVN命令行程序，也就是说它负责生成命令行调用，以及parse命令行的标准输出。

在Mac下，需要进入UVC的Settings界面，配置svn可执行程序所在的目录到Environment Path中。如果是用MacPorts安装的SVN，则路径为

    /opt/local/bin/

在Windows下，需要确保svn.exe所在的目录被加入到了PATH环境变量中。如果你使用的是TortoiseSVN且安装了Command-Line Tools，那么无需配置就能用。否则就需要鼓捣一下了。

## 功能

UVC解决了Unity项目使用SVN的核心痛点。它不仅包含了基本的CRUD功能，还使得改名和移动文件的操作可以在Unity Editor中直接进行，相应的SVN行为会自动完成。更为重要的是，它抽象掉了meta文件概念，使得meta文件时刻和主文件保持状态一致，无需额外操心。为了避免无意修改文件，默认锁定了Inspector的GUI，必须先对Asset做Open（实际是svn lock）或Open Local（实际是svn changelist）操作，才能进行编辑。

当然它还有一些不足之处。例如：缺少show log这样的重要功能，也无法diff文件，这些都需要切换到外部的SVN客户端来完成。Open Local没有对应的Close Local（Unlock只对应Open操作）。不过，在日常使用中，80%的功能它都包含了，且解决得很好。

详细的README可到 [项目源码页面][2] 阅读。

## 碰到的坑

在Mac下使用时发现Log message写中文会导致提交失败，错误信息示例如下：

    svn: E000022: Commit failed (details follow):
    svn: E000022: Error normalizing log message to internal format
    svn: E000022: Can't convert string from native encoding to 'UTF-8':
    svn: E000022: uvc ?\230?\143?\146?\228?\187?\182?\228?\184?\173?\230?\150?\135?\230?\179?\168?\233?\135?\138?\230?\181?\139?\232?\175?\149

又是常见的编码问题。解决方案是编辑SVN的配置文件

    ~/.subversion/config

将默认的

    # log-encoding = latin1

修改为

    log-encoding = UTF-8


[1]: https://www.assetstore.unity3d.com/#/content/3350
[2]: https://bitbucket.org/Kjems/uversioncontrol