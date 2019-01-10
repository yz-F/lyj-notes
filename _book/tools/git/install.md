
### 1. 创建版本库
#### 第一步，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
    $ mkdir learngit
    $ cd learngit
    $ pwd
    /Users/michael/learngit
    
#### 第二步，通过git init命令把这个目录变成Git可以管理的仓库：
    $ git init
    Initialized empty Git repository in /Users/michael/learngit/.git/
    
#### 第三步，创建一个readme.txt
    $ git add readme.txt//用命令git add告诉Git，把文件添加到仓库
    
    $ git commit -m "wrote a readme file"//用命令git commit告诉Git，把文件提交到仓库
    [master (root-commit) eaadf4e] wrote a readme file
     1 file changed, 2 insertions(+)
     create mode 100644 readme.txt
> **为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件**   

### 2. 时光穿梭机(status)
#### 第一步，修改readme.txt，运行git status命令看看结果：
    $ git status
    On branch master
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
    
        modified:   readme.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")
 

> **git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改** 

#### 运行git diff命令看看具体修改了什么内容：
    $ git diff readme.txt 
    diff --git a/readme.txt b/readme.txt
    index 46d49bf..9247db6 100644
    --- a/readme.txt
    +++ b/readme.txt
    @@ -1,2 +1,2 @@
    -Git is a version control system.
    +Git is a distributed version control system.
     Git is free software.

#### 运行git add ，git commit之前运行git status：
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
        modified:   readme.txt
        
> **git status告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了：**        

      $ git commit -m "add distributed"
    [master e475afc] add distributed
     1 file changed, 1 insertion(+), 1 deletion(-)      
 提交后，我们再用git status命令看看仓库的当前状态：
 
     $ git status
    On branch master
    nothing to commit, working tree clean
    
>**Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。**    
### 3. 版本回退
>commit像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。
#### 第一步，现在，我们回顾一下readme.txt文件一共有几个版本被提交到Git仓库里了：
版本1：wrote a readme file

    Git is a version control system.
    Git is free software.
版本2：add distributed

    Git is a distributed version control system.
    Git is free software.
版本3：append GPL

    Git is a distributed version control system.
    Git is free software distributed under the GPL.
>版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看：

    $ git log
    commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:06:15 2018 +0800
    
        append GPL
    
    commit e475afc93c209a690c39c13a46716e8fa000c366
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:03:36 2018 +0800
    
        add distributed
    
    commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 20:59:18 2018 +0800
    
        wrote a readme file
>如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

    $ git log --pretty=oneline
    1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
    e475afc93c209a690c39c13a46716e8fa000c366 add distributed
    eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
    
 #### 第二步，现在我们启动时光穿梭机，准备把readme.txt回退到上一个版本，也就是add distributed的那个版本，怎么做呢？   
 >首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
 
 >现在，我们要把当前版本append GPL回退到上一个版本add distributed，就可以使用git reset命令：
 
     $ git reset --hard HEAD^
    HEAD is now at e475afc add distributed
    
>不过且慢，然我们用git log再看看现在版本库的状态：

    $ git log
    commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:03:36 2018 +0800
    
        add distributed
    
    commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 20:59:18 2018 +0800
    
        wrote a readme file
        
>**最新的那个版本append GPL已经看不到了**    

>办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是1094adb...，于是就可以指定回到未来的某个版本：

    $ git reset --hard 1094a
    HEAD is now at 83b0afe append GPL
    
 #### 第三步，现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？
 >在Git中，总是有后悔药可以吃的。当你用$ git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令：
 
     $ git reflog
    e475afc HEAD@{1}: reset: moving to HEAD^
    1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
    e475afc HEAD@{3}: commit: add distributed
    eaadf4e HEAD@{4}: commit (initial): wrote a readme file
    
>**从输出可知，append GPL的commit id是1094adb，现在，你又可以乘坐时光机回到未来了。** 

>**总结**
- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。


- 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

- 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。


### 4. 工作区和暂存区
[工作区和暂存区](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013745374151782eb658c5a5ca454eaa451661275886c6000)
>Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
