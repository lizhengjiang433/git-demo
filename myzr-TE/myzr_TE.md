## 一、相关地址

- 在线文档：http://docs.myzr.com.cn/
- 版本仓库：http://myzr-git.lan:3000/prj/myzr-docs
- 镜像仓库：https://github.com/myzrkj/myzr-docs
- 构建地址：https://app.readthedocs.org/projects/myzr-docs/

```Bash
MYZR-GIT(源码) <--同步--> GitHub <--关联--> ReadTheDocs --自动编译--> MZYR-DOCS
```

## 二、主机环境配置

- 主机上使用 WSL 做为编译环境

### 2.1 安装 WSL

1. 开启相关的 Windows 功能
2. 依次点击 `控制面板 -> 程序和功能 -> 启用或关闭 Windows 功能`，勾选下面两项：
   1. Hyper-V
   2. 适用于 Linux 的 Windows 子系统
3. 基于 WSL 安装  Ubuntu
4. 提示：在终端界面可以使用鼠标右键粘贴。
   1. 点击下载 Ubuntu 系统文件 [Ubuntu_2004.2021.825.0_x64.appx](http://www.myzr.lan:8080/PRG-程序区/OS.系统安装/Ubuntu_2004.2021.825.0_x64.appx)（如果是无线网络，需要连接 MYZR-WIFI。有线网络直接下载）。
   2. 把下载的文件放到 D 盘，把文件扩展名改为 .zip 并解压。
   3. 在 ubuntu.exe 上点击右键以 `管理员身份` 运行。
      - ```Plain
        Enter new UNIX username: <在这里输入 myzr 作为用户名>
        New password: <在这里输入 Myzr2012 作为密码>
        Retype new password: <在这里输入 Myzr2012 确认密码>
        ```
5. 更新 WSL 的 Ubuntu
   1. ```Bash
      # 更新版本信息
      sudo apt update
      
      # 更新软件包
      sudo apt upgrade
      
      # 安装编译和运行的依赖
      sudo apt install python3-pip
      ```

### 2.2 安装并配置 Python 虚拟环境

- 提示：在 WSL 的 ubuntu 中进行
  - ```Bash
    # 1. 安装编译虚拟环境包
    sudo apt install python3.8-venv
    
    # 2. 创建新的虚拟环境
    python3 -m venv myzr-docs
    
    # 3. 激活虚拟环境
    source venv/bin/activate
    
    # 4. 升级 pip
    pip install --upgrade pip
    
    # 5. 安装最新版 Sphinx 和主题
    pip install sphinx sphinx-rtd-theme sphinx-autobuild
    
    # 6. 验证安装
    sphinx-build --version
    # 执行上面的命令后显示 sphinx-build 7.1.2 就是对的
    
    # 7. 检查依赖包
    pip list | grep sphinx
    # 执行上面的命令后显示下面包的信息：
    # sphinx-autobuild              2021.3.14
    # sphinx_rtd_theme              3.1.0
    # sphinxcontrib-applehelp       1.0.4
    # sphinxcontrib-devhelp         1.0.2
    # sphinxcontrib-htmlhelp        2.0.1
    # sphinxcontrib-jquery          4.1
    # sphinxcontrib-jsmath          1.0.1
    # sphinxcontrib-qthelp          1.0.3
    # sphinxcontrib-serializinghtml 1.1.5
    ```

### 2.3 安装 Windows 版本的 Git 工具

1. 点击下载 [Git.Open.Setup.v2.49.0.x64.exe](http://file.myzr.lan:8080/PRG-程序区/Git.Open.Setup.v2.49.0.x64.exe) 并安装。
2. 点击下载 [TortoiseGit.Open.Setup.v2.17.0.2.x64.msi](http://file.myzr.lan:8080/PRG-程序区/TortoiseGit.Open.Setup.v2.17.0.2.x64.msi) 并安装。
3. 点击下载 [TortoiseGit.LanguagePack.Open.Setup.v2.17.0.0.v64.CN.msi](http://file.myzr.lan:8080/PRG-程序区/TortoiseGit.LanguagePack.Open.Setup.v2.17.0.0.v64.CN.msi) 并安装。
4. 配置 `TortoiseGit`：
   1. ```Bash
      # 配置软件语言为中文
      TortoiseGit 设置 -> 常规设置 -> 语言：中文
      
      # 配置 SSH 客户端（配置后可支持使用 ssh 密钥连接）
      TortoiseGit 设置 -> 网络 -> SSH -> SSH 客户端：C:\Program User\Git\usr\bin\ssh.exe
      
      # 配置 Git 路径
      TortoiseGit 设置 -> 常规设置 -> Git for Windows -> Git.exe 路径：C:\Program User\Git\bin
      ```

### 2.4 安装 VSCode

- 点击下载 [vscode](http://file.myzr.lan:8080/PRG-程序区/VSCodeUserSetup-x64-1.121.0.exe) 并安装。
- 点击下载 [UbuntuMono-R](http://www.myzr.lan:8080/PRG-程序区/UbuntuMono-R.ttf) 字体并安装。
- 配置 VSCode
  - ```Bash
    'Ubuntu Mono', '微软雅黑', monospace
    ```

### 2.5 安装 Typora

- 点击下载 [Typora](http://file.myzr.lan:8080/PRG-程序区/Typora.v1117.x64.Free.Setup.exe) 并安装。
- 配置 Typora：
  - ```Bash
    # 运行 `注册表编辑器`
    计算机\HKEY_CURRENT_USER\SOFTWARE\Typora
    
    在 Typora 上点击右键，选择权限，把所有组或用户名的权限设置为 `拒绝`
    ```

## 三、编译文档

### 3.1 下载代码

- 提示：在 Windows 上进行

1. 在 D 盘新建 `my-work` 目录。
2. 使用 `TortoiseGit` 克隆 `http://myzr-git.lan:3000/prj/myzr-docs.git`

### 3.2 编译文档

- 提示：在 wsl 的 ubuntu 上进行
  - ```Bash
    # 1. 进入文档目录
    cd /mnt/d/my-work/myzr-docs
    
    # 2. 激活虚拟环境
    source venv/bin/activate
    
    # 3. 执行编译命令
    sphinx-autobuild . _build/html
    sphinx-autobuild . _build/html --host=192.168.130.73 --port=8000
    ```

### 3.3 编译开发板文档

- 复制 `myzr-docs` 目录下的 `conf.py` 到要正在编写的文档目录。
- 进入正在编写的文档目录并执行编译命令（在虚拟环境中进行）'
  - ```Bash
    sphinx-autobuild . _build/html
    ```

## 四、文档编写

### 4.1 编写技巧

- 表格制作：使用配置好的 vscode。
- 文档格式调整：在编写目录下编译并观看效果后，对有问题的格式进行调整。
- 参考模板：复制已经存在并且格式对应的文档，把内容替换上去。

### 4.2 reStructuredText 参考

**简介**

reStructuredText（简称 reST）是一种轻量级的标记语言，旨在为生成文档提供清晰的结构和格式。reST 的设计目的是易于人类阅读和编写，同时也便于机器解析和转换为其他格式的文档，如 HTML、LaTeX、PDF 等。

**特点**

1. 易读性强：reST 的语法简单直观，容易理解和记忆。
2. 灵活：支持多种文档结构和格式，如标题、段落、列表、表格、引用等。
3. 可扩展性强：可以通过自定义指令和直译（directives and roles）来扩展 reST 的功能。
4. 文档结构清晰：reST 强调文档的结构化，使得文档易于维护和组织。

**教程**

[初级入门](https://www.sphinx-doc.org/zh-cn/master/usage/restructuredtext/index.html)

### 4.3 Markdown参考

**简介**

Markdown 是一种轻量级的标记语言，它允许人们使用易读易写的纯文本格式编写文档，然后转换成结构化的 HTML（超文本标记语言）文档。Markdown 的设计初衷是为了让非技术人员也能轻松编写网页内容，同时保持文档的可读性。

**特点**

1. 简洁易懂：Markdown 的语法简单直观，容易学习和记忆。
2. 易于阅读：即使不经过转换，Markdown 文档也是易于人类阅读的。
3. 易于编写：Markdown 语法简单，可以用任何文本编辑器编写。
4. 广泛支持：许多平台和工具都支持 Markdown，如 GitHub、GitLab、Stack Overflow、Reddit 等。

**教程**

1. [入门](https://markdown.com.cn/intro.html)
2. [基本语法](https://markdown.com.cn/basic-syntax/)
3. [扩展语法](https://markdown.com.cn/extended-syntax/)