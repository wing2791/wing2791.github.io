---
title: Git的使用记录
---

#### Git 干嘛的
- 分布式版本控制系统
#### 命令汇总

##### 查看 git 版本
```shell
git -v
```
##### 初始化配置
```shell
# 配置用户名
git config --global user.name "wing2791"
# 上述的用户名可以省略双引号,如果用户名有空格，则无法省略
# 省略(local):本地配置,只对本地仓库有效
# --global: 全局配置,所有仓库生效
# --system: 系统配置,对所有用户生效
# 配置邮箱
git config --global user.email wing2791@163.com
# 保存用户名和密码，这样就不用每次都输入了
git config --global credential.helper store
# 查看配置信息
git config --global --list
```

##### 新建仓库
```shell
mkdir learn-git
cd learn-git
# 初始化空仓库,会以learn-git为仓库
git init
# 会在learn-git文件夹下创建一个my-repo文件夹,以my-repo文件夹为仓库
git init my-repo
```

```shell
# 加仓库地址，克隆仓库到本地
git clone https//....git
```

##### 添加和提交文件

```shell
# 查看状态
git status
# 查看状态的简略模式,??表示为跟踪，M表示修改
git status -s
# 将文件添加到暂存区
git add filename.suffix
# git add 支持使用通配符,只添加.txt结尾的文件
git add *.txt
# 可以提交文件夹，提交当前文件夹
git add .
# 提交文件,-m表示提交时对文件信息的描述,只提交暂存区中的文件
git commit -m "the first commit"
# 查看提交记录
git log
# 查看简洁信息
git log --oneline
```

##### git reset 回退版本

```shell
git reset --soft # 回退到某个版本,并且保留工作区和暂存区中的所有修改内容
git reset --hard # 回退到某个版本,并且丢弃工作区和暂存区的所有修改内容
git reset --mixed # 回退到某个版本,并且只保留工作区的修改内容，而丢弃暂存区中的内容（reset默认参数）
git reset --soft ID # 需要添加回退版本ID,这个需要用git log查看
git ls-files # 查看暂存区内容
git reset HEAD^ # 使用--mixed模式，HEAD^指当前 HEAD 的前一个提交
git reflog # 查看操作历史记录，可以查看操作的ID号，用于回退
```

##### git diff 查看差异

```shell
git diff # 可以查看修改的内容具体是什么，默认比较工作区和暂存区的区别
git diff HEAD # 查看工作区和版本库的区别,HEAD指的是最新提交节点
git diff --cached # 查看暂存库和版本库的区别
git diff ID1 ID2 # 比较两个特定版本库的差异，HEAD~表示上一个版本，HEAD~2表示之前的两个版本
git diff ID1 ID2 file1.txt # 只查看file1.txt的修改内容
```

##### 版本库中删除文件

```shell
rm file1.txt # 删除工作区中文件file1.txt
git add . # 更新暂存区，删除暂存区中的file1.txt
git rm file2.txt # 会在工作区和暂存区中都删除file2.txt
git commit -m "information"
```

##### .gitignore 忽略文件

