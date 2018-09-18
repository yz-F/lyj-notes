
### 1. 管理修改（add与commit）
#### 第一步，为什么说Git管理的是修改，而不是文件呢？对readme.txt做一个修改，比如加一行内容：
    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes.
    
>然后,添加

    $ git add readme.txt
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       modified:   readme.txt
    #
>然后，再修改readme.txt：

    $ cat readme.txt 
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.
    
>提交：

    $ git commit -m "git tracks changes"
    [master 519219b] git tracks changes
     1 file changed, 1 insertion(+)  
     
 >提交后，再看看状态：
 
    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
        modified:   readme.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")

> 咦，怎么第二次的修改没有被提交？

>**第一次修改 -> git add -> 第二次修改 -> git commit**

>**你看，我们前面讲了，Git管理的是修改，当你用git add命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交**
     
> 提交后，用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别：

    $ git diff HEAD -- readme.txt 
    diff --git a/readme.txt b/readme.txt
    index 76d770f..a9c5755 100644
    --- a/readme.txt
    +++ b/readme.txt
    @@ -1,4 +1,4 @@
     Git is a distributed version control system.
     Git is free software distributed under the GPL.
     Git has a mutable index called stage.
    -Git tracks changes.
    +Git tracks changes of files.
    
>可见，第二次修改确实没有被提交。

>**那怎么提交第二次修改呢？你可以继续git add再git commit，也可以别着急提交第一次修改，先git add第二次修改，再git commit，就相当于把两次修改合并后一块提交了**

    第一次修改 -> git add -> 第二次修改 -> git add -> git commit
    
>**现在，你又理解了Git是如何跟踪修改的，每次修改，如果不用git add到暂存区，那就不会加入到commit中。** 

>10:00-11:00<br>   
### 2. 撤销修改（git checkout）
#### 第一步，既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用git status查看一下：

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
        modified:   readme.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")
    
 >你可以发现，Git会告诉你，git checkout -- file可以丢弃工作区的修改：
 
     $ git checkout -- readme.txt
     
>**命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

- 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

- 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

- 总之，就是让这个文件回到最近一次git commit或git add时的状态。**     
    
>现在，看看readme.txt的文件内容：

    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.
    
>文件内容果然复原了。

>git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。

#### 第二步，你不但写了一些胡话，还git add到暂存区了：：

    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.
    My stupid boss still prefers SVN.
    
    $ git add readme.txt
    
>庆幸的是，在commit之前，你发现了这个问题。用git status查看一下，修改只是添加到了暂存区，还没有提交：

    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
        modified:   readme.txt
        
>Git同样告诉我们，用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区：

>**$ git reset HEAD readme.txt
Unstaged changes after reset:
M    readme.txt**

>再用git status查看一下，现在暂存区是干净的，工作区有修改：

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
        modified:   readme.txt
        
>还记得如何丢弃工作区的修改吗？

    $ git checkout -- readme.txt
    
    $ git status
    On branch master
    nothing to commit, working tree clean
>又到了小结时间。

- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。


### 3. 删除文件（add与commit）
#### 第一步，一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

    $ rm test.txt
    
>这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：

     $ git status
    On branch master
    Changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
        deleted:    test.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")
    
> 现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

    $ git rm test.txt
    rm 'test.txt'
    
    $ git commit -m "remove test.txt"
    [master d46f35e] remove test.txt
     1 file changed, 1 deletion(-)
     delete mode 100644 test.txt
     
>现在，文件就从版本库中被删除了.

>另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

    $ git checkout -- test.txt
    
>**命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。**    
<br>
### 4. 添加远程库（将本地提交到github上）
#### 你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作：
    
>登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库

>在Repository name填入learngit，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库(千万不要加readme)


#### 你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作：把本地仓库的内容推送到GitHub仓库

>现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：

    $ git remote add origin git@github.com:michaelliao/learngit.git
    
    
>添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

>下一步，就可以把本地库的所有内容推送到远程库上：

    $ git push -u origin master
    Counting objects: 20, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (15/15), done.
    Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
    Total 20 (delta 5), reused 0 (delta 0)
    remote: Resolving deltas: 100% (5/5), done.
    To github.com:michaelliao/learngit.git
     * [new branch]      master -> master
    Branch 'master' set up to track remote branch 'master' from 'origin'.
    
    
>**把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。**

#### 从现在起，只要本地作了提交，就可以通过命令：

    $ git push origin master
    
### 5. 从远程库clone（从github上）
#### 你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作：
    
[从远程库clone从github上](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375233990231ac8cf32ef1b24887a5209f83e01cb94b000)
    
    