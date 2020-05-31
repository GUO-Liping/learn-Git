# Git Commonds Collection  ---Created by Liping GUO

## 0 General command

```
$ cd						# change directory
$ cd ..						# return to the parent directory
$ pwd						# print working directory
$ ls						# list
$ mkdir forder				# create a new directory named "forder"
$ touch file.file			# create a new file named "file.file"
$ rm file.file				# delete file named "file.file"
$ rm -r forder				# delete a forder named "forder" (-r: recusive递归)
$ mv file.file forder		# move file.file to the forder
$ reset						# clear Git Bash window
```



## 1 Git Configuration
配置github的ssh密钥:

(1)打开Git Bash查看电脑上是否已经存在SSH密钥：
输入 
```Git
cd ~/.ssh；
```
(2)创建新的ssh key:
输入 
```Git
ssh-keygen -t rsa -C "your_email@youremail.com" 
```
一路按Enter直至Bash不再询问，会在 C:\Users\0\.ssh目录生产两个文件：id_rsa和id_rsa.pub；

（3）复制ssh key到github
用记事本打开.ssh目录下的id_rsa.pub文件，复制里面的内容，登陆GitHub账号，依次点击Click “Account Settings” > Click “SSH Keys” > Click “Add SSH key”，自定义输入ssh key的名称，然后粘贴id_rsa.pub中的内容；（打开github网站，点击右上角扳手图标，然后点击左边菜单的 ssh key， 然后右边页面的 add ssh key，将复制的内容粘贴到github的key中，title可以不填，直接保存即可。）

（4）测试 ssh连接github：
输入
```Git
ssh -T git@github.com，出现success表示连接成功
```

### 1.1 Introduce yourself to Git

```Git
$ git config --global user.name "GUO-Liping"
$ git config --global user.email liping0_0@my.swjtu.edu.cn
```

### 1.2 Open Git Manual Page in Browser

```Git
$ git help log
```



## 2 Creating a new  repository

### 2.1 meaning of add, commit, reset, checkout            
​								>>>>>>>>>>>>>>>>>>>>>>>>>>*can not* >>>>>>>>>>>>>>>>>>>>>>>>>>

​										>>>*git add files*					>>>*git commit*						>>> *push*

**Working Directory**----------------------**Stage (Index)**----------------------**Repository**----------------------**Remote**

​									<<<*git reset -- files*			<<<*git checkout -- files*			<<<*fetch/clone*

​								<<<<<<<<<<<<<<<<<<<<<<<<<<< *pull* <<<<<<<<<<<<<<<<<<<<<<<<<<<<<

### 2.2 Creating a new  repository from a tarball

(1) Switch to working directory. Importing existed project to Git revision control.

```Git
$ cd D:
$ cd "program files"
$ cd github
$ cd gitSkills
```

(2) Initial the working directory

```Git
$ git init
Initialized empty Git repository in .git/
```

(3) Add, commit, reset, checkout your created project files

```
$ git add gitSkills.md
$ git commit -m "created gitSkills.md by Liping GUO"
```

### 2.3 Creating a new  repository from a remote repository

(1) Switch again to the working directory which you want to clone a new repository in it

```Git
$ cd D:
$ cd "program files"
$ cd github
```

(2) clone a new repository from remote GitHub website into current working directory-->

```
$  git clone git@github.com:GUO-Liping/testGit.git
```



## 3 Managing branches   ---   Git Flow

#### master 分支

•主分支，永远处于稳定状态，对应当前线上版本•以 `tag` 标记一个版本，因此在 `master` 分支上看到的每一个 `tag` 都应该对应一个线上版本•不允许在该分支直接提交代码

#### develop 分支

•开发分支，包含了项目最新的功能和代码，所有开发都依赖 `develop` 分支进行•小的改动可以直接在 `develop` 分支进行，改动较多时应该切出新的 `feature` 分支进行

> 注：更好的做法是 `develop` 分支作为开发的主分支，也不允许直接提交代码。小改动也应该以 `feature` 分支提 merge request 合并，目的是保证每个改动都经过了强制代码 review，降低代码风险

#### feature 分支

•功能分支，开发新功能的分支•开发新的功能或者改动较大的调整，从 `develop` 分支切换出 `feature` 分支，分支名称为 `feature/xxx`•开发完成后合并回 `develop` 分支并且删除该 `feature/xxx` 分支

#### release 分支

•发布分支，新功能合并到 `develop` 分支，准备发布新版本时使用的分支•当 `develop` 分支完成功能合并和部分 bug fix，准备发布新版本时，切出一个`release` 分支，来做发布前的准备，分支名约定为 `release/xxx`•发布之前发现的 bug 就直接在这个分支上修复，确定准备发版本就合并到 `master` 分支，完成发布，同时合并到 `develop` 分支

#### hotfix 分支

•紧急修复线上 bug 分支•当线上版本出现 bug 时，从 `master` 分支切出一个 `hotfix/xxx` 分支，完成 bug 修复，然后将 `hotfix/xxx` 合并到 `master` 和 `develop` 分支(如果此时存在 `release`分支，则应该合并到 `release` 分支)，合并完成后删除该 `hotfix/xxx` 分支

以上就是在项目中应该出现的分支以及每个分支功能的说明。其中稳定长期存在的分支只有 `master` 和 `develop` 分支，别的分支在完成对应的使命之后都会合并到这两个分支然后被删除。



