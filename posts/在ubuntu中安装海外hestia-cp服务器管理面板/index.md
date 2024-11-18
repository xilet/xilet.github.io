# 在Ubuntu中安装海外Hestia CP服务器管理面板


# 准备工作
- 一台VPS  如果没有的话可以购买 [InterServer](https://www.interserver.net/r/938272)
- 任意 SSH 终端软件 [XShell](https://www.xshell.com/zh/free-for-home-school/) 、 [免费国产FinalShell](http://www.hostbuf.com/t/988.html)、[git-windows](https://git-scm.com/)
- 最好再有一个域名

# 配置以及安装
- 首先通过SSH链接到远程主机
    ```bash
    #更新系统
    apt-get update
    #如弹出选择框按需要选择然后回车
    apt-get upgrade
    ```
- 关闭所有iptables规则,非必须.在我的oracle免费主机上需要执行该脚本
    ```bash
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
    ```

- 访问[Hestia CP 安装参数生成页面](https://hestiacp.com/install.html)

    ```bash
    wget https://raw.githubusercontent.com/hestiacp/hestiacp/release/install/hst-install.sh
    
    #如果国内服务器无法下载的话,也可以用我提供的镜像来下载
    wget https://mirror.jiyanjiyu.com/hestiacp/hestiacp/release/install/hst-install.sh
    #这里的命令用官方提供的安装参数生成页面来生成
    bash hst-install.sh --port 2083 --apache no --named no --mysql no --mysql8 yes --exim no --dovecot no --clamav no --spamassassin no --iptables no --fail2ban no --api no --interactive no
    ```
    安装完成后有以下提示
    ```bash
    Please install an MTA on this system if you want to use sendmail!

    Congratulations!
    
    You have successfully installed Hestia Control Panel on your server.
    
    Ready to get started? Log in using the following credentials:
    
            Admin URL:  https://&lt;your host name&gt;:&lt;your port&gt;
            Backup URL: https://&lt;your host ip&gt;:&lt;your port&gt;
            Username:   admin
            Password:   The password you chose during installation.
    
    Thank you for choosing Hestia Control Panel to power your full stack web server,
    we hope that you enjoy using it as much as we do!
    
    Please feel free to contact us at any time if you have any questions,
    or if you encounter any bugs or problems:
    
    Documentation:  https://docs.hestiacp.com/
    Forum:          https://forum.hestiacp.com/
    GitHub:         https://www.github.com/hestiacp/hestiacp
    
    Note: Automatic updates are enabled by default. If you would like to disable them,
    please log in and navigate to Server &gt; Updates to turn them off.
    
    Help support the Hestia Control Panel project by donating via PayPal:
    https://www.hestiacp.com/donate
    
    --
    Sincerely yours,
    The Hestia Control Panel development team
    
    Made with love &amp; pride by the open-source community around the world.
    [ ! ] IMPORTANT: You must restart the system before continuing!
    ```

- 重启服务器
  ```bash
  reboot
   ```
然后你可以访问上面的面板地址来管理服务器了.
如果无法访问.你可能还需要在VPS后台打开 80 443 8023(HCP默认管理端口)或者自定的端口.

{{&lt; admonition tip &#34;Cloudflare&#34; &gt;}}
如果你的域名是用Cloudflare来解析的,那么你需要将Hestia CP的管理端口进行变更,因为CF并不允许使用8083端口。
  ```bash
  sudo su -
  v-change-sys-port 2083
  ```
使用 Cloudflare 源服务器SSL 可以获得长达 15 年有效期的证书，而且在相当长的一段时间内不需要担心更新证书

你可以按照以下方式将 Cloudflare 证书授权添加到你的服务器
```bash
sudo su -
wget https://developers.cloudflare.com/ssl/static/origin_ca_rsa_root.pem
mv origin_ca_rsa_root.pem origin_ca_rsa_root.crt
cp origin_ca_rsa_root.crt /usr/local/share/ca-certificates
update-ca-certificates
```

{{&lt; /admonition &gt;}}


&lt;!--more--&gt;


---

> 作者:   
> URL: https://jiyanjiyu.com/posts/%E5%9C%A8ubuntu%E4%B8%AD%E5%AE%89%E8%A3%85%E6%B5%B7%E5%A4%96hestia-cp%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AE%A1%E7%90%86%E9%9D%A2%E6%9D%BF/  

