有一台型号比较旧的打印机，因为放置时间比较久，设备更新不及时，有一些功能无法正常运行，所以在做毕业设计的途中，我这两天顺便修理了一下，现在基本修理完毕了，把这个过程记录一下。
## 一、硬件上的修理
### 1.1 打印机无法进纸
这个一开始感觉很奇怪，电脑这边提示卡纸，但是一点都不卡纸呀，心里恨不得想要拆掉打印机（后面也确实这样做了，不过拆到一半拆不动了又装回去了）。后面在bilibili上搜了[教程](https://www.bilibili.com/video/BV1VW4y167bz/)，发现是因为进纸轮需要擦，所以我拿湿纸巾，喷了一点乒乓球的胶皮清洁剂，擦拭了一下，后面可以正常进纸了。

![Image](https://github.com/user-attachments/assets/ccded983-7380-4103-9c6f-ddb34917e5f7)

![Image](https://github.com/user-attachments/assets/d0d9726f-d922-4fca-98f5-870b3b932955)


### 1.2 打印机可以进纸，但是打印了之后，出来啥都没有
在擦拭完进纸轮，可以进纸之后，我以为大功告成了。没想到这只是长征路上的第一步。
在电脑端点击打印之后，打出来一片空白。又反复试了几次，慢慢的出来了一些字，说明还是存在问题。

![Image](https://github.com/user-attachments/assets/e1d6cd53-3eec-499d-b719-92ed55fea3b5)

我又去Bilibili查找解决方案，看到了这个[视频教程](https://www.bilibili.com/video/BV1h24y117Dk/)。
跟着教程的步骤，我倒了一点水，然后把墨盒放上面，后面就好了。

这是打印的校准页：

![Image](https://github.com/user-attachments/assets/3967fad4-721a-411a-9c65-4f631b4e8766)

看到这里，你是不是以为事情已经结束了？
没有这么简单。

## 二、软件上的修理
### 2.1 第一次尝试
在打印完校准页之后，我就准备大显身手，找了一个文档，正式测试打印的效果。
文档可以正常打印，但是问题又来了。
出了什么问题呢？
问题就是，我打印完这个文档之后，在不断掉打印机电源的情况下，如果想要打印其它文档，完全没有办法打印。只能断掉打印机电源，然后重新启动，这样才能够打印，在这种情况下我判断可能是打印机驱动存在问题，一个我以为很好解决但是又花了我很长时间才解决的问题。
首先我去浏览器上搜索，尝试找相关驱动。找到一个这个：

![Image](https://github.com/user-attachments/assets/ba4629a1-8590-4160-ace1-7c557a5c8842)

让我在微软商店安装HP Smart。

我去安装了，看似可以正常打印，但是打印机依旧无法正常工作。问题和之前一样。

![Image](https://github.com/user-attachments/assets/01ce0526-ef61-44d3-908a-c7a18036a0b8)

### 2.2 第二次尝试
我只能去网络上搜索其他解决办法。
经过查找，我发现官方有推荐安装这个：

![Image](https://github.com/user-attachments/assets/39021618-792e-41f3-9f2f-084d18709668)
链接：https://support.hp.com/cn-zh/drivers/hp-deskjet-2130-all-in-one-printer-series/model/7529634

但是下载后，依旧无法正常运行。所以我又尝试其他的解决办法。

我经过查找，又碰到了这个[帖子](https://h30471.www3.hp.com/t5/da-yin-ji-shi-yong-xiang-guan-wen-ti/hp-deskjet-2130-wu-fa-da-yin/m-p/1113725)，很高兴，以为碰到了真正的解决办法。

但是失败。我卡在了第三步

![Image](https://github.com/user-attachments/assets/576b5b5c-3a03-4e9c-ba76-6af7eccefda9)
这个帖子的系统是mac，但我是windows，所以无法正常访问。
我只能尝试其他方法。（第二步的解压用360压缩软件就可以解压）

### 2.3第三次尝试
我因为偶然看到了那个端口，并且在查找的过程中偶然看到了这个帖子和我之前看到的端口相同，我以为我又遇见了真正的解决方案。
[帖子](https://h30471.www3.hp.com/t5/da-yin-ji-shi-yong-xiang-guan-wen-ti/HP-DJ-2130-series-da-yin-de-shi-hou-xian-shi-da-yin-zhuang-tai-cuo-wu-wu-fa-da-yin-zhe-yang-de-wen-ti-gai-zen-me-jie-jue-ne/td-p/1300709)
我卡在了这一步，运行后没有看到内部的.inf文件。

![Image](https://github.com/user-attachments/assets/52ce5e44-8be0-4cb9-87ac-b2d83eceb938)
我解决完了才发现后面还有，

![Image](https://github.com/user-attachments/assets/52b4a4ad-547f-4322-948d-4b3d6d6dd71c)

所以这个方法的具体效果未知。
### 2.4第四次尝试
我继续搜索，又搜到了这个[帖子](https://h30471.www3.hp.com/t5/da-yin-ji-shi-yong-xiang-guan-wen-ti/hp-deskjet-2132wu-fa-da-yin-ce-shi-ye-jioffice-text-wen-jian/td-p/1078027)，这个帖子的问题描述是无法打印测试页，我测试了一下是否可以打印测试页，发现果然无法打印。虽然这个帖子最后也没有能够解决我的问题，但是让我开心的事情是，至少能够帮我更精确的定位问题所在。
这个帖子提出的解决方案和第三次的相同，所以当时我并未继续尝试，而是继续寻求其他方案。

![Image](https://github.com/user-attachments/assets/1ca0b161-ee5c-4389-bf2c-44d3a5b7c4d6)

### 2.5 最终尝试
后面我又回到了b站，看看有什么解决方法，看到了这个[视频](https://www.bilibili.com/video/BV1qT4y1r739/?spm_id_from=333.337.search-card.all.click&vd_source=348b4747c7e04cd236a0b05918dadc36),我并未尝试这个视频的解决方法前就突然解决了我的问题。但是我觉得这个视频里讲的方法确实是有价值的，虽然并不一定使用我现在的问题。
那么我是如何解决的呢？

### 2.6 解决的具体步骤
（1）设置里搜索"打印机和扫描仪"

![Image](https://github.com/user-attachments/assets/fbbd72ee-3409-4b29-9614-7fa729476b75)

选择这个并点击：

![Image](https://github.com/user-attachments/assets/036b7c55-4c02-4d8f-8a0c-718b7f143827)

跳转到这个页面：

![Image](https://github.com/user-attachments/assets/cb797d11-c1c9-4e66-8d71-9253eb433198)

点击打印机属性：

![Image](https://github.com/user-attachments/assets/da9711c2-78a4-4e50-b341-df8765575018)

选择"高级"：

![Image](https://github.com/user-attachments/assets/b043703e-2201-41ae-849c-dc53a31119fc)

点击"新驱动程序"：

![Image](https://github.com/user-attachments/assets/01071856-12c5-4ec2-b609-ab07b542b1b7)

点击"下一页"：

![Image](https://github.com/user-attachments/assets/0285ecef-086f-402b-8196-ff30de4aad39)

厂商选择"HP"，在右侧打印机一栏找到对应型号的打印机，然后双击：

![Image](https://github.com/user-attachments/assets/8b10db98-4f0a-413e-94de-3838de8f1ea6)

跳转到这个页面：

![Image](https://github.com/user-attachments/assets/7988d5f8-dc46-4d66-9a2d-b4b29e074b18)

点击完成，然后等待一会，等驱动安装后，就可以正常打印了。

以下是测试页：

![Image](https://github.com/user-attachments/assets/a5936369-d4ee-4e2d-a28b-eeb4ddb5e268)

现在，打印机就可以正常运行了。