# git

## 1. 参考

 1. https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496
## 2. 本地仓库
1. `--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，	每个仓库自报家门，当然也可某个仓库指定不同用户名和Email。

   ```
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   ```

   

2. 初始化一个Git仓库，git init                       添加文件到Git仓库，分两步：git add                  git commit -m <message>

   版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

   ```
   $ mkdir learngit
   $ cd learngit
   $ pwd
   /Users/michael/learngit
   ```

   第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：	*瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。*

   ```
   $ git init
   Initialized empty Git repository in /Users/michael/learngit/.git/
   ```

   文件添加到版本库,一定要放到目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

   

   把一个文件放到Git仓库只需要两步:

   1. `git add`告诉Git，把文件添加到仓库：git add readme.txt

   2. `git commit`告诉Git，把文件提交到仓库：`-m`后面输入的是本次提交的说明，最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

      ```
      $ git commit -m "wrote a readme file"	
      [master (root-commit) eaadf4e] wrote a readme file
       1 file changed, 2 insertions(+)
       create mode 100644 readme.txt
      ```

      需要`add`，`commit`一共两步,`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，

      ```
      $ git add file1.txt
      $ git add file2.txt file3.txt
      $ git commit -m "add 3 files."
      ```

   3. git status 查看仓库当前状态，       git diff readme.txt 修改了哪里  git add readme.txt 修改后提交

      如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容

3. 版本回退

   快照在git中叫`commit`,	`git log`显示从最近到最远的提交日志，嫌信息太多，`git log --pretty=oneline`

   类似`1094adb...`是`commit id`（版本号）,SHA1计算出来的一个非常大的数字，用十六进制表示,身份证

   `HEAD`表示当前版本，上个版本是`HEAD^`，上上个版本是`HEAD^^`，上100个`HEAD~100`。

   ```
   $ git reset --hard HEAD^					/回退到上一个版本用git reset
   HEAD is now at e475afc add distributed
   ```

   最新的那个版本已经看不到了，想再回去已经回不去了，只要上面的命令行窗口还没有被关掉，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：`git reset --hard 1094a`

   想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？`git reflog`记录你的每一次命令;

   > - `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
   > - 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
   > - 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

4. 工作区暂存区

   能看到的目录，比如我的`learngit`文件夹就是一个工作区;

   工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

   > 分支和`HEAD`的概念我们以后再讲。
   >
   > 前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
   >
   > 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
   >
   > 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
   >
   > 你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。

5. git管理的是修改

   为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

   > 第一次修改 -> `git add` -> 第二次修改 -> `git commit`

   > 你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
   >
   > `git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

   那怎么提交第二次修改呢？	第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

   每次修改，如果不用`git add`加到暂存区，那就不会加入到`commit`中。

6. 改错了撤销修改

   `git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

   > 【撤销工作区的修改】
   >
   > 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
   >
   > 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
   >
   > 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。
   >
   > `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令

   在`commit`之前，你发现了问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

   `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

   `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。用`HEAD`时，表示最新的版本。  

   

   假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把`stupid boss`提交推送到远程版本库，你就真的惨了

   - 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

   - 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

   - 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。  

     

7. 删除文件

   在Git中，删除也是一个修改操作

   `rm`命令删了，`git status`会立刻告诉你哪些文件被删除了，你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`，文件就从版本库中被删除了。

   另一种情况是删错了，可以很轻松地把误删的文件恢复到最新版本，git checkout -- test.txt

   `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
   
8. 小结

   > ==初始化：git init;   文件入库：git add  1.txt		git commit -m "first file"		git status     git diff 1.txt==
   >
   > 回退：	==git reset --hard HEAD^==		git log		git reflog		git reset --hard commit_id
   >
   > ​			`git diff HEAD -- readme.txt`	查看工作区和版本库里面最新版本的区别
   >
   > 撤销：1.  ==git checkout -- file==	**版本库里的版本替换工作区的版本	改乱工作区，想丢弃工作区的修改**
   >
   > ​			2. 改乱工作区，还添加到暂存区，想丢弃修改，两步，一用`git reset HEAD <file>`，就回到了场				景1，第二步按场景1操作。	作用：暂存区的修改撤销掉（unstage），重新放回工作区
   >
   > ​			3.提交修改到版本库，想要撤销，用版本回退，前提是没有推送到远程库。git reset --hard commit_id
   >
   > 删除：==git rm test.txt==	从版本库中删除文件，用`git rm`删掉，并且`git commit -m "remove test.txt"`

