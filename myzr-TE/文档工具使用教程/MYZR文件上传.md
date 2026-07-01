http://127.0.0.1:8000

myzr网页版资料git仓库上传位置：D:\my-work\git\myzr-docs

## 一、环境准备（Linux系统）

1.激活虚拟环境（每次开关机都要执行）

```
cd /mnt/d/my-work/myzr-docs
source venv/bin/activate
# 退出虚拟环境
deactivate
```

> 两种环境 Sphinx 版本一致，功能无差异
>
> 全局 Python 和虚拟环境都装了相同版本 Sphinx，编译行为、输出效果完全一样，肉眼看不出区别。

2.安装网页打开工具（按需安装，仅 Linux 环境需要）

```
sudo apt install xdg-utils
```

md导出为rst（转换器为pandc）



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

在这个目录空白处按住 `Shift` + 右键 → 选择「在此处打开命令窗口」运行：

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

修改 `.rst` 文档内容后：

1. 清理旧构建产物再编译，避免缓存干扰

   ```
   rm -rf _build
   #或者
   sphinx-build -M clean . _build
   ```

2. 重新执行编译指令

   ```
   sphinx-build -b html . _build/html
   ```

   > `-b` = `--builder`，指定 Sphinx 构建器（输出格式）

2. 刷新浏览器中的网页，即可查看更新后的效果。







vscode安装扩展

reStructuredText

python环境

![image-20260626150947276](MYZR文件上传.assets/image-20260626150947276.png)