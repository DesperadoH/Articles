# Proxy

# 正向代理和反向代理

## 一、正向代理的概念

### 1. 正向代理:
      是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，
      客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。
      客户端必须要进行一些特别的设置才能使用正向代理。

正向代理的典型用途是为在防火墙内的局域网客户端提供访问Internet的途径。
正向代理还可以使用缓冲特性(由mod_cache提供)减少网络使用率。
使用ProxyRequests指令即可激活正向代理。
因为正向代理允许客户端通过它访问任意网站并且隐藏客户端自身，
因此你必须采取安全措施以确保仅为经过授权的客户端提供服务。

### 2. 举例来说

正向代理,也就是传说中的代理,他的工作原理就像一个跳板,

简单的说,

      我是一个国内用户,我直接访问不了某外国网站,但是我能访问一个代理服务器，这个代理服务器呢,他能访问那个我不能访问的外国网站。
      STEP1. 于是我先连上代理服务器,告诉他我需要那个无法访问的外国网站的内容。
      STEP2. 代理服务器去取回来,然后返回给我

从那个外国网站的角度来说,只在代理服务器来取内容的时候有一次记录。
有时候并不知道是用户的请求,也隐藏了用户的资料,这取决于代理告不告诉网站。

结论就是:

正向代理, 是一个位于客户端和原始服务器(origin server)之间的服务器，

          为了从原始服务器取得内容，
          客户端向代理发送一个请求并指定目标(原始服务器)，
          然后代理向原始服务器转交请求并将获得的内容返回给客户端。

客户端必须要进行一些特别的设置才能使用正向代理。

## 二、反向代理

反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，
然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，
此时代理服务器对外就表现为一个服务器。


代理服务器是使用非常普遍的一种将局域网主机联入互联网的一种方式，
使用代理上网可以节约紧缺的IP地址资源，而且可以阻断外部主机对内部主机的访问，使内部网主机免受外部网主机的攻击。
但是，如果想让互联网上的主机访问内部网的主机资源（例如：Web站点），
又想使内部网主机免受外部网主机攻击，一般的代理服务是不能实现的，需要使用反向代理来实现。


### 1. 反向代理 - 定义

什么是反向代理呢？

其实，反向代理也就是通常所说的WEB服务器加速，
它是一种通过在繁忙的WEB服务器和Internet之间增加一个高速的WEB缓冲服务器（即：WEB反向代理服务器）
来降低实际的WEB服务器的负载。

