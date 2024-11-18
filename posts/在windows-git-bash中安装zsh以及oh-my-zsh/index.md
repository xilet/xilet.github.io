# 在Windows Git Bash中安装Zsh以及oh My Zsh


&lt;!--more--&gt;
https://dominikrys.com/posts/zsh-in-git-bash-on-windows/

最近开始重新使用Windows。  
之前在macOS和Linux上使用定制的Zsh shell很长时间，相当顺手。
那么在Windows中我们可以在Git Bash中设置Zsh来代替Bash。
在这篇文章中来看看如何解决

**在Git Bash中安装Zsh**

1. 从MSYS2包存储库下载最新的[MSYS2 zsh](https://packages.msys2.org/package/zsh?repo=msys&amp;variant=x86_64)包，例如`zsh-5.8-5-x86_64.pkg.tar.zst`。

2. 安装能打开ZST归档的提取器，如[PeaZip](https://peazip.github.io/)或[7-Zip Beta](https://www.7-zip.org/)。

3. 将归档内容（包括etc和usr文件夹）解压到Git Bash安装目录下，通常是`C:\Program Files\Git`。

4. 打开Git Bash并运行：
   ```
   # 通过代理
   export http_proxy=http://127.0.0.1:10809
   export https_proxy=http://127.0.0.1:10809
   zsh
   ```
   重要：在Zsh首次使用向导中配置标签补全和历史记录。如果没有出现或你跳过了它，重新运行：
   ```
   autoload -U zsh-newuser-install
   zsh-newuser-install -f
   ```
   配置历史记录，按1，更改值，然后按0。配置补全，按2选择“使用新的补全系统”，然后按0。按0保存设置。

5. 通过在`~/.bashrc`文件中添加以下内容，将Zsh配置为默认shell：
   ```
   if [ -t 1 ]; then
     exec zsh
   fi
   ```
6. 设置 SSH 连接超时时间
   ```angular2html
   vim ~/.ssh/config
   ```
   ```vim
   Host *
   #每 60 秒发送一次保活信号到服务器
   ServerAliveInterval 60
   #在没有收到任何服务器响应的情况下，最多发送86400次保活信号。
   ServerAliveCountMax 86400
   ```

**安装oh-my-zsh**

此时，Git Bash将基本上像Unix Zsh shell一样运行。安装oh-my-zsh，运行：
```
sh -c &#34;$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)&#34;
```

**安装插件和主题**

使用oh-my-zsh的安装方法安装插件和主题。我安装了Powerlevel10k主题和以下插件，并确认它们有效：
- zsh-completions
- zsh-syntax-highlighting
- zsh-autosuggestions

注意：你可能在终端中遇到奇怪的图形和间距问题，特别是在版本v0.7.0。使用版本v0.6.4解决此问题：
```
cd ~/.oh-my-zsh/plugins/zsh-autosuggestions
git checkout tags/v0.6.4 -b v0.6.4-branch
```

**修复输出错乱**

Windows可能会破坏UTF-8编码文本，导致终端中显示异常字符（更多信息见Stack Overflow的回答）。在`~/.bashrc`文件中添加以下内容修复此问题，最好在设置shell为Zsh的代码之前：
```
/c/Windows/System32/chcp.com 65001 &gt; /dev/null 2&gt;&amp;1
```

**故障排除**

fworks的指南相当活跃，值得搜索或询问任何

### 参考文档
[Installing Zsh (and oh-my-zsh) in Windows Git Bash](https://dominikrys.com/posts/zsh-in-git-bash-on-windows/)


---

> 作者:   
> URL: https://jiyanjiyu.com/posts/%E5%9C%A8windows-git-bash%E4%B8%AD%E5%AE%89%E8%A3%85zsh%E4%BB%A5%E5%8F%8Aoh-my-zsh/  

