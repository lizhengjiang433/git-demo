## 查找某一类文件**内容

```
grep -r -w sun55iw6 --include "*.dtb"
```

## 用户名变色：

```yaml
echo "export PS1='\[\e[1;32m\]\u@\h\[\e[0m\]:\[\e[1;34m\]\w\[\e[0m\]\$ '" >> ~/.bashrc
source ~/.bashrc

#如果每次都要输入source ~/.bashrc才生效
vi ~/.bash_profile
#添加如下内容
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

## lxc

有容器镜像的情况下,无脑创建容器

一行一行打

容器相关，容器可以理解成 “**轻量级的虚拟电脑**

```c
# 给容器起个简短的代号“k1”，方便后续操作；
name=k1
# 用现成的镜像 myzr-u2004-chensz-20250429 创建容器，命名为 myzr-u2004-$name，指定存储池为 chensz-pool
lxc launch myzr-u2004-chensz-20250429 myzr-u2004-$name --storage chensz-pool
# 在容器里创建用户 chensz，UID设为1002，同时创建/home/chensz目录
lxc exec myzr-u2004-$name -- useradd -u 1002 -m chensz
# 给用户 chensz 添加免sudo密码的权限配置
lxc exec myzr-u2004-$name -- bash -c 'echo "chensz ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/chensz'
# 进入容器的终端，开始后续配置
lxc exec myzr-u2004-$name -- bash
# 把容器里的软件源替换为清华镜像源，提升下载速度
sed -i s#archive.ubuntu.com#mirrors.tuna.tsinghua.edu.cn# /etc/apt/sources.list
# 更新软件源列表并升级系统内的软件包
apt update && apt upgrade -y
# 安装sudo工具，确保权限配置生效
apt install sudo
# 再次添加免sudo密码的权限，防止配置文件未生效
echo 'chensz ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# 给容器设置特权模式，允许访问主机的更多资源
lxc config set myzr-u2004-$name security.privileged true
# 重启容器，让特权模式配置生效
lxc restart myzr-u2004-$name
# 把主机的/home/chensz目录挂载到容器的/home/chensz，实现文件共享
lxc config device add myzr-u2004-$name host-data disk source=/home/chensz path=/home/chensz
# 以用户chensz的身份进入容器，同时设置HOME环境变量为/home/chensz
lxc exec myzr-u2004-$name --user 1002 env HOME=/home/chensz -- bash

# ============ 二、LXC基础配置（从零搭建环境） ============
# 1. 切换到以你用户名命名的 LXD 项目（这里是chensz）
lxc project switch chensz
# 2. 在/home/chensz下创建一个隐藏目录，用来存放容器的磁盘数据
mkdir /home/chensz/.lxc_disk
# 3. 创建名为chensz-pool的存储池，使用上面创建的目录作为存储位置
lxc storage create chensz-pool dir source=/home/chensz/.lxc_disk
# 4. 列出所有存储池的基本信息，确认创建成功
lxc storage list
# 5. 查看chensz-pool这个存储池的详细配置信息
lxc storage show chensz-pool
# 6. 把chensz-pool存储池的根磁盘挂载到容器的/目录上，让容器使用这个存储池
lxc profile device add default root disk path=/ pool=chensz-pool
# 7. 创建名为chensz-net的网络，自动分配IPv4地址并开启NAT，关闭IPv6
lxc network create chensz-net \    
  ipv4.address=auto \
  ipv4.nat=true \
  ipv6.address=none
# 8. 给默认配置添加网卡设备，使用桥接模式连接到chensz-net网络，网卡名设为eth0
lxc profile device add default eth0 nic \
  nictype=bridged \
  parent=chensz-net \
  name=eth0
