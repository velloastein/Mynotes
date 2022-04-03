一、git安装

安装完成后，需要设置参数：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

二、创建仓库

```
$ mkdir learngit
$ cd learngit
$ pwd
```

```
$ git init
```



三、添加文件到git仓库

一定要先把要添加的文件放到learngit目录下

```
$ git add readme.txt
```

```
$ git commit -m "wrote a readme file"
```



四、

查看仓库当前状态

```
$ git status
```

查看具体修改了什么内容

```
$ git diff readme.txt
```

版本更改

```
$ git reset --hard commit_id
```

查看提交历史

```
$ git log
```

查看命令历史

```
$ git reflog
```

HEAD指向的是当前版本



命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。



当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令：

```
$ git checkout -- file
```

当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令

```
$ git reset HEAD file
```

就回到了场景1，第二步按场景1操作。

已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。（同有暂存区和仓库内容，删除工作区内容，恢复之后本地区的内容和暂存区一样）

`rm <file> `删除工作区文件,此时工作区和版本库就不一致了，如果是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`；另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本。`git restore <file>`取消工作区的删除。



五、远程仓库

1. 在用户主目录`root`下创建ssh key。

   ```linus
   $ ssh-keygen -t rsa -C "15520761380@163.com"
   ```

2. 在github中添加`id_rsa.pub`文件的内容,让github知道你的公钥



要关联一个远程库，使用命令

```
git remote add origin git@server-name:path/repo-name.git
```

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改



删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息

然后，根据名字删除，比如删除`origin`，此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动



从远程库克隆

在用户目录下使用命令`git clone`克隆到一个本地库

```
$ git clone git@github.com:michaelliao/gitskills.git
```

GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址



六、分支管理

​	查看分支：`git branch`

​	创建分支：`git branch <name>`

​	删除分支：`git branch -d <name>`

​	切换分支：`git checkout <name>`或者`git switch <name>`

​	创建并切换分支：`git checkout -b <name>`或者`git switch -c <name>`

 	合并某分支到当前分支：`git merge <name>`



解决分支合并冲突

```
$ git merge feature1
```

​	如果自动合并失败，Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件，可以使用`cat`直接查看文件手动修改，再提交。

```
$ git add readme.txt 
$ git commit -m "conflict fixed"
```

用带参数的`git log`也可以看到分支的合并情况

```
$ git log --graph --pretty=oneline --abbrev-commit
```

最后，删除分支

```
$ git branch -d feature1
```



分支管理策略

强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

```
$ git merge --no-ff -m "merge with no-ff" dev
```

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了



bug分支

Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。

```
$ git stash
```

用`git stash list`命令看看

```
$ git stash list
```

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

```
$ git stash apply stash@{0}
```

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支，在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。



多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
5. 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。
6. 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；



rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- git rebase提取操作有点像git cherry-pick一样，执行rebase后依次将当前（执行rebase时所在分支）的提交cherry-pick到目标分支（待rebase的分支）上，然后将在原始分支（执行rebase时所在分支）上的已提交的commit删除。
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。



创建标签

- 命令`git tag <tagname> commitid`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id

- 命令`git tag -a <tagname> -m "blablabla..." commitid`可以指定标签信息

- 命令`git tag`可以查看所有标签
- 命令`git show <tagname>`可以看到说明文字
- 标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签
- 命令`git push origin <tagname>`可以推送一个本地标签
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。



忽略特定文件

在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件

`.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！

GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`

可以用`git check-ignore`命令检查`.ignore`哪个文件写错了

```
git check-ignore -v <file>
```

强制添加

```
git add -f <file>
```

把指定文件排除在`.gitignore`规则外的写法就是`!`+文件名，所以，只需把例外文件添加进去即可

```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class

# 不排除.gitignore
!.gitignore
```



使用别名

加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用

```
git config --global alias.st status
git st
```

每个仓库的Git配置文件都放在`.git/config`文件中

当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中

配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置
