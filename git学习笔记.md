# git学习笔记

###在github上创建云端仓库

​	首先就是在github上创建一个存储项目所有代码的仓库。这个非常简单，直接点点点就可以完成。

​	然后进入你创建的仓库，发现是空的，它会提示你输入一些代码来船舰本地仓库和链接云端仓库。

```javascript
// 这条命令是创建一个名为README.md的markdown文件，向里面写入# test文本
echo "# test" >> README.md
// 运行这个命令可以初始化一个git仓库
git init
// 这条命令是将README.md文件推送到暂存区
git add README.md
// 将暂存区的内容推送到本地仓库，并写上该操作的内容，以便以后进行版本控制
git commit -m "first commit"
// 将当前分支修改名字为main 参数-M修改分支的名字
git branch -M main
// 应该是链接云端仓库
git remote add origin https://github.com/pikaCode144/test.git
// 将main分支推送到云端 参数-u云端仓库没有该分支将创建分支
git push -u origin main
```

### git的一些命令

​	做常用的三步

```javascript
// 查看git的状态，运行完之后会出现一堆红色的文件，说明文件被修改过
git status
// 将所有修改过的红色的文件推送到暂存区，点可以换成具体的文件名，这样只会将某一个文件推送到暂存区。
// 但一般情况下是将所有的文件都推送到暂存区
git add .
// 再一次查看git的状态，发现所有红色的文件都变成绿色的了。
// 说明所有更改过的文件都已经推送到暂存区了
git status
// 将文件存储到本地仓库。
// ""双引号中请详细写出本次保存都做了什么改变，一边后面进行版本控制
git commit -m ""
// 将本地的仓库push到云端，-u参数是在云端没有该分支的时候自动创建该分支，master是分支名
git push -u origin master
// 有的时候会push失败，因为本地的版本不能高于云端的版本，所以要将云端的拉去下来
git pull origin master
```

​	常用的命令

```javascript
// 查看当前所处的分支
git branch
// 切换分支master是分支名，参数-b是创建分支的时候才携带
git checkout -b master
// 合并分支，要先查看自己所处的分支一定要在主分支上，login是想要合并的分支的名字
git merge login
// 回到之前的版本，那一串字母数字是版本号，可以在云端仓库中的提交详情中查看
git reset --hard 8cbb9021d1cada2b4de7a89e50d4f37487e6f75c
```

### git使用的一些想法

​	在做项目的时候，很多人开发一个项目，使用github进行代码的托管，可以使最后的代码合并变得非常简单。

​	一般有一个主分支，然后还应该有一个测试分支，剩下的就是有多少个程序员就创建多少个分支，自己在自己的分支上进行项目的开发。

​	最好经常将自己的代码推送到云端，并写上详细的功能描述，这样方便后期的版本控制。

​	推送自己的分支之前一定要在自己本地测试好没有问题之后，在进行自己分支的推送。将自己的分支推送到了云端自己的分支后。先在本地将自己的分支合并入主分支后，再推送到云端的主分支。有的时候推送云端不成功，是因为本地的版本太高了，要将云端的主分支拉去下来再推送到云端，这样就会成功。

### git快照

![git快照](https://pikacode.oss-cn-chengdu.aliyuncs.com/NotePic/git%E5%BF%AB%E7%85%A7.png)

