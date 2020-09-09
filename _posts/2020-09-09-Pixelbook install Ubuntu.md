---
layout: post
category: Linux
title: Pixelbook install Ubuntu
tags: 
  - Pixelbook
  - Ubuntu
---

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

4. 安装工具

   ```shell
   sudo apt install -y git python ansible
   
   #配置git
   git config --global user.name xxxx
   git config --global user.email xxxx
   ```

5. 安装驱动

   ```shell
   #以下地址二选一即可
   #源地址
   git clone https://github.com/yusefnapora/pixelbook-linux
   #已解决大部分问题建议使用
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
sudo apt-get remove iio-sensor-proxy
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

