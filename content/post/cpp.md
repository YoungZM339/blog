---
title: "探寻C++最快的读取文件的方案[转载]"
date: 2019-07-31T10:32:18+08:00
draft: false
---
<!-- wp:heading -->
<h2>引言</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>在竞赛中，遇到大数据时，往往读文件成了程序运行速度的瓶颈，需要更快的读取方式。相信几乎所有的C++学习者都在cin机器缓慢的速度上栽过跟头，于是从此以后发誓不用cin读数据。还有人说Pascal的read语句的速度是C/C++中scanf比不上的，C++选手只能干着急。难道C++真的低Pascal一等吗？答案是不言而喻的。一个进阶的方法是把数据一下子读进来，然后再转化字符串，这种方法传说中很不错，但具体如何从没试过，因此今天就索性把能想到的所有的读数据的方式都测试了一边，结果是惊人的。</p>
<!-- /wp:paragraph -->
<!--more-->
<!-- wp:paragraph -->
<h2>正文</h2>
<p>竞赛中读数据的情况最多的莫过于读一大堆整数了，于是我写了一个程序，生成一千万个随机数到data.txt中，一共55MB。然后我写了个程序主干计算运行时间，代码如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>#include &lt;ctime>
int main()
{
    int start = clock();
    //DO SOMETHING
    printf("%.3lf\n",double(clock()-start)/CLOCKS_PER_SEC);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>最简单的方法就算写一个循环scanf了，代码如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXN = 10000000;

int numbers[MAXN];

void scanf_read()
{
    freopen("data.txt","r",stdin);
    for (int i=0;i&lt;MAXN;i++)
        scanf("%d",&amp;numbers[i]);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>可是效率如何呢？在我的电脑Linux平台上测试结果为2.01秒。接下来是cin，代码如下</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXN = 10000000;

int numbers[MAXN];

void cin_read()
{
    freopen("data.txt","r",stdin);
    for (int i=0;i&lt;MAXN;i++)
        std::cin >> numbers[i];
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>出乎我的意料，cin仅仅用了6.38秒，比我想象的要快。cin慢是有原因的，其实默认的时候，cin与stdin总是保持同步的，也就是说这两种方法可以混用，而不必担心文件指针混乱，同时cout和stdout也一样，两者混用不会输出顺序错乱。正因为这个兼容性的特性，导致cin有许多额外的开销，如何禁用这个特性呢？只需一个语句std::ios::sync_with_stdio(false);，这样就可以取消cin于stdin的同步了。程序如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXN = 10000000;

int numbers[MAXN];

void cin_read_nosync()
{
    freopen("data.txt","r",stdin);
    std::ios::sync_with_stdio(false);
    for (int i=0;i&lt;MAXN;i++)
        std::cin >> numbers[i];
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>取消同步后效率究竟如何？经测试运行时间锐减到了2.05秒，<strong>与scanf效率相差无几了</strong>！有了这个以后可以放心使用cin和cout了。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>接下来让我们测试一下读入整个文件再处理的方法，首先要写一个字符串转化为数组的函数，代码如下</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXS = 60*1024*1024;
char buf[MAXS];

void analyse(char *buf,int len = MAXS)
{
    int i;
    numbers[i=0]=0;
    for (char *p=buf;*p &amp;&amp; p-buf&lt;len;p++)
        if (*p == ' ')
            numbers[++i]=0;
        else
            numbers[i] = numbers[i] * 10 + *p - '0';
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>把整个文件读入一个字符串最常用的方法是用fread，代码如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXN = 10000000;
const int MAXS = 60*1024*1024;

int numbers[MAXN];
char buf[MAXS];

void fread_analyse()
{
    freopen("data.txt","rb",stdin);
    int len = fread(buf,1,MAXS,stdin);
    buf[len] = '\0';
    analyse(buf,len);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>上述代码有着惊人的效率，经测试读取这10000000个数只用了0.29秒，效率提高了几乎10倍！掌握着种方法简直无敌了，不过，我记得fread是封装过的read，如果直接使用read，是不是更快呢？代码如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXN = 10000000;
const int MAXS = 60*1024*1024;

int numbers[MAXN];
char buf[MAXS];

void read_analyse()
{
    int fd = open("data.txt",O_RDONLY);
    int len = read(fd,buf,MAXS);
    buf[len] = '\0';
    analyse(buf,len);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>测试发现运行时间仍然是0.29秒，可见read不具备特殊的优势。到此已经结束了吗？不，我可以调用Linux的底层函数mmap，这个函数的功能是将文件映射到内存，是所有读文件方法都要封装的基础方法，直接使用mmap会怎样呢？代码如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const int MAXN = 10000000;
const int MAXS = 60*1024*1024;

int numbers[MAXN];
char buf[MAXS];
void mmap_analyse()
{
    int fd = open("data.txt",O_RDONLY);
    int len = lseek(fd,0,SEEK_END);
    char *mbuf = (char *) mmap(NULL,len,PROT_READ,MAP_PRIVATE,fd,0);    
    analyse(mbuf,len);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>经测试，运行时间缩短到了0.25秒，效率继续提高了14%。到此为止我已经没有更好的方法继续提高读文件的速度了。回头测一下Pascal的速度如何？结果<strong>令人大跌眼镜</strong>，居然运行了2.16秒之多。程序如下：</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>const
    MAXN = 10000000;
var
    numbers :array[0..MAXN] of longint;
    i :longint;
begin
    assign(input,'data.txt');
    reset(input);
    for i:=0 to MAXN do
        read(numbers[i]);
end.
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>为确保准确性，我又换到Windows平台上测试了一下。结果如下表：</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class=""><tbody><tr><td>方法/平台/时间(秒)</td><td>Linux gcc</td><td>Windows mingw</td><td>Windows VC2008</td></tr><tr><td>scanf</td><td>2.010</td><td>3.704</td><td>3.425</td></tr><tr><td>cin</td><td>6.380</td><td>64.003</td><td>19.208</td></tr><tr><td>cin取消同步</td><td>2.050</td><td>6.004</td><td>19.616</td></tr><tr><td>fread</td><td>0.290</td><td>0.241</td><td>0.304</td></tr><tr><td>read</td><td>0.290</td><td>0.398</td><td>不支持</td></tr><tr><td>mmap</td><td>0.250</td><td>不支持</td><td>不支持</td></tr><tr><td>Pascal read</td><td>2.160</td><td>4.668</td><td></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>从上面可以看出几个问题</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><li>Linux平台上运行程序普遍比Windows上快。</li><li>Windows下VC编译的程序一般运行比MINGW（MINimal Gcc for Windows）快。</li><li>VC对cin取消同步与否<strong>不敏感</strong>，前后效率相同。反过来MINGW则<strong>非常敏感</strong>，前后效率相差8倍。</li><li>read本是linux系统函数，MINGW可能采用了某种模拟方式，read比fread更慢。</li><li>Pascal程序运行速度实在令人不敢恭维。</li></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>希望此文能对大家有所启发，欢迎与我继续讨论。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://www.byvoid.com/">BYVoid原创 转载请注明</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>编辑注</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>特别说明：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>本文转载自BYVoid（郭家宝），转载请注明作者。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>郭家宝在OIerDB中的资料：</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class=""><thead><tr><th>获奖</th><th>分数</th><th>全国排名</th><th>就读学校</th><th>获奖年级</th></tr></thead><tbody><tr><th>NOI2009金牌</th><td>370</td><td>17</td><td>河南省实验中学</td><td>高二</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>他目前就职于Google Inc.</p>
<!-- /wp:paragraph -->