---
layout: post
title: Objective-C转向Swift若干tips
categories:
- Study
tags:
- swift 
---

## Objective-C转向Swift若干tips

- Swift 的 playground 就像是一个可交互的文档，它是用来练手学swift的，写一句代码出一行结果（右侧），可以实时查看代码结果，是学习swift语言的利器
- 不分头文件和实现文件，而是集合到.swift文件
- swift中无需使用代码分隔符，以行作为代码分隔，如一行内有多行代码，则各行代码之间同样需使用分号;分隔
- swift没有main函数，其代码是至上而下运行，第一行代码即为程序入口
- 类型对象化（类似于java） Int、Float、Double、Bool、Character
- let 定义常量； len pi = 3.14
- var hello :NSString ?
- - var 定义变量var hello :NSString = @“test”
- - : 指定变量类型
- - ? 表示optional，即该变量可能为nil; 调用时必须加? 如 hello?.length
- - ! 表示该变量一定不为nil，否则crash
- - @”hello”不存在了,变回了”hello” 如var hello :NSString = "hello"
- 可以使用+来拼接字符串 "hello"+"world"
- 使用()可以在字符串插入变量 let lang = "swift"; "hello (lang) world"
- class 定义类
- func 定义函数
	func myTest(xxx …) -\> 返回类型
- println 带换行的print
- as 类型转换 “当作”

