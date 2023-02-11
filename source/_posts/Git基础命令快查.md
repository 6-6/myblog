---
title: Git基础命令快查
date: 2020-08-13 14:26:29
tags: Git
cover: https://raw.githubusercontent.com/6-6/blog-assets/main/img/Git%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4%E5%BF%AB%E6%9F%A5.png
categories: [开发工具]
feature: false
urlname: git-basic-commands
---

# Git基础命令快查
以下代码文件路径以```src/index.js```来作为演示

## 快查

### Git Log
```
# git log查找指定日期提交记录
git log --after="2021-04-12" --before="2021-04-13"
```

### Git COMMIT
暂存区的代码提交给本地版本库（-m代表message，不输入无法输入备注）

```
git commit -m "message"
```

可以将所有已跟踪文件中修改或删除的提交到本地仓库，不建议直接这样使用（加上-a代表git add）

```
git commit -a -m "message"
```

### 分支
查看远程分支
```
git branch -a
```

查看本地分支
```
git branch
```

切换分支
```
git checkout -b dev origin/dev
```

远程仓库里拉取一条本地不存在的分支
```
git checkout -b 本地分支名 origin/远程分支名
```

fork出来的分支与原项目合并
```
## 从上游仓库获取到分支，及相关的提交信息，它们将被保存在本地的 upstream/master分支
git remote add upstream git@github.com:original_owner/original_repository.git
git fetch upstream
## 在你本地的 master 分支上，将合并后的信息提交
git merge upstream/master
git commit -m "message"
## push 到你远程的仓库
git push origin master
```

Fork项目后和原项目进行同步更新
```
## 查看原有远程分支信息
git remote -v
## 添加源项目的远程分支并命名为upgrade，名称随意
git remote add upgrade https://github.com/origin/projectname.git
## 再次查看本地的远程分支信息，这时已经可以看到远程分支已经添加进去了
git remote -v
## 把upgrade的代码拉取到本地
git fetch upgrade
## 查看并选中dev（默认是选中master），或者其他你想合并的分支，只有一个master分支可以忽略
git branch
## *号就是选中的
> * master
> dev
git checkout -b dev
## 合并upgrade到我们自己的master分支
git merge upgrade/master
## 如果没有提示冲突，直接推送到github仓库，有冲突请继续往下看
git push origin master
```

### 撤销

撤销未add添加到暂存区
```
## 将会撤销当前文件夹下所有文件，也可以指定到某个文件
 git checkout -- .
```

撤销已经add的代码
```
## 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了
git reset HEAD 
```

修改已经commit的注释
```
git commit --amend
```

撤销已经commit的代码
```
## 不删除工作空间的改动代码 ，撤销commit
git reset --soft "HEAD^" 
## 删除工作空间的改动代码，撤销commit且撤销add
git reset --hard "HEAD^" 
```

git revert撤销已经push的代码
```
## 查看提交日志，并找到某次提交的commit-id，退出按下Q
git log
## 然后撤销执行commit-id的提交
git revert <commit-id>
```

git reset撤销已经push的代码
```
## reset操作会将之前的记录删除，比较危险，请注意
git log //查看提交日志，并找到某次提交的commit-id，退出按下Q
git reset --hard <commit-id> //然后撤销执行commit-id的提交
git push origin //推送至远程分支
```

撤销add到缓存区的更改
```
## 先看一下add 中的文件
git status
## 后面什么都不跟的话 就是上一次add 里面的全部撤销了 
git reset HEAD 
## 对指定文件进行撤销了
git reset HEAD src/index.js
```
## 教程

#### git 本地创建分支，推送到远程

1. git branch （查看本地分支）
2. git branch  -a  （查看远程端的查分支命令）
3. git checkout -b branch1 （创建分支： 本地多了branch1 ，远程端分支还不变）
4. cat test.txt     （运行文件）
5. git status    （ 查看状态，当前是没有提交任何东西）
6. vim test.txt    （编辑txt 文件）
7. git add test.txt  
8. git commit -m "提交到分支1上的内容"   
9. git push --set-upstream origin branch1

#### 添加.gitignore

1. 手动创建.gitignore
2. 将不需上传的文件写入，例如：node_modules/
3. 提交记录```git commit -m "add .gitignore"```
4. 推送远端```git push```

#### 如何 clone git 项目到一个非空目录
如果我们往一个非空的目录下 clone git 项目，就会提示错误信息：

fatal: destination path '.' already exists and is not an empty directory.

解决的办法是：

1. 进入非空目录，假设是 /workdir/proj1
2. git clone --no-checkout https://github.com/origin/projectname.git tmp
3. mv tmp/.git   #将 tmp 目录下的 .git 目录移到当前目录
4. rmdir tmp
5. git reset --hard HEAD

