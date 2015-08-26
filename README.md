#git 学习笔记

参考资料：

<http://blog.csdn.net/five3/article/details/8904635>

<http://www.bootcss.com/p/git-guide/>

<http://www.cnblogs.com/1-2-3/archive/2010/07/18/git-commands.html>

<http://www.cnblogs.com/ballwql/p/3462104.html>

<http://blog.csdn.net/dijason/article/details/9042425>

-----------------------------------------------------
# （一）基础

## 工作流
本地仓库由 git 维护的三棵“树”组成。

*   第一个是你的 工作目录，它持有实际文件；
*   第二个是 缓存区（Index），它像个缓存区域，临时保存你的改动；
*   最后是 HEAD，指向你最近一次提交后的结果。`

## 本地配置

    git config --global user.name 'onovps'
    git config --global user.email 'onovps@onovps.com' #全局联系方式，可选

## 避免每次操作需要输入密码

#### 1. 在宿主目录下创建文件存储GIT用户名和密码

    cd ~
    vim .git-credentials
        https://username:password@github.com
        https://username:password@git.oschina.net

#### 2. 添加Git Config 内容

    git config --global credential.helper store
    
执行完后查看~目录下的.gitconfig文件，会多了一项：

    [credential]
        helper = store
        
重新开启bash会发现git push时不用再输入用户名和密码

-----------------------------------------------------
# （二）仓库

## 创建仓库
创建新文件夹，打开，然后执行 `git init` 以创建新的 git 仓库。

## 克隆仓库
执行如下命令以创建一个本地仓库的克隆版本：

`git clone /path/to/repository`

如果是远端服务器上的仓库，你的命令会是这个样子：

`git clone username@host:/path/to/repository`

-----------------------------------------------------
# （三）分支

## 查看分支
查看本地分支：`git branch`

查看远程分支：`git branch -r`

查看所有分支：`git branch -a`

## 创建分支
切换分支：`git checkout <branch>`

创建本地分支：`git branch <branch>` 

创建并切换到新分支：`git checkout -b <branch>`

基于远程分支建立本地分支： `git checkout --track origin/<branch>` 

## 删除分支
删除本地分支：`git branch -d <branch>`

删除远程分支：

    git branch -r -d origin/<branch>
    git push origin :<branch>
    
    注：冒号前面必须输入一个空格！

## 提交分支：

对当前分支打标记：`git tag tagContent`

提交分支：`git push origin <branch>:<branch>`

## 合并分支：

要更新本地仓库至最新改动：`git pull`

要合并其他分支到当前分支：`git merge <branch>`

注：自动合并并非次次都能成功，并可能导致 冲突（conflicts）

在合并改动之前，也可以使用如下命令查看：`git diff <source_branch> <target_branch>`

# （四）提交代码
查看状态：`git status`


计划改动（把它们添加到缓存区）：`git add <filename>`

实际提交改动,改动提交到了 HEAD：`git commit -m "comment"`

将这些改动提交到远端仓库：`git push origin <branch>`

使用 HEAD 中的最新内容替换掉工作目录中的文件：`git checkout -- <filename>`

假如你想要丢弃你所有的本地改动与提交，可以到服务器上获取最新的版本并将你本地主分支指向到它：

    git fetch origin
    git reset --hard origin/master