# 9. 再次切换到chensz项目，确保后续操作都在这个项目下进行
lxc project switch chensz
# 10. 设置两个变量：CTR_VER是镜像版本，CTR_NAME是容器的基础名称
export CTR_VER=myzr-u2004-chensz-20250429
export CTR_NAME=myzr-u2004
# 11. 从指定地址下载LXC容器镜像压缩包
wget http://tangbin.lan:8080/RA.%E5%8F%91%E5%B8%83%E5%8C%BA/ContainerImages/lxd-image-myzr-u2004-tangb-20250429.tar.gz
# 12. 导入下载的镜像，并设置别名为myzr-u2004-chensz-20250429，方便后续引用
lxc image import lxd-image-myzr-u2004-tangb-20250429.tar.gz --alias myzr-u2004-chensz-20250429
# 13. 查看已导入的所有镜像列表，确认镜像导入成功
lxc image list
# 14. 用导入的镜像创建名为myzr-u2004的容器，使用chensz-pool存储池
lxc launch myzr-u2004-chensz-20250429 myzr-u2004 --storage chensz-pool
# 15. 查看所有容器的状态，确认容器是否创建并启动成功
lxc list
# 16. 在容器里创建用户chensz，UID设为1002，同时创建/home/chensz目录
lxc exec myzr-u2004 -- useradd -u 1002 -m chensz
# 给用户chensz添加免sudo密码的权限配置
lxc exec myzr-u2004 -- bash -c 'echo "chensz ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/chensz'
# 17. 以用户chensz的身份进入容器，同时设置HOME环境变量为/home/chensz
lxc exec myzr-u2004 --user 1002 env HOME=/home/chensz -- bash
# 18. 把容器里的软件源替换为清华镜像源，提升下载速度
sed -i s#archive.ubuntu.com#mirrors.tuna.tsinghua.edu.cn# /etc/apt/sources.list
# 19. 更新软件源列表并升级系统内的软件包
apt update && apt upgrade -y
# 20. 退出容器终端，回到主机的命令行
exit
# 21. 给容器设置特权模式，允许访问主机的更多资源
lxc config set myzr-u2004 security.privileged true
# 重启容器，让特权模式配置生效
lxc restart myzr-u2004
# 把主机的/home/chensz目录挂载到容器的/home/chensz，实现文件共享
lxc config device add myzr-u2004 host-data disk source=/home/chensz path=/home/chensz

# ============ 三、容器日常操作基础命令 ============
# 切换到chensz项目，确保后续操作都在自己的项目下进行
lxc project switch chensz
# 查看所有容器的运行状态、IP地址等信息
lxc list
# 以root用户身份进入容器的终端
lxc exec myzr-u2004 -- bash
# 更新软件源列表
apt update
# 安装sudo工具
apt install sudo
# 给用户chensz添加免sudo密码的权限
echo 'chensz ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# 以用户chensz的身份进入容器，同时设置HOME环境变量为/home/chensz
lxc exec myzr-u2004 --user 1002 env HOME=/home/chensz -- bash
# 启动已创建的容器
lxc start myzr-u2004
# 停止正在运行的容器
lxc stop myzr-u2004
# 删除已停止的容器
lxc delete myzr-u2004
```



```C
# 1. 以你用户名命名的 LXD 项目
lxc project switch chensz
# 2.创建目录
mkdir /home/chensz/.lxc_disk
# 3.创建自己的存储池
lxc storage create chensz-pool dir source=/home/chensz/.lxc_disk
# 4.列出所有存储池的基本信息
lxc storage list

+--------------+--------+-------------------------+-------------+---------+---------+
|     NAME     | DRIVER |         SOURCE          | DESCRIPTION | USED BY |  STATE  |
+--------------+--------+-------------------------+-------------+---------+---------+
| chensz-pool  | dir    | /home/chensz/.lxc_disk  |             | 3       | CREATED |
+--------------+--------+-------------------------+-------------+---------+---------+
# 5.列出指定存储池的基本信息
lxc storage show chensz-pool

name: chensz-pool
description: ""
driver: dir
status: Created
config:
  source: /home/chensz/.lxc_disk
used_by:
- /1.0/instances/chensz-u2004?project=chensz
- /1.0/instances/myzr-u2004?project=chensz
- /1.0/profiles/default?project=chensz
locations:
- none
# 6.将 chensz-pool 存储池中的一个磁盘设备挂载到容器的根目录(/)上
lxc profile device add default root disk path=/ pool=chensz-pool
# 7.创建网络
lxc network create chensz-net \    创建一个新的网络,网络名称chensz-net
  ipv4.address=auto \              自动分配 IPv4 地址段
  ipv4.nat=true \                  启用 IPv4 的 NAT，这样容器可以通过主机访问外网
  ipv6.address=none                禁用 IPv6 支持
  
  lxc network create chensz-net \    
  ipv4.address=auto \
  ipv4.nat=true \
  ipv6.address=none
  8.
  lxc profile device add default eth0 nic \    添加网络接口eth0
  nictype=bridged \                            设置为桥接模式
  parent=chensz-net \                          桥接名为之前新建的网卡
  name=eth0                                    网卡名eth0
  
  lxc profile device add default eth0 nic \
  nictype=bridged \
  parent=chensz-net \
  name=eth0
