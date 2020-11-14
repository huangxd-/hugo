---
title: vercel+hugo免费搭建高性能个人博客 真香~
date: 2020-11-14T21:52:23+08:00    # 创建时间
lastmod: 2020-11-14T21:52:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["vercel", "hugo", "白嫖", "博客"]  # 标签
categories: ["技术"]              # 分类
author: "ddb"                  # 作者
---


## 简介

### vercel

[Vercel](https://vercel.com/)是前端团队的最佳工作流。所有功能包括静态和Jamstack部署，无服务器功能，和全局CDN。

### hugo

Hugo使用go编写，编译后只有一个二进制文件，不用安装任何依赖。Hugo生成静态网站的效率极高，在文章很多的情况下编译也很快。


## 食用指南

### 注册登录vercel

vercel支持直接用github注册登录

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605362463004.png)

但有一点请注意，vercel屏蔽了QQ邮箱，请尽量使用其他邮箱注册的github登录，如果没有办法，也可以直接发邮件给vercel的support，一般会在2天左右给与解决和回复。

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605362649882.png)

### 创建项目

点击Import Project
如果你的github上已经有hugo代码库，选择左边，可以直接导入【可以直接fork[我的代码库](https://github.com/huangxd-/hugo)，删除文章并配置属于你自己的配置文件config.toml】；

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605363018074.png)

如果没有，可以点击右边选择模板，选择hugo

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605363164104.png)

填上仓库名，这样就创建成功了，vercel会自动给你的github创建一个仓库

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605363224901.png)

由于是静态博客，你所有的文章都会存储在你刚才新建的仓库中。你可以选择将仓库下来修改后提交，也可以利用 Github 的在线编辑功能在线修改提交。提交之后 Vercel 会自动触发更新，重新构建并更新你的博客。

```
ps: hugo可以选择你想要的主题，进阶配置还有图床、评论、搜索、代码高亮等功能等你探索
```

### 域名绑定

使用 Vercel 搭建的网站，它会默认提供一个 *.vercel.app 的二级域名，你可以直接使用这个域名访问网站。如果你想要绑定自己的域名，也可以在后台设置。进入网站后选择自己的项目，选择 Settings - Domains 进入域名配置界面，在输入框中添加自己的域名。它会提示你需要给域名增加 A 记录或者是 CNAME 解析。按照提示添加后后台会自动检测是否生效。

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605363341432.png)

好了，个人博客已经搭好了，国内的访问速度也是杠杠的，是不是很香~