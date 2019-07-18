## Git 笔记  

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

 

#### git 仓库初始化

git init            初始化文件夹，使之变成 git 仓库，会生成 .git 隐藏文件

git add a.txt    把 a.txt 提交到暂存区，此时并未提交到仓库

git commit -m 'xxxxx'   提交到仓库，-m 后面是提交时的说明

git commit -am 'xxxxx'  上面两个操作的合并，前提是该文件必须已经 add 进了版本库

git commit --amend -m 'xxxxx'  修改上一次提交的 commit message

git add .          一次性提交所有文件，最好不要用 git add *

git status         查看当前状态

 

![5  i  add  stage  HEAD  commit  master  5  rigin/f-fie  pull  push ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)

 

 

git rm a.txt:

1、删除了一个文件

2、把删除文件这个操作纳入到暂存区

若想恢复文件，需要两步：

1、git reset HEAD a.txt  把文件从仓库恢复到暂存区

2、git checkout -- a.txt 把文件从暂存区恢复到工作区（也即是将工作区的修改丢弃）

 

rm a.txt:

只是将文件删除，并未纳入到暂存区

若想恢复这个文件，需要执行 git checkout -- a.txt 将工作区中的修改丢弃

若确定要删除这个文件，删除完之后，需要把删除这个操作 add 进暂存区，再 commit

 

git mv a.txt b.txt  重命名，相当于把 a.txt 删除，在新增 b.txt，并把这两个操作提交到暂存区

执行 git reset HEAD a.txt，git checkout -- a.txt 会恢复 a.txt（相当于恢复 git rm a.txt）

执行 git reset HEAD b.txt，b.txt 会变成未 add 的状态，需要手动删除 b.txt

最简单的恢复的方法是把 b.txt 重命名为 a.txt

 

mv a.txt b.txt  把 a.txt 移动到 b.txt，其实是重命名

cp a.txt b.txt   复制 a.txt，并命名为 b.txt

 

git rm --cache a.txt  移除暂存区中的 a.txt，不对该文件进行跟踪

 

git checkout -- a.txt   丢弃工作区的修改，与暂存区保持一致（第一次 add 了，然后从当前到下一次 add 之前，本质就是把文件从暂存区恢复到工作区）：比如把 a.txt add 之后，用 rm 删除 a.txt，删除之后可以用这个命令恢复

git reset HEAD a.txt  丢弃暂存区的修改，与仓库保持一致（前提是至少有一次 commit），本质就是从仓库恢复最近的一次提交的文件到暂存区（所以说至少有一次 commit）

git reset --hard commit_id  已经提交到仓库了怎么撤销呢，执行这个命令（HEAD 表示当前版本，HEAD^ 上一个版本，HEAD^ 上上个版本；HEAD~1 上一个版本，HEAD~2 上上个版本）

 

git log             查看所有产生的 commit 记录，按 q 推出

git reflog         显示所有历史命令，当你回退到之前的本版之后又想回退回来，但是 git log 已经没有你想要的 commit id 了，那就使用这个命令

git blame 'file'  列出文件由谁修改，修改内容、时间

git congif --global alias.br branch  设置简写（别名）

查看远程分支的 log

git log origin/master

git log remotes/origin/master

git log refs/remotes/origin/master

 

git log -n      查看近 n 条的提交信息

git log --pretty=oneline     简短显示 commit 记录

git log --graph --pretty=oneline --abbrev-commit    查看分支合并情况（简要信息）

git log --graph   以图形化方式查看分支合并情况

git log --pretty=format:"格式"  以某种格式显示提交信息



选项   说明
 %H    提交对象（commit）的完整哈希字串
 %h    提交对象的简短哈希字串
 %T    树对象（tree）的完整哈希字串
 %t    树对象的简短哈希字串
 %P    父对象（parent）的完整哈希字串
 %p    父对象的简短哈希字串
 %an    作者（author）的名字
 %ae    作者的电子邮件地址
 %ad    作者修订日期（可以用 -date= 选项定制格式）
 %ar    作者修订日期，按多久以前的方式显示
 %cn    提交者(committer)的名字
 %ce    提交者的电子邮件地址
 %cd    提交日期
 %cr    提交日期，按多久以前的方式显示
 %s    提交说明



git diff             比较工作区和暂存区的区别，原文件是工作区，目标文件是暂存区

文件 1.txt，新增一行 1，git add . 然后再新增一行 2，用 git diff 比较，分析如下：

