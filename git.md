# git

## 1. 参考

 1. https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496
## 2.命令
1. `--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，

   ```
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   ```

   

2. 版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

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

3. 文件添加到版本库,一定要放到目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

   把一个文件放到Git仓库只需要两步:

   1. `git add`告诉Git，把文件添加到仓库：git add readme.txt

   2. `git commit`告诉Git，把文件提交到仓库：`-m`后面输入的是本次提交的说明，最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

      ```
      $ git commit -m "wrote a readme file"	
      [master (root-commit) eaadf4e] wrote a readme file
       1 file changed, 2 insertions(+)
       create mode 100644 readme.txt
      ```

      

   

4. 


## 3.sdf
1. 
