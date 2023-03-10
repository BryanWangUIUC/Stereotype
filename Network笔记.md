## 数据链路层

+ MAC地址是以太网的MAC子层所使用的地址(属于数据链路层)
+ IP地址是TCP/IP体系结构的网际层所使用的地址
+ ARP协议属于TCP/IP体系结构的网际层， 其作用是已知设备所分配到的IP地址，使用ARP协议可以通过该IP地址获取到设备的MAC地址



#### MAC地址

当多个主机连接在同一个广播信道上，要想实现两个主机之间的通信，则每个主机都必须有一个唯一的标识，即一个数据链路层地址

在每个主机发送的帧中必须携带标识发送主机和接受主机的地址。由于这类地址适用于媒体接入控制MAC(Media Access Control)，因此这类地址被称为MAC地址

MAC地址，又称硬件地址，物理地址。严格来说，MAC地址是对网络上各接口的唯一标识，而不是对网络上各设备的唯一标识。



IEEE 802局域网的MAC地址发送顺序：

+ 字节发送顺序：第一字节 -> 第六字节
+ 字节内的比特发送顺序：b0 -> b7



#### IP地址

IP地址是因特网(Internet)上的主机和路由器所使用的地址，用于标识两部分信息：

+ 网络编号：
+ 主机编号：标识同一网络上的不同主机(或路由器各接口)标识因特网上数以百万计的网络

很显然，MAC地址不具备区分不同网络的功能

+ 如果只是一个单独的网络，不介入因特网，可以只使用MAC地址
+ 如果主机所在的网络要接入因特网，则IP地址和MAC地址都需要使用

在数据包的转发过程中，源IP地址和目的IP地址始终保持不变；而源MAC地址和目的MAC地址逐段链路(或逐个网络)改变



#### ARP协议

+ 源主机在自己的ARP高速缓存表中查找目的主机的IP地址所对应的MAC地址，若找到了，则可以封装MAC帧进行发送；若找不到，则发送ARP请求(封装在广播MAC帧中)
+ 目的主机收到ARP请求后，将源主机的IP地址与MAC地址记录到自己的ARP高速缓存表中，然后给源主机发送ARP响应(封装在单播MAC帧中)，ARP响应中包含有目的主机的IP地址和MAC地址
+ 源主机收到ARP响应后，将目的主机的IP地址与MAC地址记录到自己的ARP高速缓存表中，然后就可以封装之前想发送的MAC帧并发送给目的主机

只能在一段链路或一个网络上使用，而不能跨网络使用

除ARP请求和响应之外，ARP还有其他类型的报文

ARP没有安全验证机制，存在ARP欺骗(攻击)问题





## 网络层

#### 概述

网络层的主要任务是实现网络互连，进而实现数据包在各网络之间的传输。

+ 网络层寻址问题
+ 网络层向传输层提供怎样的服务(可靠传输/不可靠传输)(TCP/IP -> 无连接，不可靠的传输)
+ 路由选择问题(目的地址、路由表)

因特网(Internet)是目前全世界用户数量最多的互联网，他使用TCP/IP协议栈

由于TCP/IP协议栈的网络层使用网际协议IP，它是整个协议栈的核心协议，因此在TCP/IP协议栈中的网络层常成为网际层



#### 网络层提供的两种服务

面向连接的虚电路服务

+ 可靠通信由网络来保证
+ 必须建立网络层的连接---虚电路VC(Virtual Circuit)
+ 通信双方沿着已建立的虚电路发送分组
+ 目的主机的地址仅在连接建立阶段使用，之后每个分组的首部只需携带一条虚电路的编号
+ 这种通信方式如果再使用可靠传输的网络协议，就可使发送的分组最终正确达到接受方
+ 通信结束后，需要释放之前所建立的虚电路

无连接的数据报服务

+ 可靠通信应当由用户主机来保证
+ 不需要建立网络层连接
+ 每个分组都有终点的完整地址
+ 每个分组可走不同的路由
+ 到达终点时不一定按发送顺序
+ 很难实现



