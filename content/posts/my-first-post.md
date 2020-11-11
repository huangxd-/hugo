---
# 常用定义
title: "An Example Post"           # 标题
date: 2018-01-01T16:01:23+08:00    # 创建时间
lastmod: 2018-01-02T16:01:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["tag-1", "tag-2", "tag-3", "popular"]  # 标签
categories: ["index"]              # 分类
author: "ddb"                  # 作者
---

# 第一段

``` python
headers={
	'Cookie': self.cookie,
	'ContentType':'text/html;charset=gbk'
}
requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=apply&id=2',headers=headers)
a=requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=draw&id=2',headers=headers)
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
```
### 第一小段

- [ ] Task item1
- [ ] Task item2
- [ ] ssssss3

### 第二小段

![enter description here](http://qiniu.dongdongbo.cn/小书匠/1605107384652.png)

# 第二段

{% codeblock [title] [lang:python] [url] [link text] %}
requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=apply&id=2',headers=headers)
a=requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=draw&id=2',headers=headers)
b=BeautifulSoup(a.text,'html.parser')          
c=b.find('div',id='messagetext').find('p').text
{% endcodeblock %}

##### Heading

requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=apply&id=2',headers=headers)
a=requests.session().get('https://www.52pojie.cn/home.php?mod=task&do=draw&id=2',headers=headers)
b=BeautifulSoup(a.text,'html.parser')          
c=b.find('div',id='messagetext').find('p').text