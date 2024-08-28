## Git 教程

#### 一、Git 安装

**1、在linux上安装git**

```shell
git #查看git信息
sudo apt-get install git ##安装git
```

**2、设置账号信息**

安装完成后，需要设置自己的账号信息，便于同步管理仓库，如果只是想下载开源代码，则不需要

个人账户为：

* username：`Wangzehao-sun`
* useremail：`zh-wang@mail.ustc.edu.cn`，`971226161@qq.com`

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

**3、git网络代理设置**

对于需要添加git网络代理的可以参考下列命令

```shell
# 添加代理
git config --global http.proxy http://username:password@proxyserver:port
git config --global https.proxy https://username:password@proxyserver:port
#http://127.0.0.1:7890
# 取消代理 --unset
# 查看代理 --get
git config --global --unset http.proxy 
git config --global --unset https.proxy
```

* `username` 和 `password` 是你的代理服务器的用户名和密码（如果有）。

* `proxyserver` 是代理服务器的地址。

* `port` 是代理服务器的端口。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

#### 二、Git创建版本库（Repository）

**1、本地创建**

首先，进入需要创建Reposity的文件夹：

当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

```shell
git init #把这个目录变成Git可以管理的仓库
```

**2、远程仓库**

在已有本地仓库的前提下，可以在github新建一个空repository与之关联：

```shell
# 本地仓库保存
git commit -m "first commit"
# 切换分支
git branch -M main
# 将远程仓库关联到本地
git remote add origin https://github.com/Wangzehao-sun/Knowledge-Notes.git 
# git remote -v 查看
# git remote rm <name> 删除
# 上传分支到远程的指定分支
git push -u origin main # -u (或 --set-upstream): 选项用于设置本地分支与远程分支的关联关系。这样以后使用 git push 和 git pull 时，不需要指定远程分支和分支名。简化了后续的操作。
```

当在本地修改需要同步到远程仓库时，需要github权限令牌：

```shell
# 设置Git以使用凭据存储，这样就不需要每次都输入用户名和令牌
git config --global credential.helper store

# 输入用户名和personal access tokens
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
```

#### 三、仓库管理

在讲解具体命令之前，我们先需要区分几个概念

<img src="C:\Users\dell\Desktop\Knowledge-Notes\工具教程\git\attachments\image-20240828164526793.png" alt="image-20240828164526793" style="zoom: 80%;" />

* 工作区：即主要的文件夹
* 版本库：`.git`文件夹
* 暂存区：Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区
* 分支：Git为我们自动创建的第一个分支`master`
* Head ：指向当前分支的指针

我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

**1、状态查看**

在对仓库中的某个文件修改后，可以使用`git status`查看当前的仓库状态，告诉我们哪些文件进行了修改，并是否`add`提交

```shell
# 对readme进行修改，但还没有git add
git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

> [!CAUTION]
>
> 直白来说，git status就是查看哪些文件还没有添加到暂存区，以及哪些修改还没有commit

更进一步，如果想要查看被修改的内容，可以使用`git diff`

```shell
git diff readme.txt 
```

如果想要查看commit保存的版本历史记录，可以使用`git log`

```shell
git log # 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL
```



**2、文件管理**

将修改保存到暂存区git add

将暂存区的所有内容提交到分支git commit

**3、撤销管理**

* `git checkout -- <file>`

​	把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

​	一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

​	一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

​	总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

* `git reset`

  既可以回退版本，也可以把暂存区的修改撤销掉（unstage），重新放回工作区：

  ```shell
  # 版本回退
  git reset --hard <版本号> #--hard会回退到上个版本的已提交状态，而--soft会回退到上个版本的未提交状态，--mixed会回退到上个版本已添加但未提交的状态。
  
  # 撤销暂存区修改
  git reset Head <file>
  ```

  > [!IMPORTANT]
  >
  > 值得注意的是，Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从当前版本指向了指定版本：

  

* `git relog`:高级用法 **==TODO==**

**4、文件删除**

将某些文件删除后，git status会告诉你，哪一些文件被删除了，如果想将这种修改添加到暂存区，使用git add是不够的，可以使用：

```shell
git rm <file>
git commit
```

**5、`.gitignore`用法**

`.gitignore` 文件用于告诉 Git 哪些文件或目录不应该被提交到版本控制系统中。通过在 `.gitignore` 文件中指定某些文件或文件夹的模式，Git 会忽略这些文件的变化，从而防止它们被添加到暂存区或提交到仓库中。

常用模式：

```*.gitignore
# 忽略所有以 .log 结尾的文件
*.log

# 忽略特定文件
secret.txt

# 忽略名为 "temp" 的目录
temp/

# 忽略文件夹 logs 中的所有文件,但不忽略文件夹
logs/*

# 忽略所有 .txt 文件
*.txt

# 但不忽略 not_this_file.txt
!not_this_file.txt

# 忽略所有内容
build/*

# 但不忽略 build/output 文件夹
!build/output/
```

* `*`：匹配零个或多个字符。

* `?`：匹配单个字符。

* `**`：匹配多层目录。例如，`a/**/z` 可以匹配 `a/z`、`a/b/z` 或 `a/b/c/z`。

* `/`：表示目录。

#### 四、分支管理

**==TODO==**



[^1]:https://liaoxuefeng.com/books/git/introduction/index.html

