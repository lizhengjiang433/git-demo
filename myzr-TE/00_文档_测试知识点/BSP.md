公司虚拟机地址：192.168.130.73

用户：lizj	密码：92wVyzU6croD17if

## BSP

固件编译打包：

LXD 容器 → SDK 依赖部署 → 源码构建 → 交叉编译 → 生成固件镜像 → 驱动适配 + 镜像烧录 → 烧录工具量产 → 硬件功能测试

> 软件开发工具包：Software Development Kit

## 存储池

```
lxc storage list
```



## 镜像

1. 查看远程仓库镜像源

   ```
   lxc image list images: ubuntu/
   ```

2. 查看本地镜像模板

   ```
   lxc image list
   ```

   输出参数说明：

   - ALIAS：镜像别名，自定义短名称
   - DESCRIPTION：镜像内置系统版本、打包日期（官方镜像）

   - type：镜像类型，container=容器镜像（非虚拟镜像）

   - upload date：镜像导入本机LXD仓库时间
   - fingerprint：镜像唯一哈希指纹ID

 3. 当别名重复或者失效时，用指纹ID操作镜像

    ```
    lxc image delete 697665475e11
    ```

 4. 用基础镜像（DESCRIPTION）启动容器，在容器内安装交叉编译工具、配置环境、源码依赖。再执行打包发布命令，把配置好的容器文件系统打包成新镜像

       ```
       lxc publish myzr-u2004-lizj-20260602 --alias myzr-u2004-3588
       ```

       lxc publish ：将运行 / 停止的容器快照打包，生成一份只读镜像模板

       > myzr-u2004-lizj-20260602：临时容器名

 5. 容器打包publish后生成镜像(只读 )，镜像launch后能生成容器，二者可以互相转换。镜像只读，容器可读可写

       ```
       lxc launch 基础镜像别名 临时容器名
       ```

       

## LXD容器

LXD 是一套完整可用的 Ubuntu 文件系统（有 /bin、/usr、apt、用户、环境变量，命令、软件用法和真机 Ubuntu 完全一致），用来统一固定编译环境，避免不同 Ubuntu / 系统版本、库版本差异导致编译报错。

1. 查看全部LXD存储池底下的容器,包括已停止运行容器

```
lxc storage info lizj-pool
```

- 进入容器


```
lxc exec myzr-u2004-8MP --user 1007 --group 1007 --env HOME=/home/lizj --cwd /home/lizj -- /bin/bash

lxc exec myzr-u2004-3588 --user 1007 --group 1007 --env HOME=/home/lizj --cwd /home/lizj -- /bin/bash

lxc exec myzr-u2204-3576 --user 1007 --group 1007 --env HOME=/home/lizj --cwd /home/lizj -- /bin/bash

lxc exec myzr-u2204-t536 --user 1007 --group 1007 --env HOME=/home/lizj --cwd /home/lizj -- /bin/bash
```

- 退出当前容器


```
exit
```

> 在容器外执行容器命令需要在命令前面加上lxc

2. 改容器名字

```
# 第一步：停止k1容器
lxc stop myzr-u2004-k1

# 第二步：改名为myzr-u2004-8MP
lxc move myzr-u2004-k1 myzr-u2004-8MP

# 第三步：启动新名称容器
lxc start myzr-u2004-8MP

# 查看改名结果
lxc list
```



## 烧录

1. 烧录模式与权限层级

   - MaskROM 模式（最高权限）：芯片内部固化的不可变引导代码。拥有最高硬件权限，可以全片擦除 Flash，重写所有分区（包括 loader 分区）。仅在 EMMC 空白、Loader 分区损坏或更换硬件时进入。
   - Loader 模式（普通开发模式）：依赖 EMMC 中已有的 Miniloader.bin 引导。无法擦除/覆盖自身所在的 Loader 分区（保护机制，防止变砖）。

2. 烧录文件类型与对应关系

   - update.img（整包）：所有模式（MaskROM / Loader）均可烧录，包含固件全部分区。
   - 分区镜像（如 boot.img / kernel.img / system.img）：只能在 Loader 模式下单独烧录。MaskROM 模式下极简引导不支持拆分开刷，仅能写入底层的 Miniloader.bin。

