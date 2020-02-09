# git教程笔记

## git简介

### 登陆

git config --global user.name "Your Name"

git config --global user.email "email@example.com"

### 创建版本库

#### 第一步

```
mkdir name         //创建一个名为name的文件夹
cd name
pwd                //显示它的位置
/name
```

#### 第二步

```
git init           //把这个目录变成Git可以管理的仓库
Initialized empty Git repository in  E:/git/Git/learngit/.git/
```

.git目录是Git跟踪管理版本库的，不能乱动

在name或其子目录里创建一个readme.txt文件，接下来把文件添加到仓库

### 添加文件到版本库

#### 第一步

```
git add readme.txt
```

没有显示消息就是添加成功

#### 第二步

用命令git commit告诉git，把文件提交到仓库

```
$ git commit -m "wrote a readme file"
[master (root-commit) db942d4] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

-m后面输入的是本次提交的说明

命令成功后会告诉你1 file changed：1个文件被改动    2 insertions：插入了两行内容（txt文本有两行内容）

## 时光机穿梭

修改txt文件

运行**git status**

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

可以知道，readme.txt被修改过了，但还没有准备提交，**git diff**可以看看具体修改了什么内容

```
$ git diff
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.   //修改的部分
 Git is free software.
```

提交修改和提交新文件

```
git add readme.txt
```

```
$ git commit -m "add distributed"
[master 7007db9] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

再输入**git status**可以发现仓库已清空

```
$ git status
On branch master
nothing to commit, working tree clean
```

### 版本回退

再进行一次文本的修改，然后上传，我们已拥有以下的版本

#### 版本1：wrote a readme file

```
Git is a version control system.
Git is free software.
```

#### 版本2：add distributed

```
Git is a distributed version control system.
Git is free software.
```

#### 版本3：append GPL

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

#### 历史记录

我们可以用**git log**命令查看

```
$ git log
commit 022bd204303a12f1d75435d047d6c574ff290a2e (HEAD -> master)
Author: lamarsan <2524960219@qq.com>
Date:   Sat Sep 15 14:04:05 2018 +0800

    append GPL

commit 7007db971204a560e319f8f3e2a8a70fb043e058
Author: lamarsan <2524960219@qq.com>
Date:   Sat Sep 15 14:00:16 2018 +0800

    add distributed

commit db942d473f3aaec03a58cf156be3cf3c49daf22f
Author: lamarsan <2524960219@qq.com>
Date:   Sat Sep 15 13:46:59 2018 +0800

    wrote a readme file
```

若嫌输出信息太多，可加上`--pretty=oneline`参数

```
$ git log --pretty=oneline
022bd204303a12f1d75435d047d6c574ff290a2e (HEAD -> master) append GPL
7007db971204a560e319f8f3e2a8a70fb043e058 add distributed
db942d473f3aaec03a58cf156be3cf3c49daf22f wrote a readme file
```

前面的022bd...为commit id（版本号）

#### 具体操作

在Git中，用`HEAD`表示当前版本，上一个版本为`HEAD^`,上上一个版本就是`HEAD^^`，若往上一百个版本，就是`HEAD~100`

要**回退**，可以使用`git reset`命令

```
$ git reset --hard HEAD^
HEAD is now at 7007db9 add distributed
```

**查看内容**

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

用`git log`发现最新版本已消失，若想**恢复**，且命令行没关，可以找到id

```
$ git reset --hard 022bd
HEAD is now at 022bd20 append GPL
```

版本号写前几位就行

若关掉电脑，忘了id可以使用`git reflog`

```
$ git reflog
7007db9 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
022bd20 HEAD@{1}: reset: moving to 022bd
7007db9 (HEAD -> master) HEAD@{2}: reset: moving to HEAD^
022bd20 HEAD@{3}: commit: append GPL
7007db9 (HEAD -> master) HEAD@{4}: commit: add distributed
db942d4 HEAD@{5}: commit (initial): wrote a readme file
```

可以找到id

### 工作区和暂存区 

**工作区**：电脑里能看到的目录

**版本库**：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库

版本库里存了很多东西，最重要的是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

添加文件的时候，分为**两步**进行

**第一步**是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区

**第二步**是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支

因为我们创建Git版本库时，Git自动创建了唯一一个`master`分支，现在`git commit`就是往`master`分支上提交更改

新建一个license文本文件，内容随便写，使用`git status`，查看一下状态

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   lalala.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        license.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Git告诉我们lalala被修改了，license还没被添加过，所以状态是Untracked

使用两次`git add`把`lalala.txt`和`license.txt`都添加后，用`git status`再查看一次

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   lalala.txt
        new file:   license.txt


```

所以git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把所有修改提交到分支

```
$ git commit -m "understand how stage works"
[master a6c0431] understand how stage works
 2 files changed, 4 insertions(+), 1 deletion(-)
 create mode 100644 license.txt
```

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是"干净"的

```
$ git status
On branch master
nothing to commit, working tree clean
```

### 管理修改

Git跟踪并管理的是修改，而并非文件

### 撤销修改

在lalala.txt中添加一行

My stupid boss.

使用`git status`

会发现

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   lalala.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Git会告诉你`git checkout -- file`可以丢弃工作区的修改

`git checkout -- lalala.txt`

命令`git checkout -- lalala.txt`意思是，把`lalala.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`lalala.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`lalala.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

看看`lalala.txt`的文件内容：

```
$ cat lalala.txt
Git is a distributed version control system.
Git is free software distributed under the GPL
Git has a muable index called stage
Git tracks changes.
```

果然已被复原

**若已把骚话用`git add`添加到暂存区了**：

```
$ cat lalala.txt
Git is a distributed version control system.
Git is free software distributed under the GPL
Git has a muable index called stage
Git tracks changes.
My stupid boss.
```

使用`git status`命令

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   lalala.txt

```

Git告诉我们，用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉，重新放回工作区；

```
$ git reset HEAD lalala.txt
Unstaged changes after reset:
M       lalala.txt

```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

再用`git status`查看一下，现在暂存区是干净的，工作区有修改

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   lalala.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

然后再使用`git checkout -- lalala.txt`

```
$ git status
On branch master
nothing to commit, working tree clean
```

世界清净了！！！

### 删除文件

使用`rm`命令，删除license文件

`$ rm license.txt`

工作区和版本库不一致，使用`git status`命令查看哪些文件被删除

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    license.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

现在有两个选择，一是确实要从版本库中删除该文件，那就用`git rm`删掉，并且`git commit`

```
$ git rm license.txt
rm 'license.txt'
$ git commit -m "remove license.txt"
[master 80def6d] remove license.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 license.txt
```

现在，文件就从版本库中被删除了

另一种情况是删错了，可以轻松地把误删的文件恢复到最新版本：

`git checkout --test.txt`

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”

 #### 遇到不能git

首先 git-bash here

`git pull origin master –allow-unrelated-histories `
`git push -u origin master -f`

# gitlab

##### Git global setup

```
git config --global user.name "傅泽鹏"
git config --global user.email "2524960219@qq.com"
```

##### Create a new repository

```
git clone https://git.civaonline.cn/root/teachcloud.git
cd teachcloud
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

##### Existing folder

```
cd existing_folder
git init
git remote add origin https://git.civaonline.cn/root/teachcloud.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

##### Existing Git repository

```
cd existing_repo
git remote rename origin old-origin
git remote add origin https://git.civaonline.cn/root/teachcloud.git
git push -u origin --all
git push -u origin --tags
```

##### 清除用户信息

`git config --system --unset credential.helper`