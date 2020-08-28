# Raspberry PI 4板级支持包说明

## 1. 简介

树莓派4B的核心处理器为博通BCM2711（四核1.5GHz，Cortex A72架构，树莓派3是四核A53）。LPDDR4内存，由5V/3A USB-C供电或GPIO 5V。

外设支持上，引入了双频Wi-Fi，蓝牙5.0，千兆网卡，MIPI CSI相机接口，两个USB口，40个扩展帧。

这份RT-Thread BSP是针对 Raspberry Pi 4的一份移植，同时本历程搭载了`Tensorflow Lite Micro`嵌入式深度学习框架, 可以实现官方自带的有关测试和历程


## 2. 编译说明

Linux下推荐使用[gcc工具][2]。Linux版本下gcc版本可采用`gcc-arm-8.3-2019.03-x86_64-aarch64-elf`。

将工具链解压到指定目录,并修改当前bsp下的`EXEC_PATH`为自定义gcc目录。

```
PLATFORM    = 'gcc'
EXEC_PATH   = r'/opt/gcc-arm-8.3-2019.03-x86_64-aarch64-elf/bin/'  
```

直接进入`bsp\raspberry-pi\raspi4`，输入scons编译即可。


## 3. 执行

### 3.1 下载**Raspberry Pi Imager**，生成可以运行的raspbian SD卡

首先下载镜像

* [Raspberry Pi Imager for Ubuntu](https://downloads.raspberrypi.org/imager/imager_amd64.deb)
* [Raspberry Pi Imager for Windows](https://downloads.raspberrypi.org/imager/imager.exe)
* [Raspberry Pi Imager for macOS](https://downloads.raspberrypi.org/imager/imager.dmg)

### 3.2 准备好串口线

目前版本是使用raspi4的 GPIO 14, GPIO 15来作路口输出，连线情况如下图所示：

![raspi2](../raspi3-32/figures/raspberrypi-console.png)

串口参数： 115200 8N1 ，硬件和软件流控为关。

### 3.3 程序下载

当编译生成了rtthread.bin文件后，我们可以将该文件放到sd卡上，并修改sd卡中的`config.txt`文件如下：

```
enable_uart=1
arm_64bit=1
kernel=rtthread.bin
```

按上面的方法做好SD卡后，插入树莓派4，通电可以在串口上看到如下所示的输出信息：

```text
heap: 0x000c9350 - 0x040c9350

 \ | /
- RT -     Thread Operating System
 / | \     4.0.3 build Apr 16 2020
 2006 - 2020 Copyright by rt-thread team
 model load successfully!!
 Heard yes (203) @1400ms
 Heard yes (201) @9400ms
 Heard yes (201) @17400ms
 Heard yes (201) @25400ms
 ...
```

目前工程执行的是根目录下`Application `中的`main.cc` 中的`main`函数, 工程实现了官方自带语音模型, 识别简单的yes和no关键字. 编译运行并装载到树莓派板子之后, 可以看到如上图所示的输出
## 4. 目录结构

`application`: 用于放置用户自定义实现文件

`driver: 目前板级的驱动文件`

`fixedpoint`: `Tensorflow Lite Micro`需要的定点数支持目录

`flatbuffers`: 为`Tensorflow Lite Micro`提供`tflite`模型提供解析支持

`ruy`:  为`Tensorflow Lite Micro`提供矩阵乘法的加速支持

`tensorflow`: `Tensorflow Lite Micro`的主体实现

## 5. 支持情况

| 驱动 | 支持情况  |  备注  |
| ------ | ----  | :------:  |
| UART | 支持 | UART0|

## 6. 联系人信息

维护人：[bernard][5]

[1]: https://www.rt-thread.org/page/download.html
[2]: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads
[3]: https://downloads.raspberrypi.org/raspbian_lite_latest
[4]: https://etcher.io
[5]: https://github.com/BernardXiong
[6]:https://tensorflow.google.cn/lite/microcontrollers
