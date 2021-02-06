# Git学习笔记

## 学习资料来源

[廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
[Git常用命令](https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf)

### Git简介

Git采用分布式版本控制方式，不需要联网，具有极其强大的分支管理功能。

#### 安装Git

- Linux：`sudo apt install git`

安装完成后，在命令行输入以下内容，设置用户名和邮箱

`git config --global user.name "Your Name"`

`git config --global user.email "Your Email"`

检查设置是否成功，使用以下命令检查

`git config user.name`

`git config user.email`

#### 创建版本库

选择一个合适的地方，创建一个空目录，进入这个目录，并将该目录变为Git可管理的仓库：

`mkdir GitRepository`

`cd GitRepository`

`git init`

该目录下会出现`.git`文件，用来跟踪管理版本库，不要手动修改此处内容

Git只能跟踪文本文件的改动，二进制文件只能跟踪大小变化，word文档无法跟踪。在该目录下创建修改文件后，将文件提交到版本库，需要两步：

`git add readme.txt` 添加文件到仓库，可以多次添加

`git commit -m "write a readme file"`  将文件提交到仓库，输入本次提交的说明

### 时光机穿梭

修改了文件之后，可以使用`git status`查看状态，确定是否有文件被修改需要提交，使用`git diff`查看本次修改与上次修改之间的差别。

#### 版本回退

`git log`可以查看历次更改，`git log --pretty=oneline`可以更清晰显示历史版本

`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上一个版本，`HEAD~100`表示之前第100个版本。`git reset --hard HEAD^`表示会退到上一个版本。

`git reset --hard commit`会退到特定版本，`commit`为某版本的版本号，借此可以在当前版本的前后版本间回退。

`git reflog`用来记录每一次命令，可以查看每次修改的版本号，利于回退。

#### 工作区和暂存区

工作区即当前文件夹，`.git`为Git的版本库，内部可分为暂存区`stage`和仓库（也成为分支）`master`，仓库存在一个指针`HEAD`，指向当前版本。

文件提交顺序为 工作区>>>暂存区>>>仓库

`git add`把文件从工作区>>>暂存区

`git commit`把文件从暂存区>>>仓库

`git diff`查看工作区与暂存区的差异

`git diff -cached`查看暂存区与仓库的差别

`git diff HEAD`查看工作区与仓库的差别

`git checkout`撤销工作区修改，即把暂存区最新版本转移到工作区

`git reset HEAD readme.txt `把仓库当前版本复制到暂存区，但不改变工作区内容

#### 管理修改

Git追踪的是文件的修改，而不是文件的本身。每次`git add`将修改提交到暂存区，`git commit`将修改从暂存区提交到仓库。如果修改没有`add`就不会添加到仓库。

#### 撤销修改

版本未`git add`到暂存区，工作区与暂存区有区别，`git restore readme.txt`撤销工作区修改，此时工作区与暂存区一致。

版本已`git add`到暂存区，但未`git commit`到仓库，`git restore --staged readme.txt`用仓库`HEAD`版本刷新暂存区，工作区未被更改，此时暂存区与工作区不一致。

`git status`有命令提示。

#### 删除文件

“删除”在Git看来，也是一种修改操作，会被记录。

要删除某文件，删除之后，使用`git add`或者`git rm`提交到暂存区，在`git commit`到仓库。此时仓库中也删除了该文件。

误删某文件，使用`git checkout -- readme.txt`将仓库版本替换到工作区，无论是否修改，均一键还原。

### 远程仓库

远程仓库一般使用GitHub，本地Git与GitHub仓库之间传输使用SSH加密，需要如下设置：

在用户主目录下，查看是否存在`.ssh`文件夹，该文件夹内是否有`id_rsa`和`id_rsa.pub`，本别为私钥和公钥。如果没有，使用下方命令。

`ssh-keygen -t rsa -C "youremail@example.com"`创建SSH Key，一路Enter即可完成。

将`id_rsa.pub`中的内容添加到GitHub主页设置中的SSH Key中，即可允许本机提交推送到GitHub。

GitHub免费托管的仓库，所有人都可以看到，只有自己能修改。不要把敏感信息放进去。

#### 添加远程库

登录GitHub之后，可以新建一个repository，复制`code`中的SSH内容，在本地使用下列命令关联本地和远程仓库：

`git remote add origin git@github.com:xuanyeleyou/Learning-Notes.git`

使用下列命令，将本地库所有内容与远程库同步：

`git push -u origin master`

此后，只要本地做了提交，就可以通过下列命令提交：

`git push origin master`

#### 从远程库克隆

GitHub创建远程库后，可以通过克隆，复制到本地。命令如下：

`git clone git@github.com:xuanyeleyou/Learning-Notes.git`

在当前目录下，出现克隆的仓库目录。

### 分支管理

`master`指向提交，`HEAD`指向`master`，而创建分支`dev`，就新建一个同名指针，指向最新提交，而`HEAD`指向`dev`。

`git branch dev`创建名为`dev`的分支

`git switch dev`切换到`dev`分支

`git switch -c dev`创建并切换到`dev`分支

`git branch`查看分支

`git merge dev`将`dev`分支合并到当前分支

`git branch -d dev` 删除分支

参考中说工作区和暂存区是公共的，但根据实测，切换分支，工作区和暂存区也会变化。

#### 解决冲突

合并分支可能出现冲突的情况，此时需要修改冲突内容，再次提交才能正常提交。使用下列命令查看分支的合并情况。

`git log --graph --pretty=oneline --abbrev-commit`

#### 分支管理策略

合并分支时，默认`Fast forward`模式，此模式下，删除分支，会丢掉分支信息，就像从未有过该分支。可以使用`--no-ff`方式强制禁用`Fast forward`，合并时会生成一个新的`commit`，从历史信息可以看出分支信息。

`git merge --no-ff -m "merge with no-ff" dev`

上述命令在合并时会创建新的`commit`，所以加上`-m`参数，填写描述。、

#### Bug分支

如果要离开当前分支，进入其他分支或创建新分支修改bug，可以使用下列命令保存当前分支现场，之后可以恢复继续工作：

`git stash` 冻结储藏当前工作现场，之后可以恢复现场继续工作。该命令可多次使用

在新建Bug分支上做完修改提交后，在master分支上合并，之后删除Bug分支（非Faster forward模式）。进入之前工作的分支，采用下列命令：

`git stash list` 列出当前分支下的工作现场

`git stash apply`恢复最近工作现场，不删除stash内容；也可恢复指定stash。

`git stash pop` 恢复最近工作现场，同事删除stash内容

`git cherry-pick 4c805e2`可以复制一个特定的提交到当前分支，例如Bug分支的修改内容。该复制操作也是一个提交，具有不同名称。

#### Feature分支

如果需要添加一个新功能，可以建立一个新分支，在该分支上进行开发，之后合并到其他分支上。

`git branch -D <name>`可以强行删除某分支

#### 多人协作

`git remote`和`git remote -v`可以查看远程库信息，后者更详细

`git push origin master`推送本地分支上的提交到远程库，master为本地分支名称，可以是其他分支

从远程库clone时，仅能看到本地的master分支，如果要在dev分支上开发，需要创建远程origin的dev分支到本地，采用下列命令：

`git checkout -b dev origin/dev`

如果推送提交出现冲突，需要先`git pull`把最近的提交从远程库抓取下来，然后本地合并，解决冲突。如果拉取失败，根据提示，使用下列命令指定链接：

`git branch --set-upstream-to=origin/dev dev`

再次`git pull`

若合并有冲突，手动解决后，再push到远程库。

#### Rebase

`git rebase`可以把本地未push的分叉提交历史整理成直线

### 标签管理

Git的标签是版本库的快照，事实上是指向某个commit的指针，但不能移动，创建与删除都是瞬间完成的。

tag可以是一个有意义的名字，它跟某个commit绑定在一起。

#### 创建标签

`git tag v1.0`在最新commit打标签，默认HEAD。

`git tag v1.0 commit_id`对特定的提交commit_id打标签

`git tag`查看标签，按照字母顺序排列，而非时间顺序

`git tag -a tag_name  -m "descrabe the tag" commit_id` 可以创建带有说明的标签

`git show <tagname>`可以看到说明文字

#### 操作标签

删除标签 `git tag -d <tagname>`

推送标签到远程 ：某个标签`git push origin <tagname>`,全部推送`git push origin --tags`

删除远程标签：本地删除`git tag -d <tagname>`，远程删除`git push origin :refs/tags/<tagname>`

### 使用GitHub

在GitHub上可以任意`Fork`开源仓库，`Fork`之后自己拥有读写权限

参与开源项目，可以在GitHub上发起一个pull request，等待对方接受。

### 自定义Git

`git config --global color.ui true` 使Git显示颜色，输出命令更显目

#### 忽略特殊文件

针对必须加入Git工作目录，但又不希望提交到远程库的情况。

[A collection of `.gitignore` templates](https://github.com/github/gitignore)，其中模板可以下载组合使用。

在Git工作区的根目录下创建`.gitignore`文件，把需要忽略的文件填入其中。

完成`.gitignore`文件后，也需要提交到Git，并可以做版本管理。

`git status`确定该文件是否完整。

强制添加被忽略的文件`git add -f <file_name>`

检查文件无法添加的规则问题`git check-ignore -v <file_name>`

`.gitignore`指定不排除某些文件，在该文件中填写`!<file_name>`，即可添加例外文件

#### 配置别名

`git config --global alias.st status`将`status`配置别名`st`，此后可以使用别名`git st`

`--global`为全局参数，加上之后针对当前用户起作用，不加的话，只针对当前的仓库起作用

这些配置文件位于每个仓库的`.git/config`文件中，直接修改文件也可以起到相同作用

`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

上述配置可用`git lg`以更清晰明了的方式显示出`git log`信息

#### 搭建服务器

此服务器用于托管源代码，为远程仓库，建立于一台运行Linux的机器上，以Ubuntu为例

1. 安装Git
	`sudo apt install git`
2. 创建一个`git`用户，用于运行`git`服务
	`sudo adduser git`
3. 创建证书登录
	收集需要登录用户的公钥，即`id_rsa.pub`文件，导入`/home/git/.ssh/authorized_keys`文件中，一行一个
4. 初始化Git仓库
	选定一个目录作为Git仓库，通常以`.git`结尾，假定为`/srv/sample.git`，在`/srv`目录下输`sudo git init --bare sample.git`，即可创建一个裸仓库。
	裸仓库没有工作区，因为服务器上的Git仓库以共享为目的，禁止用户直接登录服务器修改工作区。还需要将owner改为`git`：`sudo chown -r git:git sample.git`。
5. 禁用shell登录
	出于安全考虑，禁止第二步中创建的git用户登录shell，编辑`/etc/passwd`文件：
	`git:x:1001:1001:,,,:/home/git:/bin/bash`
	修改为
	`git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`
	此时，`git`用户可以通过ssh使用git，但无法登录shell。
6. 克隆远程仓库
	现在可以克隆远程仓库在各自电脑上运行了
	`git clone git@server:/srv/sample.git`

### 使用Source Tree

[Source Tree](https://www.sourcetreeapp.com/)是Windows和Mac下免费的Git图形界面工具，可以操作任何Git库。



### 建议使用指南

先克隆到本地： `git clone git@github.com:xuanyeleyou/DTA.git`

提交修改到暂存区：  `git add  <文件名>`

提交修改到仓库：   `git commit -m "说明"`

推送到远程仓库：`git push origin <本地分支名称>`

查看本地分支名称：`git branch`










