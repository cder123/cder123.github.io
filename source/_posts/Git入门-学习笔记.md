---
title: Git-学习笔记
tag: Git
categories:
  - 编程工具
  - 版本控制
  - Git
---

# Git-学习笔记

[toc]



## 1. Git 安装及全局配置

> **以Windows为例：**
> 1. 从 **Git官网** 直接下载安装程序，然后按 **默认** 选项安装
> 2. 若 在点击左下角的windows图标后 能够找到 **“Git”-“Git Bash”**，并且打开后出现类似 cmd 的窗口，则**安装成功。**
> 3. 打开 Git Bash【git bash中可以输入  Linux命令，相当于 linux中的终端】
> 4. 输入`git config --global user.name "用户名"`
> 5. 输入 `git config --global user.email "邮箱地址"	`
## 2. Git 的工作原理





### 2.1 git 与 svn 的差别

>   -   git 是 **分布式**的版本控制系统，在**本地和远程** 各有1个版本库,工作时可以**不用联网**
>   -   svn 是**集中式**的版本控制系统，是多个人**共用1个**版本库，工作时需要**联网**





### 2.2 几个重要的概念

+ **工作区**【workspace】：文件夹
+ **暂存区**【stage】：git add 时，从 workspace  添加到  stage
+ **本地仓库**【Repository】：git commit 时，将stage中的文件 提交到本地版本库
+ **远程仓库**【Remote】：git push 时，将 Repository 中的文件推送到远程仓库，如：github，gitee等网站。
+ **注意事项：** git**只能追踪文本文件的内容修改**，不能追踪二进制文件【如：图片】。对于二进制文件，git只能知道它有修改，但不知道修改的内容。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112516134783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70#pic_center)
## 3. 新建仓库


### 3.1 创建版本库

【创建之后的大部分命令都在版本库文件夹下输入】
1.  **版本库** 就是一个能够**被 git 管理** (增、删、改、恢复) 的**目录**
2. **创建步骤**：
> 1. 创建的文件夹，用于充当版本库【1个名为 testRepo 的版本库】,

```bash
cd e:
mkdir testRepo
```


> 2. 初始化版本库
> 【初始化后生成 .git 隐藏文件，该文件不能动，否则会破坏版本库】

```bash
cd testRepo
git init
```



### 3.2 添加文件到暂存区

 【以在 testRepo 文件夹中 创建1个名为 readme.txt 的文件为例】
```bash
//创建1个名为 readme.txt 的文件，并输入内容，保存
git add ./readme.txt
```



### 3.3  提交文件到 本地仓库

```bash
git commit -m "描述信息，一般描述修改的内容"
```



### 3.4 查看 版本库状态

git状态:指 是否提交，是否推送 等状态

```bash
git status
```



### 3.5 查看 更改的内容

【查看 当前工作区的文件（此时还没有 git add）与原来相比有不同】
查看所做的更改
```bash
git diff readme.txt
```

确认无误后，git add,和git commit 


## 4. 版本回退


### 4.1 查看历史版本记录

即：查看历史 commit 记录。越上面，记录越新
```bash
//详细显示
git log

//简要显示
git log --pretty=oneline
```



### 4.2 版本回退

```bash
//回退到上一个版本
git reset --hard HEAD^

//回退到上两个版本
git reset --hard HEAD^^

//回退上100个版本
git reset --hard HEAD~100
```



### 4.3 回退后又想恢复到回退前的版本

git reset --hard 版本号
如：`git reset --hard 6dsd2152`
```bash
//查看版本号
git reflog

//根据版本号以回退版本
git reset --hard 版本号
```



### 4.4 撤销修改

+ **撤销修改：** 撤销 提交到**本地版本库**的修改
	+ **知道哪里错了**：直接手动在文件里改，然后git add、commit等操作
	+ **不知道哪里错了**：【2种方法】
		+ 根据 本地版本库 回退：`git reset --hard HEAD^`
		+ 直接使用 撤销命令：【2种情况】
			+ **还没有 git add**，撤销后的形成的版本就是本地版本库中的版本
			+ **已经 git add 后**又做了修改，撤销后形成的版本是暂存区中的版本
```bash
//直接使用撤销命令
// 注意：命令中的 -- 很重要，有--是撤销，没有--是切换分支

git checkout -- readme.txt
```



### 4.5 删除文件

直接删除，然后 commit

```bash
//删除
rm ./abc.txt
git commit -m "删除了当前目录下的abc.txt文件"


//撤销删除
git checkout -- abc.txt
```


## 5. 远程仓库



### 5.1 生成SSH

