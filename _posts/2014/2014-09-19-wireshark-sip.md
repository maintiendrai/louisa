---
layout: post
title: Wireshark分析SIP协议
categories:
- Programming
tags:
- SIP
---
## Overview


SIP 是VOIP目前非常流行的一种协议。有关协议的详细原理参照相关文档。

SIP 组件
1.User Agent
UA 是 SIP 的基本组件，可分为 UAC（User Agent Client）和 UAS（User Agent Server）。
发起呼叫的为 UAC（X-Lite:iP 192.168.61.221），接收呼叫的为 UAS（本文手机端选用LinPhone:iP）。很多设备都可做 UA，如 IP 电话、PC、路由器等。
2.SIP server
SIP server（本文选用Asterisk）实现了Proxy Server（代理服务器）、Redirect Server（重定向服务器）、注册服务器的功能（Registrar Server）、Location Server、Back-to-back user agent (B2BUA)、Presence Server。
      
本文通过wireshark抓包分析SIP user agent（用户代理客户机，uac）与SIP serve之间的交互过程，在拨打SIP电话之前，先需要搭建相应的环境：    
 
 ----------
 
### Wireshark分析Account注册过程

设置过滤条件，只catch从UAC（ip.addr == 192.168.61.221 && sip）发出或接收的数据包。
![Mou icon](http://ww3.sinaimg.cn/large/637573b1jw1eklahsn3myj20vl0180t4.jpg)
![Mou icon](http://ww1.sinaimg.cn/large/637573b1jw1eklafbmponj20yn0bh77o.jpg)
1.首先UAC向SIP serve(112.124.57.23)发出REGISTER信息

	REGISTER:UA client 使用此 message 向 server 注册以标明自己的位置。
SIP电话的格式是：

    sip:username@UAC_IP 
    sip:username@UAC_IP_DNS （如果网络中有DNS服务器，并配置了UAC_IP对应的域名UAC_IP_DNS ）
 如上图所示，sip电话格式为：
sip:100@112.124.57.23   或   sip:100@lilkr（配置了DNS服务器）
2.sip server请求要求身份验证。
![Mou icon](http://ww1.sinaimg.cn/large/637573b1jw1eklajooh9aj20tx0bsdj6.jpg)
3.重复1

4.sip server响应，发送200 OK信息
![Mou icon](http://ww4.sinaimg.cn/large/637573b1jw1eklaid9b81j20xn0cpq6m.jpg)
5.UAC向SIP server(112.124.57.23)发出SUBSCRIBE信息

	SUBSCRIBE:告诉 server 一旦发生特定事件时，愿意接收一个通知。

![Mou icon](http://ww3.sinaimg.cn/large/637573b1jw1eklak5guwkj20wz0czn0t.jpg)

### Wireshark分析UAC跟UAS交互过程
1.UAC向UAS拨打电话，UAS拒绝
![Mou icon](http://ww2.sinaimg.cn/large/637573b1jw1eklaks830lj20xf05ttba.jpg)

	INVITE:UAC 发送此信息用以邀请 UAS 加入会话（包择一对一通话或 conference），其实就是一个 call setup message。
	ACK:为 INVITE 回复一个确认信息。	
SIP Response:

![Mou icon](http://ww4.sinaimg.cn/large/637573b1jw1eklbanaxyaj20ae039aa7.jpg)UAS直接挂断，UAC收到486.然后UAC给Sip Server发一个ACK
2.UAC向UAS拨打电话，UAC自己挂断
![Mou icon](http://ww1.sinaimg.cn/large/637573b1jw1eklalabfl5j20we06twhi.jpg)

	CANCEL:用来中止一个还没建立（在建立过程当中）的呼叫。
SIP Response:

![Mou icon](http://ww3.sinaimg.cn/large/637573b1jw1eklbbu5c1jj20aq02haa2.jpg)

UAC直接挂断，给Sip Server发一个CANCEL，Sip Server给UAC反馈状态码487，并反馈状态码200.UAC再反馈一个ACK

3.UAC向UAS拨打电话,进行通话
![Mou icon](http://ww4.sinaimg.cn/large/637573b1jw1eklam4mg8qj20yw09saf2.jpg)
ACK之后为完全接通，对方挂断的话Sip Server重新向UAC发送INVITE

VOIP整个Flow分析图：
![Mou icon](http://ww1.sinaimg.cn/large/637573b1jw1eklamkks4lj20wq0ffwk4.jpg)