#### IPv4地址概述

IPv4地址就是因特网(Internet)上的每一台主机(或路由器)的每一个接口分配一个在全世界范围内是唯一的32比特的标识符

采用点分十进制表示方法以方便用户使用(没八个分位为一组)



##### 分类编址的IPv4地址

##### 划分子网的IPv4地址

##### 无分类编址的IPv4地址



## 运输层

+ 之前课程所介绍的计算机网络体系结构中的物理层，数据链路层以及网络层共同解决了将主机通过异构网络互联起来所面临的问题，实现了主机到主机的通信。

+ 但实际上在计算机网络中进行通信的真正实体是位于通信两端主机中的进程

+ 如何为运行在不同主机上的应用进程提供直接的通信服务是运输层的任务，运输层协议又称为端到端协议。
+ 运输层向高层用户屏蔽了网络核心下面的细节，它使应用进程看见的就好像是在两个运输实体之间有一条端到端的逻辑通信通道。
+ 根据应用需求的不同，因特网的运输层为应用层提供了两种不同的运输协议，即面向连接的TCP和无连接的UDP，这两种协议就是本章要讨论的主要内容。



### 运输层端口号，复用与分用的概念

+ 运行在计算机上的进程使用进程标识符PID来标志
+ 因特网上的计算机斌表示使用统一的操作系统，不同的操作系统使用不同格式的进程标识符
+ 为了使运行不同操作系统的计算机的应用进程之间能够进行网络通信，就必须使用统一的方法对TCP/IP体系的应用进程进行标识
+ TCP/IP体系的运输层使用端口号来区分应用层的不同应用进程
+ 端口号使用16比特标识，取值范围0~65535。
+ 端口号只具有本地意义，只是为了标识本计算机应用层中的各进程，在因特网中，不同计算机中的相同端口号是没有联系的



### UDP和TCP的对比

+ UDP和TCP是TCP/IP体系结构运输层中的两个重要协议
+ UDP(User Datagram Protocal)用户数据报协议
+ + 无连接
  + 支持单播，多播，广播(一对一，一对多，一对全)
  + 面向应用报文
  + UDP向上提供无连接不可靠传输服务(适用于IP电话、视频会议等实时应用)
  + UDP用户数据报首部仅有八字节

+ TCP(Transmission Control Protocal)传输控制协议
+ + 面向连接
  + 三报文握手建立连接 & 四报文挥手释放连接
  + 仅支持单播(一对一)
  + 基于TCP连接的可靠信道
  + TCP是面向字节流的，这是TCP实现可靠传输、流量控制、拥塞控制的基础。
  + TCP向上提供面向连接的可靠运输服务(不会出现差错(误码、丢失、乱序、重复))，适用于要求可靠传输的应用全，例如文件传输
  + TCP报文段首部最小20字节，最大50字节



### TCP流量控制

+ 一般来说，我们总是希望数据传输得更快一些。但如果发送方把数据发送得过快，接收方就可能来不及接受，这就会造成数据的丢失。
+ 所谓流量控制(flow control)就是让发送方发送速率不要太快，要让接收方来得及接收
+ 利用滑动窗口机制可以很方便地在TCP连接上实现对发送方的流量控制
+ + TCP接收方利用自己的接收窗口的大小来限制发送方发送窗口的大小
  + TCP发送方收到接收方的零窗口通知后，应启动持续计时器。持续计时器超时后，向接收方发送零窗口探测报文



### TCP拥塞控制

在某段时间，若网络中某一资源的需求超过了该资源所能提供的可用部分，网络性能就会变坏，这种情况就叫做拥塞(congestion)

在计算机网络中的链路容量、交换节点中的缓存和处理机等，都是网络的资源

若出现拥塞而不进行控制，整个网络的吞吐量将随输入负荷的增大而下降



