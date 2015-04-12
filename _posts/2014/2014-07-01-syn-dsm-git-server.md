---
layout: post
title: 群晖DSM上安装Git Server
categories:
- 折腾
tags:
- DSM GIT
---


给WD、TOSHIBA开发的无线硬盘APP，也给自己公司开发的WiDiSK无线硬盘APP。都是基于WebDAV的。想知道别人的NAS的APP做的怎么样本来想用旧台式机装黑群晖，但是考虑到功率的问题，我还是选择了群晖的 nas 解决方案以及附带的 DSM 系统。并在手机上装APP进行体验。

群晖 nas 的系统 DSM 是基于 Linux 的，因此你也可以把 nas 看作一台小型的 Linux 服务器。DSM 提供了很多实用的套件供你开发 nas 的潜力，包括 PHP、Python、MySQL、Wordpress 等，同时也支持 Git 服务器。

今天这篇文章就讲讲如何使用群晖 nas 搭建私有 git 服务器。

###为什么要搭建私有 git 服务器

这个不多说了，不想搭就不用来看这文章了
以前用gitlab在ubuntu上搭过git服务器。

###nas 搭建 git 服务器

1. 进入 DSM 系统的『套件中心』，安装 Git Server

2. 创建一名用户，名为『git』（名称随意，但是叫 git 比较好，该用户仅有 git shell 的执行权限，除此之外无任何权限）在用户界面创建新用户git，没有特殊权限，放到users组即可
3. 在『控制面板』- 『终端机』中开启 SSH 登录功能

4. 用ssh的root用户登录到群晖

	vi /etc/passwd 文件
	修改git用户的home目录
	
		/var/services/homes/git to /volume1/git

	修改登录后的 shell
	
		/sbin/nologin to /bin/ash

	修改git用户home目录的权限为777
		
		chmod 777 /volume1/git/

	再在/volume1/git/创建一个.ssh文件夹,还是权限777

	将你Mac当前用户下的.ss/id_rsa.pub中的内容copy / paste到群晖上面~git/.ssh/authorized_keys里面
	
	切换为git用户:(测试一下)
	
		su – git
	
	On Mac:(测试一下)

		ssh git@192.168.199.118 ls /etc/shells
	
	输出正常的话就可以成功ssh了
	
5. 创建一个git仓库

		su - git
		pwd  /volume1/git
		mkdir repositories
		
6. 在 repositories 文件夹中创建一个新的文件夹，作为某一个项目的 base，我们叫他 wifidisk

		mkdir wifidisk
		
7. 已经在本地初始化好了项目
		
		cd wifidisk
		git init --bare .git
		
	回到本地，然后在本地使用如下命令加本地代码 push 到 git 服务器
	
		git remote add nas ssh://git@192.168.199.118/volume1/git/repositories/ihome/.git
		
		
8. 遗憾的是没有gitlab那样的web控制台
	
参考资料

1. http://undefinedblog.com/deploy-private-git-server-on-synology-nas/
2. http://www.feeltrue.cn/wordpress/?p=17
3. http://www.cnblogs.com/softman11/p/3443229.html