# 9.切换当前 LXD 操作上下文到指定的项目
lxc project switch chensz
# 10.设置变量
export CTR_VER=myzr-u2004-chensz-20250429
export CTR_NAME=myzr-u2004
# 11.下载镜像
wget http://tangbin.lan:8080/RA.%E5%8F%91%E5%B8%83%E5%8C%BA/ContainerImages/lxd-image-myzr-u2004-tangb-20250429.tar.gz
# 12.将本地的 LXD 容器镜像文件导入 LXD 中，并设置一个别名
lxc image import lxd-image-myzr-u2004-tangb-20250429.tar.gz --alias myzr-u2004-chensz-20250429
# 13.查看导入的镜像列表
lxc image list
# 14.创建容器
lxc launch myzr-u2004-chensz-20250429 myzr-u2004 --storage chensz-pool
# 15.查看容器
lxc list
# 16.创建用户
lxc exec myzr-u2004 -- useradd -u 1002 -m chensz
lxc exec myzr-u2004 -- bash -c 'echo "chensz ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/chensz'
# 17.进入容器
lxc exec myzr-u2004 --user 1002 env HOME=/home/chensz -- bash
# 18.替换源
sed -i s#archive.ubuntu.com#mirrors.tuna.tsinghua.edu.cn# /etc/apt/sources.list
# 19.更新系统
apt update && apt upgrade -y
# 20.退出容器
exit
# 21.
lxc config set myzr-u2004 security.privileged true
lxc restart myzr-u2004
lxc config device add myzr-u2004 host-data disk source=/home/chensz path=/home/chensz
# 指定 UID 和 GID 的挂载方式

# 7. 容器操作基本命令
# 切换到自己的 LXC 项目
lxc project switch chensz
# 查看容器状态
lxc list
# 进入容器
lxc exec myzr-u2004 -- bash

apt update
apt install sudo
echo 'chensz ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


lxc exec myzr-u2004 --user 1002 env HOME=/home/chensz -- bash
# 启动容器
lxc start myzr-u2004
# 停止容器
lxc stop myzr-u2004
# 删除容器
lxc delete myzr-u2004
```

## 复制内容不改权限

```
cp -a --no-preserve=ownership etc/ binary/etc/
```

## ubantu下的转函数定义工具

下载工具

```
sudo apt update

sudo apt install exuberant-ctags
```

启用

```
ctags -R
```

用法

转定义：Ctrl + ] 	返回：Ctrl + t

## ubantu自动获取ip配置

把有网的网卡共享网络给连接的网卡

把连接的网卡ip改成：192.168.137

```
vi /etc/netplan/01-netcfg.yaml
```

```
network:
 version: 2
 renderer: NetworkManager
 ethernets:
  eth0:
   dhcp4: yes
   optional: true
 version: 2
 renderer: NetworkManager
 ethernets:
  eth1:
   dhcp4: yes
   optional: true
```

使配置生效

```
netplan apply
```

## 添加硬盘并映射网络驱动器

**#安装samba**

sudo apt update

sudo apt -y install samba

#**在文件末尾添加**

sudo vi /etc/samba/smb.conf

[Csz.d]
        comment = Home Directory
        path = /home/tangbin/vdi
        browseable = no
        read only = no
        guest ok = no
        valid users = tangbin

#**设置用户密码并重启服务**

sudo smbpasswd -a `whoami`
sudo /etc/init.d/smbd restart

#映射网络驱动器

此电脑->右键网络驱动器->设置\\192.168.9.18\Csz





- **关闭防火墙**
- **更改本地组策略**
  - 在Windows系统中，你可以通过本地组策略编辑器来更改这些设置。按下 Win + R 键，输入 gpedit.msc 并回车，打开本地组策略编辑器。
  - 导航到 计算机配置 -> 管理模板 -> 网络 -> Lanman 工作站。
  - 找到“启用不安全的来宾登录”策略，并将其设置为“已启用”。
- 此电脑->右键网络驱动器->设置\\192.168.9.18\home.d



## x86下编译成arm64格式文件

```
sudo apt install gcc-aarch64-linux-gnu
aarch64-linux-gnu-gcc gpio_test.c -o gpio_test
```

## x86下编译成riscv格式文件

```
sudo apt install gcc-riscv64-linux-gnu
riscv64-linux-gnu-gcc gpio_test.c -static -o gpio_test_riscv
```

## ubuntu忘记root密码