假设如下条件

+ 数据是单方向传送的，而另一个方向只传送确认
+ 接收方总是有足够大的缓存空间，因而发送方发送窗口的大小由网络的拥塞成都来决定
+ 以最大报文段MSS的个数为讨论问题的单位，而不是以字节为单位



发送方维护一个叫做拥塞窗口cwnd的状态变量，其值取决于网络的拥塞程度，并且动态变化

+ 拥塞窗口cwnd的维护原则：只要网络没有出现拥塞，拥塞窗口就再增大一些；但只要网络出现拥塞，拥塞窗口就减少一些
+ 判断出现网络拥塞的依据：没有及时收到应当到达的确认报文(即发生超时重传)

发送方将拥塞窗口作为发送窗口swnd，即swnd=cwnd

维护一个慢开始门限ssthresh状态变量

+ 当cwnd < ssthresh，使用慢开始算法
+ 当cwnd > ssthresh，停止使用慢开始算法而改用拥塞避免算法
+ 当cwnd = ssthresh，既可以使用慢开始算法，也可以使用拥塞避免算法

#### 慢开始

慢开始是指一开始向网络注入的报文段少，并不是指拥塞窗口cwnd增长速度慢

#### 拥塞避免

并非指能够完全避免拥塞，而是指在拥塞避免阶段将拥塞串口控制为按线性规律增长，使网络比较不容易出现拥塞

#### 快重传

所谓快重传，就是使发送方尽快进行重传，而不是等超时计时器超时再重传

+ 要求接收方不要等待自己发送数据时才进行捎带确认，而是要立即发送确认
+ 即使收到了失序的报文段也要立即发出对已收到的报文段的重复确认
+ 发送方一旦受到三个连续的重复确认，就将相应的报文段立即重传，而不是等该报文段的超时重传计时器超时再重传
+ 对于个别丢失的报文段，发送方不会出现超时重传，也就不会误认为出现了拥塞(进而降低拥塞窗口cwnd为1)。使用快重传可以使整个网络的吞吐量提高约20%

#### 快恢复

发送方一旦收到三个重复确认，就知道现在只是丢失了个别的报文段，于是不启动慢开始算法，而执行快恢复算法

+ 发送方将慢开始门限ssthresh值和拥塞窗口cwnd调整为当前窗口的一半；开始执行拥塞避免算法
+ 也有的快恢复实现是把快恢复开始时的拥塞窗口cwnd值再增大一些，即等于新的ssthresh+3。既然发送方收到3个重复的确认，就表明有3个数据报文段已经离开了网络，这三个报文段不再消耗网络资源而是停留在接收方的接收缓存中，可见网络中不是堆积了报文段而是减少了3个报文段。因此可以适当把拥塞窗口扩大些。



### TCP超时重传时间的选择

+ 不能直接用某次测量得到的RTT样本来计算超时重传时间RTO
+ 利用每次测量得到的RTT样本，计算加权平均往返时间RTTs(又称平滑的往返时间)
+ 超时重传时间RTO应略大于加权平均往返时间RTTs



### TCP可靠传输的实现

+ TCP以字节为单位的滑动窗口来实现可靠传输

+ 虽然发送方的发送窗口





### TCP的运输连接管理

+ TCP是面向连接的协议，它基于运输连接来传送TCP报文段
+ TCP运输连接的建立和释放是每一次面向连接的通信中必不可少的过程
+ TCP运输连接有以下三个阶段：建立TCP连接、数据传送、释放TCP连接
+ TCP的运输连接管理就是使运输连接的建立和释放都能正常地运行





## 应用层

### 概述

应用层是计算机网络结构的最顶层，是设计和建立计算机网络的最终目的。





### 客户-服务器方式(C-S方式)和对等方式(P2P方式)

#### 客户-服务器方式(C-S方式 / Client-Srever方式)

+ 客户和服务器是指通信中所涉及的两个应用进程

