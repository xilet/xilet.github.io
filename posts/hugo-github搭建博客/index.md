# Hugo&#43;github搭建博客


{{&lt; admonition note &#34;约定&#34; &gt;}}
本教程中 POWERSHELL 中的命令在WINDOWS下的POWERSHELL或COMMAND等终端中执行。
{{&lt; /admonition &gt;}}


### 下载 Hugo
下载 Hugo extend 版本 &gt;= 0.109.0  [Hugo点击直达](https://github.com/gohugoio/hugo/releases)  
我这里下载了最新的 hugo_extended_0.120.4_windows-amd64.zip  
解压到 D:\hugo\bin 路径下 (以自己实际路径为准)  
然后在系统环境变量Path中加入 D:\hugo\bin

Win键&#43;R键掉出运行窗口,输入以下命令来打开系统环境变量设置窗口
   ```bash
   rundll32 sysdm.cpl,EditEnvironmentVariables
   ```
在用户变量框选中Path,编辑-&gt;新建-&gt;输入值 D:\hugo\bin


### 安装 Git for windows
安装[Git for windows](https://git-scm.com/download/)

###  使用基于 Git 子模块的快速入门模板 通过 Use this Template 快速初始化
&gt; 点击链接[hugo-fixit-blog-git](https://github.com/hugo-fixit/hugo-fixit-blog-git)转到FixIt的模版页面
1. 点击 **Use this template**, 并在 Github 上创建代码仓库

    &lt;img width=&#34;913&#34; alt=&#34;image&#34; src=&#34;https://github.com/hugo-fixit/hugo-fixit-blog-git/assets/33419593/d5fbd940-3ffd-4750-b1e6-4e87b50b0696&#34;&gt;

2. 当仓库创建完毕,将仓库代码拉取到本地。

   ```powershell
   #假设站点文件保存在 D:\sites 下
   D:
   cd D:\sites
   # 使用你自己的仓库地址替换
   git clone --recursive https://github.com/&lt;your_name&gt;/&lt;your_name&gt;.github.io.git
   ```
3. 初始化代码仓库
   ```powershell
   cd &lt;your_name&gt;.github.io
   git init
   ```
### 启动本地站点

   ```powershell
   hugo server
   ```

### 添加文章
   

### 构建站点静态文件

运行以下命令构建站点:

```powershell
hugo
```

### 添加文章

```powershell
# 手动更新主题
hugo new posts/first-blog.md
```

&lt;details&gt;
  &lt;summary&gt;提交更改&lt;/summary&gt;

  ```powershell
  git add .
  git commit -m &#34;update&#34;
  git branch -M main
  git push -u origin main
  ```
&lt;/details&gt;

### GitHub Pages 设置
浏览器访问 https://github.com/&lt;your_name&gt;/&lt;your_name&gt;.github.io.git  
- Settings-&gt;Pages
&gt; Branch -&gt; gh-pages  
- Settings-&gt;Actions-&gt;General
&gt; Workflow permissions -&gt; Read and write permissions
Save
## 参考文档
 [FixIt](https://github.com/hugo-fixit/FixIt) 主题仓库.

{{&lt; link href=&#34;https://fixit.lruihao.cn/documentation/&#34; content=&#34;FixIt主题说明文档&#34; title=&#34;FixIt主题说明文档&#34; card=true &gt;}}

- [基于 Git 子模块的快速入门模板][hugo-fixit-blog-git](https://github.com/hugo-fixit/hugo-fixit-blog-git)
- [基于 Hugo 模块的快速入门模板][hugo-fixit-blog-go](https://github.com/hugo-fixit/hugo-fixit-blog-go)
For a complete quick start, see this [page](https://fixit.lruihao.cn/documentation/getting-started/).

{{&lt; admonition tip &#34;其它技巧&#34; &gt;}}
暂无
{{&lt; /admonition &gt;}}


---

> 作者:   
> URL: https://jiyanjiyu.com/posts/hugo-github%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/  

