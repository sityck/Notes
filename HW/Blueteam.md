# 蓝队技巧

本章节用于记录红蓝对抗中的技巧点笔记

## Github相关

### 获取Github用户的个人信息

1. patch文件

   首先进入用户仓库中的任意一个commit：

   ![](G:\ired.team\Notes\img\Blueteam001.png)

   然后在进入后的页面URL后追加`.patch`，再次跳转后即可获取本次commit提交的用户邮箱：

   ![](G:\ired.team\Notes\img\Blueteam002.png)

2. 