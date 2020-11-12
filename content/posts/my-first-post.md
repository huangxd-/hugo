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

## 第一段

``` python
import os

print('Process (%s) start...' % os.getpid())
# Only works on Unix/Linux/Mac:
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
```
### 第一小段

- [ ] Task item1
- [ ] Task item2
- [ ] ssssss3

### 第二小段

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605192089622.png)

## 第二段

``` java
public class OracleJdbcTest {
 
	String driverClass = "oracle.jdbc.driver.OracleDriver";
	
	Connection conn;
	
	public void init(FileInputStream fs) throws IOException, ClassNotFoundException, SQLException {
		Properties props = new Properties();
		props.load(fs);
		String url = props.getProperty("db.url");
		String userName = props.getProperty("db.userName");
		String passWord = props.getProperty("db.passWord");
		Class.forName(driverClass);
		conn = DriverManager.getConnection(url, userName, passWord);
	}
	
	public void fetch() throws SQLException {
		PreparedStatement ps = conn.prepareStatement("select sysdate from dual");
		ResultSet rs = ps.executeQuery();
		while(rs.next()) {
			//do the thing you do
		}
		rs.close();
		ps.close();
	}
	
}
```

##### Heading

``` shell
#!/bin/bash
 
#line=
 
touch $2
 
for line in `baicat $1`
do
    echo $line | sed 's/<p>//;s/<\/p>//' >>$2
 
done
```

![enter description here](https://gitee.com/huangxd/imges/raw/master/小书匠/1605190908571.png)