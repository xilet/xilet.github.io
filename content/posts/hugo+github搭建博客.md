---
title: hugo+github搭建博客
date: 2023-11-30T18:08:11+08:00
tags:
  - hugo
  - FixIt
  - blog
categories:
  - Tutorial
code:
  maxShownLines: 11
# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

{{< admonition note "约定" >}}
本教程中 POWERSHELL 中的命令在WINDOWS下的POWERSHELL或COMMAND等终端中执行。
{{< /admonition >}}


### 下载 Hugo
下载 Hugo extend 版本 >= 0.109.0  [Hugo点击直达](https://github.com/gohugoio/hugo/releases)  
我这里下载了最新的 hugo_extended_0.120.4_windows-amd64.zip  
解压到 D:\hugo\bin 路径下 (以自己实际路径为准)  
然后在系统环境变量Path中加入 D:\hugo\bin

Win键+R键掉出运行窗口,输入以下命令来打开系统环境变量设置窗口
   ```bash
   rundll32 sysdm.cpl,EditEnvironmentVariables
   ```
在用户变量框选中Path,编辑->新建->输入值 D:\hugo\bin


### 安装 Git for windows
安装[Git for windows](https://git-scm.com/download/)

###  使用基于 Git 子模块的快速入门模板 通过 Use this Template 快速初始化
> 点击链接[hugo-fixit-blog-git](https://github.com/hugo-fixit/hugo-fixit-blog-git)转到FixIt的模版页面
1. 点击 **Use this template**, 并在 Github 上创建代码仓库

    <img width="913" alt="image" src="https://github.com/hugo-fixit/hugo-fixit-blog-git/assets/33419593/d5fbd940-3ffd-4750-b1e6-4e87b50b0696">

2. 当仓库创建完毕,将仓库代码拉取到本地。

   ```powershell
   #假设站点文件保存在 D:\sites 下
   D:
   cd D:\sites
   # 使用你自己的仓库地址替换
   git clone --recursive https://github.com/<your_name>/<your_name>.github.io.git
   ```
3. 初始化代码仓库
   ```powershell
   cd <your_name>.github.io
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

<details>
  <summary>提交更改</summary>

  ```powershell
  git add .
  git commit -m "update"
  git branch -M main
  git push -u origin main
  ```
</details>

### GitHub Pages 设置
浏览器访问 https://github.com/<your_name>/<your_name>.github.io.git  
- Settings->Pages
> Branch -> gh-pages  
- Settings->Actions->General
> Workflow permissions -> Read and write permissions
Save
## 参考文档
 [FixIt](https://github.com/hugo-fixit/FixIt) 主题仓库.

{{< link href="https://fixit.lruihao.cn/documentation/" content="FixIt主题说明文档" title="FixIt主题说明文档" card=true >}}

- [基于 Git 子模块的快速入门模板][hugo-fixit-blog-git](https://github.com/hugo-fixit/hugo-fixit-blog-git)
- [基于 Hugo 模块的快速入门模板][hugo-fixit-blog-go](https://github.com/hugo-fixit/hugo-fixit-blog-go)
For a complete quick start, see this [page](https://fixit.lruihao.cn/documentation/getting-started/).

{{< admonition tip "其它技巧" >}}
暂无
{{< /admonition >}}
