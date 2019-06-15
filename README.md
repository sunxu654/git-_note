>之前会用git命令,但原理一知半解,用的时候总感觉心里不自在
>
>所以再把git系统地学一遍


# 本地版本库

## 本地版本库架构

## 版本回溯

# 远程版本库

# 分支管理
## 分支原理
git的创建分支: HEAD指向dev,并继续发展

![](pic/Snipaste_2019-06-14_14-58-51.jpg)

![](pic/Snipaste_2019-06-14_14-59-04.jpg)

git的分支合并: 把master指向了dev,HEAD指向了master也指向了dev
![](pic/Snipaste_2019-06-14_14-59-12.jpg)

![](pic/Snipaste_2019-06-14_14-59-17.jpg)

## 分支创建
```
vpsmaster@ubuntu:~/git$ git checkout -b dev #创建并切换到dev
Switched to a new branch 'dev'
vpsmaster@ubuntu:~/git$ git branch #查看当前的branch
* dev
  master
vpsmaster@ubuntu:~/git$ touch testbranch  #在dev分支上创建一个文件
vpsmaster@ubuntu:~/git$ ll
total 16
drwxrwxr-x  3 vpsmaster vpsmaster 4096 Jun 14 00:03 ./
drwxr-xr-x 29 vpsmaster vpsmaster 4096 Jun 13 23:34 ../
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 13 23:00 file2
drwxrwxr-x  8 vpsmaster vpsmaster 4096 Jun 14 00:02 .git/
-rw-rw-r--  1 vpsmaster vpsmaster   12 Jun 13 23:06 readme.txt
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 14 00:03 testbranch
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 13 23:13 testrm
vpsmaster@ubuntu:~/git$ git add testbranch 
vpsmaster@ubuntu:~/git$ git commit -m "testbranch" 
[dev a8a027e] testbranch
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 testbranch
vpsmaster@ubuntu:~/git$ 

```
```
vpsmaster@ubuntu:~/git$ git checkout  master #切换回master分支
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
vpsmaster@ubuntu:~/git$ git merge dev #把master分支合并
Updating 3b2e026..a8a027e
Fast-forward
 testbranch | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 testbranch
vpsmaster@ubuntu:~/git$ ll #master分支下也拥有了testbranch文件
total 16
drwxrwxr-x  3 vpsmaster vpsmaster 4096 Jun 14 00:06 ./
drwxr-xr-x 29 vpsmaster vpsmaster 4096 Jun 13 23:34 ../
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 13 23:00 file2
drwxrwxr-x  8 vpsmaster vpsmaster 4096 Jun 14 00:06 .git/
-rw-rw-r--  1 vpsmaster vpsmaster   12 Jun 13 23:06 readme.txt
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 14 00:06 testbranch
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 13 23:13 testrm
vpsmaster@ubuntu:~/git$ 
```
## 冲突合并
创建分支dev2,修改了readme.txt
回到master,修改readme.txt
git merge dev2 后产生如下冲突
```
vpsmaster@ubuntu:~/git$ git merge dev2
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.

```
```
<<<<< HEAD
master
=======
dev2
>>>>> dev2
789
456
123

```
修改文件成如下就可以提交了
```
master dev2
789
456
123
```
```
git add readme.txt
git commit -m "merge master and dev2"

```
然后删除dev2
```
vpsmaster@ubuntu:~/git$ git branch -d dev2
Deleted branch dev2 (was e55d265).
```
### 分支策略

分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：
![](pic/Snipaste_2019-06-14_15-55-05.jpg)
## git stash

```
vpsmaster@ubuntu:~/git$ git status
On branch dev3
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	dev4
	dev5

no changes added to commit (use "git add" and/or "git commit -a")
vpsmaster@ubuntu:~/git$ git stash
Saved working directory and index state WIP on dev3: d66dade dev3
HEAD is now at d66dade dev3
vpsmaster@ubuntu:~/git$ vi readme.txt 
vpsmaster@ubuntu:~/git$ ll
total 20
drwxrwxr-x  4 vpsmaster vpsmaster 4096 Jun 14 01:29 ./
drwxr-xr-x 29 vpsmaster vpsmaster 4096 Jun 13 23:34 ../
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 14 01:20 dev3
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 14 01:21 dev4
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 14 01:27 dev5
drwxrwxr-x  2 vpsmaster vpsmaster 4096 Jun 14 01:27 dev6/
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 13 23:00 file2
drwxrwxr-x  8 vpsmaster vpsmaster 4096 Jun 14 01:29 .git/
-rw-rw-r--  1 vpsmaster vpsmaster   24 Jun 14 01:29 readme.txt
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 14 00:06 testbranch
-rw-rw-r--  1 vpsmaster vpsmaster    0 Jun 13 23:13 testrm
vpsmaster@ubuntu:~/git$ git stash pop
On branch dev3
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	dev4
	dev5

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (1c9c9f43a70df0a2d15685ba8ef323ff403e4672)
vpsmaster@ubuntu:~/git$ vi readme.txt 

```
### 开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
# 标签管理





