---
title: 白嫖SakuraFrp进行内网穿透
date: 2020-11-14T07:25:23+08:00    # 创建时间
lastmod: 2020-11-14T07:25:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["内网穿透", "frp", "SakuraFrp", "白嫖", "popular"]  # 标签
categories: ["技术"]              # 分类
author: "ddb"                  # 作者
---


## 简介

Sakura Frp 是一个免费的内网穿透frp服务端，frp请看另一篇博文[内网穿透神器frp](https://blog.dongdongbo.cn/2020_11_14_frp.html)

## 用途

1. 轻松搭建自己的 Minecraft 游戏服务器，让其他玩家与你一起联机玩耍。
2. 您可以使用 Sakura Frp 轻松搭建属于您自己的网站，而无需租用服务器。
3. 您可以使用 Sakura Frp 远程管理家中、公司的电脑，支持 Windows 和 Linux。

## 官网地址

https://www.natfrp.com

## 官方使用文档

https://doc.natfrp.com/

## 限速说明

普通流量可以通过签到获得，如果有普通流量，那么隧道限速10 Mbps。普通流量耗尽后只是临时限速到 4Mbps（免费用户）或 8Mbps（VIP用户）, 24小时后自动恢复限速并重置流量, 不会停止映射的隧道。

## 食用指南

### 注册登录

进入[Sakura Frp](https://www.natfrp.com)官网，注册并登录账号

### 创建隧道

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605310430581.png)

```
TCP 隧道
基础的 TCP 映射，适用于大多数服务，例如远程桌面、SSH、Minecraft、泰拉瑞亚等

UDP 隧道
基础的 UDP 映射，适用于域名解析、部分基于 UDP 协议的游戏等

HTTP 隧道
搭建网站专用映射，通过 80 端口访问。

HTTPS 隧道
带有 SSL 加密的网站映射，通过 443 端口访问，映射目标需要支持 SSL。

XTCP 隧道
客户端之间点对点 (P2P) 连接协议，流量不经过服务器，适合大流量传输的场景，两台设备上都需要运行客户端。
* XTCP 映射成功率并不高，成功与否取决于 NAT 设备的复杂度
```

### 配置客户端frp.ini

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605310478774.png)

下载frp，复制配置内容粘贴到frp.ini，并启动

### 使用官方客户端

[下载地址](https://www.natfrp.com/tunnel/download)

原理相同，Sakura Frp改的frp客户端，不过更简便了
