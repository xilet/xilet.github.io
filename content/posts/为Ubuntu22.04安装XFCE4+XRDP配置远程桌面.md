---
title: 为Ubuntu22.04安装XFCE4+XRDP配置远程桌面
date: 2024-01-17T10:10:50+08:00
tags:
  - ubuntu
  - xfce4
  - xrdp
  - remote desktop
  - vps
categories:
  - Tutorial
code:
  maxShownLines: 11
---

# 引言

xrdp 是一个开源的远程桌面协议服务器，允许非 Windows 操作系统通过 Microsoft RDP 协议进行远程桌面会话。本教程将指导您在 Ubuntu 22.04 上安装和配置 xrdp。


# 准备工作
- VPS [InterServer](https://www.interserver.net/r/938272)
- 任意 SSH 终端软件 [XShell](https://www.xshell.com/zh/free-for-home-school/)、[免费国产FinalShell](http://www.hostbuf.com/t/988.html)、[git-windows](https://git-scm.com/)
- 最好再有一个域名
## 安装步骤

### 1. 准备系统
首先，以 root 用户登录，并更新系统软件包：
```bash
sudo apt-get update
sudo apt-get upgrade
```

### 2. 创建新用户
为了安全起见，建议使用非 root 用户运行 xrdp。创建一个新用户并添加到 sudo 组：
```bash
sudo adduser user3389
sudo usermod -aG sudo user3389
groups user3389
```

### 3. 安装 xrdp
切换到新创建的用户（user3389），然后安装 xrdp 服务：
```bash
sudo apt install xrdp -y
```

### 4. 安装桌面环境
xrdp 需要桌面环境才能工作。在本教程中，我们使用轻量级的 Xfce 桌面环境：
```bash
sudo apt install xfce4 xfce4-goodies -y
```

### 5. 配置 xrdp
配置 xrdp 以使用 Xfce 作为桌面环境：
```bash
cd ~
echo "xfce4-session" | tee .xsession
```

### 6. 配置防火墙
允许远程桌面连接通过 Ubuntu 防火墙：
```bash
sudo ufw allow from any to any port 3389 proto tcp
```

### 7. 安装 Web 浏览器
为了在远程桌面会话中使用网络浏览器，安装 Firefox 和 Chromium：
```bash
sudo apt install firefox
sudo apt install chromium-browser
```

### 8. 启动 xrdp 服务
启动 xrdp 服务，并确保它在系统启动时自动启动：
```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

## 连接到您的 Ubuntu 22.04 远程桌面
完成以上步骤后，就可以从任何支持 RDP 的设备（如 Windows 操作系统 中的 mstsc）连接到 Ubuntu 22.04 VPS桌面了。

1. 打开远程桌面连接工具,例如 mstsc 。
2. 输入 Ubuntu 服务器的 IP 地址，并点击连接。
3. 输入之前创建的用户 `user3389` 的用户名和密码。

现在，应该能够看到 Ubuntu 的桌面环境，并可以通过远程桌面进行操作。

## 结语
通过这篇教程，演示了如何在 Ubuntu 22.04 上安装和配置 xrdp，使得可以通过远程桌面协议轻松地访问服务器。

