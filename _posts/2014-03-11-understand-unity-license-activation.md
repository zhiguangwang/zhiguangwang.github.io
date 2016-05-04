---
layout: post
title: 搞懂了Unity License的激活
---

之前我一直认为Unity Pro的License是和激活时所登陆的Unity Account永久绑定的。这导致我在纠结这个问题：在大公司内的开发团队里，开发者应该用公司邮箱申请的Account进行绑定，还是用个人邮箱进行绑定呢？

前者似乎更合乎规范，但如果这个员工离职，相应的公司邮箱也就被回收，不就意味着这个License难于转移给团队其他成员继续使用了吗？如果是这样，不如用员工个人邮箱来绑定，起码离职了这个员工也能继续使用它，不浪费。

今天公司的Unity Pro的License采购下来了，带着这个疑问进行激活，却并没有发现有任何绑定意味的提示。我怀疑之前的观点有问题，于是仔细查了下资料，结果在 [Unity FAQ][1] 中发现：

>Will I assign my serial permanently to the Unity Account used during activation?
>
>No, it’s assigned to the installation of Unity, you don’t sign over ownership.

这就是说Unity License是对物理机器上的那份已安装的程序授权，和Unity Account、Email统统无关。在Unity 4.x的 [新的激活系统][2] 下，如果员工更换机器或是离职，只要记得在原机器上 Unity -> Manage License -> Return License，成功之后就可以换地方或是换人用了。

如果出现系统崩溃，或机器硬件故障等状况，无法主动Return License，怎么办呢？此时只能通过 [support@unity3d.com][3] 联系Unity公司，让他们帮你解决了。

[1]: http://unity3d.com/unity/faq
[2]: http://unity3d.com/unity/faq#section-449
[3]: mailto:support@unity3d.com
