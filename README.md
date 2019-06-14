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

# 标签管理





