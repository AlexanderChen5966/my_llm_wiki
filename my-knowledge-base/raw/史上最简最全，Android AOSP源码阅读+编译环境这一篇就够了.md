---
title: "史上最简最全，Android AOSP源码阅读+编译环境这一篇就够了"
source: "https://zhuanlan.zhihu.com/p/574856795"
author:
  - "[[ZeroFreeze酷安APP开发者、带你了解深入Android/Linux知识]]"
published:
created: 2026-06-10
description: "Android系统作为一个庞大的开源项目，除了一些谷歌自带服务之外，其他所有代码均以AOSP（Android Open Source Project）的形式开源。对于框架开发者来说，熟悉AOSP是必不可少的知识。即使是普通开发者，为了优化代…"
tags:
  - "clippings"
status: processed
---
79 人赞同了该文章

Android系统作为一个庞大的开源项目，除了一些谷歌自带服务之外，其他所有代码均以AOSP（Android Open Source Project）的形式开源。对于框架开发者来说，熟悉AOSP是必不可少的知识。即使是普通开发者，为了优化代码或者是调用一些系统底层API，有时也需要参考底层源码。那么就需要搭配一个合适的源码阅读环境了。

首先要说的是，如果仅仅是阅读AOSP的源码，我们不必要直接下载源码。 Google提供了这个网站：

用于直接在线阅读AOSP的源码。虽然它很方便快捷，不过由于一些众所周知的原因，在国内是不能直接访问到的。并且如果你是框架开发者，那么还是免不了编译的过程，就只能一步步下载源码了。

下载AOSP源码并不简单，网上教程很多但也很杂。我尽量通过本文，用最简洁的篇幅帮你搞定所有的步骤，以及帮你避免所有你将要踩的坑。

**阅读本文之前，我默认你已经了解Linux和Linux常用命令的基本使用，以及 `git` 的基本知识和命令使用。**

### PC配置准备

硬件要求：

- 64 位CPU，核数越多越好。
- 只检出分支查看代码， **至少预留出 190GB 磁盘空间** ；如果要进行编译，则需要 300+GB。
- 不管是虚拟机还是实机编译， **最低要求是要有 16GB 的系统内存RAM** 。

### 编译环境准备

