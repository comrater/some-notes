### Git 的简单理解和基本使用方法

Git是一个分布式的版本控制系统。

## Git的基本用法

假设是在 本地 <-> GitHub 上互相传输

Step 0. 在本地安装和配置Git

Step 1. 对想要做版本管理的文件夹，**在本地创建Git仓库**

Step 2. 在GitHub上创建仓库

---------- 反正不论是谁先谁后，在本地和GitHub都选好位置创建好仓库 ----------

Step 3. 在 本地 <-> GitHub 的两个仓库间**建立对应关系**

Step N. 在本地**使用Git命令**与GitHub通信，更新版本信息

Step Z. 删除Git仓库（如果不需要了）

### 在本地安装和配置Git

安装在官网很容易。

第一次使用前需要配置SSH key，怎么配置忘了。

### 在本地创建和删除Git仓库

在Git Bash中

```bash
$ cd 到目标文件夹
$ git init  #创建Git仓库
```

如果目标文件夹中出现了隐藏文件夹 .git 说明创建成功。

删除本地Git仓库只需要删除这个 .git 文件夹即可。

### 在 本地 <-> GitHub 的两个仓库间建立对应关系（有点没明白）

首先找到GitHub上仓库的SSH，如图，把【这个链接】复制下来。

![image-20220509013926867](C:\Users\yq\AppData\Roaming\Typora\typora-user-images\image-20220509013926867.png)

在Git Bash中

```bash
## 如果是从本地往GitHub上上传就：
$ cd 到目标文件夹
$ git init  #创建Git仓库
$ git add .  # 把所有更改（因为第一次所以相当于文件夹里所有文件）都加入暂存区
$ git commit -m "备注信息"  # 存为commit
在GitHub上创建一个新仓库，复制【这个链接】
$ git remote add origin 【这个链接】 # 建立对应关系
$ git push origin master  # 把commit push到GitHub上

## 如果是从GitHub上往本地下载就：
$ cd 到目标文件夹的父文件夹（不需要提前建文件夹和git init）
$ git clone 【这个链接】 # 把GitHub上项目全部下下来。这一步会直接复制GitHub上的东西，生成一个【与项目同名的文件夹】里，今后就在这里操作就可以了
```

### 使用Git命令进行版本管理

首先我们要理解一下Git的基本原理。

![image-20220509012630439](C:\Users\yq\AppData\Roaming\Typora\typora-user-images\image-20220509012630439.png)

Git所有控制的范围就是你创建 .git 的【目标文件夹】。在创建Git仓库后生成的 **.git **文件夹是版本库，相当于图中的右侧蓝色区域，负责版本管理。目标文件夹里存的电脑里真实存在的、你实际在编辑的其他文件是**工作区**。

（不知道名词，用add和commit分别表示git add和git commit之后的结果）

每个branch都是一个线性结构。commit -> commit -> ... -> commit

Git的基本原理是首先你会处在一个branch的一个commit里，这个commit叫做**HEAD**。

当你在工作区改了文件之后，通过git add操作可以把**HEAD记录的文件状态**和**当前工作区文件的实际状态**做对比，将差异/改动存到**暂存区**。

通过git commit操作，可以把**当前暂存区的记录**真的存起来，作为这个branch的一个新的**commit**。

注意：add和commit记录的都是两个版本之间的**差异**，而不是新版本的最终**结果**！

注意：虽然不严谨地用“清空”来形容暂存区。但其实就算不修改，暂存区里也一直是有东西的，意会即可。

**基本命令解读：**

- git add：把当前工作区相对HEAD的修改存到暂存区【工作区 -> 暂存区】

```bash
$ git add 指定文件  # add指定文件 
$ git add .  # add所有文件
```

- git commit：把当前暂存区记录的修改存成commit【暂存区 -> commit】

```bash
$ git commit -m "备注信息"  
```

- git push：【对于当前名称的branch：本地的许多commit -> GitHub】

```bash
$ git push origin master  # 把当前branch的commit都push到远程主机origin的master分支上
```

- git fetch：【对于当前名称的branch：GitHub的许多commit -> 本地】

https://www.cnblogs.com/kevingrace/p/5896706.html对比git pull和git pull --rebase

- git merge：

![image-20220509144813994](E:\yqDATA\等待备份区\BUAA cs文件夹\文章\Git 的简单理解和基本使用方法\image-20220509144813994.png)

merge就是把C -> E的更改 ΔE 和C -> D的更改 ΔD 加起来，得到M = C +  ΔE + ΔD。不过这样两个分支就变成了菱形的奇怪关系。（假设ΔE 与 ΔD无冲突）

- git rebase：

![image-20220509145217701](E:\yqDATA\等待备份区\BUAA cs文件夹\文章\Git 的简单理解和基本使用方法\image-20220509145217701.png)

rebase就是在merge的基础之上把本地的commit E删掉，使得merge的结果不出现菱形。

- git pull = git fetch + git merge：假设当前HEAD是本地一个commit，工作区和暂存区没修改过的，pull的是GitHub上的目标branch。那么这个过程就是首先找到HEAD和目标branch的最近公共祖先，然后把目标branch从祖先开始的所有修改作用到HEAD上。（假设不冲突）如果HEAD = branch就相当于下载branch。

```bash
$ git pull origin master  # 把远程主机origin的master分支拉取过来，与本地当前分支合并
```

- git pull --rebase = git fetch + git rebase：

```bash
$ git pull --rebase origin master
```

**进阶命令：**

- git branch：查看本地有哪些分支以及当前分支
- git checkout：切换分支

```bash
$ git checkout 指定分支名  # 切换到名为【指定分支名】的分支上
$ git checkout -b 指定分支名  # 创建名为【指定分支名】的分支并切换过去
$ git checkout -d 指定分支名  # 删除名为【指定分支名】的分支
## 以上对分支的切换会改变工作区和暂存区（清空一切没有commit的修改）

$ git checkout -- 指定文件名  # 把暂存区【指定文件名】这个文件恢复到工作区
$ git checkout .  # 把暂存区的全部文件恢复到工作区
## 以上操作都是用暂存区的记录改变工作区没（清空没有add的修改）
```

- git log：查看commit记录
- git reset：撤销对工作区的修改和add

```bash
$ git reset  # 重置暂存区的文件与上一次的commit保持一致（清空暂存区，工作区不变）
$ git reset --hard  # 重置所有内容到上一次的commit（清空暂存区和工作区）
```

- git status：查看文件的状态（有没有add、有没有commit……）
- git diff：预览差异

- git stash：把相对于HEAD的所有未提交的修改（工作区+暂存区）临时存起来（就可以放心地抛弃当前做到一半的东西然后切换branch了）
- git stash pop：把git stash保存的修改作用到当前状态（工作区+暂存区），并删除stash的保存记录





```bash
# 某文章的推荐方案：不明觉厉

# 多人基于同一个远程分支开发的时候，如果想要顺利 push 又不自动生成 merge commit，建议在每次提交都按照如下顺序操作：

# 把本地发生改动的文件贮藏一下
$ git stash

# 把远程最新的 commit 以变基的方式同步到本地
$ git pull --rebase

# 把本地的 commit 推送到远程
$ git push

# 把本地贮藏的文件弹出，继续修改
$ git stash pop

# 链接：https://juejin.cn/post/6844903895160881166
```

### 