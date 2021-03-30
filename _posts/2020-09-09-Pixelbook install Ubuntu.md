---
layout: post
category: Linux
title: Pixelbook install Ubuntu
tags: 
  - Pixelbook
  - Ubuntu
---

#### 刷写SeaBIOS,安装UBUNTU

1. Pixelbook关机状态按下`ESC+Refresh+Power`进入Developer模式,`Ctrl+D`进入系统

2. 安装SeaBIOS,`CTRL+ALT+T`打开终端

   ```shell
   shell
   cd; curl -LO mrchromebox.tech/firmware-util.sh
   sudo install -Dt /usr/local/bin -m 755 firmware-util.sh
   sudo firmware-util.sh
   ```

   根据提示安装Full ROM Firmware

3. 正常安装ubuntu 19.10

   [Ubuntu releases](http://old-releases.ubuntu.com/releases/19.10/)

   Ubuntu 19.10版本官方不再维护,安装完成后需要手动替换源地址
   ```shell
   sudo gedit /etc/apt/sources.list
   #使用以下源替换sources.list内的全部内容
   deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan main restricted universe multiverse
   deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-security main restricted universe multiverse
   deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-updates main restricted universe multiverse
   deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-proposed main restricted universe multiverse
   deb http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-backports main restricted universe multiverse
   deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan main restricted universe multiverse
   deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-security main restricted universe multiverse
   deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-updates main restricted universe multiverse
   deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-proposed main restricted universe multiverse
   deb-src http://mirrors.ustc.edu.cn/ubuntu-old-releases/ubuntu/ eoan-backports main restricted universe multiverse
   ```

4. 安装工具

   ```shell
   sudo apt install -y git python ansible
   
   #配置git
   git config --global user.name xxxx
   git config --global user.email xxxx
   ```

   如需切换python版本，使用如下命令
   ```shell
   #查看已有python版本
   whereis python
   #具体版本号根据whereis python查看
   sudo update-alternatives  --install /usr/bin/python python /usr/bin/python2.7 1
   sudo update-alternatives  --install /usr/bin/python python /usr/bin/python3.5 2
   #列出已添加的python版本
   sudo update-alternatives --list python
   #切换版本
   sudo update-alternatives --config python
   ```

5. 安装驱动

   ```shell
   #以下地址二选一即可
   #原始版本
   git clone https://github.com/yusefnapora/pixelbook-linux
   #修改版(包替换为国内地址)
   git clone https://github.com/mxzeng/pixelbook-linux
   
   cd pixelbook-linux
   ./run-ansible.sh
   #所有脚本执行完成后重启
   sudo reboot
   ```

#### 安装过程中遇到的问题

```shell
#Pixelbook安装ubuntu光标颠倒/触摸显示错误
#https://askubuntu.com/questions/1061403/upside-down-mouse-cursor-and-inverted-position-on-ubuntu-18-04
sudo apt-get update
sudo apt-get upgrade
#ubuntu20.04/20.10更新重启即正常
#19.10版本使用命令翻转屏幕，待安装驱动后即可正常使用
#inverted 上下翻转,normal 正常显示,left 向左旋转90度,right 向右旋转90度
sudo xrandr -o inverted
#依然不正常,尝试移除重力方向调节
#sudo apt-get remove iio-sensor-proxy
```

```shell
#Unexpected templating type error occurred on ({{ kpartx_output.stdout | regex_search(regexp, '\\1') | first }}): 'NoneType' object is not iterable

vi pixelbook-linux/ansible/roles/eve-recovery-files/tasks/main.yml
#将 (loop[0-9]p3) 修改为为 (loop[0-9]+p3)
```

```shell
#Destination directory /etc/libinput does not exist

sudo mkdir /etc/libinput/
```

```shell
#Error Building cras -> Pixelbook Speakers Undetected and Not Working

sudo vi /opt/eve-linux-setup/adhd/cras/src/makefile
#注释掉Werror所在两行代码
#COMMON_CPPFLAGS = -02 -Wall -Werror -Wno-error=cpp
#COMMON_SIMD_CPPFLAGS = -03 -Wall -Werror -Wno-error=cpp
```

#### 键盘背光灯亮度调节 ####

```shell
# set brightness to 50%
eve-keyboard-brightness.sh 50

# turn backlight off:
eve-keyboard-brightness.sh 0

# increase brightness by 10:
eve-keyboard-brightness.sh +10

# decrease by 20:
eve-keyboard-brightness.sh -20

#可以将脚本添加到键盘快捷键中
```

#### 清除安装文件

```shell
sudo rm -rf /opt/eve-linux-setup
```

__Do NOT remove__ `/opt/google` \- it contains some files needed by the audio setup.

> 参考资料
>
> [Pixelbook 2017 安装Ubuntu 替代 ChromeOS，屏幕亮度声音触碰版背光一切正常。](https://blog.csdn.net/ZG123456h/article/details/105505627)
>
> [Pixelbook-linux](https://github.com/yusefnapora/pixelbook-linux)
>
> [Installing 'real' linux on a Google Pixelbook](https://github.com/yusefnapora/pixelbook-linux#flashing-uefi-firmware)
>
> [Chromebook Modding work](https://coolstar.org/chromebook/)
>
> [MrChromebox.tech](https://mrchromebox.tech/#fwscript)