[Android 官方声明 AOSP](https://link.zhihu.com/?target=https%3A//android.googlesource.com/) 历来是在 [Ubuntu](https://zhida.zhihu.com/search?content_id=215878707&content_type=Article&match_order=1&q=Ubuntu&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEyNDIyNjgsInEiOiJVYnVudHUiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMTU4Nzg3MDcsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.pJp4LLjU6i85WC2oNNodXzbH5Ecm-IfFZJmtGS_9yEI&zhida_source=entity) LTS 版本中进行开发和测试。 **所以为了避免任何其他不必要的麻烦，请使用 Ubuntu 及其他基于 Ubuntu 的发行版。**

最低环境要求：

- Ubuntu 14.04+
- JDK1.8+
- python3
- GNU make
- git

首先配置安装好Ubuntu，这里虚拟机还是实机都可，如果不是为了编译过老的Android版本，我建议一律使用18.04以上的版本。安装Ubuntu的网上教程很多，这里就不再多废话了。

进入系统，打开终端，然后执行以下命令安装好所有环境。

```bash
$ sudo apt update
$ sudo apt install flex bison build-essential zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc fontconfig -y
$ sudo apt install make git-core gnupg zip unzip curl python3 openjdk-11-jdk -y
$ sudo apt clean && sudo apt autoremove -y
```

### 下载和签出代码

由于AOSP项目代码是超过上百个git项目的集合，为了管理庞大的仓库，我们必须使用谷歌提供的 `repo` 工具克隆并签出代码。 首先安装 `repo` 工具：

```bash
$ sudo apt install repo -y
```

安装好后执行 `repo -h` 检查一下，输出如下说明安装成功。

![](https://pic4.zhimg.com/v2-95df594a4da0e2a424e8c348ba73d527_1440w.jpg)

AOSP代码部署在谷歌的git服务器上，由于一些众所周知的原因国内访问不到。这里我们使用来自清华大学的镜像源进行快速下载。

清华源的使用说明： [mirrors.tuna.tsinghua.edu.cn](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/help/AOSP/)

建立一个目录，在这里我就叫 `aosp` ，我们就在这个文件夹里直接签出AOSP的代码：

```bash
$ mkdir -p ~/aosp && cd ~/aosp
$ repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest
```

这里会提示无法连接到 [gerrit.googlesource.com](https://link.zhihu.com/?target=http%3A//gerrit.googlesource.com) ，需要我们修改一下bash的环境变量，然后重新初始化仓库。

```bash
$ echo \`export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'\` >> ~/.bashrc 
$ source ~/.bashrc 
$ repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest
```

输出如下说明成功：

![](https://pic2.zhimg.com/v2-3e7d2a285b210d8d78d8af1db20c6d93_1440w.jpg)

接下来，只需再运行这一条命令即可开始同步仓库：

```bash
$ repo sync
```

第一次下载耗时会有数个小时，因为要下载签出这200GB数据，如果是固态硬盘会快一些。以后再更新整个AOSP仓库，也只需要执行这条命令。如果在下载中途遇到网络中断，重新执行这条命令即可。

至此源码下载完成。

### 阅读环境准备

阅读AOSP源码有很多方式，命令行爱好者喜欢使用vim，有些人习惯SourceInsight，当然也可以使用功能更完善的IDE， [Android Studio](https://zhida.zhihu.com/search?content_id=215878707&content_type=Article&match_order=1&q=Android+Studio&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEyNDIyNjgsInEiOiJBbmRyb2lkIFN0dWRpbyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjIxNTg3ODcwNywiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.NtV08zOqI1rALhRcCkOAdZuO__Cge7drmXR9jEmjb7Q&zhida_source=entity) 。使用AS的优点是能提供代码跳转、智能语法高亮等功能，并且享受IDEA的一些强大功能，缺点是初始化需要一些时间。

本文主要说明如何通过Android Studio来阅读。

如果直接用Android Studio打开AOSP代码，是没有代码跳转等高级功能的。为了使用这些功能，需要编译一个AOSP中的idegen模块。 idegen用来生成可以导入Eclipse和IDEA的工程配置文件( 也就是`.ipr` 文件 )，这个模块位于 `development/tools/idegen/` 。

首先通过snap商店安装Android Studio：

```bash
$ snap install android-studio
```

桌面上出现图标后，打开Android Studio。随便创建一个项目，在 "帮助" --> "更改内存设置" 中把内存调整到8192MB，点击保存，然后关掉Android Studio。

![](https://pic4.zhimg.com/v2-59e2f075db4116f1fc3594aa8e0b0955_1440w.jpg)

进入AOSP目录，通过sh脚本加载环境变量，然后执行 `mmm` 编译idegen模块。

```bash
$ cd ~/aosp
$ source build/envsetup.sh
$ mmm development/tools/idegen/
```

编译过程中请保证系统内存充足，否则会失败。也可以使用 `-j2` 或 `-j4` 减少编译的进程数，减少内存占用保证编译通过。

编译成功后在 `/out/host/linux-x86/framework` 目录下会生成 `idegen.jar` 文件。执行sh脚本，它将使用这个jar生成所需的 `.ipr` 文件：

```bash
$ development/tools/idegen/idegen.sh
```

注意这一步要在AOSP仓库根目录执行，不要在其他地方执行。

成功后，在AOSP代码根目录下可以找到 `android.iml` 和 `android.ipr` 两个文件。打开Android Studio，点击File --> Open，选中生成的 `android.ipr` 文件即可。

等待一段时间的扫描后，Android Studio的高级功能就可用了。

### 进行一次完整编译

一次完整编译比较耗时，以及需要大量的硬盘空间。请准备好充足的内存和硬盘资源，然后我们跑一次完整编译流程。

进行编译的第一步是加载shell环境变量。所有的环境变量都通过 `build/envsetup.sh` 进行加载。只有加载了变量后，对应的命令才可以使用。

```bash
$ source build/envsetup.sh
```

第二步是选择编译哪种版本。执行 `lunch` 命令，输出一个菜单，它列出了当前Android源码支持的所有设备代号和编译类型。输入相应数字选择编译类型。

```bash
$ lunch

You're building on Linux

Lunch menu .. Here are the common combinations:
     1. aosp_arm-eng
     2. aosp_arm64-eng
     3. aosp_barbet-userdebug
     4. aosp_bluejay-userdebug
     5. aosp_bramble-userdebug
     6. aosp_bramble_car-userdebug
     7. aosp_car_arm-userdebug
     8. aosp_car_arm64-userdebug
     9. aosp_car_x86-userdebug
     10. aosp_car_x86_64-userdebug
     11. aosp_cf_arm64_auto-userdebug
     12. aosp_cf_arm64_phone-userdebug
     13. aosp_cf_x86_64_auto-userdebug
     14. aosp_cf_x86_64_foldable-userdebug
     15. aosp_cf_x86_64_pc-userdebug
     16. aosp_cf_x86_64_phone-userdebug
     17. aosp_cf_x86_64_tv-userdebug
     18. aosp_cf_x86_phone-userdebug
     19. aosp_cf_x86_tv-userdebug
     20. aosp_coral-userdebug
     21. aosp_coral_car-userdebug
     22. aosp_flame-userdebug
     23. aosp_flame_car-userdebug
     24. aosp_oriole-userdebug
     25. aosp_oriole_car-userdebug
     26. aosp_raven-userdebug
     27. aosp_raven_car-userdebug
     28. aosp_redfin-userdebug
     29. aosp_redfin_car-userdebug
     30. aosp_redfin_vf-userdebug
     31. aosp_slider-userdebug
     32. aosp_sunfish-userdebug
     33. aosp_sunfish_car-userdebug
     34. aosp_trout_arm64-userdebug
     35. aosp_trout_x86-userdebug
     36. aosp_whitefin-userdebug
     37. aosp_x86-eng
     38. aosp_x86_64-eng
     39. arm_krait-eng
     40. arm_v7_v8-eng
     41. armv8-eng
     42. armv8_cortex_a55-eng
     43. armv8_kryo385-eng
     44. beagle_x15-userdebug
     45. beagle_x15_auto-userdebug
     46. car_ui_portrait-userdebug
     47. car_x86_64-userdebug
     48. db845c-userdebug
     49. gsi_car_arm64-userdebug
     50. gsi_car_x86_64-userdebug
     51. hikey-userdebug
     52. hikey64_only-userdebug
     53. hikey960-userdebug
     54. hikey960_tv-userdebug
     55. hikey_tv-userdebug
     56. poplar-eng
     57. poplar-user
     58. poplar-userdebug
     59. qemu_trusty_arm64-userdebug
     60. rb5-userdebug
     61. sdk_car_arm-userdebug
     62. sdk_car_arm64-userdebug
     63. sdk_car_md_x86_64-userdebug
     64. sdk_car_portrait_x86_64-userdebug
     65. sdk_car_x86-userdebug
     66. sdk_car_x86_64-userdebug
     67. sdk_pc_x86_64-userdebug
     68. silvermont-eng
     69. uml-userdebug
     70. yukawa-userdebug
     71. yukawa_sei510-userdebug

Which would you like? [aosp_arm-eng]

Pick from common choices above (e.g. 13) or specify your own (e.g. aosp_barbet-eng): 2

Hint: next time you can simply run 'lunch aosp_arm64-eng'

============================================

PLATFORM_VERSION_CODENAME=UpsideDownCake
PLATFORM_VERSION=UpsideDownCake
TARGET_PRODUCT=aosp_arm64
TARGET_BUILD_VARIANT=eng
TARGET_ARCH=arm64
TARGET_ARCH_VARIANT=armv8-a
TARGET_CPU_VARIANT=generic
TARGET_2ND_ARCH=arm
TARGET_2ND_ARCH_VARIANT=armv8-a
TARGET_2ND_CPU_VARIANT=generic
HOST_OS=linux
HOST_OS_EXTRA=Linux-5.15.0-43-generic-x86_64-Ubuntu-22.04-LTS
HOST_CROSS_OS=windows
BUILD_ID=AOSP.MASTER
OUT_DIR=out
============================================
```

简要说明编译的类型：

- user：限制所有权限，用于发布给用户使用的最终版本。
- userdebug： 开放部分权限，允许root。
- eng：工程师模式，开放所有权限并且有额外的调试工具。

如果只是编译出一个正常的AOSP环境，直接选择 `aosp_arm64-eng` 编译arm版本，或者选择 `aosp_x86_64-eng` 编译x86版本也可。最后输出编译的环境变量。

输出提示告诉我们也可以直接使用名称选择版本： `lunch aosp_arm64-eng` 。

第三步，使用make即可开始编译：

```bash
$ make -j8
```

`-j` 后面数字表示同时运行的编译任务数。如果编译中因为内存不足而停止，可以减少数字重新编译，不过编译时间会增加。

经过漫长等待，编译完成。所有编译出的结果都在 `/out` 目录中可以找到。其中有三个重要的镜像文件简单介绍一下：

- `system.img` ：系统镜像，里面包含了 Android 系统主要的目录和文件，通过 `init.rc` 进行解析并挂载到 `/system` 目录下。
- `userdata.img` ：用户镜像，是 Android 系统中存放用户数据的，通过 `init.rc` 进行解析并挂载到 `/data` 目录下。
- `ramdisk.img` ：根文件系统镜像，包含一些启动 Android 系统的重要文件，比如 `init.rc` 。

编译完成之后，可以通过使用模拟器来模拟运行，或者使用真机进行刷机。

模拟器运行直接运行这条指令：

```bash
$ emulator
```

通过 `adb` 连接手机，也可以把系统镜像刷入真机。

先通过 `adb` 让手机重启到fastboot模式：

```bash
$ adb reboot bootloader
```

在fastboot模式下，使用该命令刷入：

```bash
$ fastboot flashall -w
```

其中 `-w` 表示擦除掉 `/data` 分区。

### 附录：AOSP常用编译和查找命令

- `m` ：等同于 `make` ，进行一次完整编译。
- `mm` ：在当前目录下编译所有模块，但不包括依赖项。
- `mmm [目录]` ：编译指定目录下的所有模块，但不包括依赖项。只编译指定模块： `mmm [dir/:target1,target2]`
- `mma` ：在当前目录下编译所有模块，包括依赖项。
- `mmma [目录]` ：编译指定目录下的所有模块，包括依赖项。
- `make clean` ：删除整个 `out/` 目录。
- `make clobber` ：用于清除编译缓存。
- `make snod` ：当重新编译部分模块完成后，快速生成img文件。
- `cgrep` ：对C/C++文件执行 `grep` (只搜寻C/C++文件类型，也包括.h类型)。
- `jgrep` ：只对Java文件执行 `grep` (只搜寻Java文件类型)。
- `resgrep` ：只匹配 `/res` 目录下的 xml 文件。
- `sepgrep` ：只查找sepolicy文件。

参考：

- [一步一步教你如何在AndroidStudio查看Android源码（AOSP源码）](https://link.zhihu.com/?target=https%3A//blog.csdn.net/u011578734/article/details/108339776)
- [玩转AOSP源码编译](https://link.zhihu.com/?target=https%3A//blog.csdn.net/yzm65580952/article/details/120396315)

---

如果本文有帮到你的话，还请慷慨点赞收藏。我是ZeroFreeze，一名Android开发者，未来将持续向大家分享大量Android、Linux相关的技术知识文章。点点关注不迷路，我们下篇文章见~

编辑于 2023-02-03 21:52・北京[豆包让小白也能玩转 AI 漫画图片生成](https://www.doubao.com/chat/?channel=dbweb_zhihu_xxl_pc_cpc_ty_shengt_stcj_dmtp_512&source=dbweb_zhihu_xxl_pc_cpc_ty_shengt_stcj_dmtp_512&keywordid=3734823&ad_platform_id=zhihu_feed_lead&ug_callback_url=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D48ce739b-c656-4a6b-9574-af4bb5b5997a%26os%3D3%26zid%3D1629%26zaid%3D3744379%26zcid%3D3734823%26cid%3D3734823%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784531%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1Gdx87VSAsMi9Cb1cXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYJvLH-AH4Or5&cb=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D48ce739b-c656-4a6b-9574-af4bb5b5997a%26os%3D3%26zid%3D1629%26zaid%3D3744379%26zcid%3D3734823%26cid%3D3734823%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784531%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1Gdx87VSAsMi9Cb1cXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYJvLH-AH4Or5&ug_semver=v1.0.0&spu=biz%3D0%26ci%3D3734823%26si%3D471f3198-9987-4ff6-b7ba-bd384b737858%26ts%3D1781069469%26zid%3D1629)

[

豆包让小白也能玩转 AI 漫画图片生成

](https://www.doubao.com/chat/?channel=dbweb_zhihu_xxl_pc_cpc_ty_shengt_stcj_dmtp_512&source=dbweb_zhihu_xxl_pc_cpc_ty_shengt_stcj_dmtp_512&keywordid=3734823&ad_platform_id=zhihu_feed_lead&ug_callback_url=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D48ce739b-c656-4a6b-9574-af4bb5b5997a%26os%3D3%26zid%3D1629%26zaid%3D3744379%26zcid%3D3734823%26cid%3D3734823%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784531%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1Gdx87VSAsMi9Cb1cXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYJvLH-AH4Or5&cb=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3D48ce739b-c656-4a6b-9574-af4bb5b5997a%26os%3D3%26zid%3D1629%26zaid%3D3744379%26zcid%3D3734823%26cid%3D3734823%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784531%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1Gdx87VSAsMi9Cb1cXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYJvLH-AH4Or5&ug_semver=v1.0.0&spu=biz%3D0%26ci%3D3734823%26si%3D471f3198-9987-4ff6-b7ba-bd384b737858%26ts%3D1781069469%26zid%3D1629)