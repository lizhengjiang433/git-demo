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
https://github.com/lizhengjiang433/git-demo.git
```

提交到暂存和本地仓库

```
git commit -a -m ""
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

![image-20260621130512299](git%E4%BD%BF%E7%94%A8.assets/image-20260621130512299.png)

终止合并

```
git merge --abort
```

给命令起别名

```
alias graph="git log --oneline --graph"
```



![image-20260621141747156](git%E4%BD%BF%E7%94%A8.assets/image-20260621141747156.png)生成SSH秘钥 

```
cd .ssh
ssh-keygen -t rsa -b 4096

```

