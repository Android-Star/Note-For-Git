# Note-For-Git
记录一下git学习使用过程中的知识点以及难点，方便以后回顾复习
## 本地仓库

### 创建仓库：
* 1.首先创建一个空的目录
* 2.控制台跳转到该目录 cd+目录路径
* 3.$ git init
当前目录下会多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
如果你没有看到.git目录，那是因为这个目录默认是隐藏的。


#### 添加文件到Git仓库，分两步：
* 第一步，使用命令$ git add <fileName>，注意，可反复多次使用，添加多个文件；
* 第二步，使用命令$ git commit，完成。

#### 常用命令
* $ git add <fileName>
* $ git commit -m "message"  （引号中填写提交信息）
* $ git status  查看仓库当前的状态
* $ git diff <fileName> 查看difference
* $ git diff HEAD -- <fileName>  查看工作区和版本库里面指定文件区别
  ##### 如果$ git status告诉你有文件被修改过，用$git diff可以查看具体修改内容。

 -----------------------------------------------------------------------------

 ### 版本回退（适用于已经commit但是还没有push的情况）
 
   ##### 每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。版本控制系统Git中，我们用git log命令查看历史提交记录.
   ##### Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
   
 #### 常用命令
 * $ git log  显示从最近到最远的提交日志
 * $ git log --pretty=oneline  单行显示信息
 * $ git reset --hard HEAD^  把当前版本回退到上一个版本


   ##### 从老版本回到新版本必须知道新版本的commitId，然后使用命令：
 * $ git reset --hard commitId
   ##### 如果忘记提交版本的commitId可以使用命令$ git reflog来查看自己的每次操作。

 #### 小结
 * HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
 * 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
 * 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

-------------------------------------------------------------------------------------------------------
 ### 撤销修改
 
 #### 常用命令
 * $ git checkout -- <fileName>  丢弃工作区的修改(此时文件还没被添加到暂存区)：
   ###### 命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令。
 * $ git reset HEAD <fileName>  丢弃暂存区的修改,回到工作区状态

 #### 小结
 * 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
 * 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步：
      ###### 第一步：用命令git reset HEAD file，就回到了场景1；
      ###### 第二步：按场景1操作。
 * 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。
--------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------

## 远程仓库

### 创建仓库
   ##### 在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果没有创建SSH Key：$ ssh-keygen -t rsa -C "youremail@example.com"
 
 #### 常用命令
 * $ ssh-keygen -t rsa -C "youremail@example.com"  创建SSH Key
 * $ git remote add origin ssh/https地址  本地仓库关联远程仓库
 * $ git push -u origin master  推送本地master到远端master并关联本地master和远端的master
 * $ git push origin master  推送本地master到远端master
   #### 把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

 #### 小结
 * 要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
 * 关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
 * 此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

---------------------------------------------------------------------------------------------------
### 从远程库克隆
 #### 常用命令
$ git clone ssh/https地址  克隆一个本地库

 #### 小结
 * 要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
 * Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

-----------------------------------------------------------------------------------------------------
### 分支管理
 #### 常用命令
 * $ git branch  查看当前分支
 * $ git checkout -b 【分支名称】 创建并切换新分支，-b参数表示创建并切换
   ##### git checkout命令加上-b参数表示创建并切换，相当于以下两条命令
 * $ git branch 【分支名称】
 * $ git checkout 【分支名称】
 * $ git merge【分支名称】 合并指定分支到当前分支

   ##### 合并分支时候，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
   ##### 如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

 * $ git merge --no-ff -m "【msg】"【分支名称】
   ##### 请注意--no-ff参数，表示禁用Fast forward,因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

 * $ git branch -d【分支名称】  删除分支

 #### 小结
  ##### Git鼓励大量使用分支：

 * 查看分支：git branch

 * 创建分支：git branch <name>

 * 切换分支：git checkout <name>

 * 创建+切换分支：git checkout -b <name>

 * 合并某分支到当前分支：git merge <name>

 * 删除分支：git branch -d <name>

-----------------------------------------------------------
### 解决冲突

  ##### 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

  ##### 用git log --graph命令可以看到分支合并图。

----------------------------------------------------------

### 贮藏（stash）（stash贮藏适用于当前分支任务没有commit但是有新任务需要开始做）

 #### 常用命令
 * $ git stash  stash当前工作区内容
 * $ git stash list  查看stash列表
 * $ git stash apply【使用list命令看到的某个stash标识】  恢复stash(恢复后，stash内容并不删除，使用git stash list还可以查看到)
 * $ git stash drop 【使用list命令看到的某个stash标识】  删除stash
 * $ git stash pop 【使用list命令看到的某个stash标识】  恢复并删除stash

  ##### 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

  ##### 当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

-------------------------------------------------------------------
### 删除分支
 #### 常用命令
 * 删除分支：$ git branch -d【分支名称】
 * 删除未合并的分支：$ git branch -D【分支名称】
 * 查看远程仓库信息：$ git remote
 * 查看远程仓库详细信息：$ git remote -v
 * 克隆远端分支到本地并切换到该分支：$ git checkout -b dev origin/dev

--------------------------------------------------------------------

### 多人协作

 #### 多人协作的工作模式通常是这样：

 * 1.首先，可以试图用git push origin branch-name推送自己的修改；

 * 2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

 * 3.如果合并有冲突，则解决冲突，并在本地提交；

 * 4.没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
 ##### 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to=origin/branch-name branch-name。

 #### 小结
 * 查看远程库信息，使用git remote -v；
 * 本地新建的分支如果不推送到远程，对其他人就是不可见的；
 * 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
 * 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
 * 建立本地分支和远程分支的关联，使用git branch --set-upstream-to=origin/branch-name branch-name；
 * 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

------------------------------------------------------------------------------------------------------------------

### 标签
 ##### tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
 #### 常用命令
 * 查看所有标签：$ git tag
 * 打标签：$ git tag <name>
 * 查看标签信息：$ git show <tagname>
 * 创建带有说明的标签，用-a指定标签名，-m指定说明文字 $ git tag -a <name> -m "msg" <commitId>
 ##### 默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的commit id，然后打上就可以了：
 * 查看历史提交版本：$ git log --pretty=oneline --abbrev-commit
 * 对某次提交打tag：$ git tag <name> <commitId>
 ##### 注意，标签不是按时间顺序列出，而是按字母排序的。

 #### 小结
 * 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
 * git tag -a <tagname> -m "blablabla..."可以指定标签信息；
 * 命令git tag可以查看所有标签。


------------------------------------------------

### 操作标签：
 #### 常用命令
 * 删除标签：$ git tag -d <标签名字>
   ##### 因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
 * 推送某个标签到远程  $ git push origin <tagname>

 * 推送全部尚未推送到远程的本地标签  $ git push origin --tags

 ##### 要删除远程标签:
 * 1.从本地删除：  $ git tag -d <标签名字>
 * 2.从远程删除:  $ git push origin :refs/tags/<标签名字>

 #### 小结
 * 命令git push origin <tagname>可以推送一个本地标签；
 * 命令git push origin --tags可以推送全部未推送过的本地标签；
 * 命令git tag -d <tagname>可以删除一个本地标签；
 * 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。



------------------------------------------------------------------------

### 忽略特殊文件：

 ##### 在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
 ##### 不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[开发必备](https://github.com/github/gitignore)

 ##### 有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了，如果你确实想添加该文件，可以用-f强制添加到Git：

 #### $ git add -f filename

 ##### 可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用git check-ignore命令检查：

 #### $ git check-ignore -v filename
 ##### Git会告诉我们，.gitignore的第几行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

