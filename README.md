## Git 笔记  

> 在 B 站学习 Git 过程中记的笔记，如有错误，欢迎指正。

![](https://i.loli.net/2019/07/18/5d3024326f60867129.png)

优先级 3>2>1

&nbsp;

**局部：**

配置文件位置：当前项目/.git/config（文件）

使用 git config --local user.name "name"，git config --local user.email "email" 设置局部的变量

 &nbsp;

**全局**：

配置文件位置：~/.gitconfig（文件）

使用 git config --global user.name "name"，git config --global user.email "email" 设置全局的变量，别名一般是全局的，就在这里设置

 &nbsp;

git config --list   列出配置信息

删除项目用户名 git config --local --unset user.name

删除项目邮箱名 git config --local --unset user.email

 &nbsp;

#### 常用命令

mkdir test     创建 test 文件夹

cd test           进入 test 文件夹

touch a.txt     创建 a.txt 文件

cat a.txt           查看 a.txt 里面的内容

pwd               查看当前实在哪个文件夹进行操作

ls                    查看当前文件夹里面的所有文件，不包括隐藏文件

ls  -al              查看当前文件夹里面的所有文件，包括隐藏文件

ls -ah              查看隐藏的文件

vim a.txt       使用 vim 编辑文件

insert      vim 进入编辑模式

esc         vim 退出编辑模式

:wq        保存文件，退出 vim

:q!         不保存，强制退出

 &nbsp;

#### git 仓库初始化

git init            初始化文件夹，使之变成 git 仓库，会生成 .git 隐藏文件

git add a.txt    把 a.txt 提交到暂存区，此时并未提交到仓库

git commit -m 'xxxxx'   提交到仓库，-m 后面是提交时的说明

git commit -am 'xxxxx'  上面两个操作的合并，前提是该文件必须已经 add 进了版本库

git commit --amend -m 'xxxxx'  修改上一次提交的 commit message

git add .          一次性提交所有文件，最好不要用 git add *

git status         查看当前状态

![](https://i.loli.net/2019/07/18/5d301e6460ab439254.png)

 &nbsp;

**删除与恢复**

git rm a.txt:

1、删除了一个文件

2、把删除文件这个操作纳入到暂存区

若想恢复文件，需要两步：

1、git reset HEAD a.txt  把文件从仓库恢复到暂存区

2、git checkout -- a.txt 把文件从暂存区恢复到工作区（也即是将工作区的修改丢弃）

&nbsp;

rm a.txt:

只是将文件删除，并未纳入到暂存区

若想恢复这个文件，需要执行 git checkout -- a.txt 将工作区中的修改丢弃

若确定要删除这个文件，删除完之后，需要把删除这个操作 add 进暂存区，再 commit

&nbsp;

git mv a.txt b.txt  重命名，相当于把 a.txt 删除，在新增 b.txt，并把这两个操作提交到暂存区

执行 git reset HEAD a.txt，git checkout -- a.txt 会恢复 a.txt（相当于恢复 git rm a.txt）

执行 git reset HEAD b.txt，b.txt 会变成未 add 的状态，需要手动删除 b.txt

最简单的恢复的方法是把 b.txt 重命名为 a.txt

&nbsp;

mv a.txt b.txt  把 a.txt 移动到 b.txt，其实是重命名

cp a.txt b.txt   复制 a.txt，并命名为 b.txt

&nbsp;

git rm --cache a.txt  移除暂存区中的 a.txt，不对该文件进行跟踪

&nbsp;

git checkout -- a.txt   丢弃工作区的修改，与暂存区保持一致（第一次 add 了，然后从当前到下一次 add 之前，本质就是把文件从暂存区恢复到工作区）：比如把 a.txt add 之后，用 rm 删除 a.txt，删除之后可以用这个命令恢复

git reset HEAD a.txt  丢弃暂存区的修改，与仓库保持一致（前提是至少有一次 commit），本质就是从仓库恢复最近的一次提交的文件到暂存区（所以说至少有一次 commit）

git reset --hard commit_id  已经提交到仓库了怎么撤销呢，执行这个命令（HEAD 表示当前版本，HEAD^ 上一个版本，HEAD^ 上上个版本；HEAD~1 上一个版本，HEAD~2 上上个版本）

 &nbsp;

**日志**

git log             查看所有产生的 commit 记录，按 q 推出

git reflog         显示所有历史命令，当你回退到之前的本版之后又想回退回来，但是 git log 已经没有你想要的 commit id 了，那就使用这个命令

git blame 'file'  列出文件由谁修改，修改内容、时间

git congif --global alias.br branch  设置简写（别名）

- 查看远程分支的 log

git log origin/master

git log remotes/origin/master

git log refs/remotes/origin/master

&nbsp;

git log -n      查看近 n 条的提交信息

git log --pretty=oneline     简短显示 commit 记录

git log --graph --pretty=oneline --abbrev-commit    查看分支合并情况（简要信息）

git log --graph   以图形化方式查看分支合并情况

git log --pretty=format:"格式"  以某种格式显示提交信息

&nbsp;

**比较不同**

git diff             比较工作区和暂存区的区别，原文件是工作区，目标文件是暂存区

文件 1.txt，新增一行 1，git add . 然后再新增一行 2，用 git diff 比较，分析如下：

![](https://i.loli.net/2019/07/18/5d301ed5546ea78586.png)

&nbsp;

git diff HEAD          比较工作区与 HEAD（当前分支，最新提交）的区别，原文件是工作区，目标文件是暂存区

git diff --cached     比较暂存区和 HEAD（当前分支，最新提交）的区别，原文件是暂存区，目标文件是最新提交

git diff 分支名         比较当前分支和(分支名)之间的区别

git diff commit_id    与某一次提交进行比较

 &nbsp;

**分支**

git branch        查看当前在哪个分支里，前面有个 * 号

git branch a     创建 a 分支

git checkout a  切换到 a 分支

git checkout -b a   创建并切换到 a 分支 

git checkout commit_id    切换到某个历史提交点（此时处于在游离的分支上的状态），在这个游离的分支上可以修改、提交、切换分支，切换之后会有如下提示：

执行 git branch <new-branch-name> commit_id 会创建一个新的分支

![](https://i.loli.net/2019/07/18/5d301ef6de06869114.png)

&nbsp;

git branch -d a   删除 a 分支

git branch -D a        强制删除 a 分支，有些时候可能会删除失败，比如如果 a 分支的代码还没有合并到 master，你执行 git branch -d a 是删除不了的，它会智能的提示你 a 分支还有未合并的代码，但是如果你非要删除，那就执行 git branch -D a 就可以强制删除 a 分支

git push origin --delete dev  删除远程 dev 分支

git push origin :dev   删除远程 dev  分支

 &nbsp;

**合并**

git merge a      把 a 分支合并到当前分支，需要先切换到当前分支，然后再执行此操作

![](https://i.loli.net/2019/07/18/5d30232eefffb87216.png)

![](https://i.loli.net/2019/07/18/5d3020a8256bd13076.png)

如果 master 和 dev 分支修改了同一个地方：把 dev 合并到 master 上时，会出现冲突，需手动解决，解决之后，执行 git status 查看状态，需要 git add "file"，然后 git commit 就行

再回到 dev 分支上，此时 dev 分支上的内容一直没变过，但已经落后 master 一次了，需要把 master 分支合并到 dev，即在 dev 分支上执行 git merge master，会出现快进（fast-forward）

![](https://i.loli.net/2019/07/18/5d302125d5fdd57423.png)

&nbsp;

**远程**

git remote show   与当前项目关联的所有的远程仓库（不止一个），一般是 origin

git remote show origin   远程仓库的详细信息

git branch -a    查看本地和远程所有分支

git branch -v    查看当前所处的分支的最新的提交信息 

git branch -av    查看本地和远程所有分支及最新的提交信息

git branch -m oldname newname  更改分支名称

重命名远程分支：先删掉远程分支，重命名本地分支，把本地分支推送到远程

git remote rename origin origin2  重命名远程仓库

git remote rm origin 删除远程仓库

git remote add origin 仓库名    恢复被删除的远程仓库

git pull = git fetch + git merge : 

git fetch 从远程 master 拉取最新的到本地 origin/master，不会失败

git merge 从本地 origin/master 合并到本地 master，可能会出现冲突

&nbsp;

两人修改了相同的地方，其中一个人先 push，另一个人一般是先拉取分支，如果这个人有文件没有 commit 的话，会提示先 commit，不然无法 pull，commit 之后，再 pull 的话，会提示有冲突，冲突内容已经写入到冲突文件中了，需要先解决冲突，完了之后 git add . 再 git commit（不加任何东西）会进入编辑模式，默认就行，此时，本地分支已经比远程分支多了两次提交，最后执行 git push  就行，这个人的文件就是最新的，先 push 的那个人需要 git pull 拉取最新的内容

&nbsp;

- 把分支推送到远程

git push --set-upstream origin dev（等价于 git push -u origin dev）如果想把本地 dev 推送到远程的 dev2，就需要 git push --set-upstream origin dev:dev2，但是在执行 git push 的时候会报错，提示本地分支和远程分支不同名，解决办法：写全 git push 的命令，完整写法是 git push origin src:dst，这里需要写成 git push origin HEAD:dev2（HEAD 指当前分支，也可以写成 dev）,如果本地和远程分支同名，完整的写法是 git push origin dev，把远程分支省略掉了，进而更简洁的写法是 git push

 &nbsp;

- 把远程分支拉取到本地

把 dev 分支推送到远程，另一个人 git pull 的时候，会把 origin/dev 拉取下来，但是本地并没有 dev 分支，想要创建本地 dev 分支，需要执行

git branch dev origin/dev（等价于 git checkout --track origin/dev，在本地创建一个跟远程分支名字一样的分支，若像创建一个与远程分支名不一样，就用前面的）意思是创建一个追踪远程 dev 的本地 dev

&nbsp;

一个人删除了一个远程分支，另一个人使用 git remote show origin 查看远程仓库的信息，会出现下面情况（本地 master，dev，test，远程master，dev，test，删除远程 dev）

![](https://i.loli.net/2019/07/18/5d30214b4f89d61303.png)

 &nbsp;

**保存和恢复**

git stash        把当前工作现场“储藏”起来，等以后恢复现场后继续工作（使用场景：在一个分支上开发，突然要切换到另一个分支上，而目前这个分支还没开发完，没法提交，但是不提交的话就无法切换分支，这时候可以用 git stash 暂存起来）

git stash save 'hello'    save 后面可以加信息

git stash list       查看储藏起来的列表

git stash apply   （只保存一个工作现场，多个的话需要加参数）恢复储藏的列表，恢复后，stash 内容并不删除

git stash drop    （只保存一个工作现场，多个的话需要加参数）删除储藏起来的列表

git stash pop      合并上面两步操作

![](https://i.loli.net/2019/07/18/5d3021660900660984.png)

 &nbsp;

**git 标签**

![](https://i.loli.net/2019/07/18/5d30218b9058f39798.png)

&nbsp;

git show v1.0      显示标签内容

一个标签对应两个 id，一个是标签本身的 id，另一个是标签对应的分支提交点的 commit id

&nbsp;

- 标签推送到远程：

git push origin v1.0 

git push origin refs/tags/v1.0:refs/tags/v1.0  完整写法

git push origin v1.0 v2.0 v3.0  批量推送标签

git push origin --tags    一次性推送所有标签

- 只拉取标签

git fetch origin tag v1.0

- 删除远程标签：

git push origin :refs/tags/v1.0

git push origin --delete tag v1.0

&nbsp;

**.gitignore**  

![](https://i.loli.net/2019/07/18/5d3021a1f175678980.png)

&nbsp;

对于已经添加版本库并推送到远程的文件，后来又加入到了 .gitignore，但是远程并不会删除该文件

git rm -r --cached .  删除暂存区所有的文件

git add .

git commit -m "xxx"

git push

结束

&nbsp;

**git gc**  垃圾回收

 &nbsp;

**git rebase**

![](https://i.loli.net/2019/07/18/5d3021c3107c568358.png)

![](https://i.loli.net/2019/07/18/5d3021d5ee70351159.png)

 ![](https://i.loli.net/2019/07/18/5d3021e4665da71037.png)

 ![](https://i.loli.net/2019/07/18/5d3021f20960650897.png)
