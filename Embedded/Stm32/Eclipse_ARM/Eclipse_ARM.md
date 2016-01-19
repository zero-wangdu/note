Eclipse stm32 编辑 编译 下载 调试 （ ST-Link J-Link ） 环境搭建 
=========================

# （一）下载

1.  [下载Eclipse IDE for C/C++ Developers 环境](http://www.eclipse.org/downloads/)

2. [下载GNU ARM Eclipse Plug-in最新版本](http://sourceforge.net/projects/gnuarmeclipse/ ) <br/>
* [插件wiki](http://gnuarmeclipse.github.io/)

3. 工具链 ，GNU ARM Eclipse Plug-in 插件支持的工具链有很多，如下<br/>
![](./asset/0e2aed80-a2c6-472f-ad8d-38c0196822ef.png)<br/>
比如[第一个](https://launchpad.net/gcc-arm-embedded)：！！！！！！不带make，所以用这个还需要下载一个make功能的软件（识别makefile）
<br/>或者 [sourcery g++ lite 的EABI版本](http://www.codesourcery.com/sgpp/lite/arm/portal/subscription?@template=lite) ，不用再下载make，自带，不过名字不是make，是cs-make


# (二)基本安装

1. 安装java、配置环境变量
2. 解压Eclipse，安装GNU ARM plug in（在Help-->Install New Software）
![](./asset/bfecdeff-f3e2-42a8-8e14-c83c6749ccbe.png)
![](./asset/d4cec48a-adf6-43ab-aaf7-031dedf341d2.png)


# （三）建立工程

1. ![](./asset/c4bf5b79-01ab-4b55-b564-8bab7e399224.png)
2.  在trace output选择semihosting，就可以在Eclipse的控制台中打印调试信息了![](./asset/2ebaf969-27af-47b3-9b4f-7ee67b617fde.png)
3. 下两步设置工程文件夹等可以默认<br/>
![](./asset/024d3931-c609-4bc2-9e84-631eefaca22e.png)
4. 如果遇到找不到make命令或者cs-make命令
![](./asset/42f3aaa0-abcb-4234-b789-556de8788ce1.png)
就是没有make程序的原因，或者填错了名称，如果使用sourcery g++ lite EABI，在工程设置（project-->properties-->C++build-->settings）中改成cs-make就行了,如下图
![](./asset/aa2a4913-df82-4bd0-a0fc-0d0ae1cec447.png)
但是如果是使用不带make程序的工具链，要手动安装，自行搜索安装make的方法（或者使用MinGW或Cygwin（初次接触可自行搜索并了解）进行安装
比如cygwin安装这个
![](./asset/72849c26-916d-49a1-8fab-74b76e0a0dfb.png)）
安装完后记得设置环境变量，最终在控制台输入 make -v 能看到make的版本信息为止

然后进行编译，如果步骤没错，就可以了
5. 工程设置：
<br/>
只链接用到的代码，降低二进制文件大小
![](./asset/7f55e946-e3ac-45a5-afbd-2d0494020562.png)
![](./asset/00786f97-a08d-4043-883c-013de03d75dc.png)

<br/>其它设置，参照模板内的，如果自己建新的空工程


# （四）下载、调试

### ST-Link
* 下载
  * 安装STM32 ST-LINK Utility
            > Windows：官网直接下载安装即可：
http://www.st.com/web/en/catalog/tools/PF258168
            > Linux：    需下载源代码自行编译安装 
              https://github.com/texane/stlink
              
  * 配置下载程序（使用ST-link utility ，只能下载程序，不能调试的方法，使用GDB进行调试的在后面）<br/>
Run-->External tools-->External tools Configurations
然后左上角新建一个配置，按照下图设置<br/>
              ![](./asset/e96ca9b9-f0b6-48a2-a4b5-5df9eeb62f36.png)
              ![](./asset/8e7f8874-ad4d-434a-a822-31740a777ca1.png)
  * 点击这个就可以下载了<br/>![](./asset/4be4313e-9ea0-4afb-a53b-e37cce7b7ca3.png)
  * 可能会出现这个问<br/>题![](./asset/182a3ae1-fc73-4ee2-8fd8-be4ee0f4676a.png)
  * 在project-->clean处清理一下工程就行了
  * 这是正在下载<br/>![](./asset/6f20e9c8-1fa5-4986-aada-8f0940844898.png)<br/>下载完毕<br/>![](./asset/4b4bc2d8-3533-4106-b775-cf8974c97d7d.png)

* 调试
  * [下载openocd](http://www.openocd.net/)（[其它地址](http://www.freddiechopin.info/en/download/category/4-openocd)）或者找插件内的（我没找到ㄒoㄒ
  * 然后解压到一个文件夹
  * 选择DebugConfigurations<br/>![](./asset/1cd7d504-8ffc-4ca1-9cea-b5a05b7d6f7b.png)![](./asset/890fb136-8a28-4460-a0df-9965a392c319.png)

> config options 中的内容来自于openocd文件夹下，根据不同的芯片和st-link版本选择不同的文件，都在同一个文件夹下
Executable中的内容是openocd.exe可执行文件的地址，可以使用变量，如图，或者直接用绝对地址比如D:\Program Files (x86)\openocd\openocd-0.9.0\bin-x64\openocd.exe
Executable中的内容是GDB的位置，使用变量，如图；或者绝对地址如：C:\Program Files\GNU_ARM_toolchain\bin\arm-none-eabi-gdb.exe

  * 如果变量忘记了没关系，有提示，指到前面的文字<br/>
		![](./asset/719af768-df4a-4a35-b2aa-6b8f3852c860.png)
  * 这样st-link调试和下载就基本可以了,效果图<br/>
		![](./asset/5bbe0e0d-2775-49a3-a2ce-02e47fc44ac3.png)
  * 关于寄存器查看，可以使用插件
  * [插件官网：](http://embsysregview.sourceforge.net/)
  * 安装：<br/>
Help-->Eclipse marketplace出现下图
搜索embsysregview,然后点击Install。。然后下一步下一步下一步。。。。<br/>![](./asset/19583255-2058-4e87-8b83-57b6a9dc5608.png)


### J-Link

* 方法：

> 1. 似st-link 只是配置文件不同
> 2. 使用JLinkGDBServer

###### 参考资料：
* [GNU ARM Eclipse wiki](http://gnuarmeclipse.github.io/)
* [J-Link debugging Eclipse plug-in wiki](http://gnuarmeclipse.github.io/debug/jlink/)

> 探索中，出现了问题，已经可以用的可以告知我一下么，谢谢啦






# 其它问题

* enum 找不到。
  * ecplipse中enum成员有时会提示找不到，这是eclipse的bug ， 使用project->C/C++Index -> rebuild 就行了
<br/><br/><br/>
<kbd>[更多请到这里](http://blog.neucrack.com)</kbd>