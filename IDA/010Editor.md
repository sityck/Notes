# NOP初试-破解010Editor

本章节介绍使用NOP指令来破解010Editor软件的办法，只用于软件逆向研究，不用于任何商业用途。

> 未经授权请勿利用文章中的技术资料对任何计算机系统软件进行入侵操作，利用此文章所提供的信息而造成的直接或间接后果和损失，均由使用者本人负责。

## 破解过程

1. 寻找切入点

   我们知道，在未注册010Editor时，输入随意的一串注册密码会引发报错，如下图所示：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor028.png)

   那么，此刻我们可以根据这一报错语句作为切入点来查看软件中关于注册码的校验逻辑。

2. 反汇编010Editor

   我们使用IDA打开010Editor.exe（路径：010Editor64BitPortable\AppData），查看内部的代码逻辑。

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor001.png)

   默认选择pe64.dll并点击OK进入，待IDA针对exe程序分析结束后，选中Search->TExt search功能，搜索上文中我们获取到的报错关键字“Invalid name”：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor002.png)

3. 找到代码逻辑

   选择进入搜索出的代码位置：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor003.png)

   可以发现我们已经进入到了注册码的判定模块，我们向上向下浏览一下整个函数（若没有进入到下图的视图，需要先按F5让IDA自动展示伪代码）

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor004.png)

   在浏览整个伪代码模块后，我们可以发现当v17==219时，代码逻辑会走到成功注册的结果上：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor005.png)

   而v17来自于上方的sub_1400084F4函数，且219对应的16进制为0xDB。所以我们是否可以让v17恒等于0xDB，使得我们不管输什么注册码都能够注册成功？

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor006.png)

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor007.png)

   带着上方的想法，我们追踪一下v17是怎么生成的（双击对应的函数）：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor008.png)

   继续追踪可以发现，v17来自于sub_14034AD70函数中的result，那么现在问题就变成了如何使result的值恒为0xDB

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor009.png)

4. 修改目标函数

   选中result参数，然后切换到IDA View-A视图，可以发现该行伪代码对应的汇编操作逻辑会被绿色底色标注出来

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor010.png)

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor011.png)

   这里可以发现右下角有个0xDB被放到了eax中了，合理猜测下，这个分支就是成功分支，而我们的result值会先存放在eax中。

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor012.png)

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor013.png)

   那么接下来我们要使得这个函数恒返回0xDB就是使得函数最后的逻辑是：

   ```
   mov eax 0DBh
   retn
   ```

   为了达到这一目的，我们首先需要请出NOP指令，将main函数模块的跳转给置空，使得该函数直接走到返回result的逻辑。先选中

   ```
   jz short loc_14034AD95
   ```

   然后在菜单中选择Edit->Keypatch->Patcher功能：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor014.png)

   将jz指令改为nop指令，然后点击Patch

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor015.png)

   关闭自动弹出的下一指令修改窗口

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor016.png)

   检查当前的代码逻辑，可以发现现在整个v17的生成函数已经没有分支了，我们接下来只需实现`mov eax 0DBh`，即可达到目的。 

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor017.png)

   再次打开Patcher工具，将此处的eax值改为0DBx：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor018.png)

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor019.png)

   此时已经完成了整体代码的破解。

5. 打包补丁

   在菜单中选择Edit->Patch program->Apply patches to input file工具：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor020.png)

   按照默认值点击OK：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor021.png)

   完成补丁应用：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor022.png)

   退出IDA，记得选择和下图一样的选项：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor023.png)

6. 最后一步：实现任意注册

   打开010Editor.exe：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor024.png)

   进入Help->about页面，选中Redister按钮：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor025.png)

   填充任意值进行注册：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor026.png)

   可以发现最终成功注册：

   ![](https://takiobuff.oss-cn-hongkong.aliyuncs.com/image/Editor027.png)

   

## 知识点

1、Nop指令可以用于跳过关键判定，使得校验无效化。