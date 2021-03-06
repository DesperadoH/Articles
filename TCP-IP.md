# TCP-IP-protocol-stack
TCP/IP协议栈
## 一、摘要
概述出TCP/IP的协议栈模型，最后根据实例对各层包头进行分析。

## 二、标准TCP/IP协议栈模型
标准TCP/IP协议是用于计算机通信的一组协议，通常被称为TCP/IP协议栈，以它为基础组建的互联网是目前国际上规模最大的计算机网络。正因为互联网的广泛应用，使得TCP/IP成为了事实上的网络标准。
### 1、OSI模型和TCP/IP协议模型
图1是OSI模型和TCP/IP协议模型的对比。

![](https://github.com/DesperadoH/Articles/raw/master/imgs/TCP-IP-protocol-stack/1.jpg) 

### 2、TCP/IP协议模型分层

####（1）网络接口层

  TCP/IP协议模型的基层，负责数据帧的发送和接收。对应OSI模型中的物理层和数据链路层，是TCP/IP的最底层，不过通常在描述TCP/IP模型时还是会划分具体为物理层(PHY)和数据链路层(MAC)。
  
#### （2）网络层

  通过互联协议将数据包封装成互联网数据包，并运行必要的路由算法。这里有4种互联协议。

　　(a)网际协议IP：负责在主机和网络之间的路径寻址和数据包路由。

　　(b)地址解析协议ARP：获得同一物理网络中的主机硬件地址。

　　(c)网际控制消息协议ICMP：发送消息，并报告有关数据包的传送错误。

　　(d)互联组管理协议IGMP：用来实现本地多路广播路由器报告。

#### （3）传输层

传输协议在主机之间提供通信会话。传输协议的选择根据数据传输方式而定。主要有以下2种传输协议：

　　(a)传输控制协议TCP：为应用程序提供可靠的通信连接，适用于要求得到响应的应用程序。

　　(b)用户数据包协议UDP：提供无连接通信，且不对传输包进行可靠性确认。
　　
　　
#### （4）应用层

　　应用程序通过这一层访问网络，主要包括常见的FTP、HTTP、DNS和TELNET协议。

### 3、TCP/IP协议模型对数据的封装

![](https://github.com/DesperadoH/Articles/raw/master/imgs/TCP-IP-protocol-stack/2.jpg) 

图2 各层数据包之间的相关性

在TCP/IP协议模型的4层协议中，各层数据包封装情况如图2所示。在发送数据时，将数据从最上层到最下层一次打包（加上包头和部分尾部信息）；在接收数据时，则将从数据从最下层到最上层依次拆包（去掉包头和部分尾部信息）。这些打包和拆包操作就是由TCP/IP协议栈实现的。下面根据实例对上述包头进行详细分析。


## 三、TCP/IP协议栈中各层包头的分析
### 1、获取数据包
(a) 以“DIY_DE2之DM9000A网卡调试系列例程（二）——DM9000A测试、自收发、实现UDP”为实例

　　获取数据包的方式有两种：通过wireshark抓包工具抓取数据包和通过NIOS II端中断的方式获取PC端发送的数据包，获取的数据包分别如下：

通过wireshark获得：

                                           e0 cb 4e b7 9e d1 01 60

                                           6e 11 02 0f 08 00 45 00

                                           05 d8 00 00 00 00 80 11

                                           b1 97 c0 a8 00 2c c0 a8

                                           02 01 04 00 04 00 05 c4

                                                    00 00


这42个字节即是该例程的数据发送到PC端的包头，42个字节之后就是有效数据，最后的4个字节则为校验位。通过上述例程语句也能很清楚的分析各个数据的含义。

通过NIOS II端获得：

                                            ff   ff   ff  ff   ff   ff  e0 cb 

                                            4e b7 9e d1 08 00 45 00 

                                            00 1e 3c ac 00 00 80 11 

                                            3b 7a c0 a8 02 01 ff   ff  

                                            ff  ff  04  00 04 00 00 0a

                                                       04 31


同上。

　　由于该例程主要是实现UDP，所以TCP包头不够明了，为了全面的分析各个包头，将使用下面的例程。
　　
　　(b) 以“DIY_DE2之DM9000A网卡调试系列例程（四）——基于NicheStack协议栈的TCP/IP实现”为实例

　　通过wireshark获取的数据包如下：


                                            00 07 ed ff 06 00 00 0f

                                            ea fd 9f 96 08 00 45 00

                                            00 29 38 13 40 00 40 06 

                                            7d 60 c0 a8 02 0a c0 a8

                                            02 01 18 98 00 17 37 8d

                                            49 3b 00 46 74 e0 50 18

                                            fe d9 ea f7 00 00 32

     下面将对其做具体分析。

### 2、MAC包头
MAC包头占有14字节，即：

　　00 07 ed ff 06 00 00 0f ea fd 9f 96 08 00

　　很容易看出来 00 07 ed ff 06 00 和 00 0f ea fd 9f 96 分别是DIY_DE2和PC的MAC地址，后面的 08 00，不知道是什么。

### 3、IP包头

![](https://github.com/DesperadoH/Articles/raw/master/imgs/TCP-IP-protocol-stack/3.jpg) 

IP包头占有20个字节，即：

　　45 00 00 29 38 13 40 00 40 06 7d 60 c0 a8 02 0a c0 a8 02 01

　　(1) “45”，其中“4”是IP协议的版本（Version），说明是IP4。“5”是IHL位，表示IP头部的长度，是一个4bit字段，最大就是1111了，值为12，IP头部的最大长度就是60字节。而这里为“5”，说明是20字节，这是标准的IP头部长度，头部报文中没有发送可选部分数据。　

　　(2) “00”，服务类型（Type of Service）。这个8bit字段由3bit的优先权子字段（现在已经被忽略），4 bit的TOS子字段以及1 bit的未用字段（现在为0）构成.4 bit的TOS子字段包含：最小延时、最大吞吐量、最高可靠性以及最小费用构成，这四个1bit位最多只能有一个为1，本例中都为0，表示是一般服务。　

　　(3) “00 29”，IP数据报文总长，包含头部以及数据，这里表示41字节。这41字节由20字节的IP头部以及21字节的TCP头构成（最后的一个字节为数据）。因此目前最大的IP数据包长度是65535字节。　

　　(4) “38 13”，两个字节的标志位，这个是让目的主机来判断新来的分段属于哪个分组。　

　　(5) “40”，转换为二进制就是“0100 0000”，其中第一位是IP协议目前没有用上的，为0。接着的是两个标志DF和MF。DF为1表示不要分段，MF为1表示还有进一步的分段（本例为0）。然后的“0 0000”是分段便移（Fragment Offset）。　

　　(6) “00”，待定。

　　(7) “40”这个字节就是TTL（Time To Live）了，表示一个IP数据流的生命周期，用Ping显示的结果，能得到TTL的值，很多文章就说通过TTL位来判别主机类型。因为一般主机都有默认的TTL值，不同系统的默认值不一样。比如WINDOWS为128。不过，一般Ping得到的都不是默认值，这是因为每次IP数据包经过一个路由器的时候TTL就减一，当减到0时，这个数据包就消亡了。这也时Tracert的原理。本例中为“40”，转换为十进制就是64了，我用的WinXP。　

　　(8) “06”，这个字节表示传输层的协议类型（Protocol）。在RFC790中有定义，6表示传输层是TCP协议。　

　　(9) “7d 60”这个16bit是头校验和（Header Checksum）。　

　　(10) “c0 a8 02 0a”，这个是源地址，也就是PC的IP地址，转换为十进制的IP地址就是：192.168.2.10。

　　(11) “c0 a8 02 01”，这个是目标地址，也就是DIY_DE2的地址，转换为十进制的IP地址就是：192.168.2.1。


### 4、TCP包头

![](https://github.com/DesperadoH/Articles/raw/master/imgs/TCP-IP-protocol-stack/4.jpg) 

TCP包头占有20个字节，即：

　　18 98 00 17 37 8d 49 3b 00 46 74 e0 50 18 fe d9 ea f7 00 00

　　(1) “18 98”，表示本地端口号，转换为十进制就是3368。

　　(2) “00 15”，表示目标端口号，转换为十进制就是23，因为我是连接TELNET站点，所以，这个就是23。

　　(3) “37 8d 49 3b”，是顺序号（Sequence Number），简写为SEQ。

　　(4) “00 46 74 e0”，是确认号（Acknowledgment Number），简写为ACKNUM。

　　(5) “50 18”，转换为二进制，“0101 0000 0001 1000”。这两个字节，总共16bit，有好多东西。第一个4bit“0101”，是TCP头长，十进制为5，表示20个字节。接着的6bit现在TCP协议没有用上，都为0。最后的6bit“01 1000”是六个重要的标志。这是两个计算机数据交流的信息标志。接收和发送断根据这些标志来确定信息流的种类。下面是一些介绍：　

　　URG：（Urgent Pointer field significant）紧急指针。用到的时候值为1，用来处理避免TCP数据流中断。

　　ACK：（Acknowledgment fieldsignificant）置1时表示确认号（AcknowledgmentNumber）为合法，为0的时候表示数据段不包含确认信息，确认号被忽略。

　　PSH：（Push Function），PUSH标志的数据，置1时请求的数据段在接收方得到后就可直接送到应用程序，而不必等到缓冲区满时才传送。

　　RST：（Reset the connection）用于复位因某种原因引起出现的错误连接，也用来拒绝非法数据和请求。如果接收到RST位时候，通常发生了某些错误。　

　　SYN：（Synchronize sequence numbers）用来建立连接，在连接请求中，SYN=1，ACK=0，连接响应时，SYN=1，ACK=1。即，SYN和ACK来区分Connection Request和Connection Accepted。　

　　FIN：（No more data from sender）用来释放连接，表明发送方已经没有数据发送了。　

　　这6个标志位，对号入座。本例中SYN=0，ACK=1，当然就是表示连接请求了。在分析TCP包头时候，要注意这两位的变换。　

　　(6) “fe d9”，窗口值，用来控制实现流量控制。

　　(7) “ea f7”，检验和，TCP的检验和是强制的。

　　(8) “00 00”，紧急指针。



