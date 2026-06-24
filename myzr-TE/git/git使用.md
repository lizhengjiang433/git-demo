## 初始化设置



```
#保存用户名和密码
git config credential.helper store
```



## 创建仓库

本地

```
C:\Users\86186>git init lzjabc
Initialized empty Git repository in C:/Users/86186/lzjabc/.git/  
```

远程仓库

```
git@github.com:lizhengjiang433/git-demo.git
https://github.com/lizhengjiang433/git-demo
```

提交到暂存和本地仓库

```
git commit -a -m ""
```

## 推送文件

使用SSH拉取远程仓库



配置SSH密钥

```
#回到用户根目录 C/User/MYZRcd 
cd
cd .ssh
#ssh generate -t指定协议为rsa -b 指定生成大小为4096
ssh-keygen -t rsa -b 4096
```

本地（myzr）生成的文件test（私钥文件），test.pub（公钥文件）

![image-20260624094406761](../../../Desktop/myzr-TE/git/git使用.assets/image-20260624094406761.png)

匹配公钥（公钥太多了，得指定一个公钥）

```
notepad config
#输入内容（指定使用公钥）
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/test
#改配置文件后缀
mv config.txt config
```

核对本地公钥是否和网页版一致

```
#本地密钥查看
ssh-keygen -lf test.pub
```



```
#查看配置文件的最后五行 
tail -5 config
```



测试联通

```
ssh -T git@github.com
```

推送文件至远程仓库

```
git push origin main
```



## git diff 

默认比较工作区和暂存区的内容

git diff HEAD

比较工作区和版本库之间的差异

git diff --cached

比较暂存区和版本库之间的差异

git diff < id> < id>

比较版本库和版本库之间的差异

本地仓库→远程仓库

```
git push -u origin main :main
```

-u: upstream（逆流而上）update

origin：远程仓库别名

后面的main:本地仓库main分支

远程仓库→本地仓库

```
git pull origin main
git pull
#或远程仓库的修改内容
git fetch  
```

分支

```
git switch <分支名>
```

合并

```
git merge dev
#将dev合并到当前分支
```

解决合并冲突

```

```

打开vscode

```
code .
```

![image-20260621130512299](../../../Desktop/myzr-TE/git/git使用.assets/image-20260621130512299.png)

终止合并

```
git merge --abort
```

给命令起别名

```
alias graph="git log --oneline --graph"
```



![image-20260621141747156](../../../Desktop/myzr-TE/git/git使用.assets/image-20260621141747156.png)生成SSH秘钥 

```

```