![](https://github.com/DesperadoH/Articles/raw/master/imgs/Proxy/1.gif)

Web服务器加速（反向代理）是针对Web服务器提供加速功能的。
它作为代理Cache，但并不针对浏览器用户，而针对一台或多台特定Web服务器（这也是反向代理名称的由来）。
实施反向代理（如上图所示），只要将Reverse Proxy Cache设备放置在一台或多台Web服务器前端即可。
当互联网用户访问某个WEB服务器时，通过DNS服务器解析后的IP地址是Reverse Proxy Server的IP地址,
而非原始Web服务器的IP地址,这时Reverse Proxy Server设备充当Web服务器，浏览器可以与它连接，无需再直接与Web服务器相连。
因此，大量Web服务工作量被卸载到反向代理服务上。
不但能够防止外部网主机直接和web服务器直接通信带来的安全隐患，而且能够很大程度上减轻web服务器的负担，提高访问速度。

### 2. 反向代理 - 原理

反向代理服务器位于本地WEB服务器和Internet之间。
当用户浏览器发出一个HTTP请求时，通过域名解析将请求定向到反向代理服务器
（如果要实现多个WEB服务器的反向代理，需要将多个WEB服务器的域名都指向反向代理服务器）。
由反向代理服务器处理器请求。
反向代理一般只缓存可缓冲的数据（比如html网页和图片等），而一些CGI脚本程序或者ASP之类的程序不缓存。
它根据从WEB服务器返回的HTTP头标记来缓冲静态页面。有四个最重要HTTP头标记： 

     Last-Modified:   告诉反向代理页面什么时间被修改 
     expires:         告诉反向代理页面什么时间应该从缓冲区中删除 
     Cache-Control:   告诉反向代理页面是否应该被缓冲 
      Pragma:          告诉反向代理页面是否应该被缓冲. 

例如：在默认情况下，ASP页面返回” Cache-control: private.” ，所以ASP页面时不会在反向代理服务器缓存的

------

反向代理服务器通常有两种模型，它可以作为内容服务器的替身，也可以作为内容服务器集群的负载均衡器。
### 1，作内容服务器的替身  

如果您的内容服务器具有必须保持安全的敏感信息，如信用卡号数据库，可在防火墙外部设置一个代理服务器作为内容服务器的替身。
当外部客户机尝试访问内容服务器时，会将其送到代理服务器。
实际内容位于内容服务器上，在防火墙内部受到安全保护。
代理服务器位于防火墙外部，在客户机看来就像是内容服务器。

当客户机向站点提出请求时，请求将转到代理服务器。
然后，代理服务器通过防火墙中的特定通路，将客户机的请求发送到内容服务器。
内容服务器再通过该通道将结果回传给代理服务器。代理服务器将检索到的信息发送给客户机，好像代理服务器就是实际的内容服务器（参见图 2）。
如果内容服务器返回错误消息，代理服务器会先行截取该消息并更改标头中列出的任何 URL，然后再将消息发送给客户机。
如此可防止外部客户机获取内部内容服务器的重定向 URL。

 这样，代理服务器就在安全数据库和可能的恶意攻击之间提供了又一道屏障。
与有权访问整个数据库的情况相对比，就算是侥幸攻击成功，作恶者充其量也仅限于访问单个事务中所涉及的信息。
未经授权的用户无法访问到真正的内容服务器，因为防火墙通路只允许代理服务器有权进行访问。

![](https://github.com/DesperadoH/Articles/raw/master/imgs/Proxy/2.gif)

 可以配置防火墙路由器，使其只允许特定端口上的特定服务器（在本例中为其所分配端口上的代理服务器）有权通过防火墙进行访问，而不允许其他任何机器进出。
 
 
### 2，作为内容服务器的负载均衡器

可以在一个组织内使用多个代理服务器来平衡各 Web 服务器间的网络负载。
在此模型中，可以利用代理服务器的高速缓存特性，创建一个用于负载平衡的服务器池。
此时，代理服务器可以位于防火墙的任意一侧。如果 Web 服务器每天都会接收大量的请求，则可以使用代理服务器分担 Web 服务器的负载并提高网络访问效率。

对于客户机发往真正服务器的请求，代理服务器起着中间调停者的作用。代理服务器会将所请求的文档存入高速缓存。
如果有不止一个代理服务器，DNS 可以采用“循环复用法”选择其 IP 地址，随机地为请求选择路由。
客户机每次都使用同一个 URL，但请求所采取的路由每次都可能经过不同的代理服务器。

可以使用多个代理服务器来处理对一个高用量内容服务器的请求，这样做的好处是内容服务器可以处理更高的负载，并且比其独自工作时更有效率。
在初始启动期间，代理服务器首次从内容服务器检索文档，此后，对内容服务器的请求数会大大下降。

![](https://github.com/DesperadoH/Articles/raw/master/imgs/Proxy/3.gif)


### 3. 举例说明

例用户访问 http://ooxx.me/readme，但ooxx.me上并不存在readme页面。
他是偷偷从另外一台服务器上取回来,然后作为自己的内容吐给用户，但用户并不知情。
这很正常,用户一般都很笨

这里所提到的 ooxx.me 这个域名对应的服务器就设置了反向代理功能。
结论就是 

反向代理，和正向代理正好相反，对于客户端而言它就像是原始服务器，

          并且客户端不需要进行任何特别的设置。
          客户端向反向代理 的命名空间(name-space)中的内容发送普通请求，
          接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，
          就像这些内容 原本就是它自己的一样。


### 4. 反向代理 - 比较

下面将对几种典型的代理服务作一个简单的比较。在网络上常见的代理服务器有三种： 

#### 1)． 标准的代理缓冲服务器 
一个标准的代理缓冲服务被用于缓存静态的网页（例如：html文件和图片文件等）到本地网络上的一台主机上（即代理服务器）。
当被缓存的页面被第二次访问的时候，浏览器将直接从本地代理服务器那里获取请求数据而不再向原web站点请求数据。
这样就节省了宝贵的网络带宽，而且提高了访问速度。

但是，要想实现这种方式，必须在每一个内部主机的浏览器上明确指明代理服务器的IP地址和端口号。
客户端上网时，每次都把请求送给代理服务器处理，代理服务器根据请求确定是否连接到远程web服务器获取数据。
如果在本地缓冲区有目标文件，则直接将文件传给用户即可。
如果没有的话则先取回文件，先在本地保存一份缓冲，然后将文件发给客户端浏览器。 

#### 2)． 透明代理缓冲服务器 
透明代理缓冲服务和标准代理服务器的功能完全相同。
但是，代理操作对客户端的浏览器是透明的（即不需指明代理服务器的IP和端口）。
透明代理服务器阻断网络通信，并且过滤出访问外部的HTTP（80端口）流量。
如果客户端的请求在本地有缓冲则将缓冲的数据直接发给用户，
如果在本地没有缓冲则向远程web服务器发出请求，其余操作和标准的代理服务器完全相同。
对于Linux操作系统来说，透明代理使用Iptables或者Ipchains实现。
因为不需要对浏览器作任何设置，所以，透明代理对于ISP（Internet服务器提供商）特别有用。 

#### 3)． 反向代理缓冲服务器 
反向代理是和前两种代理完全不同的一种代理服务。
使用它可以降低原始WEB服务器的负载。反向代理服务器承担了对原始WEB服务器的静态页面的请求，防止原始服务器过载。
它位于本地WEB服务器和Internet之间，处理所有对WEB服务器的请求，组织了WEB服务器和Internet的直接通信。
如果互联网用户请求的页面在代理服务器上有缓冲的话，代理服务器直接将缓冲内容发送给用户。
如果没有缓冲则先向WEB服务器发出请求，取回数据，本地缓存后再发送给用户。
这种方式通过降低了向WEB服务器的请求数从而降低了WEB服务器的负载。 

### 5. 反向代理 - 反向代理服务器软件介绍

#### 1). Fikker反向代理服务器 
Fikker 是一款利于反向代理原理实现的专业级的网站加速服务器软件，全界面化管理配置，利用页面缓存技术（webcache），
网站管理员或网站开发人员通过 Fikker 管理平台将指定的页面缓存起来，用户在访问已缓存页面的时候，就不需要网站读取数据库后再生成页面了，
Fikker 直接返回用户需要的页面，成倍的提成网站响应速度；

另外 Fikker 通过 gzip 将页面（html，asp，php，css，js）压缩起来，减少了传输尺寸，提高传输效率和减少带宽占用。  
作为网站的前置服务器，Fikker 还提供了强大的实时监控功能，防盗链，源站负载均衡，伪静态（url静态化），Ajax跨域操作，
防CC攻击，黑名单管理，访问统计等一站式解决方案，网站管理简单到极致，但功能非常强大。

Fikker 软件从原始架构开始设计，跨平台（支持 Windows 和 Linux）和面向服务器类软件方向设计，
经过多年的精雕细琢，稳定性，功能性和易用性大大提升，实现了很多创新，例如：公共缓存，会员缓存和游客缓存设计。

#### 2). Nginx :  

Nginx ("engine x") 是一个高性能的 HTTP 和 反向代理 服务器，也是一个 IMAP/POP3/SMTP 代理服务器。 
Nginx 是由 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的，
Nginx 已经因为它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名了。

Nginx的优点

Nginx做为HTTP服务器，有以下几项基本特性： 
  A. 处理静态文件，索引文件以及自动索引；打开文件描述符缓冲． 
  B. 无缓存的反向代理加速，简单的负载均衡和容错．
  C. FastCGI，简单的负载均衡和容错．
  D. 模块化的结构。


#### 3). Squid 

对于Web用户来说，Squid是一个高性能的代理缓存服务器，可以加快内部网浏览Internet的速度，提高客户机的访问命中率。
Squid不仅支持HTTP协议，还支持FTP、gopher、SSL和WAIS等协议。
和一般的代理缓存软件不同，Squid用一个单独的、非模块化的、I/O驱动的进程来处理所有的客户端请求。
Squid将数据元缓存在内存中，同时也缓存DNS查寻的结果，除此之外，它还支持非模块化的DNS查询，对失败的请求进行消极缓存。
Squid支持SSL，支持访问控制。由于使用了ICP，Squid能够实现重叠的代理阵列，从而最大限度的节约带宽。
Squid由一个主要的服务程序Squid，一个DNS查询程序 dnsserver，几个重写请求和执行认证的程序，以及几个管理工具组成。
当Squid启动以后，它可以派生出指定数目的dnsserver进程，而每一个dnsserver进程都可以执行单独的DNS查询，这样一来就大大减少了服务器等待DNS查询的时间。
Squid的另一个优越性在于它使用访问控制清单(ACL)和访问权限清单(ARL)。访问控制清单和访问权限清单通过阻止特定的网络连接来减少潜在的Internet非法连接，
可以使用这些清单来确保内部网的主机无法访问有威胁的或不适宜的站点。

## 三、两者区别

### 从用途上来讲：

#### A. 正向代理的典型用途：

     是为在防火墙内的局域网客户端提供访问Internet的途径。正向代理还可以使用缓冲特性减少网络使用率。

#### B. 反向代理的典型用途：
      是将防火墙后面的服务器提供给Internet用户访问。
      反向代理还可以为后端的多台服务器提供负载平衡，或为后端较慢的服务器提供缓冲服务。
      另外，反向代理还可以启用高级URL策略和管理技术，
      从而使处于不同web服务器系统的web页面同时存在于同一个URL空间下。

### 从安全性 来讲：

#### A. 正向代理允许客户端通过它访问任意网站并且隐藏客户端自身，
   因此你必须采取安全措施以确保仅为经过授权的客户端提供服务。

#### B. 反向代理对外都是透明的，访问者并不知道自己访问的是一个代理。


