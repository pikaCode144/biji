# git

## git检查状态

```cmd
git status
```

## git推送到暂存区

```cmd
git add .
```

## git推送到本地仓库

```cmd
git commit -m "add files"
```

## git推送到网络仓库（比如码云）

```cmd
git push -u origin master
```

## git创建分支

```cmd
git checkout -b login // login为分支的名字
```

## git查看分支

```cmd
git branch
```

## git切换分支

```cmd
git checkout master
```

## git合并分支

​	在合并分支之前要切换到主分支

```cmd
git checkout master
git merge login
```

​	login为要合并的分支名

## git将本地的分支推送到云端

```cmd
git push -u origin login
```

## git回到以前的版本

```javascript
git reset --hard //可复制的一串编码，每一次提交都会有一个版本号，在提交详情之中可以查看
```

# git推送到云端的步骤

​	在些完一些功能之后先查看所处的分支

```javascri
git branch
```

​	之后创建一个分支比如login分支，并进入该分支

```javascript
git checkout -b login // -b 为创建分支 login 为为分支起的名字
```

​	查看所处的分支，要确定当前分支在所创建的分支

```javascri
git branch
```

​	查看状态，如果有很多红色的文件，说明有很多新添加的文件并没有在本地进行保存

```javascript
git status
```

​	将没有保存的的数据保存到暂存区

```javascript
git add .
```

​	运行完上面代码，再次查看状态

```javascript
git status
```

​	此时如果有很多绿色的文件，说明文件已经在暂存区了，但还未推送到本地仓库

​	执行以下的代码，可以将暂存区的文件推送到本地仓库

```javascript
git commit -m "完成某项功能"	// 为当前操作取一个名字
```

​	再次查看状态，会发现该分支非常的干净

```javascript
git status
```

​	执行以下的代码将进行云端推送，云端没有该分支所以要用-u参数进行创建分支，login为云端分支的名字

```javascript
git push -u origin login
```

​	查看当前分支

```javascript
git branch
```

​	切换到主分支

```javascript
git checkout master
```

​	查看当前是否处于主分支

```javascript
git branch
```

​	当前处于主分支可以进行分支合并

```javascript
git merge login
```

​	分支合并之后，因为云端仓库已经有了master，所以可以直接使用push进行推送，后面不用跟-u

```javascript
git push
```

