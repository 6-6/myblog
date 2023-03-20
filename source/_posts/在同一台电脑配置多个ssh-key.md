---
title: 在同一台电脑配置多个ssh-key
date: 2022-07-06 17:04
tags: git
cover: https://s2.loli.net/2023/03/20/lYkWxLz5stQMKAh.png
categories: [版本管理]
feature: true
urlname: sshkey-Multi-platform
---

这里主要说的是GitHub和Coding平台。ssh是常见的连接两台服务器（或者客户端）方式，这里只说我遇到的GitHub和Coding平台上添加ssh-key的问题，电脑是windows 11。

# 基础配置
```sh
git config --global user.name "xxx"           //配置用户名
git config --global user.email "xxx"          //配置注册邮箱
git config --global --list                    //查看当前用户（global）配置
```

# 创建公钥

## 生成github的公钥
powershell或者git bash都可以操作命令，最好是使用管理员打开终端工具。
```sh
ssh-keygen -t rsa -b 4096 -C "xxxxxxxx@email.com"
```
在提示 `Enter passphrase` 和 `Enter same passphrase again ：` 都按回车即可
在提示`“Enter file in which to save the key”`（保存在哪） 时输入 github_rsa，这样就可以保存密钥。

## 生成 coding_rsa公钥
```sh
ssh-keygen -m PEM -t rsa -b 4096 -C "xxxxxxxx@email.com"
```
在提示 `Enter passphrase` 和 `Enter same passphrase again ：` 都按回车即可
在提示`“Enter file in which to save the key”`（保存在哪） 时输入 coding_rsa，这样就可以保存密钥。

# 配置文件config
## 查看.ssh目录下生成的文件
![image.png](https://s2.loli.net/2023/03/20/jWvGTAKIX5VhQFt.png)
1. id_rsa：保存私钥
2. id_rsa.pub：保存公钥
3. authorized_keys：保存已授权的客户端公钥
4. known_hosts：保存已认证的远程主机ID，用于client端验证server端
    
## 添加config
创建一个名为config的文件在`~/.ssh`，将以下配置粘贴保存。

```
# coding
Host *.coding.net
  HostName e.coding.net
  HostkeyAlgorithms +ssh-rsa
  PubkeyAcceptedAlgorithms +ssh-rsa
  IdentityFile ~/.ssh/coding_rsa

# github
Host github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/github_rsa
```

如果是修改过密钥的名称，不仅需要在config中修改路径，还得需要执行以下命令：
```sh
# ssh agent 程序可以根据配置自动加载并管理这些密钥
ssh-agent bash
# `ssh-add` 命令将某个私钥交给 ssh-agent 保管
ssh-add -k ~/.ssh/github_rsa
# 显示ssh-agent中的密钥
ssh-add -l
```

# 添加ssh-key
打开coding_rsa.pub，将里面的内容全部复制到以下粘贴，GitHub也是类似的操作。
![image.png](https://s2.loli.net/2023/03/20/ZmbwQG2rSOtyLAU.png)

# 验证ssh是否成功
GitHub验证
```sh
ssh -T git@github.com
```

Coding验证
```sh
ssh -T git@git.coding.net
```


# 常见问题
很多奇怪的问题都有可能出现，我解决的方式是验证有可能出错的地方。如果没有问题，重启系统，更换网路，甚至不行尝试删除`.ssh`中的所有文件重新开始。

## 1. Permission denied (publickey)
```sh
git@github.com: Permission denied (publickey). fatal: Could not read from remote
```
**分析：** 

以上报错，经常在添加ssh-key的时候遇到。字面意思理解就是“拒绝访问（公钥相关问题）无法从远端获取”。首先要理解为什么会报这个错，大部分错误追踪溯源都可以解决。

通过[图解SSH原理](https://www.jianshu.com/p/33461b619d53)这篇文章可以了解ssh的原理。简单知晓就是add ssh-key是采用的非对称加密，add ssh-key在平台上的是公钥，本机上的github_rsa存储的是私钥。client和server之间的数据交互，比如git pull（公钥加密，私钥解密）和git push（私钥加密，公钥解密），需要两者配合使用。

**解决：** 

带日志的验证方式
```sh
# GitHub
ssh -Tv git@github.com
# Coding
ssh -Tv git@git.coding.net
```

![image.png](https://s2.loli.net/2023/03/20/mPH1VLxQZ2G74od.png)

显示ssh-agent中的密钥
```sh
ssh-add -l
```

通过以上命令发现密钥是同一个没问题
![image.png](https://s2.loli.net/2023/03/20/SpmtWeyrRxYqi8D.png)

![image.png](https://s2.loli.net/2023/03/20/71ulCYPaTAGc8No.png)

逐一排查问题，如果是之前生成过，就删掉.ssh文件夹里面生成的文件。通常都是路径不对之类的问题，重新按照步骤走一遍就好了。我的遇到问题是config配置的项有问题，删掉无效配置项即可。
    
## 2. fatal: Could not read from remote repository.

```sh
kex_exchange_identification: Connection closed by remote host
Connection closed by 81.69.155.167 port 443

Please make sure you have the correct access rights
and the repository exists.
```

22端口不行之后，我换了443端口也是不成功。可以通过 `ping 81.69.155.167`测试一下IP地址是否可以正常连接，如果不行确实是网络问题。我尝试换了个手机热点网络依然不行，切换回来原来的网络，竟然又可以了？

    
# 参考
1. [图解SSH原理](https://www.jianshu.com/p/33461b619d53)
2. [同一台机器配置多个SSH，同时绑定Coding,Github和Gitee](https://blog.csdn.net/Mr__Shen/article/details/105346707)
3. [github提示Permission denied (publickey)，如何才能解决？](https://www.zhihu.com/question/21402411?utm_source=wechat_session)
