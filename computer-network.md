## IPV4和IPV6

IPV4使用32位，IPV6使用128位

## 协议

应用层：pop3（110，TCP）

表示层：FTP（20/21，TCP）、HTTP（80，TCP）、DHCP（67，UDP）、TFTP（69，UDP）

会话层：TELNET（23，TCP）、SMTP（25，TCP）、SNMP（161，UDP）、DNS（53，UDP）

传输层：TCP、UDP

网络层：IP、ICMP、IGMP、ARP、RARP

## 三次握手

![在这里插入图片描述](https://img-blog.csdnimg.cn/eccf435f384041a69698b5ca18c30028.PNG?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAanVuMjAxNjQyNQ==,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

![这里写图片描述](https://img-blog.csdn.net/20180809183055554?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1bjIwMTY0MjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

第一次握手

客户端发送SYN段数据包，初始序列号seq = x，客户端状态转变为SYN_SEND

第二次握手

服务器发送SYN +ACK段数据包，初始序列号seq = y，ack = x + 1，服务端状态转变为SYN_RCVD

第三次握手

客户端发送ACK段数据包，初始序列号ack = y + 1，客户端状态转变为ESTABLISHED；服务端收到包后状态转变为ESTABLISHED

若是没有收到第三次握手，服务端会重复进行5次第二次握手，之后自动关闭连接

## 四次握手

![img](https://img-blog.csdn.net/20180717204202563?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4OTUwMzE2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

第一次握手

客户端发送FIN段的数据包，初始序列号seq = x，客户端状态转变为FIN-WAIT-1

第二次握手

服务端发送ACK段的数据包，初始序列号seq = a，ack = x + 1，服务端状态转变为CLOSE-WAIT；客户端收到数据包状态转变为FIN-WAIT-2

第三次握手

服务端发送FIN + ACK段的数据包，初始序列号seq = y，ack = x + 1，服务端状态转变为LAST-ACK

第四次握手

客户端发送ACK段的数据包，初始序列号seq = x + 1，ack = y + 1，客户端状态转变为TIME-WAIT，等待2MSL，状态再转变为CLOSED；服务端收到包后状态转变为CLOSED

## 其他问题

【问题1】为什么连接的时候是三次握手，关闭的时候却是四次握手？

答：因为当Server端收到Client端的SYN连接请求报文后，可以直接发送SYN+ACK报文。其中ACK报文是用来应答的，SYN报文是用来同步的。但是关闭连接时，当Server端收到FIN报文时，很可能并不会立即关闭SOCKET，所以只能先回复一个ACK报文，告诉Client端，"你发的FIN报文我收到了"。只有等到我Server端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送。故需要四步握手。

【问题2】为什么TIME_WAIT状态需要经过2MSL(最大报文段生存时间)才能返回到CLOSE状态？

答：所以TIME_WAIT状态就是用来重发可能丢失的ACK报文。在Client发送出最后的ACK回复，但该ACK可能丢失。Server如果没有收到ACK，将不断重复发送FIN片段。所以Client不能立即关闭，它必须确认Server接收到了该ACK。Client会在发送出ACK之后进入到TIME_WAIT状态。Client会设置一个计时器，等待2MSL的时间。如果在该时间内再次收到FIN，那么Client会重发ACK并再次等待2MSL。所谓的2MSL是两倍的MSL(Maximum Segment Lifetime)。MSL指一个片段在网络中最大的存活时间，2MSL就是一个发送和一个回复所需的最大时间。如果直到2MSL，Client都没有再次收到FIN，那么Client推断ACK已经被成功接收，则结束TCP连接。

【问题3】若是建立连接，客户端出现故障？

服务端每收到依次客户端请求后都会重新复位一个计时器，通常2小时，若两小时还没收到客户端数据，服务端会发送探测报文段，每个75秒发送依次，发送10次后关闭连接

## TCP和UDP

TCP是面相字节流，传输数据前必须先建立连接，数据传送后释放连接，全双工通信，而UDP不建立连接，面相报文

TCP提供可靠交付，按序到达，而UDP尽最大努力交付

TCP是单播形式，UDP是广播形式

## TCP可靠性传输

提供校验和、序列号应答、超时重传、最大消息长度、滑动窗口机制、拥塞控制等方法实现可靠性传输

## 滑动窗口机制

流量控制时为了控制发送方发送速率，保证接收方来记得接收；TCP头包含window字段，16bit位，代表窗口的字节容量。这个字段是接收端告诉发送端自己还有多少缓冲区可以接受数据。于是发送端就可以根据接收端的处理能力来发送数据，而不会导致接收端处理不过来。接收窗口的大小是约等于发送窗口的大小。

## 拥塞控制

![img](https://uploadfiles.nowcoder.com/files/20211017/530285728_1634456987429/%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6.jpg)

**慢开始**

把拥塞窗口 cwnd 设置为一个最大报文段MSS的数值。每经过一个**传输轮次**，拥塞窗口 cwnd 就翻倍。 设置一个慢开始门限ssthresh状态变量。

 当 cwnd < ssthresh 时，使用**慢开始算法**。

 当 cwnd > ssthresh 时，使用**拥塞避免算法**。

 当 cwnd = ssthresh 时，既可使用**慢开始算法**，也可使用**拥塞避免算法**。

**拥塞避免**

每经过一个**往返时间RTT**就把拥塞窗口cwnd大小加1。无论在慢开始阶段还是在拥塞避免阶段，只要发送方判断网络出现拥塞（其根据就是没有收到确认），就门限ssthresh设置为发送方窗口值的一半（但不能小于2），然后把拥塞窗口cwnd重新设置为1，执行**慢开始算法**。

**快重传**

要求接收方每收到一个报文段（包括失序的报文段）后就立即发出重复确认，使发送方及早知道有报文段没有到达对方。

发送方只要一连收到三个重复确认就应当立即重传对方尚未收到的报文段，而不必继续等待重传计时器到期。由于发送方尽早重传未被确认的报文段，因此采用快重传后可以使整个网络吞吐量提高约20%。

**快恢复**

当发送方连续收到三个重复确认，就会把门限ssthresh减半，接着把拥塞窗口cwnd值设置为ssthresh一样的值，然后开始执行**拥塞避免算法**。

## get和post

- get在浏览器回退时是无害的，而post会再次提交请求
- get请求会被浏览器存进历史记录，而post不会
- get请求只能进行url编码，而post支持多种编码方式
- get请求在url有长度限制(输入框大小)，而post没有
- 对参数的数据类型，get只接受ASCLL字符，而post没有限制
- get参数存放url中，post放在请求体中，gei比post不安全

## Http和Https

http默认端口号80，https默认端口号443

http明文传输，对外暴露，https使用TLS/SSL加密传输

https需要CA证书

## http请求方法

- 1.0
  - get
  - post
  - head；获取报文首部

- 1.1
  - put；上传文件
  - delete；删除服务器对象
  - connect；要求与代理服务器通信时建立隧道，使用隧道进行TCP通信
  - options；询问支持请求方式，用来跨域请求
  - trace；回显服务器收到的请求，用于测试
  - patch

## http版本

**http1.0**

使用非持久连接

**http1.1**

使用持久连接，多个http请求复用同一个TCP连接，以此避免使用非持久连接时每次需要建立连接的时延

新增请求方法

新增host字段

## CA

**签发**

用户想得到一份属于自己的证书，他应先向 CA 提出申请。在 CA 判明申请者的身份后，便为他分配一个公钥，并且将公钥与申请者的身份信息绑在一起，并为之签字后，便形成证书发给申请者 。

**认证**

如果一个用户想鉴别另一个证书的真伪，他就用 CA 的公钥对那个证书上的签字进行验证，一旦验证通过，该证书就被认为是有效的 。 

## HTTPS工作原理

浏览器与服务端建立TCP连接；

浏览器生成一个随机数，将自己支持的加密规则、支持的SSL版本、随机数发送给服务端；

服务器确认SSL版本和加密方式，生成一个随机数，并将自己身份信息以证书形式(网站地址、公钥、证书颁发即构)发回给浏览器；

浏览器验证证书是否有效，生成一个随机值，使用公钥对随机值加密发送给服务端；

服务器用私钥解密得到浏览器随机值，非对称加密结束；

双方使用三个随机数生成对称密钥，对称加密网页内容；

[https加密](https://blog.csdn.net/WoTrusCA/article/details/100105031)

## DNS

1. 浏览器缓存
2. OS缓存
3. hosts文件
4. 向本地域名服务器进行递归查询
5. 本地域名服务器迭代查询，根域名服务器、顶级域名服务器、权限服务器
10. 权限服务器告诉本地域名服务器所查询的主机的IP地址
11. 本地域名服务器最后把查询结果告诉主机

（1）递归查询：本机向本地域名服务器发出一次查询请求，就静待最终的结果。如果本地域名服务器无法解析，自己会以DNS客户机的身份向其它域名服务器查询，直到得到最终的IP地址告诉本机
（2）迭代查询：本地域名服务器向根域名服务器查询，根域名服务器告诉它下一步到哪里去查询，然后它再去查，每次它都是以客户机的身份去各个服务器查询。

## 进程和线程

进程是管理和分配资源的基本单位，是动态概念，竞争计算机系统资源的基本的单位

线程是进程的一个执行单元，是进程调度的实体

线程间共享本进程地址空间和资源，进程间相互独立

线程崩溃本进程跟着崩溃

创建线程比创建进程开销小，线程切换比进程切换开销小

## Unicode和UTF-8

Unicode是字符集

UTF-8是编码规则

# 加密与非对称加密

1、对称加密：加密与解密使用同一秘钥。

　　特点： 1、加密强度不高，但效率高; 2、密钥分发困难。

　　(大量明文为了保证加密效率一般使用对称加密)

　　常见对称密钥加密(共享密钥加密)算法：AES、DES(56位)、 3DES(112位)。

2、非对称加密：公钥加密，相应的私钥解密。

　　特点：加密速度慢，但强度高。

　　常见非对称密钥加密算法： RSA、DSA

# SSL加密

A将自己支持的SSL版本、支持的加密规则、生成的随机值发送给B；

B确认SSL版本和加密方式，生成随机值，以证书的形式包含公钥发送给A；

A确认证书是否有效，生成随机值用公钥加密发送给B；

B使用私钥解密获得随机值，用三个随机值生成堆成对话密钥