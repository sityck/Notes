# 获取微信小程序源码包

本章节用于介绍获取微信小程序源码包的几种方式及其流程。在微信小程序中，如果涉及到加解密的破解、敏感信息泄露的检查；反编译出源码包是必不可少的。

## 获取原二进制包

首先，我们需要拿到微信小程序的原代码包，该代码包实际上是个二进制文件，由微信按照其格式生成，其后缀为wxapkg。

> *关于微信小程序wxapkg包的结构分析可参考该文章：https://lrdcq.com/me/read.php/66.htm*

微信小程序在PC和手机上都能使用，所以我们分别在这两种场景下尝试找到对应的源码包，需要注意的是，由于依赖或者兼容性的问题，PC版的微信可能打不开一些小程序，所以建议还是用手机模拟器获取对应的报文。

### PC小程序包获取

1. 打开微信，在设置页面确认微信的文件存储位置：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx1.png)

2. 进入存储位置的Applet目录，其中以wx开头的目录即代表着一个小程序。

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx2.png)

3. 这里建议删除该目录下的所有wx开头的目录，然后重新打开目标小程序，进入新生成的小程序目录，找到目标源码包。

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx3.png)

### 模拟器小程序包获取

1. 安装网易MUMU模拟器，并在设置中打开ROOT权限：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx4.png)

2. 安装RE文件管理器，并授权ROOT权限，然后进入到/data/data/com.tencent.mm/MicroMsg目录。查找${userid}目录，该目录通常是一串类似MD5值的字符串，长度为32位。

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx5.png)

3. 进入该目录的appbrand/pkg目录，建议删除掉里面已有的wxapkg包，重新加载目标微信小程序，重新生成的wxapkg文件即为目标文件。

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx6.png)

## 解密小程序包

对于微信小程序包，通常还会做一层加密防护，所以我们在进行反编译动作前，需要先解密对应的小程序包，本文介绍两个解密工具，哪个可以使用用哪个即可。如果第一个报找不到APPID，就试下第二个。

### UnpackMiniApp.exe

下载对应的exe文件后，按照使用说明进行操作即可。

![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx7.png)

### wechatMiniAppReverse

该项目结合了解密和加密能力，直接参考项目的README进行操作即可。项目地址如下：

https://github.com/superBiuBiuMan/wechatMiniAppReverse

## 反编译小程序包

最后一步即获取小程序源码信息，除了上一章节介绍的wechatMiniAppReverse，大家通常还使用wxappUnpacker工具，另外还有个unveilr项目也支持反编译小程序包。

### wxappUnpacker

项目地址：

https://github.com/Tech-Chao/wxUnpacker或https://gitee.com/ksd/wxappUnpacker

使用方法：

按照项目README中提示的依赖安装对应组件，然后按照以下两个命令操作即可：

1. 解包主包 `./bingo.sh testpkg/master-xxx.wxapkg`
2. 解包子包 `./bingo.sh testpkg/sub-1-xxx.wxapkg -s=../master-xxx`

如果遇到报错缺少模块，使用npm安装对应模块即可；

如果遇到提示为子包，无法解包，请先解出主包，然后使用解包子包的命令操作。

### unveilr

该工具还在迭代更新中，暂未使用过。

https://github.com/r3x5ur/unveilr

------

在完成上述操作后，即可使用微信开发者工具导入对应的文件夹，进行小程序代码审计工作。