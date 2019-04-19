### 一些git的命令和使用方法

##### 初始化命令

- git config   --global user.name '你的用户名'     **设置git账户 userName为你的git账号**

- git config   --global user.email 'email'   设置你的邮箱

##### ssh for git (window)

- cd  ~/ .ssh  **如果提示没有该路径 则手动新建一个  madir .ssh**

- ssh-keygen  -t rsa  -C  'email'  一直回车直到生成ssh， 在目录中会生成连个文件 id_rsa和id_rsa.pub

- vim id_rsa.pub,   在打开的文件中复制ssh

- 打开自己的github 创建一个秘钥 （百度怎么创建 然后把复制的ssh粘贴）

##### 远程仓库 基本命令

- git clone 【url】 下载你远程仓库的文件

- git remote  -v 查看远程仓库 

- git remote add  [[ name] ]  [[url] ] 添加远程仓库 如 git  remote  -add origin https://github.com/TQF980522/git.git

- git remote rm [[name]]  删除远程仓库 

- git remote set -url  --push [[name]]  [[newUrl]]  修改远程仓库 

- git pull [[remoteName]] [[localBranchName]]  拉取远程仓库 

- git push [[remoteName]] [[localBranchName]] 拉取远程仓库

  

##### 本地仓库基本命令

1. git  init 初始化仓库 把本地的一个文件夹设置成git可以管理的仓库，会在文件夹内生成一个隐藏的git文件

2. git status 查看本地仓库状态 

3. touch  ,gitignore 仓库忽略文件 写入不需要的文件名或文件 每个元素占一行即可 如 /node_modules/

4. git add  [[fileName]]  或 全部添加 git add [.]

5. git  commit -m   '备注  一般 modify  --by  谁'  提交到本地仓库 -m 备注



#### 分支的话现在没用到 用到再写

开始使用的话 如果是新建文件夹 先git init  把文件夹设置为git管理的仓库 

然后 克隆文件到这个文件夹下 用git clone .....

修改后如果提交的话 先查看状态 git status 

或者直接 git add . 

然后 git commit -m '备注'

再  git push  -u origin master



> #### 注意！！  每次操作文件时，最好先 git pull 把远程仓库的文件先拉下来 在修改。避免冲突




