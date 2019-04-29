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

> #### 注意！！  每次操作文件时，最好先 git pull 把远程仓库的文件先拉下来 在修改。避免冲突

### 简单的分支用法

- 首先 在远程仓库创建 login-register分支

- 然后在本地仓库里使用命令 git branch xxxx（分支的名字 为了方便用，最好把分支的名字和远程仓库分支的名字起同一个）

- 查看分支 git branch -a 

- 创建了分支 使用 git checkout xxxx（分支的名字 就是你上面创建的）

- 可以执行git pull origin master（master表示把远端的项目也就是全部 pull下来，如果只是要拉分支的话 可以origin后加分支名字） 把远端的分支的代码拉下来

- 然后修改了的话 同步到远程仓库上面的分支 使用 git push origin xxxx（分支名字）

分支的进一步理解：比如C负责开发login-register，在本地建立login-register分支，git pull  origin 拉下远端的分支，编写后通过 push 到远程仓库的login-register分支上去 这样子的话远程仓库login-register分支上也有项目了

`push之前还是老三步  git add .  git commit  git push origin xxxx`

github 上 可以点击compare 来比较代码

完善一点的话：C在自己本地的login-register 上进行开发，开发完成后合并到自己本地的master中 再从本地的master中提交到远端的login-register，组长去github上 compare对比代码 然后审核代码 有必要的话建立pull&request

`还是注意！ 在要提交之前，先把分支拉下来 git pull origin xxxx 在提交 git push origin xxxx`

git  checkout xxxx  切换到分支  

** 合并项目的话  先切换到分支把代码拉下来  **

**然后回到master 还是命令 git checkout master  **

**执行命令 git merge xxxx（分支名字）**

`合并好后 执行老三步 `

`git add . `

`git commit -m ''`

`git push origin master  提交到远程仓库的master`

在经历过一段时间开发后 login-register已经开发完成了，组长在本地login-register中拉下远程仓库的login-register，切换到本地的master中 与本地的login-register进行合并 merge， 在从本地的master提交到远端的master 建立pull&request

主分支master 组员不能动 只能组长自己从自己本地仓库的master 提交到远程仓库时使用

图解：

![5cc5cfc110dbe](https://i.loli.net/2019/04/29/5cc5cfc110dbe.png)

然后就这样了。。。。

## js中forEach，for in，for of循环的用法

1. **一般的遍历数组的方法**

       var array = [1,2,3,4,5,6,7]; 
       for (var i = 0; i < array.length; i) {  
           console.log(i,array[i]);  
       }

2. **用for in的方遍历数组**

   **tips: for in总是得到对象的key或数组,字符串的下标**

       for(let index in array) {  
           console.log(index,array[index]);  
       };

3. **用for of的方遍历数组**

   **tips: for of总是得到数组,字符串的value值，但是不能遍历对象**

       for(let v of array) {  
           console.log(v);  
       }; 
       
         let s = "helloabc"; 
       
         for(let c of s) {  
       
         console.log(c); 
       
        }

4. **forEach**

   **tips: foreach能得到数组，字符串的值。优点是能对对象使用**

   ```
   array.forEach(v=>{  
       console.log(v);  
   });
   array.forEach（function(v){  
       console.log(v);  
   });
   ```

5. **Set**

   ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

   `Set`本身是一个构造函数，用来生成 Set 数据结构。

   ```javascript
   const s = new Set();
   
   [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
   
   for (let i of s) {
     console.log(i);
   }
   // 2 3 5 4
   ```

   上面代码通过`add()`方法向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值。

6. **JavaScript 提供了 foreach()  map() 两个可遍历 Array对象 的方法**　　　

       forEach和map用法类似，都可以**遍历到数组的每个元素**，而且参数一致；

   ```
   Array.forEach(function(value , index , array){ //value为遍历的当前元素，index为当前索引，array为正在操作的数组
     //do something
   },thisArg)      //thisArg为执行回调时的this值
   ```

   **不同点：**

     forEach() 方法对数组的每个元素执行一次提供的函数。总是返回undefined；

     map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。返回值是一个新的数组；

     例子如下：

   ```
   var array1 = [1,2,3,4,5];
   
   var x = array1.forEach(function(value,index){
   
       console.log(value);   //可遍历到所有数组元素
   
       return value + 10
   });
   console.log(x);   //undefined    无论怎样，总返回undefined
   
   var y = array1.map(function(value,index){
   
       console.log(value);   //可遍历到所有数组元素
   
       return value + 10
   });
   console.log(y);   //[11, 12, 13, 14, 15]   返回一个新的数组
   ```
