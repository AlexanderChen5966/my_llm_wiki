---
title: "从零开始 打造全能怀旧游戏机 初篇 EmuELEC+N1 安装与配置"
source: "https://zhuanlan.zhihu.com/p/79779621"
author:
  - "[[可爱的排骨]]"
published:
created: 2026-08-18
description: "零. 前言 一. 硬软件清单 二. EmuELEC 安装流程 三. Emulation Station (RS) 初始设置 四. Retroarch (RA) 初始设置 五. 上传 ROM 并运行 六. 总结 大家好, 我是可爱的排骨 :) 零. 前言可曾记得儿时在街机厅流连忘…"
tags:
  - "clippings"
status: "processed"
---
77 人赞同了该文章

```
零. 前言
一. 硬软件清单
二. EmuELEC 安装流程
三. Emulation Station (RS) 初始设置
四. Retroarch (RA) 初始设置
五. 上传 ROM 并运行
六. 总结
```

> 大家好, 我是可爱的排骨:)

![](https://pic3.zhimg.com/v2-9a6b7ad38ebbb2c44b59184863f92bd6_1440w.jpg)

## 零. 前言

可曾记得儿时在街机厅流连忘返? 可曾回想起与同学好友在电视机前酣战? 可曾遗憾那些买不起卡带和未能通的关卡? 人越老就越依旧, 如果有一个不花多少钱就能重温那一切的方法, 不尝试一下?

本文讲述 **将一个安卓电视盒打造成全能的怀旧游戏机** 的全过程.

爱折腾的排骨花了约两周的时间, 尝试使用 EmuELEC 原版镜像和 [N1](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=N1&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiJOMSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjEwNTg1ODM3MCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.j-00G7sWau9Kq7kMuwYIVzn5oUR5AkxgovnbFHMKt1A&zhida_source=entity) 盒子制作这个怀旧游戏机 (也许是全网首篇?), 重在分享打造过程, 希望大家喜欢.

## 一. 硬软件清单

**硬件部分**: 一个游戏机一般分为3个部分: 主机, 游戏介质(卡带/光盘), 控制器(手柄).

## 安卓盒子: 100元?

![](https://picx.zhimg.com/v2-425ca8b4b61bda8fcb8fbafc559cb95d_1440w.jpg)

本文中使用刷了 **官改固件的传奇热门坑爹单品, 一代神盒 N1** 作为游戏机的主机. 原因有三:

1. 现在入手很便宜 (100不到).
2. 配置不错 (64位CPU, 2G内存, 千兆网品, [双频WiFi](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E5%8F%8C%E9%A2%91WiFi&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLlj4zpopFXaUZpIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MTA1ODU4MzcwLCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ._7cc6PGGHBWcKTRHADM7BPTaIUrSDucJWB8vlCA4gnE&zhida_source=entity), 双USB, 4K硬解, HDR).
3. 开放性好 (在大神们的努力下root固件随意刷. 可刷 linux 系统).

**官改固件的刷写请参考其它教程, 本文不作介绍.** 有兴趣的朋友可以买个配置更高 (如 t95z) 的盒子尝试.

## USB 存储设备 (U盘或存储卡+读卡器): 50元?

![](https://pic4.zhimg.com/v2-8272687aa29c0161d20d559f82516cef_1440w.jpg)

本文中使用 **2G TF卡 + 绿联 USB 3.0 读卡器** 作为游戏介质. 使用 U 盘也是等同的效果. 存储设备的容量至少需要2G, 主要用来存放 [模拟器系统](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E6%A8%A1%E6%8B%9F%E5%99%A8%E7%B3%BB%E7%BB%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLmqKHmi5_lmajns7vnu58iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.68p7nFxSwBeyQZSqjo4GGGIvMR04fqUojlYCYxTzLdU&zhida_source=entity), 软件, [配置文件](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLphY3nva7mlofku7YiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.mjz2ljXg9bJI3to3JpmPT1CgJgFM8jLv-zWwdKwo-lc&zhida_source=entity), 资源文件和游戏的 ROM 文件.

## 游戏手柄: 150元?

![](https://pic2.zhimg.com/v2-11147bfa9f798741ca5f7aedf124c80d_1440w.jpg)

本文中使用 **北通的 [阿修罗](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E9%98%BF%E4%BF%AE%E7%BD%97&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLpmL_kv67nvZciLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.FfXUKo-zbMLIpYpJUvvxB3THBnv0AglJJ1vwT7flbrk&zhida_source=entity) 2 无线手柄** 作为游戏控制器. 也可以用 USB 有线手柄或者键盘当作游戏控制器. 蓝牙手柄按理说也可以, 只是排骨手头没有所以这里就不吹牛了. 仅有的 WiiMote 并不能正常工作.

## USB HUB: (选配)

![](https://pic4.zhimg.com/v2-1146d016d1f596d61a5a90124f83e14b_1440w.jpg)

本文中使用 **绿联 4 口 USB 3.0 分线器**. N1 只有两个 USB 口, 一个口要插读卡器, 剩一个口只能接一个控制器. 如果想 **双打** 就得接两个手柄, 想玩麻将之类的游戏就得接键盘, 那就得用上 USB HUB 就来扩充接口.

**软件部分**:

## EmuELEC 镜像文件:

![](https://pica.zhimg.com/v2-815903a61c597c5e1ebbeac6f2f2251c_1440w.jpg)

**EmuELEC (下面简称EE)** 是 **shantigilbert** 针对 **[Amlogic](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=Amlogic&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiJBbWxvZ2ljIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MTA1ODU4MzcwLCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.OLv3_COsKBuCu2Zec7-9edZynjjkRbxoUVfFTdlQQys&zhida_source=entity) (晶晨)** 方案的盒子整合的模拟器集成系统. 相当于树莓派上 **RetroPie** 的 Amlogic 版. EE 中包含了 EmulationStation (下面简称 ES), RetroArch (下面简称 RA) 和几个独立 [模拟器程序](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E6%A8%A1%E6%8B%9F%E5%99%A8%E7%A8%8B%E5%BA%8F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLmqKHmi5_lmajnqIvluo8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.4RvjyhTtQRRDSYX00zRzzk1h-x7W-NyWny2qnir0qvs&zhida_source=entity). EE 的系统基于 CoreELEC 和 [Lakka](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=Lakka&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiJMYWtrYSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjEwNTg1ODM3MCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.swtGQ8JAABM4bfU8MWM9xkIMBRY0m4cr5WrdoJDKLqo&zhida_source=entity) (两者都基于 LibreELEC), 剥离了 CoreELEC 带的 KODI 影音播放管理系统. EE 的模拟器以 Lakka 中的 RetroArch 为主, 附带一些为 DC/PSP 游戏准备的独立模拟器. EE 的默认 GUI (图形化界面) 是 ES, 也可切换成 RA 的 GUI 使用.

目前 EE 最新版本为 **2.5.4**, 镜像文件名为 **EmuELEC-Amlogic.arm-2.5.4-Generic.img.gz**, 文件大小为 **428MB**.

## EE 的 N1 补丁包:

![](https://picx.zhimg.com/v2-17fff598306cca1c8809ff41e0d5a7fd_1440w.jpg)

EE 的原版镜像并不能直接在 N1 上引导启动, 这个问题可以通过提取 momokind 的最新 CoreELEC 镜像中提取一些文件解决.

![](https://pica.zhimg.com/v2-1b2dbf893fcb23145b021e6b89f58882_1440w.jpg)

用解压工具将红框中的7个文件解压出来作为补丁包备好. 其中 **uEnv.ini** 文件中的 **boot=LABEL=COREELEC** 需要改为 **boot=LABEL=EMUELEC**, 不然启动会报 mount 错误.

## 怀旧游戏 ROM 文件:

将怀旧玩玩的游戏列个清单, 找到对应的 ROM 文件 (一般为 zip 包) 下载并按文件夹分类准备好. 由于 ROM 文件的获取可能涉及版权问题, 本文不会提供, 网上随便一搜索一大堆, 请自行解决.

## 固件(镜像)写入工具:

有多种工具可以将 EE 写入U盘或存储卡中. 以下 3 款任君挑选.

- USB Image Tool: 排骨推荐的镜像写盘工具, 只有300KB+.
- Win32DiskImager: 一款常见的镜像写盘工具, 安装包有12MB+.
- [balenaEtcher](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=balenaEtcher&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiJiYWxlbmFFdGNoZXIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.nqb_QM7zNFd3hH6rvKkX64y7yT2nCXnvRnynTRC3ezU&zhida_source=entity): 支持 Mac 镜像的写盘工具, 执行文件高达 110MB+.

本文中使用 USB Image Tool.

## 二. EmuELEC 安装流程

## 1\. 首先下载 EE 系统镜像, 写入 USB 存储设备.

下载好镜像后, 将 USB 存储设备插入电脑, 启动 USB Image Tool.

![](https://pic3.zhimg.com/v2-72dbb7e0d1dafb0f36343d6d2d2f1bb0_1440w.jpg)

选中左侧的 USB 存储设备, 点击 Restore 按钮.

![](https://picx.zhimg.com/v2-cff2c638be97f314f94e87f73c56d463_1440w.jpg)

选择镜像文件. 注意后缀名是.img.gz 而不是.img.

![](https://pic1.zhimg.com/v2-d3904dca0148be750cf81ac42fac088e_1440w.jpg)

等待镜像写入完成.

![](https://pica.zhimg.com/v2-0e5debb1ed30321c329a3236652f7c00_1440w.jpg)

写入完成后, Windows 会弹出某盘未格式化的提醒窗口, 一定要取消关闭!

## 2\. 然后将补丁文件复制到 EE 的引导分区.

![](https://pica.zhimg.com/v2-88822f3f7ec13a40fe68efa98e7b376a_1440w.jpg)

镜像写入后, USB 存储设备被分成两个区. 第一个是引导区, 通用的 FAT32 格式, 可以直接在 Windows 中读写访问. 第二个是系统区, Linux 专用的 EXT4 格式, 就是上面提示要格式化的分区.

![](https://picx.zhimg.com/v2-ad6cfcc8d6c7e25b70d919b9938ce8bb_1440w.jpg)

引导区中只有 SYSTEM 文件有用, 其它文件可以删除.

![](https://pic1.zhimg.com/v2-2a5c3d746374e1810801dd5219ee0d60_1440w.jpg)

将补丁包中的所有文件复制到导引区中.

## 3\. 让 N1 进入线刷模式进入 EE 系统.

启动 N1 进入官改系统, 从系统设置中查看并记录当前 IP 地址.

![](https://pica.zhimg.com/v2-88399a084123e41fa78f27c3b563fc04_1440w.jpg)

然后将刚才写入了 EE 镜像的 USB 存储设备插入 N1 的 **USB 口 (靠近 HDMI 口的)**. 最后将游戏手柄插入 N1 的另一个 USB 口.

方法一: 可以使用安卓开发调试工具 SDK Platform Tools, 执行下图的两行命令 连接 N1 并重启到线刷模式.

![](https://pic2.zhimg.com/v2-673b539e64b9a01605076f258e7f8dc7_1440w.jpg)

(由于上面两个截图时间不同, 出现了两个不同的 IP. 实际操作中是同一个)

方法二: 使用 W大 的 N1 降级工具包进入线刷模式

![](https://pic1.zhimg.com/v2-0eb4d8610a236982ccab5b13abefc3f6_1440w.jpg)

(实际上跟方法一完全一样...)

方法三: 安装 "Reboot to LibreELEC" 应用直接进入线刷模式.

![](https://picx.zhimg.com/v2-5c2e71245d7ff0bfb2a1b108ee47f415_1440w.jpg)

![](https://pic2.zhimg.com/v2-304201635421f40e1b630592c38f7d77_1440w.jpg)

(这个 apk 可能不太好找, 排骨是在 apkpure 下载到的)

## 三. Emulation Station (RS) 初始设置

![](https://pica.zhimg.com/v2-ae47fabad04b00d4fc8be0ead147feb2_1440w.jpg)

N1 重启到线刷模式, 几分钟后会显示出如上图所示的 EE 系统初始化界面, 图中 EE 已经成功引导, 正在扩容系统分区和解压系统文件. (作者偷懒了, **CoreELEC** 的启动画面还没换掉) 开机画面的更换请见 "中篇".

![](https://picx.zhimg.com/v2-3f1b054c25544797332f54a81296b4fb_1440w.jpg)

当你看到上图这个界面时, 表示 EE 已经安装并成功进入了 ES 前端界面, 准备进入按键设置流程.

![](https://pic1.zhimg.com/v2-05f7056a995643c2ca34511c759dde32_1440w.jpg)

北通阿修罗2 手柄的按键布局与 Xbox 手柄完全一致, 按下图设置布局绑定一下键位就行了. A 确定, B 取消, AB 组合要记牢.

![](https://pic4.zhimg.com/v2-d4782bbe2a6849fb93d4f909c0cbbe03_1440w.jpg)

长按任意键3秒可以忽略当前键位的绑定. 最后会多出一个 HOTKEY, 可以绑定到不常用的 SHOULDER 或 THUMB 键上. 便于在模拟器无法控制时按 HOTKEY+START 退出模拟器.

![](https://pica.zhimg.com/v2-b514bf6b124da41764bc2f9ab1d3c994_1440w.jpg)

按键绑定完成后会进入 **ES** 主界面. EE 2.5.4 默认给 ES 设置了 **ComicBook** 主题. 这里显示 **RetroPie 是因为作者偷懒,** 直接拿 RetroPie 的 ES 主题包来用了. ES 主题修改的更换请见 "中篇".

接着我们进到 **RA** 里再设置一次手柄按键.

![](https://pic1.zhimg.com/v2-5fc0bf588c6a25fbbd869e6cee8bfca6_1440w.jpg)

按下手柄上的 **START** 键打开 **ES 主菜单**, 下拉到最后选择 **QUIT** (退出).

![](https://pic3.zhimg.com/v2-841585068156e43cee563471edfff362_1440w.jpg)

再选择 **START RETROARCH** 进入 RA.(**ES 没有中文版, 也无法切换中文, 因为 ES 的作者将所有界面文本硬编码在 [源代码](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E6%BA%90%E4%BB%A3%E7%A0%81&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLmupDku6PnoIEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.4LZhmNEkIHjluvCAvcw9ku7RYRY52h42sfkBUt7-P9w&zhida_source=entity) 里了...**)

## 四. Retroarch (RA) 初始设置

![](https://pica.zhimg.com/v2-1ed4c3bfcb3c70bb89dd12b577d360be_1440w.jpg)

这个酷似 PS/PSP 界面的蓝白主题程序就是 RA. 根据上面 ES 绑定的按键, **在 RA 中用十字方向键切换菜单和控制光标, A 键表示进入确认, B 键表示后退取消.**

## 1\. 按键绑定

![](https://pic1.zhimg.com/v2-1b6d207c65d0ed9bf6e471374444a62e_1440w.jpg)

按右切换到 **Settings (设置)** 菜单, 向下依次选择 **Input (输入) -> User 1 Binds (用户 1 绑定) -> User 1 Bind All (绑定所有键)** 开始进行手柄按键绑定.

## 2\. 更改快捷菜单键 (建议)

![](https://pic1.zhimg.com/v2-9884a3074ab9d36666a0dcecf31a7a12_1440w.jpg)

建议在 **Input (输入)** 选择中把快捷菜单键从 **默认的 L3+R3 换成 SELECT+START**. 因为有些手柄没有 L3/R3 键, 也可能在 ES 按键绑定时把 L3/R3 绑定到了 HOTKEY 上, 这样会造成进了游戏模拟器后无法退出的情况.

### 3\. WiFi 连接

为了方便用电脑管理游戏机的设置和内容, 我们需要将它连入网络. 如果有网线直连可以跳过这一步.

![](https://picx.zhimg.com/v2-6664937f099446fef88ae5f01b048269_1440w.jpg)

在 **Settings (设置)** 菜单下选择 **Wi-Fi** 设置, RA 会扫描周围的 SSID 并列出. 选中自家的 SSID 后会出现黑色的密码输入界面.

![](https://picx.zhimg.com/v2-c6f65f8c14e8b8442a34923ef68179b1_1440w.jpg)

输入 WiFi 密码后, 选中右边的 **回车符** 提交密码.

![](https://pic3.zhimg.com/v2-368aec37a3e084481638c963b6883c24_1440w.jpg)

WiFi 连接过程可能需要好几秒并且没有进度提示. 连接成功后所选 SSID 右边会出现 **Online** 字样.

![](https://pic4.zhimg.com/v2-9b23141d50f32de8227bed8317435595_1440w.jpg)

在 **Main Menu (主菜单)** 下面的 **Information (信息) -> Network Information (网络信息)** 中可以看到分配到的 IP 地址, 记下这个 IP, 后续操作要用到.

RA 内置中文语言包, 但是默认字体不支持中文, 直接切换会出现大量空白或方块, RA 字体更换请见 "中篇".

RA 的初始设置已经完成了, 在 **Main Menu (主菜单)** 最下面选择 **Quit RetroArch (退出 RA)** 回到 ES.

![](https://picx.zhimg.com/v2-d09e8592f340041153fd58e5a86fdbbf_1440w.jpg)

回到 ES 后按 START 打开主菜单, 在右下角可以看到当前分配的 IP (就是 RA 中的那个).

至此 ES/RA 的初始设置都完成了. 接下来我们上传游戏 ROM 到游戏机.

## 五. 上传 ROM 并运行

模拟器系统刷好, 手柄也设置好, 万事具备只差 ROM 了. 只需要把下载的 ROM 文件传到游戏机里, 就可以开始玩游戏了.

![](https://pic4.zhimg.com/v2-74a2655e3abd69d55261494925a8e78b_1440w.jpg)

ES 默认只会读取 /storage/roms 路径下面的 ROM 文件, /storage/roms 在 EXT4 分区中. Windows 无法直接读写访问, 需要安装第三方工具, 还要把读卡器拔来拔去, 实在麻烦.

![](https://pic4.zhimg.com/v2-9443362702a37fc3abc8b3df07de6385_1440w.jpg)

如果已经将 ES/RA 连入网络, 那么我们可以用 Windows 直接通过 SMB 或 SSH 服务访问游戏机.

![](https://pic1.zhimg.com/v2-fbf5a5beadf2d69783ef68f7c3118e28_1440w.jpg)

在文件管理器地址栏输入 **\\192.168.1.20 (假设游戏机的 IP 是这个)** 打开游戏上的共享文件夹. 图上的 roms 文件夹就是存放 ROM 文件的.

![](https://pic2.zhimg.com/v2-73a6aeea183e157b7ecc3e98306de7c9_1440w.jpg)

如果用 Windows 10 无法成功连接, 可以打开 gpedit.msc ([组策略编辑器](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E7%BB%84%E7%AD%96%E7%95%A5%E7%BC%96%E8%BE%91%E5%99%A8&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLnu4TnrZbnlaXnvJbovpHlmagiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.snsvPDxZ9JaQTZe5KCpNb3zmuWGB2KfrSB0Xxyl5m2I&zhida_source=entity)), **启用不安全的来宾登录**.

![](https://pica.zhimg.com/v2-5e3fd17dcebb090d1b5c5e60883d8f5e_1440w.jpg)

在 roms 文件夹内创建 **mame** 和 **snes** 两个文件夹, 分别存放街机和超任游戏的 ROM 文件.

![](https://pic1.zhimg.com/v2-787b11622d16bf28e88c0c64e9d9a344_1440w.jpg)

文件夹名称不能任意取, 建成 **街机** 或 **超任** 无法被 ES 识别.

![](https://pic2.zhimg.com/v2-9139aa66d10ccab61105d3d48868b00f_1440w.jpg)

放入 **街机虎克船长和超任三国志3** 两个 ROM 试一试.

![](https://pic4.zhimg.com/v2-7cc76fd5f747945fee3778b9e349579f_1440w.jpg)

重启 ES, ES 在启动时会自动搜索 roms 文件夹并生成对应的游戏平台集合.

![](https://pic3.zhimg.com/v2-401dd6d0d51708a45ce104838eed92d4_1440w.jpg)

ES 重启后就能看到多出 **MAME** 和 **SUPER NES** 两个游戏集合了.

![](https://pic3.zhimg.com/v2-3f36563175828cc49b4d8fc9767e9c24_1440w.jpg)

选择 MAME 会进入 **MAME 游戏列表**, 其中就有上面刚刚放入的 **hook.zip ([虎克船长](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=2&q=%E8%99%8E%E5%85%8B%E8%88%B9%E9%95%BF&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLomY7lhYvoiLnplb8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.aD6pDLjjsG6AHaJPETCUFlOQ3nWrUE5chmTd7zVTmf8&zhida_source=entity))**.

游戏的中文名称, 相关介绍, 封面图片等 [配置方法](https://zhida.zhihu.com/search?content_id=105858370&content_type=Article&match_order=1&q=%E9%85%8D%E7%BD%AE%E6%96%B9%E6%B3%95&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyMTEwOTksInEiOiLphY3nva7mlrnms5UiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMDU4NTgzNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.c-BdTkln0eD1MpwKzfskQhV1n5czRqVdBgc5I_kmCnI&zhida_source=entity) 请见 "中篇".

![](https://pic2.zhimg.com/v2-ea938733a8c7569fcd2f403db1d66ae7_1440w.jpg)

选择 HOOK 按 A 就会启动 mame 模拟器运行 hook.zip 了.

![](https://pic1.zhimg.com/v2-57d02f721d5d52a8fdc3dd73a3721624_1440w.jpg)

在游戏中同时按下 SELECT+START (默认是按 L3+R3) 调出 Quick Menu (快捷菜单), 可以重启, **退出游戏**, 也可以截屏, 录像, 存档, **设置按键 (只对当前游戏有效)** 等.

退出游戏后会回到 RA 界面, 然后再退出 RA 才能回到 ES.

在 ES 里设置的 HOTKEY 好像不起作用, 也许是跟 RA 按键冲突, 先不管 哈哈哈?

![](https://pic4.zhimg.com/v2-5f158bcbc8acfb0776fe5ad7ec5cc14d_1440w.jpg)

![](https://picx.zhimg.com/v2-85be0189ed06875c2736b6d78f001f67_1440w.jpg)

运行超任三国志3 也是相同的方法.

## 六. 总结

完成以上步骤后, 我们就可以开心的玩游戏啦. 当然这个游戏机还不完美, 还有很多可以改进完善的地方. 如果看到这里你还有兴趣继续下去, 关注收藏排骨就能第一时间看到 " **从零开始 打造全能怀旧家用游戏机 中篇** " 了!

发布于 2019-08-26 10:35[豆包AI工作任务模式-本地文件智能管理与优化助手](http://www.doubao.com/download/desktop?ug_apk_token=NJ8a2&ad_platform_id=zhihu_feed_lead&ug_callback_url=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D55d14316-4b12-49dc-9c91-75059a79bdae%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&cb=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D55d14316-4b12-49dc-9c91-75059a79bdae%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&ug_semver=v1.0.0&spu=biz%3D0%26ci%3D3805660%26si%3Dcaee2006-35d7-4eb0-b39a-0a086093d1a3%26ts%3D1787038300%26zid%3D1629)

[

解锁多元办公技能，支持在本地电脑完成文件分类、文档处理、表格分析与PPT制作等工作，助力简化日常办公流程。

](http://www.doubao.com/download/desktop?ug_apk_token=NJ8a2&ad_platform_id=zhihu_feed_lead&ug_callback_url=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D55d14316-4b12-49dc-9c91-75059a79bdae%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&cb=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D55d14316-4b12-49dc-9c91-75059a79bdae%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&ug_semver=v1.0.0&spu=biz%3D0%26ci%3D3805660%26si%3Dcaee2006-35d7-4eb0-b39a-0a086093d1a3%26ts%3D1787038300%26zid%3D1629)

赞同 77