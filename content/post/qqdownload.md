---
title: "如何加速下载QQ群文件里的大文件？"
date: 2018-07-31T19:32:18+08:00
draft: false
---
<h2>引言</h2>
<p>众所周知，用QQ客户端下载QQ群文件的速度实在是缓慢，这里讲一下如何利用多线程快速下载腾讯QQ群文件。</p>
<!--more-->
<p>原始事件发生在2018年8月4日，笔者的同学制作了一个毕业短片并且将其上传到了班级的QQ群文件里面 ，群主十分激动。文件体积虽然只有200MB，但是下载速度真的是很慢，令笔者非常难受，于是就有了这篇文章。</p>
<p><em>关于多线程如何快速下载</em>群文件的文章其实我已经写过了，但是在网站数据迁移的过程中<s>不小心</s>把备份文件全部删除了。<s>虽然我有经常备份的习惯</s>，但还是只恢复了一部分，所以写这篇文章简单复述一下流程</p>
<h2>思路</h2>

<p>1.获取QQ群文件的下载直链</p>
<p>2.使用现有的多线程下载工具进行多线程下载</p>
<blockquote class="wp-block-quote"><p>当然，本思路也普遍适用于提升 仅限制单个线程下载速度的下载 的下载速度</p><p>（经测试下载百度云盘文件用此方法开太多线程会导致404，不过少一些速度也很快）</p></blockquote>
<h2>实现过程</h2>

我们这里仅用Windows 10操作系统，Chrome游览器和洛谷群群文件进行演示

<blockquote class="wp-block-mdx-warning mdx-warning"><p><i class="mdui-icon material-icons"></i> 注意<br/><strong>我们这里仅用Windows 10操作系统，Chrome游览器和洛谷群群文件进行演示</strong></p></blockquote>

<p>经过一番搜索，我发现QQ群群文件是有页面版的，我们不妨登入页面：</p>
<p><a href="http://qun.qzone.qq.com/group">http://qun.qzone.qq.com/group</a></p>

<p>当然，你也可以使用链接 </p>

<p>http://qun.qzone.qq.com/group#!/群号/share </p>

<p>比如洛谷用户群 <a href="http://qun.qzone.qq.com/group#!/515055655/share">http://qun.qzone.qq.com/group#!/515055655/share</a> </p>

<p>如果你打开的是第一个链接，你将会看到</p>
<img src="https://blog.youngzm.com/wp-content/uploads/2019/12/HHOOLXDJV902UO2M-1024x270.png" alt="" class="wp-image-70"/>悬浮在标题栏“我的群”上，选择你需要得到群组，我这里选择了 Hello Luogu群作为演示

<img src="https://blog.youngzm.com/wp-content/uploads/2019/12/S5BM5G6QB939BVL4.png" alt="" class="wp-image-71"/>

进入一个新的页面后，你将会看到这样的div区块，点击第二个按钮“群文件”
<p>如果你已经点击了那个按钮或者你打开是的是第二个链接，你将会看到：</p>
<img src="https://blog.youngzm.com/wp-content/uploads/2019/12/54LGTXPY6OIW_OLO_WH.png" alt="" class="wp-image-72"/><figcaption>点击白色方块，即“下载”
<p>然后通过下载链接调用你的多线程下载工具。</p>
<h2>关于多线程下载工具</h2>
<p>关于多线程下载工具，我这里推荐</p>
<!-- /wp:paragraph -->

<ul><li>ADM （即 Advanced Download Manager ）</li><li>IDM（即Internet Download Manager）</li></ul>

<p>详细的使用方法我不在赘述，可以自行翻阅官方文档。</p>

<p>这样，你就学会了如何利用多线程快速下载腾讯QQ群文件。</p>


##本方法已经失效
原因是腾讯已经关闭了QQ群空间