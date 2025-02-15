今天父母给我找出了一台基本上已经完全被淘汰的电脑，我打算用它来做一些实验性的事情，比如说安装一些不常见的操作系统，或者是一些不常见的软件。这台电脑型号是Acer Aspire One ZG5，打开时是Windows XP。这台电脑的配置基本上是2002-2006年的水平，所以我打算用它来做一些实验性的事情。

![Image](https://github.com/user-attachments/assets/f5dcaea2-52e8-4b88-b7b0-9ee9f534459b)

![Image](https://github.com/user-attachments/assets/ac2556eb-e097-4de9-a82b-3e40d8c72723)

我首先尝试了安装Ubuntu 22.04.3,在Ubuntu官网找到对应的版本后，同时下载rufus来制作系统启动盘，开机后连按F12进入bios,
但是由于这台电脑的配置实在是太低了,进去了之后一直卡在了光标闪烁界面，所以第一次安装宣告失败。

![Image](https://github.com/user-attachments/assets/39b79d56-8b5b-4037-81c3-b26aacc6c945)

<!-- Failed to upload "image.png" -->

接着我又想，既然Ubuntu 22.04.3不行，那么早一点的呢？比如最早的Ubuntu 4.10。于是我又下载了Ubuntu 4.10的镜像，本来以为马上就结束了，谁知这才是开始。。。

![Image](https://github.com/user-attachments/assets/239b5af9-c5ce-4a24-9155-91c17d0d565a)

我一开始尝试的是warty-release-install-amd64.iso，但是这个版本的Ubuntu是64位的，
而这台电脑是32位的，所以我又下载了warty-release-install-i386.iso，
这次终于成功，进入了显示页面。

![Image](https://github.com/user-attachments/assets/3bda1aca-a525-479f-a06d-2c3f33d46843)

但是，因为我是用U盘进的，压根没有CD-ROM，而且我也无法找到我的U盘对应的存储位置，所以卡在了这个界面根本绕不过去，。

<!-- Failed to upload "image.png" -->

![Image](https://github.com/user-attachments/assets/7063c942-f2f0-4216-9752-c6f6b1aa63da)

之后，我专门去搜索了Acer Aspire One ZG5对应的官方出版时，预装的是Linpus Linux Lite 1.9.9.E版本，但是由于这个系统实在是太过于陈旧，虽然有提供官方系统的网站，但是点击之后就是链接无法访问了，所以我也没有找到对应的镜像，所以这个尝试也宣告失败。

![Image](https://github.com/user-attachments/assets/faada5a0-d423-45c2-a959-ee1dd478a0a4)

![Image](https://github.com/user-attachments/assets/e8fc234e-3ccd-40f7-a73b-442fb512f846)

后边我实在找不到了，对照着这个Linux版本选择了近似年代的Ubuntu 13.04，
这个倒是成功了，但是由于这个版本的Ubuntu已经不再维护了，在安装成功后，很多指令都无法正常执行，
要么就是过时了，指令不对，要么就是版本太新压根下不了，要么就是下的那个软件只有64位，无法适配32位，因此我也放弃了这个版本。


最后，我选择了Lubuntu 18.04.6，Lubuntu是一个基于Ubuntu的轻量级桌面环境，
它的特点是占用资源少，运行速度快，适合配置低的电脑。而且这个版本是LTS版本，也就是长期支持版本，这样我就不用担心版本过旧无法使用了。 
所以我下载了对应的镜像，用rufus制作启动盘，进入bios，选择U盘启动，终于成功了！

![Image](https://github.com/user-attachments/assets/5ee478dd-2a64-45bf-8747-8e51691d3209)

![Image](https://github.com/user-attachments/assets/bc07de13-9833-46d7-9674-a67e8a238ac0)

![Image](https://github.com/user-attachments/assets/53284296-dd7f-4b70-952b-ee43a25997ef)


