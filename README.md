git 学习笔记

参考资料：

<http://git.oschina.net/progit/index.html>

<http://www.bootcss.com/p/git-guide/>

<http://www.cnblogs.com/1-2-3/archive/2010/07/18/git-commands.html>

<http://www.cnblogs.com/ballwql/p/3462104.html>

# 起步

## 安装

`$ apt-get install git`

## 配置

-   配置文件

    `/etc/gitconfig`：

    系统中对所有用户都普遍适用的配置。读写 `git config --system`

    `~/.gitconfig`：

    用户目录下的配置文件只适用于该用户。读写 `git config --global`

    `.git/config`：

    这里的配置仅仅针对当前项目有效。读写 `git config`

-   用户信息

    个人的用户名称和电子邮件地址，这两条配置很重要，每次 Git 提交时都会引用这两条
    信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：

    ```
    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com
    ```

-   命令别名

    ```
    $ git config --global alias.co checkout
    $ git config --global alias.br branch
    $ git config --global alias.ci commit
    $ git config --global alias.st status

    $ git config --global alias.unstage 'reset HEAD --'
    $ git config --global alias.last 'log -1 HEAD'
    ```

    现在，如果要输入 git commit 只需键入 git ci 即可。

    有时候我们希望运行某个外部命令，而非 Git 的子命令，只需要在命令前加上 ! 就行。

    例如，我们可以设置用 git visual 启动 gitk

    ```
    $ git config --global alias.visual '!gitk'
    ```

-   查看配置信息

    `$ git config --list`

## 避免密码

-   在宿主目录下创建文件存储GIT用户名和密码

    ```
    cd ~
    vim .git-credentials
        https://username:password@github.com
        https://username:password@git.oschina.net
    ```

-   添加Git Config 内容

    ```
    git config --global credential.helper store
    ```
    
    执行完后查看~目录下的.gitconfig文件，会多了一项：
    
    ```
    [credential]
        helper = store
    ```    
            
    重新开启bash会发现git push时不用再输入用户名和密码

## 查看帮助

三种方法：

`$ git help <verb>`

`$ git <verb> --help`

`$ man git-<verb>`

# 仓库

## 创建新仓库

`$ git init`

## 克隆仓库

`$ git clone git://github.com/schacon/grit.git`

Git 支持许多数据传输协议：

-   git:// 
-   http(s):// 
-   user@server:/path.git 表示的 SSH 传输协议。

## 查看当前的远程库

`git remote [-v]`

Git 默认使用 origin 来标识你所克隆的原始仓库

加上 -v 选项，显示对应的克隆地址

`git remote show [remote-name]`

查看远程仓库信息

# 分支

## 查看分支
查看本地分支：`git branch`

查看远程分支：`git branch -r`

查看所有分支：`git branch -a`

查看哪些分支已被并入当前分支：`git branch --merge`

查看尚未合并的工作：`git branch --no-merged`

## 创建分支

创建本地分支：`git branch <branch>` 

基于本地分支创建远程分支： `git push --set-upstream origin <branch>`

切换分支：`git checkout <branch>`

基于当前分支创建并切换到新分支：`git checkout -b <branch>`

跟踪远程分支： `git checkout --track origin/<branch>` 

重命名本地分支：`git branch -m <oldname> <newname>`

## 删除分支
删除本地分支：`git branch -d <branch>`

git branch -d 删除未合并进来的分支会提示错误，可以用大写的删除选项 -D 强制执行

删除远程分支：

    git branch -r -d origin/<branch>
    git push origin :<branch>
    
    注：冒号前面必须输入一个空格！

## 合并分支
要合并其他分支到当前分支：`git merge <branch>`

注：自动合并并非次次都能成功，并可能导致 冲突（conflicts）

在合并改动之前，也可以使用如下命令查看：`git diff <source_branch> <target_branch>`

任何包含未解决冲突的文件都会以未合并（unmerged）的状态列出。Git 会在有冲突的文件里加入标准的
冲突解决标记，可以通过它们来手工定位并解决这些冲突。可以看到此文件包含类似下面这样的部分：

`$ git merge iss53` 

```
<<<<<<< HEAD:index.html
    <div id="footer">contact : email.support@github.com</div>
    =======
    <div id="footer">
    please contact us at support@github.com
    </div>
    >>>>>>> iss53:index.html
```

可以看到 ======= 隔开的上半部分，是 HEAD（即 master 分支，在运行 merge 命令时所切换到的分
支）中的内容，下半部分是在 iss53 分支中的内容。解决冲突的办法无非是二者选其一或者由你亲自整合
到一起。

## 衍合分支

`$ git rebase master`

## 上行分支

设置：

`$ git push --set-upstream origin <branch>`

取消设置：

`$ git branch --unset-upstream"`

# 基础
## 检查当前文件状态

`$ git status`

## 跟踪新文件或暂存已修改文件

`$ git add benchmarks.rb`

git add 命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：

1. 可以用它开始跟踪新文件
2. 把已跟踪的文件放到暂存区
3. 合并时把有冲突的文件标记为已解决状态。