然后就可以进行各种正常操作了。

#### 嵌套git仓库（git submodule子模块）

**添加子模块**
运行成功后子模块就添加到项目了，且会自动新增.gitmodules文件，内部记录了子模块的信息

```javacript
git submodule add <repoUrl> <directory name>
```

如果子模块添加后，文件夹内没有文件，可以执行
```
git submodule update --init --recursive
```

**更新子模块**
当子模块有更新时，在主仓库需要更新一下远程子模块代码
```
git submodule update --remote
```

**删除子模块**
git submodule没有提供删除子模块的命令，需要自己手动删一下，并且要删除的地方有点多，不要漏了

第一步：删除子模块文件夹
```rm -rf 子模块目录名```

第二步：删除.gitmodules里的子模块的信息

第三步：删除.git/modules/子模块名
```cd ./.git/modules/```
```rm -rf 子模块目录名```

第四步：删除.git/config里的子模块配置信息
```vi .git/config```
进入编辑模式，删除子模块相关信息（这里建议不熟悉VI命令的直接用IDE编辑器打开）

以上，就删除成功啦

tips:
如果以上都删除了，还是不能添加的话，就删除一下缓存吧
```git rm --cached 子模块目录名```

参考：https://blog.csdn.net/haoyanyu_/article/details/119857693


## 问题

#### git bash使用时，无法正确显示中文命名文件

**解决1：**
git bash 终端输入命令：```git config --global core.quotepath false```

如果还是出现乱码，bash终端也要设置成中文和utf-8编码。

**解决2：**
右击bash终端的空白处，然后：Options->Text->Locale改为zh_CN，Character set改为UTF-8

**解决3：**
还有一种情况出现乱码而且上面的方法解决不了的，就是你windows安装git bash的时候框选了字体，然后你系统中没有那个字体导致。这种情况下只能先卸载git bash，重装不要选字体。

当然区分这两种情况很简单，没有字体那种实际上敲ls命令查看的时候还是能看到中文的，但在git status的时候就会出现乱码。

路径下C:\Program Files\Git\etc，
编辑profile文件，在文件末尾添加如下内容：
```shell
export LESSHARESET=utf-8
```


参考：https://blog.csdn.net/u012145252/article/details/81775362

#### 解决 The file will have its original line endings in your working directory
首先出现这个问题主要原因是：我们从别人github地址上通过git clone下载下来，而又想git push到我们自己的github上，那么就会出现上面提示的错误信息

```
git rm -r --cached .
git config core.autocrlf false
git add .
```

#### warning: LF will be replaced by CRLF
简单解决的办法就是crtl+enter换行就不会报错了，相关解决方案：[git如何避免”warning: LF will be replaced by CRLF“提示？
](https://www.zhihu.com/question/50862500)

#### Git stash save "message"无法使用
实际上是因为我在webstorm客户端操作，并使用任务管理器删除

#### git reset --hard commit_id如何恢复？
1. ```git reflog```可以看到每次对目录树的操作日志，HEAD@{1}为最近提交操作的序号，前面的hash值就是commit_id。
2. ```git reset --hard commit_id```回退到HEAD@{1}这个版本
参考：[git reset后如何恢复](https://www.jianshu.com/p/0fb25392f84d)
> git reset操作一定要谨慎

#### 解决git push代码到github上一直提示输入用户名及密码的问题
**问题：**
将github上的工程clone到本地后，修改完代码后想要push到github，但一直会有提示输入用户名及密码。

**分析：**
git clone的时候使用的是http的方式，自然git push的时候也是这样。

**解决：**
1. 先查看当前方式：```git remote -v```
2. 把http方式改为ssh方式。先移除旧的http的origin：```git remote rm origin```
3. 再添加新的ssh方式的origin：```git remote add origin git@github.com:6-6/myblog.git```
4. 检查一下有没改变成功：```git remote -v```
5. 改动完之后直接执行git push是无法推送代码的，需要设置一下上游要跟踪的分支，与此同时会自动执行一次git push命令。```git push --set-upstream origin dev```（注意这里的分支改为自己指定的分支）

#### Untracked files无法直接用checkout清除
```
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        JavaScript/EcmaScript6/chapter1/

nothing added to commit but untracked files present (use "git add" to track)
```

```
git clean -f -d
```

#### 冲突后无法git pull
```
error: Pulling is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.
```

**部分参考：**
* https://blog.csdn.net/namechenfl/article/details/81257973
* https://www.cnblogs.com/arieslee/p/8288223.html
* https://www.oschina.net/question/54100_167919
* https://blog.csdn.net/qq_43657442/article/details/118710269
* https://blog.csdn.net/yychuyu/article/details/80186783