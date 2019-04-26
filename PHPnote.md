### PHP foreach用法及解释

1. **foreach是PHP的一种语法结构，其实就是一个工具，(工具：就是工作的时候用到的器具)，那么在程序开发过程中，为了达到程序效果，就用到了foreach。**

2. **怎么用**

   **官方：**

   ```
   foreach (array_expression as $value)
       　　statement
   　　foreach (array_expression as $key => $value)
     　　  statement
   ```

   **例子：**

   `$arr``=``array``(1,2,3,4);``//定义数组`

   `foreach``(``$arr``as``&``$value``) {`

   `$value``=``$value``*2;``//循环遍历并修改数组中的值`

   `}`

   `echo``"<pre>"``;`

   `print_r(``$arr``);``//打印输出最终结果`

   **结果：其中(&$value是引用赋值)**

   ```
   <div>
   Array
    (
    [0] => 2
    [1] => 4
    [2] => 6
    [3] => 8
    　　)
   </div>
   ```

   **例2：**

   `$a``=``array``(`

   `"one"``=> 1,`

   `"two"``=> 2,`

   `"three"``=> 3,`

   `"seventeen"``=> 17`

   `);`

   `foreach``(``$a``as``$k``=>``$v``) {`

   `echo``"<pre>"``;`

   `echo``"[$k] => $v\n"``;`

   `}  `

   结果：

   [one] => 1

   [two] => 2

   [three] => 3

   [seventeen] => 17

   解说：k其实指的就是key,v其实指的就是value



## TP5 跨控制器调用方法。

`其实文档有很详细的介绍，但是我就是想写`



**1.在application\index\controller\文件夹里新建User.php**

```

<?php
    namespace app\index\controller;
    class User{
        public function index(){
            return('我是User控制器的index方法');
        }
    }
```

**2.在application\index\controller\文件夹下的Index.php调用User的控制器**

```
<?php
namespace app\index\controller;

use app\index\controller\User;

class Index extends Controller
{
    
    public function diaoyong(){
        //方法一
        $model=new \app\index\controller\User;
        echo $model->index();
        echo "______________________<br>";
        
        //方法二
        $model2=new User;
        echo $model2->index();
        echo "______________________<br>";
        
        //方法三
        $model3=controller('User');
        echo $model3->index();
        
        
    }
    
}
```

3.系统方法一般在tp5\thinkphp\helper.php里面。

4.use同名时请使用as。比如 ：use app\index\controller\User as UserController;然后创建实例的时候直接用  $model2=new UserController;

`注：如果发现网上有雷同，全是我抄的，毕竟懒，不负责任哈哈`