```
简单总结
master: 线上稳定版本分支
develop: 开发分支，衍生出feature 分支和 release 分支
release: 发布分支，准备待发布版本的分支，存在多个，版本发布之后删除
feature: 功能分支，完成特定功能开发的分支，存在多个，功能合并之后删除
hotfix: 紧急热修复分支，存在多个，紧急版本发布之后删除
```

**master**					**o**----------------------**o**----------------------**o**----------------------**o**

hotfixes					----------**o**-------------------------**o**-----------------------**o**---------

release branches	------------------------**o**----------------------**o**--------------**o**-------

**develop**					----**o**---------------**o**-----**o**---**o**---**o**---**o**--------**o**----------**o**------

feature branches	**o**----------------------**o**----------------------**o**----------------------**o**

### 3.1 Git branches operation

```Git
$ cat gitSkills.md			# show this file in detail
$ git branch				# list all local branches in this repository
$ git branch newBranch		# create branch "newBranch" starting at current HEAD
$ git switch newBranch		# switch working directory to branch "newBranch"
$ git branch -d newBranch	# delete branch "newBranch". can not delete current branch
$ git switch -c new			# create and switch to a branch named "new" at the same time 
```

### 3.2 Update and examine branches form the repository you coloned

```
$ git fetch					# update from remote to depository
$ git branch -r				# show  list of branch from remote github website
$ git branch -a				
# show list of branches form both remote and current directory

$ git switch -c masterwork origin/master			
# create and switch  new branch named "masterwork" to remote at the same time
```

### 3.3 Fetch a branch from a different repository, and give it a new name in your repository

```
$ git fetch git://example.com/project.git theirbranch:mybranch
$ git fetch git://example.com/project.git v2.6.15:mybranch
```

### 3.4 Keep a list of repositories you work with regularly

```Git
$ git remote add origin git@github.com:GUO-Liping/gitSkills.git			
# connect current depository to remote origin

$ git push -u origin master						# first time push to remote, ”-u” needed
$ git push origin								# push everything
$ git remote show origin
  Fetch URL: git@github.com:GUO-Liping/gitSkills.git
  Push  URL: git@github.com:GUO-Liping/gitSkills.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)

$ git fetch origin		 # update branches from example
$ git pull origin master # pull remote repositories to origin
$ git branch -r			 # list all remote branches
```



## 4 Exploring history

### 4.1 log and diff command

```
$ gitk                      # visualize and browse history
$ git log                   # list all commits
$ git log src/              # ...modifying src/
$ git log v2.6.15..v2.6.16  # ...in v2.6.16, not in v2.6.15
$ git log master..test      # ...in branch test, not in branch master
$ git log test..master      # ...in branch master, but not in test
$ git log test...master     # ...in one branch, not in both
$ git log -S'foo()'         # ...where difference contain "foo()"
$ git log --since="2 weeks ago"
$ git log -p                # show patches as well
$ git show                  # most recent commit
$ git diff v2.6.15..v2.6.16 # diff between two tagged versions
$ git diff v2.6.15..HEAD    # diff with current head
$ git grep "foo()"          # search working directory for "foo()"
$ git grep v2.6.15 "foo()"  # search old tree for "foo()"
$ git show v2.6.15:a.txt    # look at old version of a.txt
```

### 4.2 Search for regressions:

```
$ git bisect start
$ git bisect bad                # current version is bad
$ git bisect good v2.6.13-rc2   # last known good revision
Bisecting: 675 revisions left to test after this
                                # test here, then:
$ git bisect good               # if this revision is good, or
$ git bisect bad                # if this revision is bad.
                                # repeat until done.
```



## 5 Making changes

Make sure Git knows who to blame:

```
$ cat >>~/.gitconfig <<\EOF
[user]
        name = Your Name Comes Here
        email = you@yourdomain.example.com
EOF
```

Select file contents to include in the next commit, then make the commit:

```
$ git add a.txt    # updated file
$ git add b.txt    # new file
$ git rm c.txt     # delete old file c.txt
$ git mv c.txt changeNameC.txt		# change file name "c.txt" to "changeNameC.txt"
$ git commit
```

Or, prepare and create the commit in one step:

```
$ git commit d.txt # use latest content only of d.txt
$ git commit -a    # use latest content of all tracked files
```



## 6 Merging

```
$ git merge test   # merge branch "test" into the current branch
$ git pull git://example.com/project.git master
                   # fetch and merge in remote branch
$ git pull . test  # equivalent to git merge test
$ git merge --no-ff -m "this merge is a normal merge which saved the merge record"
# 以普通模式合并workBranch到master分支，默认模式为Fast-
```



## 7 Sharing your changes

Importing or exporting patches:

```
$ git format-patch origin..HEAD # format a patch for each commit
                                # in HEAD but not in origin
$ git am mbox # import patches from the mailbox "mbox"
```

Fetch a branch in a different Git repository, then merge into the current branch:

```
$ git pull git://example.com/project.git theirbranch
```

Store the fetched branch into a local branch before merging into the current branch:

```
$ git pull git://example.com/project.git theirbranch:mybranch
```

After creating commits on a local branch, update the remote branch with your commits:

```
$ git push ssh://example.com/project.git mybranch:theirbranch
```

When remote and local branch are both named "test":

```
$ git push ssh://example.com/project.git test
```

Shortcut version for a frequently used remote repository:

```
$ git remote add example ssh://example.com/project.git
$ git push example test
```

## 8 Repository maintenance

Check for corruption:

```
$ git fsck
```

Recompress, remove unused cruft:

```
$ git gc
```
