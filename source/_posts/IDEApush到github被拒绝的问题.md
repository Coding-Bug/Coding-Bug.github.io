---
title: IDEApush到github被拒绝的问题
date: 2021-03-22 22:49:21
categories:
- Diary
tags:
- JAVA
---

##  上传时被拒绝

上传代码到github出现rejected的问题是因为新传的文件和原来的不是同一时间上传等，解决办法：

```
git pull --rebase origin master
git push -u origin master -f     //这句不一定要
```


##  删除Github上面的文件夹

先pull（把github上的文件pull下来）
再执行
```
git rm --cached -r "要删除的文件夹"
```
然后再commit，再push就行了。
