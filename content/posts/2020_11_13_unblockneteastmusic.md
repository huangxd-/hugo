---
title: 网易云音乐解锁灰色歌曲
date: 2020-11-13T22:56:23+08:00    # 创建时间
lastmod: 2020-11-13T22:56:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["音乐", "解锁", "popular"]  # 标签
categories: ["技术"]              # 分类
author: "ddb"                  # 作者
---


## github开源仓库

[源码库](https://github.com/nondanee/UnblockNeteaseMusic)

## 特性

 1. 使用 QQ / 虾米 / 百度 / 酷狗 / 酷我 / 咪咕 / JOOX 音源替换变灰歌曲链接 (默认仅启用一、五、六)
 2. 为请求增加 X-Real-IP 参数解锁海外限制，支持指定网易云服务器 IP，支持设置上游 HTTP / HTTPS 代理
 3. 完整的流量代理功能 (HTTP / HTTPS)，可直接作为系统代理 (同时支持 PAC)

## 安装

### 安装nodejs

这里不再赘述，请自行google

### 下载

``` shell
git clone https://github.com/nondanee/UnblockNeteaseMusic
```

### 启动

``` shell
node app.js -e https://music.163.com -p 5111:8081
```

### 配置服务

``` shell
vim /etc/systemd/system/UnblockNeteaseMusic.service

[Unit]
Description=UnblockNeteaseMusic
After=network.target
Wants=network.target

[Service]
Type=simple
PIDFile=/var/run/UnblockNeteaseMusic.pid
ExecStartPre=cd /www/server/UnblockNeteaseMusic
ExecStart=/bin/bash -c "cd /www/server/UnblockNeteaseMusic && /usr/bin/node app.js -e https://music.163.com -p 5111:8081"
RestartPreventExitStatus=23
Restart=always

[Install]
WantedBy=multi-user.target
```

这样就可以通过service UnblockNeteaseMusic start/stop/restart来控制了

### 配置自启动

``` shell
vim /etc/rc.d/rc.local
/usr/sbin/service UnblockNeteaseMusic restart
```

### IOS/Android/Mac/Windows客户端配置

请参考：

[食用指南](https://github.com/nondanee/UnblockNeteaseMusic/issues/22)
[iOS 食用指南](https://github.com/nondanee/UnblockNeteaseMusic/issues/65)
[Mac版指南](https://github.com/nondanee/UnblockNeteaseMusic/issues/597)
[进阶配置](https://github.com/nondanee/UnblockNeteaseMusic/issues/48)

### 公益节点

Shadowrocket

👉[导入节点](shadowrocket://add/sub://aHR0cHM6Ly9sb2xpY28ubWUvc3Vic2NyaWJlL1NoYWRvd3JvY2tldC9zZXJ2ZXIudHh0#%F0%9F%8E%B8%E7%BD%91%E6%98%93%E4%BA%91%E9%9F%B3%E4%B9%90)

👉[导入配置](shadowrocket://config/add/https://lolico.me/subscribe/Shadowrocket/rules.conf)

QuantumultX

👉[导入节点](https://lolico.me/subscribe/QuantumultX/NeteaseMusicServer.txt)

Loon

👉[导入节点](https://api.dler.io/sub?target=loon&url=https%3A%2F%2Flolico.me%2Fsubscribe%2FShadowrocket%2Fserver.txt)

## 效果图

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605281531830.png)

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605281574101.png)
