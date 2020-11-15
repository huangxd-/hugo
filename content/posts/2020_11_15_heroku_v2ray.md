---
title: heroku搭建永久免费v2ray
date: 2020-11-15T14:13:23+08:00    # 创建时间
lastmod: 2020-11-15T14:13:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["heroku", "v2ray", "白嫖", "popular"]  # 标签
categories: ["技术"]              # 分类
author: "ddb"                  # 作者
---


## 简介

[Heroku](https://dashboard.heroku.com/) 是 Salesforce 旗下云服务商，提供方便便捷的各种云服务，如服务器，数据库，监控，计算等等。并且他提供了免费版本，这使得我们这些平时想搞一些小东西的人提供了莫大的便捷，虽然他有时长和宕机的限制，但是对于个人小程序来说已经足够了。

## 搭建v2ray

1. 点击如图的 “Sign Up”进行注册

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421281432.png)

2. 跳转到注册页面可以自行填写相关信息如下图，都填写完成后进行人机验证，然后点击“CREATE FREE ACCOUNT”. 没有人机验证请先爬墙.

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421332088.png)

3. 填写完以上注册信息，会跳转到另外一个页面，提示让您去您当时注册时使用的邮箱去验证账户.如下图所示:

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421353247.png)

4. 进入邮箱点击Heroku发送的验证链接,如图所示:

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421374426.png)

5. 点击后会要求您设置新的密码，这是设置您的新密码，同意它服务条款，最后点击登录.如下图所示:

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421392110.png)

6. 登录后会显示到您的后台了， 如下图所示, 这时候您需要把之前的github链接复制: https://github.com/xueliqq/v2ray-heroku/blob/master/README.md，然后新建一个无痕窗口，再粘贴上，然后再点击”Deploy to Heroku”如下图：

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421488339.png)

7. 如图会跳转到填写app名称, 这里名称可以任意写，没被占用就可以，服务器位置目前只有美国和欧洲，美国区的速度比欧洲的节点速度稍微快一些, 选择好了之后点击”Deploy app”. 然后会进入自动搭建，大概2分钟后，下面会显示5个绿色的勾，就说明已经搭建好了，这时候点击”Manage App” :

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421507600.png)

8. 随后会跳转到后台，然后咱们点击这个Settings, 再点击中间的Reveal Config Vars, 这样就可以看到 UUID，然后下拉就可以看到您的二级域名.

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421526935.png)

9. 然后咱们要用到的就是UUID和二级域名,如下图所示:

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421543396.png)

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421552556.png)

10. 到这里一个免费v2ray搭建完成了，在v2ray客户端或者ios小火箭或者其他把上面的二级域名和UUID都复制进去，注意二级域名后面的/ 和https://都要删除，否则会连不上网. 端口443， 额外ID:64, 别名任意写，传输协议ws(websocket),底层传输安全tls

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421643366.png)


## cloudflare反向代理

上面的节点其实已经可以使用了，但连接速度较慢，下面我们用cloudflare来增加访问速度。

1. 登录自己的[cloudflare](https://www.cloudflare.com/), 没有账户的自己注册一个，然后点击workers如下图:

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421817356.png)

2. 点击创建Worker如下图:

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421837719.png)

3. 选中左侧旧的脚本全部删除, 然后复制下方新的脚本，用Ctrl+V粘贴上

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421865957.png)

``` javascript
addEventListener(
  "fetch",event => {
    let url=new URL(event.request.url);
    url.hostname="vmess1991.herokuapp.com";
    let request=new Request(url,event.request);
    event. respondWith(
    fetch(request)
    )
  }
)
```

Ctrl+V 粘贴后，然后把V2ray里面刚才新建的二级域名复制进去.然后点击保存并部署.

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421987187.png)

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605421993924.png)

然后部署成功后返回一下，再复制生成的二级域名，然后粘贴到V2ray的伪装域名那里.

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605422025676.png)

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605422035469.png)

4. 下载[优选IP软件](https://proxy.freecdn.workers.dev/?url=https://github.com/badafans/better-cloudflare-ip/releases/latest/download/windows.zip)，以管理员身份运行CF优选IP.bat, 然后弹出优选窗口，在带宽大小那里输入数值，如果您时100M的带宽就输入20, 如果您是200M的带宽可以输入50， 如果您是300M的带宽可以输入100，这个具体您可以根据自己的速度大小去调试哦. （如果一直反复刷IP，请关掉防火墙).

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605422130892.png)

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605422145602.png)

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605422155306.png)

然后软件会自动跑程序去优选IP，整个过程大概需要2分钟左右，最终会优选出一个最好的IP，这是您把它复制到您的V2ray新建的服务器地址(address)那一栏，然后点击保存就可以了哦.然后把新建的这个V2ray节点设为活动服务器, 测试大功告成，看youtube 4K视屏爽歪歪啦.

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605422182078.png)