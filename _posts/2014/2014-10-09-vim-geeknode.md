---
layout: post
title: Vim写博客同步到evernote
categories:
- Programming
tags:
- Tools
---

   
>   通过VIM写博客通过geeknote同步到evernote

Vim/vim-markdown/markdown preview plus < -> GeekNote -> Evernote

不需要任何第三方服务器的方案，就是使用Vim编写markdown。
给Vim配上markdown着色的插件vim-markdown。

mac os下默认的python是narrow build path

python 解决 narrow build path 导致的 ValueError: unichr() arg not in
range(0x10000) (narrow Python build)

    ./configure --enable-unicode=ucs4 --prefix=/usr

给chrome装上实时查看markdown效果的插件markdown preview plus，这插件非常好用！特
别是开启了“Enable auto-reload”之后，每次用Vim保存文件时都能马上刷新。
然后用类似上一个方案的用法使用GeekNote，生成富文本同步到Evernote。
为了更方便的使用GeekNote，可以在Dropbox里新建一个evernotebank的文件夹，然后按照
你想同步的notebook的名字新建同名文件夹；
比如我在evernotebank里建立blog.chetui.org的文件夹，并在该目录下建立如下脚本，命
名为sync：

	#!/bin/bash
	
	allFiles() {
	    for file in $1/*
	    do
	        if [ -d $file ]; then
	            echo $file
	            /Users/lilkr/Tools/hack/geeknote/geeknote/gnsync.py --path $file --mask "*.md" --format markdown --logpath log--notebook "goandfight"
	            allFiles $file
	        fi
	    done
	}
	
	testdir=./louisa/_posts/
	allFiles $testdir

然后加入可执行权限：

	chmod +x sync
	
每次在该目录下写完md格式的文件，就跑一下./sync，就把富文本同步到Evernote里了。
单向编辑的缺陷实在是很难解决，因为markdown生成富文本只能是单向的，而Evernote官方
又没有打算内置markdown。