![因 为 比 较 的 是 同 一 个 文 件 的 工 作 区 和 皙 存 区 的 区 别 。  所 以 要 用 不 同 的 代 号 来 示  C ： \Use• s\cc\Desktop\1  (master)  ； git difS  diff --git a/1.txt b/1.txt  index d99491f 一 1191247 199644  a/1.txt  示 原 始 文 件 从 第 一 行 到  + + + b/1 ． txt  第 一 行 （ 最 后 一 行  1  就 了 苎 标 文 件 ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

git diff HEAD          比较工作区与 HEAD（当前分支，最新提交）的区别，原文件是工作区，目标文件是暂存区

git diff --cached     比较暂存区和 HEAD（当前分支，最新提交）的区别，原文件是暂存区，目标文件是最新提交

git diff 分支名         比较当前分支和(分支名)之间的区别

git diff commit_id    与某一次提交进行比较

 

**分支**

git branch        查看当前在哪个分支里，前面有个 * 号

git branch a     创建 a 分支

git checkout a  切换到 a 分支

git checkout -b a   创建并切换到 a 分支 

git checkout commit_id    切换到某个历史提交点（此时处于在游离的分支上的状态），在这个游离的分支上可以修改、提交、切换分支，切换之后会有如下提示：

执行 git branch <new-branch-name> commit_id 会创建一个新的分支

![mygit  git checkout master  Warning: you are leaving 1 camit behind, not connected to  any of your branches:  61e5936 my camit  If you want to keep it by creating a new branch,  to do so with:  pit branch 61e5936  Switched to branch 'master '  this may be a good time ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image003.png)

git branch -d a   删除 a 分支

git branch -D a        强制删除 a 分支，有些时候可能会删除失败，比如如果 a 分支的代码还没有合并到 master，你执行 git branch -d a 是删除不了的，它会智能的提示你 a 分支还有未合并的代码，但是如果你非要删除，那就执行 git branch -D a 就可以强制删除 a 分支

git push origin --delete dev  删除远程 dev 分支

git push origin :dev   删除远程 dev  分支

 

git merge a      把 a 分支合并到当前分支，需要先切换到当前分支，然后再执行此操作

![计算机生成了可选文字: ·HEAD指向的是当前分支 当前分支是master, 所以HEAD指向master ·master指向提交 HEAD](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)

 

![一 次 合 并  创 建 了 新 分 支 dev  'GAD  这 种 台 并 称 为 ” 快 进 “  (fastforward)  在 dev 重 憷 交 了 一 次  因 为 没 有 任 何 的 冲 突  吧 dev 台 并 到 master  HEAD ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)

 

如果 master 和 dev 分支修改了同一个地方：把 dev 合并到 master 上时，会出现冲突，需手动解决，解决之后，执行 git status 查看状态，需要 git add "file"，然后 git commit 就行

再回到 dev 分支上，此时 dev 分支上的内容一直没变过，但已经落后 master 一次了，需要把 master 分支合并到 dev，即在 dev 分支上执行 git merge master，会出现快进（fast-forward）

![rygit cit: (rmster) X git status  On branch master  You have umerged paths.  (fix conflicts and run "gif carnit")  (use "git add to mrk resolution)  Umnerged paths:  both rodified:  test2. txt  no changes added to camit (use "git add" and/or  —B mygit  git add test2.txt  mygit  g it status  On branch master  All conflicts fixed byt you are still merging.  (use "git camit" to conclude merge)  nothing to ccmnit, vorking directory clean  mygit  git camit  [master 551b5ddJ krge branch 'dev'  nygit  ngit comit ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image006.png)

 

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

 

两人修改了相同的地方，其中一个人先 push，另一个人一般是先拉取分支，如果这个人有文件没有 commit 的话，会提示先 commit，不然无法 pull，commit 之后，再 pull 的话，会提示有冲突，冲突内容已经写入到冲突文件中了，需要先解决冲突，完了之后 git add . 再 git commit（不加任何东西）会进入编辑模式，默认就行，此时，本地分支已经比远程分支多了两次提交，最后执行 git push  就行，这个人的文件就是最新的，先 push 的那个人需要 git pull 拉取最新的内容

 

**把分支推送到远程**

git push --set-upstream origin dev（等价于 git push -u origin dev）如果想把本地 dev 推送到远程的 dev2，就需要 git push --set-upstream origin dev:dev2，但是在执行 git push 的时候会报错，提示本地分支和远程分支不同名，解决办法：写全 git push 的命令，完整写法是 git push origin src:dst，这里需要写成 git push origin HEAD:dev2（HEAD 指当前分支，也可以写成 dev）,如果本地和远程分支同名，完整的写法是 git push origin dev，把远程分支省略掉了，进而更简洁的写法是 git push

 

**把远程分支拉取到本地**

把 dev 分支推送到远程，另一个人 git pull 的时候，会把 origin/dev 拉取下来，但是本地并没有 dev 分支，想要创建本地 dev 分支，需要执行

git branch dev origin/dev（等价于 git checkout --track origin/dev，在本地创建一个跟远程分支名字一样的分支，若像创建一个与远程分支名不一样，就用前面的）意思是创建一个追踪远程 dev 的本地 dev

 

一个人删除了一个远程分支，另一个人使用 git remote show origin 查看远程仓库的信息，会出现下面情况（本地 master，dev，test，远程master，dev，test，删除远程 dev）

![C: \Users\cc\Desktop\2 (dev)  git remote show origin  remote origin  Fetch URL: https://github.com/leaicc/l.git  Push URL: https://github.com/leaicc/l.git  HEAD branch: master  Remote branches:  master  tracked  refs/remotes/origin/dev stale (use git remote prune' to remove)  test  tracked  gii: pfLlne Ofigin  Local branches configured for git pull •  dev  merges with remote dev  master merges with remote master  test merges with remote test  Local refs configured for git push' •  master pushes to master (up to date)  pushes to test  (up to date)  test ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image007.png)

 

**保存和恢复**

git stash        把当前工作现场“储藏”起来，等以后恢复现场后继续工作（使用场景：在一个分支上开发，突然要切换到另一个分支上，而目前这个分支还没开发完，没法提交，但是不提交的话就无法切换分支，这时候可以用 git stash 暂存起来）

git stash save 'hello'    save 后面可以加信息

git stash list       查看储藏起来的列表

git stash apply   （只保存一个工作现场，多个的话需要加参数）恢复储藏的列表，恢复后，stash 内容并不删除

git stash drop    （只保存一个工作现场，多个的话需要加参数）删除储藏起来的列表

git stash pop      合并上面两步操作

![• git stash apply (stash18ö#ÖhffIJßR, stash drop  • git stash pop  • git stash apply stash@{0} ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image008.png)

 

**g****it** **标签**

![Gitfi  • (lightweight)  (annotated)  git tag  git tag -I  • git tag VI.O.I  • git tag -a VI ,0.2 -m 'release version'  • git tag -d tag _ name ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image009.png)

git show v1.0      显示标签内容

一个标签对应两个 id，一个是标签本身的 id，另一个是标签对应的分支提交点的 commit id

 

标签推送到远程：

git push origin v1.0 

git push origin refs/tags/v1.0:refs/tags/v1.0  完整写法

git push origin v1.0 v2.0 v3.0  批量推送标签

git push origin --tags    一次性推送所有标签

只拉取标签

git fetch origin tag v1.0

删除远程标签：

git push origin :refs/tags/v1.0

git push origin --delete tag v1.0

 

.gitignore

![gitignore  。 * 、 a # 忽 略 所 有 a 结 尾 的 文 件  。 !lib a # 但 lib.a 除 外  · /TODO # 仅 仅 忽 略 项 目 根 目 录 下 的 TODO 文 件 ， 不 包 括  subdir/TODO  / 0d0 忽 子 目 录 下 的 todo 艾 件  /**/todo 忽 根 目 录 下 所 有 的 todo 艾 件  。 build/ # 忽 略 build/ 目 录 下 的 所 有 文 件  。 doc/* txt # 会 忽 略 doc/notes.txt 但 不 包 括 doc/server/  arch.txt  doc/*/*.txt 忽 所 有 子 目 录 下 的 t × t 艾 件  doc/**/*.txt 忽 doc 目 录 下 的 所 有 的 t × t 艾 件 ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image010.png)

 

git gc  垃圾回收

 

git rebase

![origin  merge  02  mywork ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image011.png)

 

![rebase  origin  mywork ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image012.png)

 

![· rebase 过 程 中 也 会 出 现 冲 突  · 解 决 冲 突 后 ， 使 用 git add 添 加 ， 然 后 执 行  cit rebase 一 一 continue  ． 接 下 来 Git 会 继 续 应 用 余 下 的 补 丁  · 任 何 时 候 都 可 以 通 过 如 下 命 令 终 止 rebase ， 分 支 会 帧 复 到  rebase 开 始 前 的 状 态  Cit rebase 一 一 abort ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image013.png)

 

![· 不 要 对 master 分 支 执 行 rebase, 否 则 会 引 起 很 多  问 题  一 般 来 说 ， 执 行 rebase 的 分 支 都 是 自 己 的 本 地 分  支 ， 没 有 推 送 到 远 程 版 本 库 ](file:///C:/Users/cc/AppData/Local/Temp/msohtmlclip1/01/clip_image014.png)