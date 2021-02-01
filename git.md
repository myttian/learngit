# git

## 1. 参考

 1. https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496
## 2.命令
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

5. 管理修改

   为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

   > 第一次修改 -> `git add` -> 第二次修改 -> `git commit`

   > 你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
   >
   > `git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

6. 



## 3.sdf
1. 
