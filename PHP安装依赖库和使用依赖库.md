# 一， PHP安装composer 依赖库，使用依赖库下载导出表格插件

1. `安装依赖库`

   https://www.phpcomposer.com/

   根据上面网址去下载和安装好composer

2. `使用win+r ，输入cmd进入命令行模式，输入composer回车，看到composer及相关信息，表示安装成功；`

![](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=3080452731,1603554693&fm=173&app=49&f=JPEG?w=501&h=459&s=A9605B22CBAEAECC0E7DB80F000050C1)

3.`然后命令行cd一波操作进入你的项目文件夹根目录下 执行 composer install 或者composer updata  之后会在你项目根目录新建一个vendor文件夹 这个文件夹就是用来存放你所有下载的插件，类似于node.js的npm install 一样下载依赖`

在这之前 先在你项目根目录新建一个composer.json文件

因为用的是别人写好的 项目根目录自带了composer.json文件  所以我只要稍作修改如下

![5cc1c27fd0110](https://i.loli.net/2019/04/25/5cc1c27fd0110.png)

在文件里加入内容如图所示 

requier表示要引入的插件，如果项目开始没有的话，cmd执行composer install就会自动帮你下载插件到vendor文件夹

开始之前我都是去https://packagist.phpcomposer.com 找好我要用的插件

然后cmd到项目根目录 执行插件的安装方法 如：

![5cc1c5522be2a](https://i.loli.net/2019/04/25/5cc1c5522be2a.png)

因为之前用过node.js 所以感觉跟Vue安装依赖一样。

这样插件就会下载到你的项目根目录里的vendor文件夹里

到这里，安装composer（他有个优雅的名字叫作曲家），和下载插件（习惯叫依赖）就已经弄好了。

-----

# 二， PHP使用插件 phpoffice/phpspreadsheet



1. `先在控制器里新建一个office.php文件或者common文件夹的控制器里封账一个导出表格的方法`

   如：（这个是新建的office文件）

   ```
   <?php

   namespace app\admin\controller;

   use PhpOffice\PhpSpreadsheet\Spreadsheet;
   use PhpOffice\PhpSpreadsheet\Writer\Xlsx;

   class Office
   {
       /**
        * 导出excel表
        * $data：要导出excel表的数据，接受一个二维数组
        * $name：excel表的表名
        * $head：excel表的表头，接受一个一维数组
        * $key：$data中对应表头的键的数组，接受一个一维数组
        * 备注：此函数缺点是，表头（对应列数）不能超过26；
        *循环不够灵活，一个单元格中不方便存放两个数据库字段的值
        */
       public function outdata($name = '', $list = [], $head = [], $keys = [])
       {
           $count = count($head);  //计算表头数量
   
           $spreadsheet = new Spreadsheet();
           $sheet = $spreadsheet->getActiveSheet();

           for ($i = 65; $i < $count + 65; $i++) {     //数字转字母从65开始，循环设置表头：
               $sheet->setCellValue(strtoupper(chr($i)) . '1', $head[$i - 65]);
           }

           /*--------------开始从数据库提取信息插入Excel表中------------------*/
   
           foreach ($list as $key => $item) {             //循环设置单元格：
               //$key+2,因为第一行是表头，所以写到表格时   从第二行开始写

               for ($i = 65; $i < $count + 65; $i++) {     //数字转字母从65开始：
                   $sheet->setCellValue(strtoupper(chr($i)) . ($key + 2), $item[$keys[$i - 65]]);
                   $spreadsheet->getActiveSheet()->getColumnDimension(strtoupper(chr($i)))->setWidth(20); //固定列宽
               }
           }
   
           header('Content-Type: application/vnd.ms-excel');
           header('Content-Disposition: attachment;filename="' . $name . '.xlsx"');
           header('Cache-Control: max-age=0');
           $writer = new Xlsx($spreadsheet);
           $writer->save('php://output');
           //删除清空：
           $spreadsheet->disconnectWorksheets();
           unset($spreadsheet);
           exit;
       }
   }
   
   ```

   `公共方法类似 只要引入好需要用的文件 我现在只用到这两个 其他要用的话自行百度怎么用`

       ` use PhpOffice\PhpSpreadsheet\Spreadsheet;`
       ` use PhpOffice\PhpSpreadsheet\Writer\Xlsx;`

然后在要用的控制器的方法里 

如：

```
    /**
     * 导出execl表格
     * param ： id ： order_id
     * 
     */
    public function export_excel()
    {
        $data = input('param.');
        $id = $data['id'];
        if (!$id) {
            $this->error('数据异常');
        }
        $model = new OrderGoodsModel();
        $list = $model->with('profile')->where('order_id', 'in', $id)->select();

        foreach ($list as $k => $v) {
            // $array[$k]['status'] = json_decode($array['status'], true);
            //状态
            switch ($v['status']) {
                case -1:
                    $list[$k]['status'] = '已取消';
                    break;
                case 0:
                    $list[$k]['status'] = '待付款';
                    break;
                case 1:
                    $list[$k]['status'] = '已付款';
                    break;
                case 2:
                    $list[$k]['status'] = '待发货';
                    break;
                case 3:
                    $list[$k]['status'] = '配送中';
                    break;
                case 4:
                    $list[$k]['status'] = '已收货';
                    break;
            }
            //配送类型
            switch ($v['order_type']) {
                case 1:
                    $list[$k]['order_type'] = '商家配送';
                    break;
                case 2:
                    $list[$k]['order_type'] = '自提';
                    break;
            }
        }
        //设置表头：
        $head = ['订单号', '微信用户', '订单价格', '收件人', '联系电话', '配送方式', '服务费', '网点名称', '网点地址', '快递名称', '快递单号', '下单时间', '状态'];
        //数据中对应的字段，用于读取相应数据：
        $keys = ['order_number', 'nickname', 'price', 'realname', 'phone', 'order_type', 'service', 'mention_name', 'mention_address', 'express_name', 'express_number', 'sales_time', 'status'];

        $excel = new Office();
        $excel->outdata('订单表', $list, $head, $keys);
        // 调用方法 传递数据到Office
    }
```

end.... ps：请求到这个方法 传递参数不能用ajax，只能用url带参数的方式，不然导不出表格，至于为什么，不懂解释。。只知道ajax只支持string json类型数据


