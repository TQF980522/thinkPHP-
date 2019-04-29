# `一，使用http-proxy-middleware 代理解决（项目使用vue-cli脚手架搭建）`

`例如请求的接口url:“http://f.apiplus.cn/bj11x5.json”`

`1、打开config/index.js,在proxyTable中添写如下代码：`

`proxyTable: {`

`'/api': {  //使用"/api"来代替"http://f.apiplus.c"`

`target: 'http://f.apiplus.cn', //源地址`

`changeOrigin: true, //是否跨域`

`secure:  false, // 如果是https接口，需要配置这个参数`

`pathRewrite: {`

`'^/api': 'http://f.apiplus.cn' //路径重写`

`}`

`}`

`}`

`2、使用axios请求数据时直接使用“/api”：`

`getData () {`

`axios.get('/api/bj11x5.json', function (res) {`

`console.log(res)`

`})`

##### `通过这中方法去解决跨域，打包部署时还按这种方法会出问题。解决方法如下：`

`let serverUrl = '/api/'  //本地调试时`

`// let serverUrl = 'http://f.apiplus.cn/'  //打包部署上线时`

`export default {`

`dataUrl: serverUrl + 'bj11x5.json'`

`}`

`调试时定义一个serverUrl来替换我们的“/api”，最后打包时，只需要将“http://www.xxx.com”替换这个“/api”就可以了。`



# 二， TP5 后台设置跨域请求头

1. **在公共文件common里新建behavior文件夹，然后在文件夹里创建CronRun.php文件**

   `添加代码`

   ```
   <?php
   /**
    * Created by VsCode.
    * User: tqf
    * Date: 2019/4/29
    * Time: 09:34
    * 跨域处理
    */

   namespace app\common\behavior;
   use think\Exception;
   use think\Response;
   

   class CronRun
   {
       public function run(&$dispatch)
       {
           $host_name = isset($_SERVER['HTTP_ORIGIN']) ? $_SERVER['HTTP_ORIGIN'] : "*";
           $headers = [
               "Access-Control-Allow-Origin" => $host_name,
               "Access-Control-Allow-Credentials" => 'true',
               "Access-Control-Allow-Headers" => "X-Token,x-uid,x-token-check,x-requested-with,content-type,Host"
           ];
   
           if ($dispatch instanceof Response) {
               $dispatch->header($headers);
           } else if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
               $dispatch['type'] = 'response';
               $response = new Response('', 200, $headers);
               $dispatch['response'] = $response;
           }
       }
   }
   
   // header('Access-Control-Allow-Origin:' . $host);
   // header('Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS');
   // header('Access-Control-Expose-Headers:token'); //暴露会话信息
   // header("Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept,community-id , auth-key,token, session-id,cookie");
   // header("Access-Control-Max-Age:60");
   // header("Access-Control-ALLOW-Credentials:true");
   
   ```

2. **然后在tags.php里修改代码为**

   ```
   <?php
   // +----------------------------------------------------------------------
   // | ThinkPHP [ WE CAN DO IT JUST THINK ]
   // +----------------------------------------------------------------------
   // | Copyright (c) 2006~2016 http://thinkphp.cn All rights reserved.
   // +----------------------------------------------------------------------
   // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
   // +----------------------------------------------------------------------
   // | Author: liu21st <liu21st@gmail.com>
   // +----------------------------------------------------------------------
   
   // 应用行为扩展定义文件
   return [
       // 应用初始化
       'app_init'     => [],
       // 应用开始
       'app_begin'    => [
           'app\\common\\behavior\\CronRun'
       ],
       // 模块初始化
       'module_init'  => [],
       // 操作开始执行
       'action_begin' => [],
       // 视图内容过滤
       'view_filter'  => [],
       // 日志写入
       'log_write'    => [],
       // 应用结束
       'app_end'      => [
           'app\\common\\behavior\\CronRun'
       ],
   ];
   ```