## 提交更新

启动文本编辑器以便输入本次提交的说明

`$ git commit`

另外也可以用 -m 参数后跟提交说明的方式，在一行命令中提交更新：

`$ git commit -m "Story 182: Fix benchmarks for speed"`

跳过使用暂存区域，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交

`$ git commit -a -m 'added new benchmarks'`

## 推送数据到远程仓库

`$ git push [remote-name] [branch-name]`

## 从远程仓库抓取数据

`$ git fetch`

fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支

`$ git pull`

git pull 命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支

# 修改

## 移除文件

`$ git rm grit.gemspec`

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f ，以防误删除文件后丢失修改
的内容：

`$ git rm -f grit.gemspec`

把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。
换句话说，仅是从跟踪清单中删除：

`$ git rm --cached readme.txt`

## 移动文件

`$ git mv file_from file_to`

## 撤销操作

取消对文件的修改：

`git checkout -- <file>...`

取消已经暂存的文件：

`git reset HEAD <file>...`

重新提交：

`$ git commit --amend`

## 忽略某些文件

可以创建一个名为 `.gitignore` 的文件

格式规范：

*   所有空行或者以注释符号 ＃ 开头的行都会被 Git 忽略。
*   可以使用标准的 glob 模式匹配。
*   匹配模式最后跟反斜杠（/）说明要忽略的是目录。
*   要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

glob 模式是指 shell 所使用的简化了的正则表达式：

*   星号（*）匹配零个或多个任意字符；
*   问号（?）只匹配一个任意字符；
*   [abc] 匹配任何一个列在方括号中的字符；
*   如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配

# 查看

## 查看已暂存和未暂存的更新

比较工作目录中当前文件和暂存区域快照之间的差异：

`$ git diff`

比较已经暂存起来的文件和上次提交时的快照之间的差异:

`$ git diff --cached`

查看分支之间的差异：

`$ git diff <source_branch> <target_branch>`

## 查看提交历史

默认不用任何参数的话，git log 会按提交时间列出所有的更新，最近的更新排在最上面：

`$ git log`

-   常用选项

    ```
    选项 说明
        -p 按补丁格式显示每个更新之间的差异。
        --stat 显示每次更新的文件修改统计信息。
        --shortstat 只显示 --stat 中最后的行数修改添加移除统计。
        --name-only 仅在提交信息后显示已修改的文件清单。
        --name-status 显示新增、修改、删除的文件清单。
        --abbrev-commit 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
        --relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。
        --graph 显示 ASCII 图形表示的分支合并历史。
        --pretty  使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，
                  fuller 和 format（后跟指定格式）。
    ```

-   限制输出长度

    ```
    选项 说明
        -(n) 仅显示最近的 n 条提交
        --since, --after 仅显示指定时间之后的提交。
        --until, --before 仅显示指定时间之前的提交。
        --author 仅显示指定作者相关的提交。
        --committer 仅显示指定提交者相关的提交。
    ```

-   `-p`

    选项展开显示每次提交的内容差异

-   `-<n>`
      
    仅显示最近的若干条提交

-   `--stat`

    仅显示简要的增改行数统计

-   `--pretty`

    可以指定使用完全不同于默认格式的方式展示提交历史。

    比如 oneline, short, full, fuller, format 等


-   `--pretty=format`

    ```
    $ git log --pretty=format:"%h - %an, %ar : %s"
        ca82a6d - Scott Chacon, 11 months ago : changed the version number
        085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
        a11bef0 - Scott Chacon, 11 months ago : first commit
    ```

    常用的格式占位符写法及其代表的意义:

    ```
    选项 说明
        %H 提交对象（commit）的完整哈希字串
        %h 提交对象的简短哈希字串
        %T 树对象（tree）的完整哈希字串
        %t 树对象的简短哈希字串
        %P 父对象（parent）的完整哈希字串
        %p 父对象的简短哈希字串
        %an 作者（author）的名字
        %ae 作者的电子邮件地址
        %ad 作者修订日期（可以用 -date= 选项定制格式）
        %ar 作者修订日期，按多久以前的方式显示
        %cn 提交者(committer)的名字
        %ce 提交者的电子邮件地址
        %cd 提交日期
        %cr 提交日期，按多久以前的方式显示
        %s 提交说明
    ```

-   `--graph`
    
    用 oneline 或 format 时结合 --graph 选项，可以看到开头多出一些 ASCII 字符串表示的简单
    图形，形象地展示了每个提交所在的分支及其分化衍合情况。

    ```
    $ git log --pretty=format:"%h %s" --graph
        * 2d3acf9 ignore errors from SIGCHLD on trap
        * 5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
        |\
        | * 420eac9 Added a method for getting the current branch.
        * | 30e367c timeout code and tests
        * | 5a09431 add timeout protection to grit
        * | e1193f8 support for heads with slashes in them
        |/
        * d6016bc require time for xmlschema
        * 11d191e Merge branch 'defunkt' into local
    ```
