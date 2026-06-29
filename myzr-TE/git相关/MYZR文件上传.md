1. 进入文档目录 

   ```
   cd /mnt/d/my-work/myzr-docs 
   ```

2. 激活虚拟环境 

   ```
   source venv/bin/activate
   ```

3. 执行编译命令

   ```
   sphinx-autobuild . _build/html
   ```

   修改conf.py配置文件

4. 清除构建缓存

   ```
   sphinx-build -M clean . _build
   ```



[I 260629 15:15:10 server:331] Serving on http://127.0.0.1:8000
[I 260629 15:15:10 handlers:62] Start watching changes
[I 260629 15:15:10 handlers:64] Start detecting changes

## 1. 第一行：网页预览地址

```
Serving on http://127.0.0.1:8000
```

含义：本地启动了网页服务器，你复制这个链接到浏览器打开，就能实时看编译好的手册页面。

## 2. 后两行：文件实时监听功能

- `Start watching changes`：开始监控所有 `.rst`、图片、配置文件
- `Start detecting changes`：持续检测文件修改







只要你在 VS Code 修改 rst 文档并保存，程序会**自动重新编译文档**，浏览器刷新就能立刻看到修改后的效果，不用手动反复执行编译命令，这就是 `sphinx-autobuild` 的核心功能。