使用远程仓库需要先在 github、gitee 等 代码托管网站先注册账户
> **以 gitee 为例**
> 1. 进入用户的主目录 `C:\Users\admin`
> 2. 查看 是否有 .ssh文件夹 及 .ssh文件夹内 是否 有 `id-rsa` 和 `id_rsa.pub` 2个文件。如果没有，则 进行第三步	
> 3. 打开 git-bash,输入`ssh-keygen -t rsa –C “邮箱”`，生成上述的 **2个文件**
> 4. 用记事本打开 `id_rsa.pub`	文件，全选后复制
> 5. 浏览器打开 gitee 并登录
> 6. 进入 gitee, 设置 =》	安全设置 =》SSH公钥
> 7. 输入标题【标题随便写】
> 8. 公钥就是刚才从`id_rsa.pub`中复制的内容
> 9. 点击 确定





### 5.2 新建远程库

> 1. 点击gitee 右上角的<kbd>+</kbd>  =》 新建仓库
> 2. 输入仓库名称【需要和本地版本库的名称一致(文件夹名)】
> 3. 其他项可以默认
> 4. 点击 创建
> 5. 根据提示输入命令





### 5.3 与 远程库 建立关联

```bash
// 以下仅为示例


// origin为本地仓库与远程仓库的链接的名字，代表了1个仓库，
// 因此，账户中有多个不同项目的仓库时，把origin换成不同的名字。
// 添加remote时的仓库名格式(ssh): 
//      git@托管网站的域名:用户名/仓库名.git

// 在 本地建立 与 远程仓库 的链接。
git remote add origin git@gitee.com:admin/testRepo.git

// 第一次推送需要 -u,之后可以不用
git push -u origin master

// 第2次及以后的推送
git push origin master




// 如果推送时发生冲突，可能是因为远程和本地的内容有部分不一致.

// 解决办法1：先pill，再合并，再解决冲突，再推送
git pull origin master

// 解决办法2【pull也失败时】：先建立分支与远程库之间的关联
git branch --set-upstream dev origin/dev
git pull
//解决冲突后,再add、commit、push

```





### 5.4 查看远程库的信息

远程库的信息都是成对出现的，包括：

 - fetch： 抓取
 - push： 推送

```bash
//查看远程库的信息
git remote

//查看远程库的详细信息
git remote -v
```







### 5.5 将 远程库 克隆 到本地

```bash
cd 进入要存放代码库的文件夹
git clone git@gitee.com:admin/testRepo.git
```




### 5.6 删除 关联 本地与远程库 的链接


```bash
git remote rm origin
```







## 6. 分支

Git 将 每次的提交串成1条时间线，master就是这条 主时间线的名字，HEAD 指向 master，master指向提交。创建并合并分支类似于将master复制 1份，在分支上修改后，再用分支的内容覆盖掉原来的master。
**master分支**：一般存放稳定的版本，开发和修复bug一般创建新的分支，在分支上操作。





### 6.1 创建、切换分支

```bash
//创建并切换到分支
git checkout -b 分支名

//上面1句命令 等价于
git branch 分支名
git checkout 分支名
```





### 6.2 查看分支

```bash
//查看分支，其中，分支名前带*，表示当前为该分支
git branch
```





### 6.3 合并到主分支

默认情况下，git 使用 Fast-forward（快进模式）来合并分支
+ Fast-forward（快进模式）：直接将master指向分支的提交，删除分支后，会丢掉分支信息
+ 关闭快进模式： `git merge –no-ff -m “注释” 分支名`

```bash
// 1. 先切换到 master分支
git checkout master

// 2. 再合并[【其中，dev为我们自建创建的分支的名字】
git merge dev
```





### 6.4 删除分支

```bash
// 删除分支
git branch -d 分支名
```





### 6.5 解决分支之间的冲突

**情景：**   创建1个名为dev的分支，在dev分支上修改并提交，然后切换回master分支，如果在master上再进行与dev分支不同的修改并提交,则合并 master与dev 时,会出现冲突。  

**解决步骤：** 
 - cat 查看一下产生冲突的文件的内容【git 会用<<<，===，>>>标识产生冲突的部分】
 - 将2个分支的内容改成一致即可
 - 合并、添加、提交
 - `git log ` 查看合并情况

```bash

// 创建分支
git checkout -b dev
git add readme.txt
git commit -m "add merge"

// 关闭快进模式，并合并
git checkout master
git merge --no-ff -m "关闭快进模式" dev

// 删除分支
git branch -d dev

// 查看分支
git branch

// 查看删除分支后 是否仍然保留分支号
git log --graph --pretty=oneline --abbrev-commit
```





### 6.6 隐藏现场

需要临时做其他分支的工作但又因为手中的事情还没做完不能提交时，可以使用隐藏现场功能，隐藏现场后，查看状态时，可以发现状态是干净的。

```bash
//隐藏现场
git stash

//查看状态
git status

//查看隐藏的现场
git stash list
```





### 6.7 还原现场

1.git stash apply恢复，恢复后，stash内容并不删除，你需要使用命令git stash drop来删除。
2.另一种方式是使用git stash pop,恢复的同时把stash内容也删除了。
```bash
//方法1
// 还原现场，然后删除最新的一条现场记录
git stash apply
git stash drop

//方法2
// 还原现场并删除记录
git stash pop

```

