http://127.0.0.1:8000

## 一、环境准备（Linux系统）

1.激活虚拟环境（每次开关机都要执行）

```
source venv/bin/activate
```

2.安装网页打开工具（按需安装，仅 Linux 环境需要）

```
sudo apt install xdg-utils
```

## 二、Sphinx 文档编译步骤

1.文件准备

把所有 `.rst` 文件、`index.rst` 放到你的 Sphinx 项目根目录（和 `conf.py` 同文件夹）。

2.关键配置修改

编辑项目根目录的 `conf.py` 文件，修改主文档入口配置

```python
# 将默认的 master_doc 改为实际的主文档名（示例为 myzr_TE）
master_doc = 'myzr_TE'
```

3.执行编译

在项目文件夹里打开 CMD/PowerShell，运行：

```
sphinx-build -b html . _build/html
```

看到 `Build finished` 就是编译完成。

## 三、打开网页查看

- Linux快捷方式

```
xdg-open _build/html/index.html
```

- 通用方法

打开查看，进入文件夹 _build\html，双击 index.html，浏览器直接打开查看最终网页效果。

## 四、文档更新流程

修改 `.rst` 文档内容后，无需重复环境准备步骤，仅需：

1. 重新执行「二、3 执行编译命令」；
2. 刷新浏览器中的网页，即可查看更新后的效果。