---
title: 使用腾讯云函数实现每日签到
date: 2020-11-14T08:01:23+08:00    # 创建时间
lastmod: 2020-11-14T08:01:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["腾讯云", "云函数", "白嫖", "签到", "popular"]  # 标签
categories: ["技术"]              # 分类
author: "ddb"                  # 作者
---


## 简介

云函数 (Serverless Cloud Function，SCF) 是腾讯云为企业和开发者们提供的无服务器执行环境。无服务器并非真的没有服务器，而是说用户无需购买服务器，无需关心服务器 CPU、内存、网络配置、资源维护、代码部署、弹性伸缩、负载均衡、安全升级、资源运行情况监控等，也就是说不用专门安排人力做这些，只需专注于代码编写并上传即可。很大程度上降低了研发门槛，提升业务构建效率。

由于 Serverless 拥有近乎无限的扩容能力，核心的代码片段完全由事件或者请求触发，平台根据请求自动平行调整服务资源，用户只需为运行中的云函数付费，若云函数未运行，则不产生任何费用。

## 免费额度

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605359668605.png)

## 食用指南

### 创建云函数

1. 登录[云函数控制台](https://console.cloud.tencent.com/scf)，点击左侧导航栏「函数服务」，在函数服务页面上方选择地域，单击「新建」，如下图所示：

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605359790476.png)

2. 在「新建函数」页面填写函数名称，选择「运行环境」，控制台目前已支持的语言包括：Python 2.7 & 3.6、Node.js 6.10 & Node.js 8.9、Node.js 10.5、Java 8、Php 5 & Php 7。例如，我们选择运行环境：Python 3.6 ，选择空白模版函数创建，之后点击「下一步」：

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605359935157.png)

3. 在index.py输入你的签到脚本

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605360068687.png)

4. 点击高级设置，一般只要设置内存和超时时间

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605360147161.png)

5. 创建完成后，在函数管理可以修改函数代码，点击测试可以直接运行测试

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605360344604.png)

### 创建触发器

在已创建函数的详情页面，选择左侧「触发管理」，单击「创建触发器」，在弹出的「创建触发器」窗口中，将触发方式设置为「定时触发」，可以选择触发周期或者cron自定义

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605360417799.png)

## 签到脚本

