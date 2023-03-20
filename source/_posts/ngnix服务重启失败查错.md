---
title: ngnix服务重启失败查错
date: 2021-07-27 14:37:51
tags: ngnix
cover: https://s2.loli.net/2023/03/20/3XwlkpybZxdWhOG.png
categories: [服务器]
urlname: The-NGNIX-service-failed
---

## 问题：
使用以下命令重启ngnix失败。
```
sudo service nginx restart
```

![](https://s2.loli.net/2023/03/20/3XwlkpybZxdWhOG.png)

报错信息：
> Nginx: Failed to start A high performance web server and a reverse proxy server


## 分析：
使用以下命令，查找指定错误。
```
nginx -t -c /etc/nginx/nginx.conf
```

![](https://s2.loli.net/2023/03/20/bdLC9jY6tJcqyfV.png)

通过输出的结果，原来是nginx.conf 配置项有一个属性重复定义了

## 解决：
找到指定目录下的文件修改，删掉一个即可。

```
vim /etc/nginx/nginx.conf
```