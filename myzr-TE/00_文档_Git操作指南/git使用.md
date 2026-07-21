# 仓库相关







## 推送文件

使用SSH拉取远程仓库

```
#查看当前远程地址类型
git remote -v
#切换成SSH远程地址
git remote set-url origin git@github.com:lizhengjiang433/git-demo.git
```

配置SSH密钥

```
#回到用户根目录 C/User/MYZRcd 
cd
cd .ssh
#ssh generate -t指定协议为rsa -b 指定生成大小为4096
ssh-keygen -t rsa -b 4096
```

本地（myzr）生成的文件test（私钥文件），test.pub（公钥文件）

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

> #查看配置文件的最后五行 
> tail -5 config

测试联通

```
ssh -T git@github.com
```

## 查看仓库大小

```
git count-objects -vH
#输出
count: 132
#松散对象总大小，也就是当前未打包的版本历史占用空间
size: 6.06 MiB
in-pack: 0
packs: 0
#commit提交次数较少
size-pack: 0 bytes
prune-packable: 0
garbage: 0
size-garbage: 0 bytes

```

# 命令笔记

## 提交相关

1. 查看提交历史

   ```
   git log --oneline
   ```

2. 查看文件状态

   ```
   git status
   ```

 3. 提交到暂存和本地仓库

    ```
    git commit -am q""
    ```


 4. 推送文件至远程仓库

    ```
    git push origin main
    ```

    > :warning:若在网页端更新过文件，那么在推送文件之前需要先拉取远程文件
    >
    > 推之前先拉取远程仓库，避免冲突

 5. 拉取远程仓库文件

    ```
    git pull origin main
    ```
    
    首次推送绑定分支
    
    ```
    git push -u origin main
    ```
    
    后续直接git push

## 分支合并

1. 本地分支绑定远程分支

   ```
   git push --set-upstream origin <分支名>
   ```

2. 将分支合并进main

   ```
   #切回主线
   git checkout main
   #拉取最新main
   git pull origin main
   #执行合并
   git merge <分支名>
   ```

3. 查看当前分支

   ```
   #带*的是当前分支
   git branch 
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

终止合并

```
git merge --abort
```

给命令起别名

```
alias graph="git log --oneline --graph"
```





# 笔记

## 初始化设置

```
#配置用户名
git config --global user.name "Your Name"
#配置邮箱
git config --global user.email "mail@example.com"
#存储配置
git config --global credential.helper store
```

保存用户名和密码（避免重复输入）

```
git config credential.helper store
```

## 创建仓库

1. 创建本地仓库


```
git init lzjabc
```

本人电脑本地仓库地址：C:/Users/86186/lzjabc/.git/

公司（myzr）本地仓库位置：D:\abc-git

​	2. 远程仓库

```
git@github.com:lizhengjiang433/git-demo.git
```

远程仓库地址;https://github.com/lizhengjiang433/git-demo

## 四个区域

1. 工作区(working directory)

   Windows盘符目录

2. 暂存区(stage/index)

   暂存区也叫索引，用来临时存放未提交的内容，一般在.git目录下的index中

3. 本地仓库(repository)

   git在本地的版本库，仓库信息存储在.git这个隐藏目录中

4. 远程仓库(remote repository)

   托管在远程服务器上的仓库。常用的有github、gitlab、gitee

## 文件状态

Modified：修改了但是没有保存到暂存区的文件

staged：修改后已经保存到暂存区的文件

commited：把暂存区的文件提交到本地仓库后的状态

main/master：主分支

## 特殊文件

1. **.git**：Git仓库的元数据和对象数据库

2. **.gitignore**：忽略文件，不需要提交到仓库的文件

   ```
   #校验文件是否被忽略
   git check-ignore -v 1.txt
   ```

   > 将不需要的文件名字放在该文件中

3. 

4. 

5. 

6. 

## 添加和提交

添加一个文件到暂存区

```
git add <文件名>
```

添加所有暂存区的文件到本地仓库

```
git commit -m "message"
```

提交所有已修改的文件到本地仓库

```
git commit -am "message"
```

> 若有新建文件不会提交进去



## 分支

1. 查看所有本地分支，当前分支前面会有一个星号*

   ```
   git branch
   ```

   > -r查看远程分支，-a查看所有分支。

2. 创建一个新的分支

   ```
   git branch <分支名>
   ```

3. 创建分支并切换到该分支下

   ```
   git checkout -b <分支名>
   ```

4. 删除一个已经合并的分支

   ```
   git branch -d <分支名>
   ```

   > 前提条件是分支所有的提交已经合并到main上

5. 删除一个分支，不管是否合并

   ```
   git branch -D <f>
   ```

   

6. 

7. git merge --no-ff -m message <branch-name>

   合并分支，--no-ff参数表示禁用Fast Forward模式，合并后的历史有分支，能看出曾经做过合并，而-ff参数表示使用FastForward模式，合并后的历史会变成一条直线。

   - 将分支合并进main

     ```
     #切回主线
     git checkout main
     #拉取最新main
     git pull origin main
     #执行合并
     git merge <分支名>
     ```

8. 

9. gi



1. 将本地和远程仓库进行关联

   ```
   git remote add <远程仓库别名> <远程仓库地址>
   ```

2. git remote -v

   查看远程仓库

3. git remote rm <remote-name>

   删除远程仓库。

   ```
   #删除本地记录里的远程仓库别名与地址的绑定关系
   ```

4. 获取所有的远程分支

   ```
   git fetch 
   ```

   

分支与版本标记说明

1. main/master：默认主分支

   
   
2. origin

   默认远程仓库

3. HEAD

   指向当前分支的指针

4. HEAD^

   上一个版本

5. HEAD~

   往上数第4个版本

1. 

9. 

stash

1. 

   ```
   #在切换分支之前执行命令
   git stash
   #切回分支后 恢复现场
   git stash pop
   ```

## 撤销和恢复

```
#撤销暂存区文件，重新放回工作区，add的反向操作
git restore
```



## 查看状态或差异

1. 查看未暂存的文件更新了哪些部分

   ```
   git diff
   ```

   查看两个提交之间的差异

   ```
   git diff <commit-id> <commit-id>
   ```

2. 查看仓库状态，列出还未提交的或修改的文件

   ```
   git status
   ```

3. 查看提交历史，--oneline表示简洁模式

   ```
   git log --oneline
   ```


## stash

```
#把当前工作现场储藏起来，等以后恢复现场后继续工作
git stash
```

```
#恢复最近一次stash
git stash pop
#恢复第3个stash
git stash pop stash@{2}
```

## 远程仓库

```
#添加远程仓库
git remote add <name> <url>
```

```
#查看远程仓库
git remote -v
#删除
git remote rm <name>
#重命名
git remote rename <old-name> <new-name>
```

```
#为了有一个干净的提交历史

```



## 英文释义

|            英文             |      中文      |
| :-------------------------: | :------------: |
| URL=Uniform Resource Locato | 统一资源定位符 |
|                             |                |
|                             |                |
|                             |                |
|                             |                |
|                             |                |
|                             |                |
|                             |                |
|                             |                |

