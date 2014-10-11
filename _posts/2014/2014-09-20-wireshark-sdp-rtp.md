---
layout: post
title: Wireshark分析SIP交换中的SDP及RTP的工作过程
categories:
- Programming
tags:
- SIP
---
 
### 一、SIP协议告知对方UDP端口号，协商媒体类型
0.0  整个通信过程分析图

![Mou icon](http://ww1.sinaimg.cn/large/637573b1gw1ekm90hpuo4j20kk0bbju3.jpg)

1.1  主叫方发给被叫方的INVITE请求

![Mou icon](http://ww4.sinaimg.cn/large/637573b1gw1ekm80zx8vrj20kj0a5jtn.jpg)

1.2  被叫方回给主叫方的180消息

![Mou icon](http://ww1.sinaimg.cn/large/637573b1gw1el653fadnzj20kk0aj76r.jpg)

### 二、RTP媒体流

2.1  主叫方发给被叫方的一个RTP包，UDP端口号是SDP协商好的，包的序列号是0

![Mou icon](http://ww4.sinaimg.cn/large/637573b1gw1ekm8m5uz36j20kk0al769.jpg)

第二个RTP包，包的序列号是0，其它一样

![Mou icon](http://ww2.sinaimg.cn/large/637573b1gw1ekm8owhyqqj20kk0abmz1.jpg)

### 三、RTCP包

3.1  每发完一批RTP包的时候，就发一个RTCP包，告诉接收方我刚才发了多少RTP包，多少个字节

![Mou icon](http://ww3.sinaimg.cn/large/637573b1gw1ekm8y9ou8tj20kk0abjtk.jpg)
