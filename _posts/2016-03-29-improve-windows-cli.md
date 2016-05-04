---
layout: post
title: 改善Windows命令行的表现
---

用惯了Mac或Linux的人恐怕都会对Windows原生的命令行工具（cmd.exe）大为不满：难看的字体（Raster Fonts），极窄的窗口宽度（80个字符），屏幕缓冲区过小，输出一多就刷没了。

其实经过一点简单的设置，就能够极大改善命令行的表现了，虽达不到*nix系统，起码能够忍受。进行下面设置时使用的系统是 **Windows 7 Ultimate 64bit 英文版**。

## 修改默认codepage

在非Unicode字符默认为中文的Windows系统下，默认的codepage为GBK (936)，想要修改命令行的显示字体，需要先将其codepage修改为为UTF-8 (65001)：

    chcp 65001

我们可以通过修改注册表将这一过程自动化：

1. `Start -> Run -> regedit.exe`
2. 展开 `HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor`
3. 新建 `String Value (REG_SZ)`，名称为 `Autorun`，值为 `chcp 65001`

## 设置命令行表现

打开 `Start -> Run -> cmd.exe -> 窗口标题上右键 -> 属性（Properties）`

在选项（Options）tab下，勾上 `快速编辑模式（QuickEdit Mode）`，这样可以选中屏幕输出、右键复制/粘贴、用鼠标滚轮对输出进行翻页。

在字体（Font）tab下，字体选 `Consolas`，字号 **18**。

在布局（Layout）tab下，设置窗口和屏幕缓冲区大小。在1080p的显示器下，我发现如下设置比较合适：

- 屏幕缓冲区：宽度 **230**，高度 **3000**
- 窗口大小：宽度 **230**，高度 **50**

除了cmd.exe外，可能还会希望设置一下 `Start -> Run -> Command Prompt`。由于它本身是个快捷方式，直接在快捷方式的属性上就能设置表现了。

## UPDATE

实际使用时我发现，上述配置虽然能用，但并不能一劳永逸地解决问题。例如有些应用程序启动的独立Console窗口依然是默认设置。最佳的解决方法，是通过修改注册表，来改变cmd的默认配置：

1. `Start -> Run -> regedit.exe`
2. 若已按照上面第一步在注册表修改了默认的codepage，则删除相应的Autorun值
3. 删除 `HKEY_CURRENT_USER\Console`，若无此键则跳过
4. 下载 [Regedit-Console.zip][1]，解压出 `Console.reg` 并导入注册表即可

此配置的设置和上面手动设置完全相同。如果想要恢复命令行到原始配置，则打开注册表，删除 `HKEY_CURRENT_USER\Console` 即可。

[1]: /download/Regedit-Console.zip

