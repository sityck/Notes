# 笔记

## 杂项

### 破解Excel文件的密码

首先使用以下命令获取hash值：

`python /usr/share/john/office2john.py 2.xlsx >hack.txt`

然后使用John破解对应Hash值并获取密码：

`john hack.txt`

> 若出现以下报错，则有可能该文件曾被爆破成功过：
>
> `Using default input encoding: UTF-8
> Loaded 1 password hash (Office, 2007/2010/2013 [SHA1 128/128 AVX 4x / SHA512 128/128 AVX 2x AES])
> No password hashes left to crack (see FAQ)`
>
> 可以从该文件查看历史爆破结果：
>
> `cat ~/.john/john.pot`

