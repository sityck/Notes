# 蓝队技巧

本章节用于记录红蓝对抗中的技巧点笔记

## 应急



## 溯源

### Github相关

#### 获取Github用户的邮箱信息

1. patch文件

   首先进入用户仓库中的任意一个commit：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Blueteam001.png)

   然后在进入后的页面URL后追加`.patch`，再次跳转后即可获取本次commit提交的用户邮箱：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Blueteam002.png)

2. git log日志

   首先将需要溯源的github仓库clone到本地，然后输入`git log`即可获取邮箱：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Blueteam003.png)

3. Github-Email：

   Github用户信息收集工具：https://github.com/paulirish/github-email

### 社工信息收集相关

1. TG社工库机器人

2. 个人域名的备案信息

   [360威胁情报中心](https://ti.360.net/#/homepage)

   [站长之家](http://whois.chinaz.com/)

3. IP地址精准定位

   https://chaipip.com/aiwen.html

### 恶意文件分析

​	1.[微步](https://s.threatbook.cn/)

​	2.[哈勃](https://habo.qq.com/)