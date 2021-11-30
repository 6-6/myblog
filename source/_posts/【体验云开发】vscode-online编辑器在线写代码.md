---
title: 【体验云开发】vscode online编辑器在线写代码
date: 2021-11-29 17:49:44
tags: vscode
cover: https://7675-vuepress-7g6mefe5ad729c51-1258812673.tcb.qcloud.la/Image/2021/vscode-online/coding_vscode.png?sign=e8f902c51c3d0bee7fd42015ed3d3304&t=1638255251
categories: [New Toy]
---

## 前言
4399在线小游戏大家都玩过吧，通过浏览器就可以访问的小游戏。

云开发也是相同的体验，您可以通过手机、ipad、电脑任意一台设备，随时随地写代码。


## 服务器搭建vscode online
我有一台ubuntu server服务器，于是参考以下教程尝试安装：[[VS Code]在自己的Ubuntu服务器上构建VSCode Online](https://www.cnblogs.com/lee-li/p/12041546.html)。进行到yarn watch启动服务的时候，服务器直接卡主或者宕机。这时候我猜测是因为服务器配置太低了，查了下推荐运行的配置**4核8G内存**，于是乎放弃这个方案。


## coding.io版vscode
[coding.io](coding.io)是国内比较早做云IDE的，现在被腾讯收购了。我也用过他们早期版本的cloud studio，虽然问题多多，但是当时感觉是挺惊艳的。现在的cloud studio的IDE采用了vscode的方案，看了下版本是v1.56.1，而github版本的是v1.62.3。

正好我的服务器也是腾讯云，绑定好了就可以使用vscode来使用云主机中的资源了。目前我的hexo博客也是在上面完成的，十分的方便。

![cloud studio](https://7675-vuepress-7g6mefe5ad729c51-1258812673.tcb.qcloud.la/Image/2021/vscode-online/coding_vscode.png?sign=e8f902c51c3d0bee7fd42015ed3d3304&t=1638255251)


## Github版vscode
Github可以算是亲儿子了，毕竟被微软收购了，可以去[codespaces](https://github.com/features/codespaces)看看。**目前仅GitHub Team or GitHub Enterprise Cloud可使用**。

个人用户想体验下也是可以的，进入Github某个仓库的页面，点击键盘上的句号按钮```.```，可以触发快捷键跳转到```github.dev```，就可以看到以下界面。也可以把仓库的```.com```域名改为```.dev```我尝试了下终端命令，果然还是不能够使用的。

![github vscode](https://7675-vuepress-7g6mefe5ad729c51-1258812673.tcb.qcloud.la/Image/2021/vscode-online/github_vscode2.png?sign=8f9c9d281241d0b93886564feda9cb78&t=1638255389)


## 结语
体验过AirDroid、web photoshop到近期用过的kodbox，经过这几年web应用的变迁，功能越来越强大。这些技术的背后，是如何实现的？indexedDB持久化存储、沙箱机制、docker等，我还是知其一不知其二。让我感到好奇，想要了解，也许以后会尝试。