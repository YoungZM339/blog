---
title: "UNIX 介绍 | 由YoungZM翻译 | 源自 UNIX Introduction"
date: 2021-07-20T11:23:25+08:00
draft: false
---

## 译者前言
UNIX是什么？这个问题一直令我困惑。近期在[阮一峰的网络日志](https://www.ruanyifeng.com/blog/archives.html)的推荐中找到[University of Surrey](https://www.surrey.ac.uk/)的[UNIX Introduction](http://www.ee.surrey.ac.uk/Teaching/Unix/unixintro.html)，现借助Google Translate在本人的修正与优化后做出本译本，供大家参考。

为方便大家阅读本文，本文将使用
_（斜体加括号）_
的方式对部分概念增加一定的注释，如对您的阅读产生不适，敬请见谅。

请注意：对于本译本，译者[YoungZM@lavacup2233](https://blog.youngzm.com/)保留翻译权利，其余权利归作者[University of Surrey](https://www.surrey.ac.uk/)所有。
<!--more-->

## UNIX 介绍

### 什么是 UNIX？

UNIX 是最早于 1960 年代开发的操作系统，此后一直在不断发展。 操作系统是指使计算机工作的程序套件。 它是一个稳定的、多用户、多任务系统，适用于服务器、台式机和笔记本电脑。

UNIX 系统也具有类似于 Microsoft Windows 的可提供易于使用的环境的图形用户界面 (GUI)。 但是，对于没有包含图形程序的操作，或者当没有可用的窗口界面（例如，在 telnet 会话中）时，需要 UNIX 的知识。

### UNIX 的类型

UNIX 有许多不同的版本，尽管它们有共同的相似之处。 最受欢迎的 UNIX 变体是 Sun Solaris、GNU/Linux 和 MacOS X。

在学校，我们在服务器和工作站上使用 Solaris，在服务器和台式机上使用 Fedora Linux。

### UNIX 操作系统

UNIX 操作系统由三部分组成； **kernel**, **shell** 和**programs**。

#### kernel
UNIX 的内核是操作系统的中心：它为程序分配时间和内存，并处理文件存储和通信以响应系统调用。

作为 shell 和内核协同工作方式的说明，假设用户键入 `rm myfile`（它具有删除文件 `myfile` 的效果）。 shell 在文件存储中搜索包含程序 `rm` 的文件，然后通过系统调用请求内核在 `myfile` 上执行程序 `rm`。当进程`rm myfile` 运行完毕后，shell 将 UNIX 提示符 `%` 返回给用户，表明它正在等待进一步的命令。

#### shell
shell 充当用户和内核之间的接口。当用户登录时，登录程序会检查用户名和密码，然后启动另一个名为 shell 的程序。 shell 是一个命令行解释器 (CLI)。它解释用户输入的命令并安排它们执行。命令本身就是程序：当它们终止时，shell 会给用户另一个提示（在我们的系统上为 `%`）。

熟练的用户可以自定义自己的shell，用户可以在同一台机器上使用不同的shell。

shell 具有帮助用户输入命令的某些功能。

**文件名补全(Filename Completion)** - 通过键入命令、文件名或目录的部分名称并按 [Tab] 键，shell 将自动补全名称的其余部分。如果 shell 找到多个以您输入的字母开头的名称，它会发出哔哔声，提示您在再次按下 Tab 键之前​​再输入几个字母。

**历史(History)** - shell 会保存您输入的命令的列表。如果您需要重复一个命令，请使用光标键上下
_(即↑↓)_
滚动列表（）或键入 history 以获取先前命令的列表。

### 文件和进程

UNIX 中的一切都是文件或进程。

进程是由唯一 PID（进程标识符）标识的正在执行的程序。

文件是数据的集合。 它们是由用户使用文本编辑器、运行编译器等创建的。

文件示例：

- 文件（报告、论文等）
- 用某种高级编程语言编写的程序文本
- 机器可以直接理解而普通用户无法理解的指令，例如，一组二进制数字（可执行文件或二进制文件）；
- 一个目录，包含有关其内容的信息，它可能是其他目录（子目录）和普通文件的混合。

### 目录结构

所有文件都在目录结构中组合在一起。 文件系统以分层结构排列，就像一棵倒置的树。 层次结构的顶部传统上称为根（写为斜杠 / ）

![unix-tree](https://blog.youngzm.com/imgs/unix-tree.png)

在上图中，我们看到本科生“`ee51vn`”的主目录包含两个子目录（docs 和 pics）和一个名为 `report.doc` 的文件。

文件 `report.doc` 的完整路径是`/home/its/ug1/ee51vn/report.doc` 。

### 启动 UNIX 终端

要打开 UNIX 终端窗口，请单击应用程序/附件菜单中的“Terminal”
_(终端)_
图标。

![侏儒菜单](https://blog.youngzm.com/imgs/gnome-window.gif)

然后将出现一个带有 `%` 提示的 UNIX 终端窗口，等待您开始输入命令。

![Unix 终端窗口](https://blog.youngzm.com/imgs/unix-xterm0.gif)