## 3. 远程仓库

1. 添加到远程库

   a. 本地Git库和GitHub仓库间的传输通过SSH加密，创建SSH Key：

   ​	`$ ssh-keygen -t rsa -C "email@example.com"`

   ​	用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，		`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人

   b. 登陆GitHub，打开“Account settings”，“SSH Keys”,“Add SSH Key”，填上任意Title，在Key文本框里粘贴	`id_rsa.pub`内容

   >  关联一个远程库:	git remote add origin git@github.com:myttian/learngit.git
   >
   > 把本地库的所有内容推送到远程库上：git push -u origin master	把本地库的内容推送到远程，用`git push`实际上是把当前分支`master`推送到远程。`git push origin master -f`，强行让本地分支覆盖远程分支
   >
   > https://segmentfault.com/q/1010000002736986

2. 从远程库克隆

   git clone git@github.com:michaelliao/gitskills.git     Git支持多种协议，包括`https`，但`ssh`协议速度最快。

   

## 4. 分支管理

1. 创建合并删除

   > 新建dev指针，HEAD指向dev，表当前分支在dev；
   >
   > master指针指向dev的当前提交，完成合并；
   >
   > 删除dev指针就是删除dev分支；  

   git checkout -b dev 	 `-b`创建并切换	`git branch`查看当前分支

   git checkout master    切换回`master`分支

   `git merge dev`    	合并指定分支到当前分支,`dev`分支的成果合并到`master`分支

   git branch -d dev  		删除`dev`分支

   > Git鼓励大量使用分支：
   >
   > 查看分支：`git branch`
   >
   > 创建分支：`git branch <name>`
   >
   > 切换分支：`git checkout <name>`或者`git switch <name>`
   >
   > 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`
   >
   > 合并某分支到当前分支：`git merge <name>`
   >
   > 删除分支：`git branch -d <name>`

   

2. 解决冲突

   当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

   解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

   用`git log --graph`命令可以看到分支合并图。

3. 分支管理策略

   合并分支时，`Fast forward`模式，删除分支后，会丢掉分支信息。		--no-ff`方式的`git merge

   禁用`Fast forward`，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

   ```
    git merge --no-ff -m "merge with no-ff" dev   
   ```

   因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

   按照几个原则进行分支管理：`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

   干活都在`dev`分支上，你和你的小伙伴每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

4. Bug分支

   bug通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

   Git提供了一个`stash`功能，把当前工作现场“储藏”起来，等以后恢复现场后继续工作；类似3dmax暂存  

   git stash						git stash list

   > Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：		 git stash apply stash@{0}
   >
   > 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；
   >
   > 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：		

   `git cherry-pick 4c805e2`，让我们能复制一个特定的提交到当前分支

5. Feature分支

   每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

   如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

6. 多人协作

   git remote -v 	远程库的信息						推送分支 git push origin master

   创建远程`origin`的`dev`分支到本地			git checkout -b dev origin/dev

   `git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送： 

   git branch --set-upstream-to=origin/<branch> dev

   

7. Rebase

   rebase操作可以把本地未push的分叉提交历史整理成直线；

   rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 3. 标签管理

1. 创建标签

   tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。	比commit id好记。

   git tag v1.0				git log --pretty=oneline --abbrev-commit		git tag v0.9 f52c633		git show <tagname>

   git tag -a v0.1 -m "version 0.1 released" 1094adb

2. 操作标签

   删除：git tag -d v0.1			

   推送标签到远程，`git push origin <tagname>`		推送全部尚未推送远程的：git push origin --tags

   删除远程：先删本地，git tag -d v0.9    再删远程，git push origin :refs/tags/<tagname>	删除一个远程标签

## 3. github

1. 在GitHub上，可以任意Fork开源仓库；

   自己拥有Fork后的仓库的读写权限；

   可以推送pull request给官方仓库来贡献代码。

 
1. 很爱很爱你

