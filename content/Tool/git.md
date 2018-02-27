---
title: "git"
layout: page
date: 2016-03-18 07:00:00
collection: "[tool]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

> 工程的经验来源于场景和思索实践.

---

[TOC]
###驱动
>最重要的是要理解无论是stage还是commit都是对modify的累积合并.

###场景
- [场景1]在本地写了一部分代码，如何上传到空空的远程git上.
```c
1. 对本地代码仓库化
    git init .
2. 在远端创建仓库,使用网页创建即可.
3. 设置本地仓库的remote
   git remote add origin https://****
4. 将本地仓库与远程仓库合并
    git pull origin master
5. 提交本地仓库到远程分支
    git add .
    git commit -m "*****"
    git push -u origin master
[背景知识]
git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。

$ git pull <远程主机名> <远程分支名>:<本地分支名>
比如，取回origin主机的next分支，与本地的master分支合并，需要写成下面这样。

$ git pull origin next:master
如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

$ git pull origin next
```


###常用
- 查看版本库(HEAD)与当前工作区文件的diff
    `git diff HEAD -- file.txt`
- 把未stage的修改进行撤销,即丢弃工作区的修改
    `git checkout -- file.txt`
- 对当前stage区域进行unstage,即删除暂存的修改
    `git reset HEAD file.txt`
- git stash
    `[场景]:当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交,并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？`
    `[含义]这个命令涉及到对应用场景和知识背景的掌握,同一个本地仓库开发下的多个分支共用同一个工作区和stage,一个分支下的工作区和stage在切换的时候会扰乱另一个工作区和stage,为什么我们担心工作区和stage被扰乱呢?因为一旦add和commit,就会把其他分支的stage一并commit了,这样分支之间就混乱了.`
    `[注意]这是一个很有用的命令,但是没有被stage或者head的文件，是无法被stash的,这一句话可能说的有误解,简单说一下,只要是git track的文件都可以被stash,即工作区里的modify文件和stage的暂存文件都被stash到栈中了。未被stash会将工作区的modify和stage中的暂存显示在各个分支里。让你迷惑它到底该属于哪个分支。`
- git tag
```
git tag很不错
➜  pics git:(master) git tag <name>(针对当前分支的最新commit打tag)

[a]. 如果想对历史commit打tag
    1. git log --pretty=oneline --abbrev-commit(查找commit hash)
    2. git tag v0.1 <hash value>
[b]. 查看tag
     git tag

[c]. 可以用git show <tagname>查看标签信息：

[d]. 还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
    $ git tag -a v0.1 -m "version 0.1 released" 3628164
[e]. 通过-s用私钥签名一个标签：
    $ git tag -s v0.2 -m "signed version 0.2 released" fec145a
    签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错.
```


###ref
[1]. [git tag](http://airk000.github.io/git/2013/09/30/git-tag-with-gpg-key)

###FANQ
- 私认为stage的对象是修改,它会对文件的修改进行stage,但是如果想对文件进行stage,前提是对文件进行track.
- 非fast-forward的merge有可能存在冲突,因为分支出现了分叉.
    `解决冲突:用git status定位冲突文件`
- 禁用Fast forward模式
    `git merge --no-ff -m "merge with no-ff" dev`
- 强行删除分支
    `git branch -D branchName`
- 本地分支绑定远程分支
    `git branch --set-upstream-to=origin/<branch> <local branch>`
- tag与commit的关系
    `tag类似域名,commit类似ip`
- 强制添加.gitignore中匹配的文件
    `git add -f filename`
- 当前用户的Git配置文件放在哪?
    `用户主目录下的一个隐藏文件.gitconfig中：`

###最佳实践

1. 分支实践
<center><img src="https://github.com/SrcAgile/pics/blob/master/git.jpg?raw=true"/></center>

2. 配置git
```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

###video

<video width="320" height="240" controls>
    <source src="./1.mp4" type="video/mp4">
您的浏览器不支持video标签。
</video>
      