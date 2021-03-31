---
layout: post
category: Linux
title: Linux Clash设置
tags: 
  - Linux
  - Ubuntu
---

#### 安装Clash
下载Clash:[clash](https://github.com/Dreamacro/clash/releases)

```shell
#解压
gunzip clash-linux-amd64-v1.4.2.gz
sudo mv clash-linux-amd64-v1.4.2 /usr/local/bin/clash
sudo chmod +x /usr/local/bin/clash
mkdir ~/.config/clash
#下载config.yml  Country.mmdb 文件，并mv到~/.config/clash目录
wget -O config.yml https://xxx
wget -O Country.mmdb https://xxx
mv config.yml ~/.config/clash/
mv Country.mmdb ~/.config/clash/
```
#### 创建clash.service
`sudo vim /etc/systemd/system/clash.service`
填入以下内容:
```
[Unit]
Description=clash service
After=network.target

[Service]
Type=simple
User=#当前用户
Restart=on-abort
ExecStart=/usr/local/bin/clash

[Install]
WantedBy=multi-user.target
```
#### 为用户账户运行clash系统实例
```shell
#重新加载systemd模块
systemctl daemon-reload
#启动clash服务
systemctl start clash
#设置开机启动
systemctl enable clash
```
#### Clash web 管理页面
[clash web port](http://clash.razord.top/)