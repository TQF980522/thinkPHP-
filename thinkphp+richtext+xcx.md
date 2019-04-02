# thinkphp使用富文本编辑器和小程序使用富文本解析组件

#### php使用

- **1**,**下载百度ueditor富文本文件**

  *下载好之后放到框架下的public目录*

  ![5ca32bc393999](https://i.loli.net/2019/04/02/5ca32bc393999.png)

- **2,在代码部分引入文件的js和配置**

  ![5ca32badd2fb7](https://i.loli.net/2019/04/02/5ca32badd2fb7.png)

  一般我放在底部的html-->foot.html

 

```
<!--ueditor-->
<script type="text/javascript" charset="utf-8" src="__ueditor__/ueditor.config.js?v=1"></script>
<script type="text/javascript" charset="utf-8" src="__ueditor__/ueditor.all.min.js"></script>
<!--建议手动加在语言，避免在ie下有时因为加载语言失败导致编辑器加载失败-->
<!--这里加载的语言文件会覆盖你在配置项目里添加的语言类型，比如你在配置项目里配置的是英文，这里加载的中文，那最后就是中文-->
<script type="text/javascript" charset="utf-8" src="__ueditor__/lang/zh-cn/zh-cn.js"></script>
```



_ueditor_代码里提示的这个配置，是提前在配置项里配置好了的静态文件访问目录

*也就是在thinkphp框架里的config.php里写的* 

![5ca32b254a300](https://i.loli.net/2019/04/02/5ca32b254a300.png)



* **3,在需要用到的html文件里面添加代码**

```
          <div class="form-group">

              <label class="layui-form-label">商品详情</label>

              <div class="layui-input-block" style="z-index:0;">

                  <!-- 加载编辑器的容器 -->

                  <script id="UEditor" name="detail" type="text/plain" style="width: 100%;height: 400px;"></script>

              </div>

          </div>
```



JS代码添加

```
    $(function () {
        var editor = new UE.ui.Editor();
        editor.render("UEditor");
        editor.ready(function () {
            //设置编辑器的内容
            // editor.setContent('{$data.detail|default=""}'); //如果点击编辑按钮 需要赋值原本的内容 取消注释这句 注释掉下面

            editor.setContent('');
        });
    });
```

很简单，这样就可以使用了。



#### 小程序解析富文本

- 下载wxParse小程序富文本解析组件

  https://github.com/icindy/wxParse

- 解压后把包放在小程序的工具包units文件夹里

  ![5ca32b767262e](https://i.loli.net/2019/04/02/5ca32b767262e.png)

- 在使用的page的js，css和html里引入文件 

  var  WxParse  = require('../../utils/wxParse/wxParse.js');


 <view class='comm-col'>
      <import src="/utils/wxParse/wxParse.wxml" />
      <template is="wxParse" data="{{wxParseData:details.nodes}}" />
    </view>



        @import "/utils/wxParse/wxParse.wxss";

js里面还要做一些配置

let details = res.goods.detail;//取到后台穿过来的富文本 商品详情

WxParse.wxParse('details',  'html', details, _this,  10,  App.api_url); //第一个参数是定义给wxml 引用的名字 第二个是文档类型 第三个是取到的数据第4个是在哪里调用（不清楚看wxParse 文档上面很清楚） 然后最后一个是因为前面富文本的图片出不来，配置的一个富文本取用全局的url（下面重点讲解怎么配置）



在wxParse.js文件里面 函数入口区添加参数 server_url=''

![5ca32a8117c46](https://i.loli.net/2019/04/02/5ca32a8117c46.png)



然后在html2json.js里面 我们在引入 server_url=''

![5ca32ee34391a](https://i.loli.net/2019/04/02/5ca32ee34391a.png)

还有这里

![5ca32f52e7347](https://i.loli.net/2019/04/02/5ca32f52e7347.png)