[serverless_checkin github地址](https://github.com/huangxd-/serverless_checkin)

### 机场签到

``` python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

"""
@Time ： 2020/10/23 9:52
@Auth ： huangxd-
@Des : sspanel自动每日签到脚本
@File ：sspanel_qd.py
@IDE ：PyCharm
@Motto：Another me.
sspanel自动每日签到脚本，基于项目https://github.com/zhjc1124/ssr_autocheckin修改
"""
import requests
import re

requests.packages.urllib3.disable_warnings()

class SspanelQd(object):
    def __init__(self):
        # 机场地址
        self.base_url = 'https://xxx.xxx.xxx'
        # 登录信息
        self.email = ''
        self.password = ''
        # wxpusher Token
        self.appToken = ''
        # wxpusher topicId
        self.topicId = ''

    def checkin(self):
        email = self.email.split('@')
        email = email[0] + '%40' + email[1]
        password = self.password

        session = requests.session()

        session.get(self.base_url, verify=False)

        login_url = self.base_url + '/auth/login'
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36',
            'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
        }

        post_data = 'email=' + email + '&passwd=' + password + '&code='
        post_data = post_data.encode()
        response = session.post(login_url, post_data, headers=headers, verify=False)

        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36',
            'Referer': self.base_url + '/user'
        }

        response = session.post(self.base_url + '/user/checkin', headers=headers, verify=False)
        msg = (response.json()).get('msg')
        print(msg)

        info_url = self.base_url + '/user'
        response = session.get(info_url, verify=False)
        """
        以下只适配了CordCloud主题
        """
        try:
            used = re.findall(r'已用 \d+\.\d+% (\d+\.\d+\w+)', response.text)[0]
            rest = re.findall(r'剩余 \d+\.\d+% (\d+\.\d+\w+)', response.text)[0]
            expire = re.findall(r'(\d{4}-\d{1,2}-\d{1,2} \d{1,2}:\d{1,2}:\d{1,2})', response.text.split("等级过期时间")[1])[0]
            msg = "- 今日签到信息："+str(msg)+"\n- 已用流量："+str(used)+"\n- 剩余流量："+str(rest)+"\n- 到期时间："+str(expire)
            print(msg)
            return msg
        except:
            return msg

    # wxpusher推送
    def wxpusher_send(self, msg):
        if "续命过了" not in msg:
            url = "http://wxpusher.zjiecode.com/api/send/message"
            params = {"appToken": self.appToken, "topicId": self.topicId, "content": "CordCloud签到：\n\n" + msg}
            response = requests.get(url=url, params=params).text
            print(response)

    def main(self):
        msg = self.checkin()
        self.wxpusher_send(msg)

# 云函数入口
def main_handler(event, context):
    run = SspanelQd()
    run.main()

if __name__ == '__main__':
    run = SspanelQd()
    run.main()

```

### 京东签到

``` python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

"""
@Time ： 2020/10/23 9:52
@Auth ： huangxd-
@Des : 京东签到脚本
@File ：jd_qd.py
@IDE ：PyCharm
@Motto：Another me.
京东每日签到脚本
"""

import datetime
import pytz
import requests
import re
import time

requests.packages.urllib3.disable_warnings()

class JdQd(object):
    def __init__(self):
        # 京东签到地址
        self.base_url = 'http://sfly.vip:8000/JdSigns'
        # wxpusher Token
        self.appToken = ''
        # wxpusher topicId
        self.topicId = ''
        # jd token
        self.token = ''

    def checkin(self):

        params = {"jdcookie": self.token}
        msg = requests.get(url=self.base_url, params=params).text
        print(msg)
        return msg

    # wxpusher推送
    def wxpusher_send(self, msg):
        print("成功," in msg)
        print("失效" in msg)
        tz = pytz.timezone('Asia/Shanghai') #东八区
        t = datetime.datetime.fromtimestamp(int(time.time()), pytz.timezone('Asia/Shanghai')).hour
        print(t)
        if ("成功," in msg  or "失效" in msg) and t in (0, 8, 12, 20):
            url = "http://wxpusher.zjiecode.com/api/send/message"
            params = {"appToken": self.appToken, "topicId": self.topicId, "content": "京东签到：\n\n" + msg}
            response = requests.get(url=url, params=params).text
            print(response)

    def main(self):
        msg = self.checkin()
        self.wxpusher_send(msg)

# 云函数入口
def main_handler(event, context):
    run = JdQd()
    run.main()

if __name__ == '__main__':
    run = JdQd()
    run.main()
```

### 吾爱破解签到

``` python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

"""
@Time ： 2020/11/11 21:08
@Auth ： huangxd-
@Des : 吾爱破解每日签到
@File ：wuaipojie_sign.py
@IDE ：PyCharm
@Motto：Another me.
吾爱破解每日签到
"""
import requests 
from bs4 import BeautifulSoup

requests.packages.urllib3.disable_warnings()

class WuaiQd(object):
    def __init__(self):
        # cookie
        self.cookie = ''
        # wxpusher Token
        self.appToken = ''
        # wxpusher topicId
        self.topicId = ''

    def checkin(self):
        headers={
            'Cookie': self.cookie,
            'ContentType':'text/html;charset=gbk'
        }
        requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=apply&id=2',headers=headers)
        a=requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=draw&id=2',headers=headers)
        print(a.text)
        b=BeautifulSoup(a.text,'html.parser')
        c=b.find('div',id='messagetext').find('p').text

        msg = ""
        if "您需要先登录才能继续本操作"  in c: 
            msg = "Cookie失效"
        elif "恭喜"  in c:
            msg = "吾爱破解签到成功"
        else:
            msg = "吾爱破解签到失败"
        print(c)
        return msg

    # wxpusher推送
    def wxpusher_send(self, msg):
        url = "http://wxpusher.zjiecode.com/api/send/message"
        params = {"appToken": self.appToken, "topicId": self.topicId, "content": "吾爱破解签到：\n\n" + msg}
        response = requests.get(url=url, params=params).text
        print(response)

    def main(self):
        msg = self.checkin()
        self.wxpusher_send(msg)

# 云函数入口
def main_handler(event, context):
    run = WuaiQd()
    run.main()

if __name__ == '__main__':
    run = WuaiQd()
    run.main()
```

## 网站token获取

在这里只是简单说明下获取token的两种方法，具体请google

1. chrome或者其他浏览器打开console调试界面，并选择network标签，经过打开网页之后，查看请求中的cookie
2. ios请使用网球（HTTP Catcher）、锤子（Thor）等软件进行抓包

## WxPusher token获取

请查看另一篇[博客](https://blog.dongdongbo.cn/2020_11_14_wxpusher.html)