- .gitignore 中的内容就是指所有忽略的文件名，也可以是文件夹名，文件夹名是相对于 `.gitignore` ,例如`temp/`
- 如果文件夹是空文件夹，不会被版本管理
- [git 官网匹配规则](https://git-scm.com/docs/gitignore)

```shell
*.log # 就是忽略所有的.log文件
!a.log # 表示在*.log忽略的规则下，不忽略a.log文件
/TODO # 表示只忽略当前目录下的TODO文件，不忽略subdir/TODO
build/ # 忽略任何目录下名为build的文件夹
doc/*.txt # 只忽略doc文件夹下的所有.txt文件,但是不忽略doc/subdir/*.txt的文件，即不忽略子文件夹下的文件
# 如果某个文件已经被添加到仓库中,如忽略*.log，但是仓库中有other.log，更新other.log文件的时候，也会一起更新other.log文件
doc/**/*.pdf # 葫芦doc/目录及其所有子目录下的.pdf文件
git rm --cached other.log # 从暂存区中移除文件 other.log
```

##### github 的使用和远程仓库操作

```shell
# git@github.com:wing2791/wing2791.github.io.git
# git开头的仓库使用的是SSH协议,在push的时候不需要验证用户名和密码,但是需要在github上添加ssh公钥的配置，添加密钥的时候如果修改了名称，需要额外配置，这里不赘述
git push <remote> <branch> # 本地仓库的指定分支推送到远程仓库，这里配置好一般直接git push，配置在后面
git pull <remote> # 远程仓库拉取到本地
```

##### 关联本地仓库和远程仓库

```shell
git remote add <shortname> <url> # 关联一个远程仓库
# git remote add roigin git@....git
git remote -v # 查看当前仓库对应的远程仓库的别名和地址
git branch -M main # 指定分支名为main
git push -u origin main:main # u是upstream的缩写,把本地仓库和别名为origin仓库的远程仓库关联起来，把本地仓库的main分支推送给远程仓库的main分支，如果本地分支和远程仓库分支名称一样,可以main:main改为main，第一个main本地分支,第二个main远程分支

git pull <远程仓库名> <远程分支名>:<本地分支名> # 拉取远程仓库修改内容，起其中仓库名和分支名都可以省略，默认为拉取仓库别名为orgin的main分支
git fetch # 只会获取远程仓库的修改，但是并不合并到本地仓库中，需要手动合并
```

##### 分支的使用

```shell
git branch 分支名 # 创建一个新的分支
git checkout 分支1 # 切换到分支1，checkout也可以用来恢复文件，会产生歧义，故换一个切换分支的命令切换到分支1
git switch 分支1 # 切换到分支1
git push -u origin 分支1 # 将新建的分支1上传到远程仓库 
git merge 分支1 # 假如现在处在main分支，运行该命令可以将分支1中的内容合并到main中
git log --graph --oneline --decorate --all # 查看分支图
git branch -d branch-name # 当branch-name被合并后，可以用-d进行删除
git branch -D branch-name # 强制删除分支branch-name
```

##### 解决合并冲突

```shell
git commit -a -m "commit content" # -a参数会直接添加到暂存区，然后提交到本地仓库，完成两个步骤，这个只对已经添加过的文件生效，新文件无法使用，-a -m 可以写成-am
git status # 可以查看冲突文件的列表
git diff # 可以查看冲突的具体内容
# 然后直接查看原文件，可以手动修改冲突内容，重新添加，提交即可
git add .
git commit -m "commit content"
git merge --abort # 中止合并
```

##### 回退和 rebase

```shell
git rebase branch-name
git checkout -b dev ID # 从指定的 ID 创建一个新分支 dev，并立即切换到这个新分支上
alias graph="git log --graph --oneline --decorate --all" # 可以把查看图形化的提交记录命令改为graph
```

[![pA6aUaT.md.png](https://s21.ax1x.com/2024/11/09/pA6aUaT.md.png)](https://imgse.com/i/pA6aUaT)


merge
- 优点
  - 不会破坏原分支的提交历史，方便回溯和查看
- 缺点
  - 会产生额外的提交节点，分支图比较复杂

rebase
- 优点
  - 不会新增额外的提交记录，形成线性历史，比较直观和干净
- 缺点
  - 会改变提交历史，改变了当前分支 branch out 的节点，避免在共享分支使用

##### 分支管理和工作流模型
[![pA6a0G4.md.png](https://s21.ax1x.com/2024/11/09/pA6a0G4.md.png)](https://imgse.com/i/pA6a0G4)

[![pA6aBRJ.md.png](https://s21.ax1x.com/2024/11/09/pA6aBRJ.md.png)](https://imgse.com/i/pA6aBRJ)

[![pA6aDz9.md.png](https://s21.ax1x.com/2024/11/09/pA6aDz9.md.png)](https://imgse.com/i/pA6aDz9)

#### 实际案例
##### SSH设置

```shell
ssh-keygen -t rsa -b 4096 -C "yourname"
# 注：yourname为你git上的用户名或者邮箱，可以随便取，不重要
# 一般全部回车，如果之前生成过，覆盖的话输入y
```
- `ssh-keygen`：启动密钥生成工具。
- `-t rsa`：指定密钥类型为 RSA。
- `-b 4096`：设置密钥长度为 4096 位，增加密钥的安全性。缺省长度为 2048 位。
- `-C "yourname"`：为密钥添加注释，方便识别。注释通常是邮箱或用户名，这里使用 `"yourname"` 作为注释。

- 生成的密钥在`C:/Users/username/.ssh/`(windows)或者`~/.ssh/`(linux)下，有两个重要的文件在这个文件夹下
- id_rsa
	- 私钥
- id_rsa.pub
	- 公钥

- 下面用Github和windows的可视化界面操作，linux纯命令行我暂时没研究
- 打开[Github](https://github.com/)，点击右上角头像

[![点击右上角头像](https://s21.ax1x.com/2024/11/10/pA6LBM4.png)](https://imgse.com/i/pA6LBM4)

- 点击Settings设置
[![点击Settings设置](https://s21.ax1x.com/2024/11/10/pA6L4Qe.png)](https://imgse.com/i/pA6L4Qe)

- 找到SSH and GPG keys，点击
- 点击 new SSH key按钮
 [![SSH and GPG keys => new SSH key](https://s21.ax1x.com/2024/11/10/pA6L7dI.png)](https://imgse.com/i/pA6L7dI)

- 添加电脑公钥
[![ 添加电脑公钥](https://s21.ax1x.com/2024/11/10/pA6LjSS.png)](https://imgse.com/i/pA6LjSS)

##### Github创建仓库
- 找到创建仓库的按钮(New repository)
[![找的创建仓库的按钮(New repository)](https://s21.ax1x.com/2024/11/10/pA6OKT1.png)](https://imgse.com/i/pA6OKT1)
- 创建仓库选项设置
[![创建仓库选项设置](https://s21.ax1x.com/2024/11/10/pA6OQFx.png)](https://imgse.com/i/pA6OQFx)
- 仓库地址链接获取
- HTTPS和SSH都可以，SSH需要配置上面的SSH设置
- 建议使用SSH
[![仓库地址链接获取](https://s21.ax1x.com/2024/11/10/pA6OlY6.png)](https://imgse.com/i/pA6OlY6)

##### Git的使用
> 这里就暂时不展示没有仓库，自己本地建一个，后续自己进阶学习的时候再补充，仓库就用上面Github创建仓库的空白仓库吧

###### 打开VsCode终端
- 以VsCode为例
- 以某个文件夹为workspace（工作区），打开VsCode，这里就不演示了
- 按照下面图片打开一个新终端

[![打开一个新终端](https://s21.ax1x.com/2024/11/10/pA6O20s.png)](https://imgse.com/i/pA6O20s)
- 右下角换一个终端，Command Prompt
- 我这里PowerShell没配置好，有的命令不能用
- 换哪个都行，我哪个能用就换哪个

[![换成Command Prompt终端](https://s21.ax1x.com/2024/11/11/pAcuUBj.png)](https://imgse.com/i/pAcuUBj)

- 输入下面的命令克隆远程仓库到本地
- 因为我们已经进行了SSH设置，可以直接用SSH的链接进行克隆
- 如果代码库很大，只克隆前十次的提交结果
```git
git clone git@github.com:xxx/xxx.git --depth 10
git submodule update --init --recursive # 对于一些久远的分支，需要用这个更新子模块，暂时好像不能用，以后再说
git status # 查看状态，确定是否有尚未track的子模块
# 上面两个命令我也没用过，以后再说
```
[![SSH 方式克隆代码到本地仓库](https://s21.ax1x.com/2024/11/11/pAcuaHs.png)](https://imgse.com/i/pAcuaHs)

- 因为刚下载仓库，会自动生成一个文件夹，文件夹名称就是仓库名
- 进入文件夹
```shell
cd 文件夹名
```
[![cd 进入克隆下来的文件夹](https://s21.ax1x.com/2024/11/11/pAcuwEn.png)](https://imgse.com/i/pAcuwEn)
###### 安装插件`Git Graph`
[![安装插件Git Graph](https://s21.ax1x.com/2024/11/11/pAcu0Nq.png)](https://imgse.com/i/pAcu0Nq)

###### 配置Git全局信息
- 设置 Git 配置中的全局用户名和电子邮件地址，它们将影响你在 Git 仓库中的提交记录
- 如果没有设置 `user.name` 和 `user.email`，Git 会在你第一次提交时提示你配置这两个项。Git 无法在提交记录中存储作者信息，导致提交时无法使用有效的用户名和电子邮件地址。
```git
git config --global user.name "yourname" # 设置 Git 提交的全局用户名为 `"yourname"`
git config --global user.email "your@email.com" # 设置 Git 提交的全局电子邮件地址为 `"your@email.com"`
```
- 错误信息
```shell
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch main
# Your branch is up to date with 'origin/main'.
#
# Changes to be committed:
#	new file:   makefile
#	new file:   test.cpp
#	new file:   test.h
```


###### 测试的提交文件设置
- 我这里创建了三个文件
- 最右边的U表示Update，M表示Modified
[![添加头文件，Cpp源文件，makefile管理文件](https://s21.ax1x.com/2024/11/11/pAcus3T.png)](https://imgse.com/i/pAcus3T)
- test.h
```cpp
#include <iostream>
using namespace std;
```
- test.cpp
```cpp
#include "test.h"

int main()
{
    cout << "hello world" << endl;
    return 0;
}
```
- makefile
```makefile
all: test

test: test.cpp
	g++ -o test test.cpp test.h

clean:
	rm -f test
```
###### 提交修改
- 文字说明会在commit上，在github上能看见
```git
# 这里其实需要先提交到暂存区才行
git add . # 提交当前所有可提交的文件到暂存区
# 提交文件,-m表示提交时对文件信息的描述,只提交暂存区中的文件，如果暂存区中没有文件，那么就会说no changes added to commit (use "git add" and/or "git commit -a")
git commit -m "the first commit" 
# git commit 只会提交该文件，并不会提交其他已暂存的文件，就算没有用add暂存到暂存区，则会个命令也可以执行成功
# git commit -m "提交某个文件的描述" path/to/yourfile.txt
```
[![Commit提交VsCode操作](https://s21.ax1x.com/2024/11/11/pAcu2DJ.png)](https://imgse.com/i/pAcu2DJ)
- 只提交了test.h文件的修改（此时只是在本地进行了commit，还需要提交到远程仓库上）
[![pAcGIjP.png](https://s21.ax1x.com/2024/11/12/pAcGIjP.png)](https://imgse.com/i/pAcGIjP)
[![pAcGynK.png](https://s21.ax1x.com/2024/11/12/pAcGynK.png)](https://imgse.com/i/pAcGynK)
###### 同步到远程仓库上
- 文字说明会在提交的分支上
[![同步到远程仓库上](https://s21.ax1x.com/2024/11/11/pAcuRb9.png)](https://imgse.com/i/pAcuRb9)
[![commit提交文字说明的位置](https://s21.ax1x.com/2024/11/11/pAcufER.png)](https://imgse.com/i/pAcufER)
- 使用命令来进行提交到远程分支上
```git
git push
# 如果需要无法提交，可能是分支尚未在远程上创建，可以先publish分支，再同步
# 或者下面命令，如果是VsCode上面直接Sync Changes，也可以
# git push --set-upstream origin wing2791/20241111_test
```
[![pAcG60O.png](https://s21.ax1x.com/2024/11/12/pAcG60O.png)](https://imgse.com/i/pAcG60O)

###### 创建分支 
```git
git branch 分支名 # 创建一个新的分支，分支叫"分支名"
```
- 创建分支wing2791/20241111_test，并且切换到该分支
```git
git checkout -b wing2791/20241111_test
```
- **`-b` 参数**：`-b` 是 `git checkout` 命令的一个选项，它的作用是新建一个分支并立即切换到该分支上。这里的新分支基于当前所在的分支（通常是 `main` 或 `master`）。
- **`wing2791/20241111_test`**：这个是新分支的名称。分支名中可以使用斜杠 `/` 来创建一种目录结构的效果。
    - `wing2791` 可能是开发者的标识或名字。
    - `20241111_test` 可能表示创建分支的日期（2024年11月11日）和用途（比如“test”代表测试）。
[![pAcGc7D.png](https://s21.ax1x.com/2024/11/12/pAcGc7D.png)](https://imgse.com/i/pAcGc7D)
- 能看到左边有个Publish分支的按钮
- 按过之后就能够在github（远程仓库上）上看见新建的分支了
[![pAcG2Ae.png](https://s21.ax1x.com/2024/11/12/pAcG2Ae.png)](https://imgse.com/i/pAcG2Ae)
[![pAcGWhd.png](https://s21.ax1x.com/2024/11/12/pAcGWhd.png)](https://imgse.com/i/pAcGWhd)
###### 切换分支
```git
git switch 分支1 # 切换到分支1
```

###### 随便修改了源文件，能看见git那里多了这个
- 四个图标意思是
	- 打开修改的文件
	- 撤销本次修改
	- 将修改暂存在暂存区中，类似上面的Staged Changes
		```git
		# 对应命令为,添加test.h文件到暂存区中
		git add test.h
		```
	- M 表示文件被修改，Modify
[![pAcGh9A.png](https://s21.ax1x.com/2024/11/12/pAcGh9A.png)](https://imgse.com/i/pAcGh9A)
- 点击test.cpp那里，可以看到具体的文件修改了哪里
[![pAcG41I.png](https://s21.ax1x.com/2024/11/12/pAcG41I.png)](https://imgse.com/i/pAcG41I)
###### 如果想要某一次提交的基础上创建新分支，怎么办
[![pAgP1TU.png](https://s21.ax1x.com/2024/11/13/pAgP1TU.png)](https://imgse.com/i/pAgP1TU)

[![pAgP8kF.png](https://s21.ax1x.com/2024/11/13/pAgP8kF.png)](https://imgse.com/i/pAgP8kF)

[![pAgPGY4.png](https://s21.ax1x.com/2024/11/13/pAgPGY4.png)](https://imgse.com/i/pAgPGY4)
- 空心圆表示当前在这里
[![pAgPJfJ.png](https://s21.ax1x.com/2024/11/13/pAgPJfJ.png)](https://imgse.com/i/pAgPJfJ)

- 此时创建新分支，就是在某个提交点上创建的新分支
[![pAgPtp9.png](https://s21.ax1x.com/2024/11/13/pAgPtp9.png)](https://imgse.com/i/pAgPtp9)
[![pAgPU61.png](https://s21.ax1x.com/2024/11/13/pAgPU61.png)](https://imgse.com/i/pAgPU61)

- 用命令就比较麻烦，需要知道第一列（提交哈希值）
```git
git log --oneline
```
[![pAgPaOx.png](https://s21.ax1x.com/2024/11/13/pAgPaOx.png)](https://imgse.com/i/pAgPaOx)
- 进入`20241111测试冲突1`的提交的地方
```git
git switch --detach 1ac9efa
```
`--detach` 参数使 Git 进入分离头指针模式（detached HEAD），这样你可以查看或操作该提交的内容而不会影响当前分支。
[![pAgPBTO.png](https://s21.ax1x.com/2024/11/13/pAgPBTO.png)](https://imgse.com/i/pAgPBTO)

- 先创建新分支
```git
git branch switch转换多步创建的新分支
```
- 再进入新分支
```git
git switch switch转换多步创建的新分支
```
![Pasted image 20241111113459.png](https://s2.loli.net/2024/11/13/w3AzTZofuKH1gEy.png)
![Pasted image 20241111113508.png](https://s2.loli.net/2024/11/13/O8fuWaCi6EQ1lN7.png)

- 使用下面的命令可以直接进入某个提交节点，然后创建并进入新分支
```git
git checkout -b new-branch-name 提交哈希值
```
![Pasted image 20241111113225.png](https://s2.loli.net/2024/11/13/1XdzvksOFA3jtCa.png)

###### 撤回上一次修改
```git
git reset --soft HEAD^
# git push -f # 如果提交不上去，使用强制提交
```

`git reset --soft HEAD^` 的作用是将当前分支的指针（`HEAD`）回退到上一个提交（`HEAD^`），但保留工作区和暂存区的内容。指的是当前分支中最新的一次提交，如果自己在当前分支的其他很久之前的提交的地方，它修改的也是最新的上一次提交，而不是当前的提交

具体解释如下：
1. **HEAD^**：表示上一个提交。`HEAD` 指向当前提交，而 `HEAD^` 就是它的上一个提交。
2. **`--soft`**：表示回退提交时只移动 `HEAD` 的位置，不会影响暂存区或工作区。也就是说，回退后的代码依然会停留在暂存区中，文件的修改依旧可以直接提交。
![Pasted image 20241111113827.png](https://s2.loli.net/2024/11/13/yLlsUzo8xROnuqJ.png)
![Pasted image 20241111113821.png](https://s2.loli.net/2024/11/13/V64DrTk7WpmGF1O.png)
![Pasted image 20241111113933.png](https://s2.loli.net/2024/11/13/Y73wPtTKR4IzVbN.png)

###### 如何切换远程仓库呢?
```git
git remote -v
```
`git remote -v` 命令用于查看当前 Git 仓库的远程仓库 URL 配置。它会显示所有关联的远程仓库及其 URL，通常用于验证远程仓库的配置，或查看你项目所连接的 Git 远程仓库。
输出格式通常是这样的：
```shell
origin  https://github.com/username/repository.git (fetch)
origin  https://github.com/username/repository.git (push)
```
其中：
- `origin` 是远程仓库的名称（默认远程仓库通常叫做 `origin`）。
- `https://github.com/username/repository.git` 是仓库的 URL。
- `(fetch)` 和 `(push)` 分别表示该 URL 用于拉取（fetch）和推送（push）操作。

如果有多个远程仓库，`git remote -v` 会列出所有远程仓库的 URL 配置信息。
![Pasted image 20241111114506.png](https://s2.loli.net/2024/11/13/SNXHEZUehR5FipV.png)

- 添加一个新的远程仓库
```git
git remote add origin2 git@github.com:wing2791/ProjectTest.git
```
`git remote add origin2 git@github.com:wing2791/ProjectTest.git` 这个命令的作用是向当前 Git 仓库添加一个新的远程仓库，并命名为 `origin2`。
- 解释
	- **`git remote add`**：用于添加一个新的远程仓库。
	- **`origin2`**：这是远程仓库的别名（你可以自由命名，常见的是使用 `origin` 来指代默认的远程仓库）。在这个例子中，使用 `origin2` 作为别名，你可以通过这个别名来引用远程仓库。
	- **`git@github.com:wing2791/ProjectTest.git`**：这是远程仓库的 URL。这里使用的是 SSH 协议（`git@github.com`），它通过 SSH 密钥进行认证。
![Pasted image 20241111114849.png](https://s2.loli.net/2024/11/13/OMYvIaNu3wlpAhW.png)

- 获取别名为origin2的仓库的数据
`git fetch origin2 --depth 10` 这个命令的作用是从名为 `origin2` 的远程仓库获取数据，并且仅限于获取最新的 10 次提交（深度为 10）
```git
git fetch origin2 --depth 10
```
![Pasted image 20241111115310.png](https://s2.loli.net/2024/11/13/XtWMoGj8JusSbzI.png)
例如，如果你要从远程仓库 `origin2` 的 `feature_branch` 分支切换到本地，命令如下：
- 作用是本地创建一个新的分支`feature_branch`，从远程的`origin2/feature_branch`下分出来的

```bash
git checkout -b feature_branch origin2/feature_branch
```
- 但是如果只是切换到指定的分支，在该分支下操作，暂时没找到，可以可视化操作，命令必须指定实际分支名，不能是哈希值
```git
git switch master
```
- 这里是每个分支名都不一样，如果原来的两个分支名一样，是不是不能导入呢
- 测试过了可以导入
```git
git switch --detach origin2/master # origin2是仓库别名，master是origin2下的分支名
git switch master # 重新切换origin2下的master就可以啦
```

![Pasted image 20241111130053.png](https://s2.loli.net/2024/11/13/5WGNhPkmi64doHj.png)
![Pasted image 20241111130738.png](https://s2.loli.net/2024/11/13/ylwTrgvZMtmQAb9.png)
![Pasted image 20241111130819.png](https://s2.loli.net/2024/11/13/ZcUQv7CARa6Xnux.png)

在 Git 中，`--detach` 选项用于让你进入“分离头指针（detached HEAD）”状态。

当你使用 `git switch --detach origin2/master` 时，你会切换到 `origin2/master` 远程分支的最新提交，但 **不会创建本地分支**，并且 **HEAD 不再指向任何本地分支**，而是直接指向该提交。这种状态叫做“分离头指针”状态。

- 分离头指针（detached HEAD）状态

1. **没有本地分支**：在“分离头指针”状态下，HEAD 直接指向某个提交（如 `origin2/master`），而不是本地的某个分支。
2. **临时状态**：你仍然可以在该状态下查看和修改代码，但如果在此状态下提交更改，这些更改将不会直接影响任何分支。除非你显式创建新的分支，否则这些提交将“游离”，无法轻易访问。
3. **提交后不会自动保存**：如果你在 `--detach` 状态下进行了提交，但没有创建一个新的分支来保存这些提交，那么这些提交可能会丢失。如果你切换回其他分支或退出当前状态，这些提交将不再可访问。

- 举个例子：
假设你在 `origin2/master` 上使用 `git switch --detach`，你处于一个分离头指针的状态：
```bash
git switch --detach origin2/master
```
此时，你可以修改文件并提交更改，但这些更改并不会直接影响 `master` 分支。如果你想保留这些更改，你需要创建一个新的本地分支：
```bash
git switch -c new-branch
```

这样，你就创建了一个新的本地分支，且它会包含你在分离头指针状态下所做的提交。
- 为什么使用 `--detach`？
	- **临时查看历史**：如果你想查看远程分支上的提交，但不想修改本地分支的状态，可以使用 `--detach`，它允许你暂时查看和操作历史，而不会改变当前分支的内容。
	- **避免影响本地工作**：它通常用于查阅历史、测试某些提交或操作代码而不想立即创建新分支或改变工作状态。
###### git fetch和git pull，git clone有啥区别呢
`git fetch`、`git pull` 和 `git clone` 是 Git 中用来与远程仓库交互的不同命令，它们的作用有所不同。下面是它们的区别：
1. `git clone`：
- **作用**：`git clone` 是用来从远程仓库复制整个仓库到本地的命令。它不仅会下载远程仓库的所有提交记录，还会创建一个新的本地仓库，并将当前远程仓库的内容和分支设置为本地仓库的默认内容。
- **特点**：
    - 一开始使用 `git clone` 会把远程仓库的整个历史记录、分支等数据拉取到本地。
    - 克隆时，会自动创建 `origin` 作为远程仓库的默认名称，指向你克隆的仓库。
    - 克隆会包括所有分支、标签和文件。
- **使用场景**：适用于第一次从远程仓库创建本地仓库时。

```bash
git clone https://github.com/username/repository.git
```
这会将远程仓库复制到本地。

2. `git fetch`：
- **作用**：`git fetch` 是用来从远程仓库获取最新的提交、分支、标签等信息，但不会自动将其合并到本地分支。它更新本地远程跟踪分支的内容，并且不会影响当前的工作目录或本地分支。
- **特点**：
    - 只拉取数据到本地，并更新远程追踪分支。
    - 不会自动合并，用户需要手动决定如何处理远程的更新（通常使用 `git merge` 或 `git rebase`）。
- **使用场景**：适用于你想查看远程仓库更新，但不想立刻合并的情况。通过 `git fetch`，你可以先查看远程仓库的更新，再决定是否合并到本地分支。
```bash
git fetch origin
```
这会拉取 `origin` 仓库的所有更新，但不会自动合并到当前分支。
3. `git pull`：
- **作用**：`git pull` 是 `git fetch` 和 `git merge` 的结合。它先从远程仓库拉取最新的更新，再自动将这些更新合并到当前分支。如果没有冲突，合并会直接完成；如果有冲突，则需要手动解决。
- **特点**：
    - 拉取数据并自动合并更新到当前分支。
    - 默认情况下，`git pull` 会执行一次 `git fetch`，然后执行一次合并（`git merge`）。
- **使用场景**：适用于你希望获取远程仓库的更新并立即与本地分支合并的情况。
```bash
git pull origin master
```

这会拉取远程 `origin` 仓库的 `master` 分支的更新，并合并到当前的本地分支。
- 总结：
	- `git clone`：用于克隆远程仓库到本地，第一次从远程仓库创建本地副本。
	- `git fetch`：从远程仓库拉取最新的数据（如提交、分支等），但不会自动合并到本地分支。
	- `git pull`：从远程仓库拉取数据并自动合并到当前本地分支。

###### 合并分支
- 将`switch转换多部创建的新分支`合并到main分支上（也就是当前操作的分支）
![Pasted image 20241111120255.png](https://s2.loli.net/2024/11/13/GDVYiWwQeXUoCM1.png)
- 发生冲突，需要手动解决冲突
![Pasted image 20241111120400.png](https://s2.loli.net/2024/11/13/Bx2gI37YKuAJr8i.png)

![Pasted image 20241111120719.png](https://s2.loli.net/2024/11/13/mlyqp3wuVBPDZge.png)
- Accept Incomming/ Accept表示冲突接收当前文档的修改
- Accept Combination(Incoming First)/Accept Combination(Current First)表示合并两个冲突的文档，但是谁放在前面的修改
- Ignore 表示不需要文档的这个内容
- 修改完冲突后，直接commit就可以啦
![Pasted image 20241111120804.png](https://s2.loli.net/2024/11/13/3YIptP1dbLkNDHG.png)
![Pasted image 20241111121257.png](https://s2.loli.net/2024/11/13/A9q3Nesbfg4nxHv.png)
- 回退上一次合并
```git
git reset --hard HEAD~1
```
![Pasted image 20241111121611.png](https://s2.loli.net/2024/11/13/ERlI4nJf63pvMHu.png)

- 将某个特定的提交，引入当前分支
- 不过还是需要有某个特定提交的哈希值，使用`git log --oneline`
```git
git cherry-pick <commit_hash>
```
- 或者使用git Graph看一下，这个Commit就是提交哈希值
![Pasted image 20241111122737.png](https://s2.loli.net/2024/11/13/vYkJC5dexBqtOWE.png)

- 合并后有冲突，解决冲突
![Pasted image 20241111122838.png](https://s2.loli.net/2024/11/13/ET56cPaqGnAS8Np.png)
![Pasted image 20241111122924.png](https://s2.loli.net/2024/11/13/eFh5yXSR2U9vzDm.png)


#### 其他待补充
- git 的权限管理
- 在不同编辑器上的使用（Vstudio）
- 可视化 git 管理工具
- ~~实际使用的例子~~

#### 参考视频

[【GeekHour】一小时 Git 教程](https://www.bilibili.com/video/BV1HM411377j)
[SSH+Git+Gitee+Vscode 学会了就是代码管理大师](https://www.bilibili.com/video/BV1Fw4m1C7Tq/)
