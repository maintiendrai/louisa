---
layout: post
title: Git fork workflow
categories:
- Programming
tags:
- git
---

朋友告诉我亚马逊有个免费一年的Linux主机，AWS，速度还不错，叫我试一下。只不过要先支付1美金。感觉跟VPS差不多。那我就试一下。在天朝，80端口被电信各个运营商给限了，自己有台服务器都要先备案，才能正常使用80端口。不折腾了，先这样吧。 做一件事情之前，总是要先找文档，看了亚马逊的文档，又不是母语，想快点，就找到了一个中文的BLOG，我就很信任地大干了起来。http://quanzhibaba.com/archives/315 就是这个文档。STEP1没啥问题，其实STEP2也没什么问题。我也选择了最基本的选项：“Basic 32-bit Amazon Linux AMI 1.0.” （话说在COMMUNITY AMI这地方点下去，是能选择UBUNTU的）嗯，但是我毕竟还是选了AMI的，貌似是REDHAT的系统。然后STEP3，安装编译PHP的时候，作者是这么做的：sudo yum install php。编译少了其它的一些项，给我后续的工作制造了不少的麻烦，正确应该这样 yum install php libmcrypt libmcrypt-devel php-mcrypt php-mbstring 。 STEP4!!!!!这里我特地分段了，作者居然把mysql托管到了RDS，其实数据库是不用托管的，直接在AMI里面安装就可以了，我照着作者的方法做了，结果托管后，按流量收费，被扣了4美元，血琳琳的教训啊。 接着看下正确的文档吧。外国人做学问就是严谨。http://calebogden.com/wordpress-on-linux-in-the-amazon-cloud-with-mac/ Thanks to @calebogden ， 当然还有@quanzhibaba。
