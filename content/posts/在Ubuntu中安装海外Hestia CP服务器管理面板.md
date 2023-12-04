---
title: 在Ubuntu中安装海外Hestia CP服务器管理面板
date: 2023-12-04T12:43:11+08:00
tags:
  - ubuntu
  - hestia
  - vps
  - control panel
categories:
  - Tutorial
code:
  maxShownLines: 11
# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

```bash

apt-get update
apt-get upgrade




sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X
sudo iptables -t mangle -F
sudo iptables -t mangle -X
sudo iptables -t raw -F
sudo iptables -t raw -X
sudo iptables -t security -F
sudo iptables -t security -X
sudo iptables-save | sudo tee /etc/iptables/rules.v4

#国内VPS可能无法访问 raw.githubusercontent.com 配置代理用于下载安装脚本
export http_proxy=http://user:pass@xxx.xxx.xxx.xxx:port
export https_proxy=http://user:pass@xxx.xxx.xxx.xxx:port
```

访问[Hestia CP 安装参数生成页面](https://hestiacp.com/install.html)

```bash
wget https://raw.githubusercontent.com/hestiacp/hestiacp/release/install/hst-install.sh
#这里的命令可以用官方提供的安装参数生成页面来生成
bash hst-install.sh --port 2083 --apache no --named no --mysql no --mysql8 yes --exim no --dovecot no --clamav no --spamassassin no --iptables no --fail2ban no --api no --interactive no
```
<!--more-->
