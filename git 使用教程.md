# git 使用教程

####1、安装git

* 查看是否安装git

  ```
  $ git

  The program 'git' is currently not 	installed. You can install it by typing:
  sudo apt-get install git
  ```

* Linux下安装git

  ```
  $ sudo apt-get install git
  ```

* 添加用户名和邮箱

  ```
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"
  ```

####2、git使用

* git初始化

  ```
  $ git init
  Initialized empty Git repository in /Users/.../.../.git/
  ```

* 添加一个文件到git暂存区

  ```
  $ git add fileName
  ```

* 添加该目录下所有文件到git

  ```
  $ git add .
  ```


* 将文件提交到git保存

  ```
  $ git commit -m "提交说明"
  ```

* 直接将文件添加到git保存,不添加到暂存区

  ```
  $ git commit -a
  ```

####3、查看git修改

* 查看git修改状态

  ```
  $ git status
  ```

* 查看git具体修改内容

  ```
  $ git diff
  ```

####4、查看git日志

* 查看log

  ```
  $ git log
  ```

* 查看简介历史记录

  ```
  $ git log - oneline
  ```

* 以图选项查看纪录

  ```
  $ git log - oneline - graph
  ```

* 逆向查看所有日志

  ```
  $ git log -reverse -online
  ```

 * 查找指定用户提交日志

   ```
   $ git log --v  	
   ```


* 回退到上一个版本

  * HEAD为版本号ID
  * 上一个版本就是 **HEAD^**
  * 上上一个版本就是 **HEAD^^**
  * 之后的可以用 **HEAD~100** 表示

  ```
  $ git reset --hard HEAD^
  	 HEAD is now at ea34578 add distributed
  ```

* 丢弃未提交的文件内容

  * 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令	 **git checkout -- file**。
  * 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令 **git reset HEAD file** ，就回到了场景1，第二步按场景1操作。
  * 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

  ```
  $ git checkout -- file
  ```


  ####5、删除文件

  * 在本地删除文件，同步到git，确定要删除时使用如下代码，此时文件才从git中删除

    ```
    $ git rm fileName
    $ git commit -m "remove file"
    ```


  * 强制删除

    ```
    $ git rm -f fileName
    ```


  * 如果删错了，此时因为版本库里还有，所以可以很轻松地把误删的文件恢复到最新版本

    ```
    $ git checkout -- fileName
    ```


  * **git checkout** 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

  ​

  ​

  #### 6、GitHub关联本地 git

  ​

  * 远程库的名字是origin，这是Git默认的叫法，也可以改成别的

    ```
    $ git remote add origin git@github.com:michaelliao/learngit.git
    ```


* 将本地git文件推送到GitHub

  ```
  $ git push -u origin master
  ```

* 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，从现在起，只要本地作了提交，就可以通过命令：

  ```
  $ git push origin master
  ```


* 将branch分支推送到alias远程的branch分支下

  ```
  git push alias branch
  ```
* 删除远程仓库

  ```
  git remote rm projectName
  ```

* 要关联一个远程库，使用命令


* 关联后，使用命令 **git push -u origin master** 第一次推送master分支的所有内容；

* 此后，每次本地提交后，只要有必要，就可以使用命令 **git push origin master** 推送最新修改；

   ```
$ git remote add origin git@server-name:path/repo-name.git
   ```

* 查看当前项目的远程库

  ```
  git remote
  ```
* 提取github项目到本地

  ```
  git fetch origin master
  ```

* 查看当前版本和GitHub上的版本的区别

  ```
  $ git diff origin/master 
  ```

* 本地更新修改

  ```
  git fetch origin
  ```

* GitHub更新内容同步到本地分支

  ```
  git merge origin/master
  ```

* 从远程获取最新版本合并到本地

   ```
   $ git pull origin master
   ```

* 从github克隆项目

  ```
  $ git clone gitAddress
  ```
####7、分支管理

* 创建分支	

  ```
  $ git branch branchName
  ```


* 切换分支

  ```
  $ git checkout branchName
  ```

* 创建分支并切换到该分支下

  ```
  $ git checkout -b branchName
  ```

* 合并某分支到当前分支

  ```
  $ git merge branchName
  ```

* 查看所有分支

  ```
  $ git branch
  ```

* 删除分支

  ```
  $ git branch -d branchName
  ```

####8、git标签

* 创建一个git标签

  ```
  $ git tag -a v1.0
  ```

* 查看git标签

  ```
  $ git log --decorate
  ```

* 给已经提交的纪录(85cbfa65)添加标签

  ```
  $ git tag -a v0.5 85cbfa65
  ```

* 查看所有标签

  ```
  $ git tag
  ```

* 指定标签添加信息

  ```
  $ git tag - tagName -m information
  ```

* 删除标签

  ```
  $ git tag -d v1.0
  ```

* 查看此版本所修改的内容

  ```
  $ git show v1.0
  ```
