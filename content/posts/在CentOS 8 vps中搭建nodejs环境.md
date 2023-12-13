---
title: 在CentOS 8 vps中搭建nodejs环境
date: 2023-12-13T22:41:11+08:00
tags:
  - ubuntu
  - git
  - node.js
categories:
  - Tutorial
code:
  maxShownLines: 11
# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

# 准备工作
- 一台VPS  如果没有的话可以购买 [InterServer](https://www.interserver.net/r/938272)
- 任意 SSH 终端软件 [XShell](https://www.xshell.com/zh/free-for-home-school/) 、 [免费国产FinalShell](http://www.hostbuf.com/t/988.html)、[git-windows](https://git-scm.com/)
- 最好再有一个域名

# 配置以及安装
- 首先通过SSH链接到远程主机
    ```bash
    #首先导入 NodeSource 仓库
    curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
    #更新系统
    sudo dnf update
    #安装需要的工具
    sudo dnf install git
    sudo dnf install vim
    sudo dnf install nodejs
    npm install -g pnpm
    npm install -g npm@10.2.5
    ```
- 开放防火墙端口
    ```bash
    sudo firewall-cmd --zone=public --add-port=8067/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
    sudo firewall-cmd --reload
    sudo firewall-cmd --list-ports
    ```


<!--more-->