+ 客服/服务器方式所描述的是进程之间服务和被服务的关系

+ 客户是服务请求方，服务器是服务提供方

+ 服务器总是处于运行状态，并等待客户的服务请求。服务器具有固定端口号(例如HTTP服务器的默认端口号为80)，而运行服务器的主机也具有固定的IP地址。

+ C-S方式是因特网上最传统的，同时也是最成熟的方式，很多我们熟悉的网络应用采用的都是C/S方式，包括万维网WWW，电子邮件，文件传输FTP等。

+ 基于C-S方式的应用服务常是服务集中型的，即应用服务集中在网络中比客户计算机少得多的服务器计算机上。常会出现服务器计算机跟不上众多客户机请求的情况。

  

### 动态主机配置协议DHCP



### 域名系统DNS(Domain Name System)

+ 域名系统DNS是因特网使用的命名系统，用来把便于人们记忆的具有特定含义的主机名，转换为便于机器处理的IP地址。

+ 因特网采用树状结构的域名系统
+ 域名和IP地址的映射关系必须保存在域名服务器中，供所有其他应用查询。，显然不能将所有信息都储存在一台域名服务器中，DNS使用分布在各地的域名服务器来实现域名到IP地址的转换。
+ 域名解析的过程使用两种域名查询方式：递归查询、迭代查询
+ 为了提高DNS的查询效率，并减轻根域名服务器的负荷和减少因特网上的DNS查询报文数量，在域名服务器和主机中广泛地使用了高速缓存。
+ DNS报文使用运输层的UDP协议进行封装，运输层端口号为53





### 万维网WWW

万维网WWW(World Wide Web)并非某种特殊的计算机网络。它是一个大规模的、联机式的信息储藏所，是运行在因特网上的一个分布式应用。

万维网利用网页之间的超链接将不同网站的网页链接成一张逻辑上的信息网。



浏览器最重要的部分是渲染引擎，也就是浏览器内核。负责对网页内容进行解析和显示。

+ 不同的浏览器内核对网页内容的解析也有所不同，因此同一网页在不同内核的浏览器里的显示效果可能不同。
+ 网络编写者需要再不同内核的浏览器中测试网页显示效果。



为了方便地访问在世界范围内的文档，万维网使用统一资源定位符URL来指明因特网上任何种类资源的位置

```
<协议>://<主机>:<端口>/<路径>
```



万维网的文档：

+ HTML超文本标记语言：使用多种标签来描述网页的结构和内容
+ CSS层叠样式表：从审美的角度来描述网页的样式
+ JavaScript：一种脚本语言，用来控制网页的行为



超文本传输协议HTTP(HyperText Transfer Protocol)

+ HTTP定义了浏览器(即万维网客户进程)怎样向万维网服务器请求万维网文档，以及万维网服务器怎样把万维网文档传送给浏览器。
+ HTTP/1.0采用非持续连接方式。在该方式下，每次浏览器请求一个文件都要与服务器建立TCP连接，当收到响应后就立即关闭连接。
  + ...
+ HTTP/1.1



HTTP的报文格式：HTTP是面向文本的，其报文中的每一个字段都是一些ASCII码串，并且每个字段的长度都是不确定的。



使用Cookie在服务器上记录用户信息。Cookie提供了一种机制使得万维网能够记住用户，而无需用户主动提供用户标识信息。也就是说，Cookie提供了一种对无状态Http进行状态化的技术。





万维网的缓存与代理服务器

+ 在万维网中还可以使用缓存机制以提高万维网的效率
+ 万维网缓存又称为Web缓存(Web Cache)，可位于客户机，也可位于中间系统上，位于中间系统上的Web缓存又称为代理服务器(Proxy Server)
+ Web缓存把最近的一些请求和响应暂存在本地磁盘中。当心情求到达时，若发现这个请求与暂时存放的请求相同，就返回暂存的响应，而不需要按URL的地址再次去因特网访问该资源。



























































































































