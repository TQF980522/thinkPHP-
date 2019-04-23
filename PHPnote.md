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
