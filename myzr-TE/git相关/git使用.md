# 仓库相关



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



## 仓库操作

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

## 特殊文件

1. .gitignore

   将不需要的文件名字放在该文件中

   校验文件是否被忽略

   ```
   git check-ignore -v 1.txt
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



![image-20260621141747156](../../../Desktop/myzr-TE/git/git使用.assets/image-20260621141747156.png)生成SSH

# 笔记

## 远程仓库

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
   git fetch o
   ```

   

## 四个区域

1. 工作区(working directory)

   Windows盘符目录

2. 暂存区(stage/index)

   暂存区也叫索引，用来临时存放未提交的内容，一般在.git目录下的index中

3. 本地仓库(repository)

   git在本地的版本库，仓库信息存储在.git这个隐藏目录中

4. 远程仓库(remote repository)

   托管在远程服务器上的仓库。常用的有github、gitlab、gitee

## 文件修改存储状态

1. 已修改(modfied)

   修改了但是没有保存到暂存区的文件

2. 已暂存(staged)

   修改后已经保存到暂存区的文件

3. 已提交(committed)

   把暂存区的文件提交到本地仓库后的状态

## 分支与版本标记说明

1. main/master：默认主分支

   ```
   
   ```

   

2. origin

   默认远程仓库

3. HEAD

   指向当前分支的指针

4. HEAD^

   上一个版本

5. HEAD~

   往上数第4个版本

## 特殊文件

1. **.git**：Git仓库的元数据和对象数据库

2. **.gitignore**：忽略文件，不需要提交到仓库的文件

   

3. **.gitattributes**：指向当前分支的指针

4. **.gitkeep**：使空目录被提交到仓库

5. **.gitmodules**： 记录子模块的信息

6.  **.gitconfig**： 记录仓库的配置信息

## 添加和提交

1. git add <file>

   添加一个文件到暂存区

## 分支

1. git branch

   查看所有本地分支，当前分支前面会有一个星号*，-r查看远程分支，-a查看所有分支。

2. git branch <branch-name>

   创建一个新的分支。

3. git checkout -b <branch-name>

   切换到指定分支，并更新工作区(自动创建了文件)。

   ```
   #创建分支并切换到该分支下
   git checkout -b <分支名>
   ```

   

4. git branch -d <branch-name>

   删除一个已经合并的分支。

5. git checkout -D <branch-name>

   删除一个分支，不管是否合并。

6. git tag <tag-name>

   给当前的提交打上标签，通常用于版本发布。

7. git merge --no-ff -m message <branch-name>

   合并分支，--no-ff参数表示禁用Fast Forward模式，合并后的历史有分支，能看出曾经做过合并，而-ff参数表示使用FastForward模式，合并后的历史会变成一条直线。

8. git squash <branch-name>

   合并&挤压（squash）所有提交到一个提交。

9. git checkout <dev>

   git rebase <main>

   rebase 操作可以把本地未push的分叉提交历
   史整理成直线，看起来更加直观。但是，如果
   多人协作时，不要对已经推送到远程的分支执
   行rebase操作。
   rebase不会产生新的提交，而是把当前分支的
   每一个提交都“复制”到目标分支上，然后再把
   当前分支指向目标分支，而merge会产生一个
   新的提交，这个提交有两个分支的所有修改。

## stash

1. git stash save "message"

   把工作区 + 暂存区所有未提交的修改打包存到储藏栈；

   同时把本地文件恢复到当前分支最后一次提交的干净版本

   -u 参数表示把所有未跟踪的文件也一并存储；
   -a 参数表示把所有未跟踪的文件和忽略的文件也一并存储；

   ```
   #在切换分支之前执行命令
   git stash
   #切回分支后 恢复现场
   git stash pop
   ```

   
