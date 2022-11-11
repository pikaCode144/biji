# 1. node.js 有什么特点

##1.3.1 优点  

**1）异步非阻塞的 I/O （I/O 线程池）**

**2）特别适用于 I/O 密集型应用（对比传统java服务器）**

​	频繁进行 I/O 操作 ()

**3）事件循环机制（独有的一套，与浏览器不一样）**

**4）单线程（成也单线程，败也单线程）**

​	单线程运行 ”东西“ ：如果想实现 ”异步“ ，就必须有自己的 ”事件循环模型“

**5）跨平台（几乎常见的语言都支持）**

​	1.js（跨平台）------>js引擎------>设计？谷歌

​	2.java（跨平台）------>JVM虚拟机------

​	3.node.js

## 1.3.2 不足之处

**1）回调函数嵌套太多，太深（俗称回调地狱）**

**2）单线程，处理不好CPU密集型**

# 3. 包与包管理器

​	**Node.js 的包基本遵循 CommonJS 规范，包将一组相关的模块组合在一起，形成一组完整的工具。**

​	包由包结构和包描述文件两个部分组成。

​	1） 包结构：用于组织包中的各种文件

​	2） 包描述文件：描述包的相关信息，以供外部读取分析

## 3.1 package包

###3.1.1 包结构

​	包实际上就是一个压缩文件，解压以后还原为目录。符合 CommonJS 规范的目录，应该包含如下文件和文件夹：

​	1） package.json	文件描述

​	2） bin	可执行二进制文件

​	3） lib	js 代码

​	4） doc	文档

​	5） test	单元测试

### 3.1.2 包描述文件

​	包描述文件用于表达非代码相关的信息，它是一个 JSON 格式的文件：package.json

​	包描述文件包括以下字段：name、version、description、keywords、maintainers、contributors、bugs、licenses、repositories、dependencies、homepage、os、cpu、engine、builtin、directories、implements、scripts、author、bin、main、devDependencies。

## 3.2 NPM 是什么

​	全称：Node Package Manager , Node 的包管理器

## 3.3 NPM 能干什么

​	通过 NPM 可以对 Node 的包进行搜索、下载、安装、删除、上传。

​	NPM 的常用指令：

​	一、【查找】

​		1.npm search xxxxx;

​		2,通过网址搜索：www.npmjs.com

​	二、【安装】：（安装之前必须保证文件夹内有package.json，且里面的内容格式合法）

​	1.npm install xxxxx --save 或 npm i xxxxx -s 或 npm i xxxxx;

​		备注：

​		(1).局部安装完的第三方包，放在当前目录中node_modules这个文件夹里

​		(2).安装完毕会自动生成一个package-lock.json(npm版本在5以后才有)，里面缓存的是每个下载过的包的地址，目前是下次安装是速度快一些。

​		(3).当安装完一个包，该包的名字会自动写入到package.json中的【dependencies(生产依赖)】里。npm5及之前版本要加上--save

​	2.npm install xxxxx --save-dev 或 npm i xxxxx -D  安装包并将该包写入到【devDependencies(开发依赖中)】

​		备注：

​		1.只在开发时(写代码时)才用到的库，就是开发依赖 ------ 例如：语法检查、压缩代码、扩展CSS前缀的包。

​		2.在生产环境中(项目上线)不可缺少的，就是生产依赖 ------ 例如：jquery、bootStrap等等。

​		3.注意：某些包即属于开发依赖，又属于生产依赖 ------ 例如：jquery。

​	3.npm i xxxxx -g 全局安装xxxxx包(一般来说，带有指令集的包要进行全局安装，例如：browserify、babel等)

​		全局安装的包，其指令到处可用，如果该包不带有指令，就无需全局安装。

​		查看全局安装的位置：npm root -g

​	4.npm i xxxxx@yyyyy ：安装xxxxx包的yyyyy版本

​	5.npm i ：安装package.json中声明的所有包

​	三、【移除】

​	npm remove xxxxx 在node_module中删除xxxxx包，同时会删除该包在package.json中的声明

​	四、【其他命令】

​	1.npm aduit fix ：检测项目依赖中的一些问题，并且尝试着修复。

​	2.npm view xxxxx versions ：查看远程 npm 仓库中xxxxx包的所有版本信息

​	3.npm view xxxxx version ：查看npm仓库中xxxxx包的最新版本

​	4.npm ls xxxxx ：查看我们所安装的xxxxx包的版本

​	五、【关于版本号的说明】

​	"^3.x.x" ：锁定大版本，以后安装包的时候，保证包是3.x.x版本，x默认取最新的。

​	"~3.1.x" ：锁定小版本，以后安装包的时候，保证包是3.1.x版本，x默认取最新的。

​	"3.1.1" ：锁定完整版本，以后安装包的时候，保证包必须是3.1.1版本。

## 3.4 CNPM

​	**cnpm弊端：**

​	每次安装都会产生包和快捷方式，效果体验差，而且卸载包的时候会一次性删除多个包

​	**替换npm仓库地址为淘宝镜像地址（推荐）**

​	命令：npm config set registry https://registry.npm.taobao.org

​	查看是否更新成功：npm config get registry，以后安装时，依然用npm命令，但是实际是从淘宝国内服务器下载的

## 3.5 yarn

​	yarn全局安装命令：npm i yarn -g;

​	yarn全局安装后，不能像npm一样直接使用全局命令，需要分别执行yarn global dir命令，yarn global bin命令

​	C:\Users\臧志群\AppData\Local\Yarn\Data\global

​	C:\Users\臧志群\AppData\Local\Yarn\bin

​	两条命令得到两条路径，打开系统的高级系统设置，修改环境变量，在用户path环境变量中新建路径，将上方的两条路径全都添加到里面。

​	设置淘宝镜像：yarn config set registry  https://registry.npm.taobao.org

