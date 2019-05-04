### PHP foreach用法及解释

1. foreach是PHP的一种语法结构，其实就是一个工具，(工具：就是工作的时候用到的器具)，那么在程序开发过程中，为了达到程序效果，就用到了foreach。

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

# PHP 构造方法 __construct()

---

`PHP 构造方法 __construct() 允许在实例化一个类之前先执行构造方法`

##### 构造方法:

**构造方法是类中的一个特殊方法。当使用 new 操作符创建一个类的实例时，构造方法将会自动调用，其名称必须是 __construct()**

注意：在一个类中只能声明一个构造方法，而是只有在每次创建对象的时候都会去调用一次构造方法，不能主动的调用这个方法，所以通常用它执行一些有用的初始化任务。该方法无返回值。

语法：

```
function __construct(arg1,arg2,...)
{
    ......
}

```

例子：



    <?php
    class Person {
     var $name;
     var $age;
    //定义一个构造方法初始化赋值
    function __construct($name,  $age) {
        $this->name=$name;
        $this->age=$age;
    }
    
    function say() {
        echo "我的名字叫：".$this->name."<br />";
    echo "我的年龄是：".$this->age;
    }
    }
    
    $p1=new Person("张三", 20);
    $p1->say();
    ?>



运行该例子，输出：

我的名字叫：张三
的年龄是：20

在该例子中，通过构造方法对对象属性进行初始化赋值。

### 提示

PHP 不会在本类的构造方法中再自动的调用父类的构造方法。要执行父类的构造方法，需要在子类的构造方法中调用 parent::__construct() 。





# PHP 析构方法 __destruct()



`PHP 析构方法 __destruct() 允许在销毁一个类之前执行执行析构方法。`

#### 析构方法

与构造方法对应的就是析构方法，析构方法允许在销毁一个类之前执行的一些操作或完成一些功能，比如说关闭文件、释放结果集等。析构函数不能带有任何参数，其名称必须是 __destruct() 。

语法：

```
function __destruct()
{
    ......
}

```



我们在上面的例子中加入下面的析构方法：

//定义一个析构方法
function __destruct()
{
    echo "再见".$this->name;
}

再次运行该例子，输出：

我的名字叫：张三
我的年龄是：20
再见张三

### 提示

1. 和构造方法一样，PHP 不会在本类中自动的调用父类的析构方法。要执行父类的析构方法，必须在子类的析构方法体中手动调用 parent::__destruct() 。
2. 试图在析构函数中抛出一个异常会导致致命错误。
3. 在 PHP4 版本中，构造方法的名称必须与类名相同，且没有析构方法。


