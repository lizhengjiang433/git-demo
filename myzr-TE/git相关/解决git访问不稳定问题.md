1. 修改本地hosts文件

   windows系统的hosts文件的位置如下：

   ```
   C:\Windows\System32\drivers\etc\hosts
   ```

   > 如果提示没有权限修改hosts文件，可以右键属性， 然后安全，选择Users后编辑权限，选择写入应用确认即可

2. 增加映射

```python
获取Github相关网站的ip
访问https://www.ipaddress.com，拉下来，找到页面中下方的“IP Address Tools – Quick Links”
分别输入github.global.ssl.fastly.net和github.com，查询ip地址
下面是我的配置
140.82.121.3	github.com
146.75.121.194	github.global.ssl.fastly.net
```

