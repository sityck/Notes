# 微信小程序抓包

本章节介绍微信小程序抓包的简单流程

微信小程序可以通过PC端抓取、模拟器抓取、真实手机抓取。其中PC端抓取需要安装旧版本微信，真实手机和模拟器抓取思路类似。

## PC端

### windows

windows端微信，目前最新版已无法直接挂代理抓小程序流量包。需要降级到3.7.6版本以下，使用删目录的方法进行抓包。
删目录的方法请参考：
https://blog.csdn.net/weixin_46552558/article/details/124037807

### 苹果

直接挂载代理测试即可，暂不受影响。

## 模拟器

本章节介绍模拟器抓包的办法，真实手机在与PC连接到同一个WIFI局域网（注意不要使用测试手机的热点）后，参考模拟器的步骤进行操作即可，不再赘述。

1. 在宿主机启用fiddler和BP：

   启动Fiddler并配置抓取HTTPS报文以及监听端口：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx8.png)

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx9.png)

   若要和BP对接，则还需要配置转发规则：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx10.png)

2. 在模拟器配置代理，引流到宿主机的Fiddler：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx11.png)

3. 安装Fiddler的证书：

   使用模拟器的浏览器访问宿主机的fiddler站点：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx12.png)

   下载证书后，在模拟器内安装该证书，Fiddler即可抓取到相关内容：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/wx13.png)

------

配通相关步骤后，打开微信小程序就能够在Fiddler中查看到对应的报文了，而后进行后续的测试工作。