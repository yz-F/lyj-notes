### 1. 创建版本库

#### 第一步，首先，我们创建dev分支，然后切换到dev分支：
    $ git checkout -b dev
    Switched to a new branch 'dev'
    
#### git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
    $ git branch dev
    $ git checkout dev
    Switched to branch 'dev'

#### 然后，用git branch命令查看当前分支：
    $ git branch
    * dev
    master
  