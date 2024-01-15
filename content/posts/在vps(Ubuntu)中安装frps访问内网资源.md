---
title: 在vps(Ubuntu)中安装frps访问内网资源
date: 2024-01-15T14:52:50+08:00
tags:
  - ubuntu
  - frp
  - vps
  - 公网
  - 穿透
  - 内网
categories:
  - Tutorial
code:
  maxShownLines: 11
---

# 准备工作
- 一台具有公网IP的VPS，如果没有的话可以购买 [InterServer](https://www.interserver.net/r/938272)
- 任意 SSH 终端软件 [XShell](https://www.xshell.com/zh/free-for-home-school/)、[免费国产FinalShell](http://www.hostbuf.com/t/988.html)、[git-windows](https://git-scm.com/)
- 最好再有一个域名

# 什么是[frp](https://github.com/fatedier/frp)
frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。

# 配置以及安装 frp
- 通过SSH连接到远程主机，下载 frp
  ```bash
  # 下载最新的 frp
  wget https://github.com/fatedier/frp/releases/download/v0.53.2/frp_0.53.2_linux_amd64.tar.gz
  ```
- 解压文件
  ```bash
  tar -zxvf frp_0.53.2_linux_amd64.tar.gz
  ```

- 配置 frp
  ```bash
  sudo mv frp_0.53.2_linux_amd64/frps /usr/bin
  sudo mkdir /etc/frp
  # 创建并编辑 frps.ini 配置文件
  sudo nano /etc/frp/frps.ini
  ```

  在 `frps.ini` 文件中添加以下内容：
  ```ini
  bindPort = 7000
  
  auth.token = "123456"
  # 其他配置...
  ```

- 设置 frp 为系统服务并启动
  ```bash
  sudo nano /etc/systemd/system/frps.service
  ```

  在 `frps.service` 文件中添加以下内容：
  ```ini
  [Unit]
  Description=FRP Server
  After=network.target

  [Service]
  Type=simple
  User=root
  ExecStart=/usr/bin/frps -c /etc/frp/frps.ini
  
  [Install]
  WantedBy=multi-user.target
  ```
  启动并启用 frp 服务：
  ```bash
  sudo systemctl start frps
  sudo systemctl enable frps
  ```
到这里VPS端就配置完成
# 配置 内网 端
  编辑 frpc.ini 文件
  ```ini
  #serverAddr = "your vps ip"
  serverAddr = "192.168.1.123"
  serverPort = 7000
  #auth.token和服务端相同
  auth.token = "123456"
  
  [[proxies]]
  name = "test-tcp"
  type = "tcp"
  localIP = "192.168.2.168"
  localPort = 3389
  remotePort = 3389
  
  [[proxies]]
  name = "api"
  type = "tcp"
  localIP = "192.168.2.4"
  localPort = 8080
  remotePort = 8080
  ```
在客户端运行 frpc。
# 结语
通过以上步骤，已经成功在 Ubuntu VPS 上部署了 frp 用于内网穿透,
,然后就可以通过外网的3389端口来访问内网的3389端口. 8080访问内网的8080端口.