3. 修改需求与编译模块对应关系

   - U-Boot：DDR时序调优、存储介质（eMMC/NAND/SPI NOR）初始化适配、板级电源管理（LDO/PMIC）上下电时序调整、引导加载流程定制
   - Linux 内核：外设驱动（LCD/TP/摄像头/传感器/网口/Wi-Fi/音频Codec）、引脚功能复用（I2C/SPI/UART/PWM/GPIO）、内核功能裁剪（文件系统类型、网络协议栈、调试接口）
   - Buildroot：根文件系统内容定制（第三方库集成、调试工具添加、自研业务进程部署）、开机自启动脚本（inittab/init.d）、系统服务配置、环境变量与默认配置预设

   列出所有镜像

   ```
   lxc i
   ```

   

   

1. U-Boot=电脑主板BIOS

​	板子通电后最先跑的程序，先把内存、显示屏、网口这些硬件唤醒初始化，再找到系统初始化

​	在更换底板，改内存布线，改启动方式时需要重新编译

2. Linux内核=windows系统底层核心

​	所有的硬件，包括屏幕、网口、按键、USB、芯片引脚都要靠设备树DTS记录底板引脚定义

​	在换引脚、改屏幕/案件引脚、开启内核nfs网络协议，就要重新编译内核

3. buildroot根文件系统=Windows里的软件、工具、桌面

   内核启动必须加载这个，里面包括命令，图形桌面，各类工具。

   在缺工具，加软件，改开机脚本，只需要重新编译Buildroot

SPL

Secondary Program Loader，中文：二级程序加载器

信号眼宽度在0x30~0x40之间，窗口充足，内存稳定

采样相位最小/中间/最大值，区间跨度均匀=布线匹配

verf电压：LPDDR一般25%~32%，超出区间会信号失真

结尾out=DDR全部训练完成，无内存故障

## 查看内核版本

```
#输入
uname -a
#输出
Linux myzr-rk3588-buildroot 5.10.226 #4 SMP Wed Jun 17 03:02:26 UTC 2026 aarch64 GNU/Linux
```

或者

```
#查看Buildroot文件系统版本
cat /etc/buildroot-version
```

## 清理内存

```
# 清理全部缓存安装包
sudo apt clean
# 清理不再使用的旧版本缓存包（保留当前在用版本）
sudo apt autoclean
```



## Busybox

Busybox内置精简版ln

```
# 格式：ln -s 源文件/目录 软链接名称
ln -s /etc/pulse/ pulse-link
```

## 文件系统

1.

- ext4:

Linux原生系统

存放 Linux 系统、Buildroot 根文件、程序、日志、录音；

支持权限、软链接、断电日志保护，是系统运行必需；

- FAT32：


微软标准，Windows、Linux、相机、车载全设备通用

只做跨设备交换介质：把板子文件拷到 Windows，或者电脑拷固件到开发板；

查看文件系统类型：

```
#只看跟目录
df -Th /
#输出
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/root      ext4   14G  729M   13G   6% /
```

开发板上电后，系统真正挂载为 `/` 根目录的分区格式，是设备实际运行使用的 rootfs。



**2.**	查看当前内核编译支持的全部文件系统：

```
cat /proc/filesystems
```

- Buildroot 全称 Buildroot Build System，是根文件系统编译构建工具，用来打包生成 rootfs 镜像，不是文件系统格式；
- ext4/ubifs/squashfs/f2fs 这些才是**文件系统类**型，是分区的存储格式。

## 查看DTS



```python
#查找指令
find . -name "*.dts"
#T536设备数位置
/home/lizj/my-work/T536/t536-linux-5.10.198/device/config/chips/t536/configs/myzr_t536_ek270_amp/linux-5.10-rt/board.dts
#csz
/home/lizj/my-work/T536/chensz_t536/device/config/chips/t536/configs/myzr_t536_ek270/linux-5.10-origin/board.dts
```

设备节点 = 内核给硬件的操作入口

设备树 = 硬件与节点的映射表

驱动 = 翻译官，把两者连起来

## 二进制可执行文件（out）

.out 是交叉编译生成的嵌入式二进制可执行程序，不是文本文件

反汇编查看汇编指令

```
# 全志T536是ARM64，用aarch64-linux-gnu-objdump
aarch64-linux-gnu-objdump -d ./test_app/gpio_test.t
```







```

```

