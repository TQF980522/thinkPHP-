# 学习小程序和php的杂项

---

**上拉加载**
wxml：

     <view class="weui-loadmore" wx:if="{{!isHideLoadMore}}">
    <view class="weui-loading"></view>
    <view class="weui-loadmore__tips">正在加载</view>
     </view>
     <view class="weui-loadmore" wx:if="{{isHideLoadMore}}">
     <view class="weui-loadmore__tips">到底了</view>
    </view>

js 

      data: {
            URL: App.api_url,
            sinput: '',
            page: 0,
            goodsList: [],
            isHideLoadMore: false,
        },
         onLoad: function(options) {
            let _this = this;
            // console.log(options.sinput)
            _this.setData({
              sinput: options.sinput
            });
            let sinput = _this.data.sinput;
            _this.queryDemand(sinput).then((data) => {
              wx.hideLoading();
              _this.setData({
                isHideLoadMore: true,
                goodsList: data
              })
            });
          },
    
          searchInput: function(e) {
            let _this = this;
            let sinput = e.detail.value;
            _this.setData({
              sinput: sinput
            })
          },
          // 搜索
          search: function() {
            let _this = this;
            let sinput = _this.data.sinput;
            console.log(sinput);
            //重新把page设置为0 搜索别的
            _this.setData({
              page: 0,
            })
    
            _this.queryDemand(sinput).then((data) => {
              _this.setData({
                isHideLoadMore: true,
                goodsList: data,
              })
    
            });
          },
          /**
           * 页面上拉触底事件的处理函数
           */
          onReachBottom: function() {
            let _this = this;
            // wx.showLoading({
            //   title: '玩命加载中',
            // })
    
            // 经过onload 状态改变为了true
            let sinput = _this.data.sinput;
            _this.setData({
              isHideLoadMore: false,
            });
            //上拉的时候先置为flase 在加载的时候显示转圈  完成加载判断有无数据 没有设为true
            _this.queryDemand(sinput).then((data) => {
              let goodsList = _this.data.goodsList;
              _this.setData({
                goodsList: goodsList.concat(data),
              })
              if (data == '') {
                _this.setData({
                  isHideLoadMore: true,
                })
              }
            });
              //查询请求
          queryDemand: function(sinput) {
            let _this = this;
            return new Promise((resolve, reject) => {
              App._get('goods_deals/goods_serach', {
                sinput: sinput,
                page: ++_this.data.page
              }, function(result) {
                console.log(result)
                resolve(result)
              })
            })
          },

后端PHP：

        function goods_serach(){
            $get = $this->request->get();
            $page =intval( $get['page']);
            $query = $get['sinput'];
            // var_dump($query);exit;
            $where['goods_name'] = ['like','%'.$query.'%'];
            $where['status'] = ['eq',1];
            $goods = getGoodsListModel::where($where)->page($page,Config::get('paging'))->select();
            // Config::get('paging') 获取公共配置设置的分页数  $page 是取得前端传过来的当前页数 
            return json_encode($goods);
        }

**小程序取值**
    _首先在wxml里面取得循环出来的每个item的id，如下_

     <view class="select-small" wx:for="{{goodsList}}" wx:key bindtap='goodsNext' data-goodsid='{{item.id}}'>

然后在js面取值

      //商品跳转
      goodsNext: function(e) {
        let goods_id = e.currentTarget.dataset.goodsid;
        wx.navigateTo({
          url: '/pages/goods/goods?goods_id=' + goods_id,
        })
      },

这样就取到商品的id，就可以带着id跳转了。

#### 微信小程序跳转传参

*一般都是传字符串到下一页，如果要想传对象怎么办呢？ 解决办法是先将对象转换为json字符串然后到下个页面将json字符串，再转化为对象。如下：*

```
let str=JSON.stringify(e.currentTarget.dataset.item);
wx.navigateTo({
  url: '/跳转路径/?jsonStr='+str,
})
```

另一个页面加载

> `let str=JSON.stringify(e.currentTarget.dataset.item);`
> 
> `wx.navigateTo({`
> 
> `url: '/跳转路径/?jsonStr='+str,`
> 
> `})`

####插播一段css属性

`1，box-sizing: border-box;` 规定两个并排的带边框的框 border-box 为元素设定的宽度和高度决定了元素的边框盒。 就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。

`2，border-right: 1rpx solid #d9d9d9;` /*右边框样式：厚度，实线，颜色*/

`3，flex-flow`属性是flex-direction属性和flex-wrap属性的简写形式，默认值为`row nowrap`。

#### php根据状态更改不同的字体颜色

thinkphp内置

```
{php}echo 'Hello,world!';{/php}
```

标签 

在要控制的标签里添加 如 

```
<td {php} if($vo['status']==0){echo 'class="redcode"' ;} {/php}
{php}if($vo['status']==1){echo 'class="bluecode"' ;} {/php}
{php}if($vo['status']==2){echo 'class="greencode"' ;} {/php}
>
{switch name="$vo.status" }
{case value="0" break="0或1" } 待处理{/case}
{case value="1" }处理中{/case}
{case value="2"}已完成{/case}
{default /}
{/switch}
</td>
```

if($vo['status']==0){echo 'class="redcode"' ;} 为判断语句 ，如果为0 输出 类名为 redcode的属性。 

switch也是thinkphp内置标签  当name 的值和value的值相等的时候才输出内容 

### 文本处理溢出隐藏显示省略号

##### 项目中常常有这种需要我们对溢出文本进行"..."显示的操作，单行多行的情况都有

> ### 单行
> 
> ```
>     overflow:hidden;
> 
>     text-overflow:ellipsis;
>     white-space:nowrap
> ```
> 
> ### 多行
> 
> ```
>   overflow: hidden;
> 
>   text-overflow: ellipsis;
>   display: -webkit-box;
>   -webkit-line-clamp: 2;
>   -webkit-box-orient: vertical;
> ```

**移动端浏览器绝大部分是WebKit内核的，所以该方法适用于移动端；**

- **-webkit-line-clamp 用来限制在一个块元素显示的文本的行数,这是一个不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。**

- **display: -webkit-box 将对象作为弹性伸缩盒子模型显示 。**

- **-webkit-box-orient 设置或检索伸缩盒对象的子元素的排列方式 。**

- **text-overflow: ellipsis 以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本。**



# 巧用 HTML5 data 属性



HTML 5 之前，需要使用其他的数据时，是非常糟糕的。为了使一切正常有效，你不得不将数据填充到 rel 或 class 属性

自从 HTML5 自定义 data 属性出现，你可以存储任意数据，通过一种简单，符合标准的方式。



> ### 1. 如何使用data属性呢？

一个 data 属性，本质就是：一个用于保存数据的自定义属性。它们总是以 data- 作为前缀，后面跟随着描述性的（只允许小写字母和连接字符-hyphens）。

一个元素可以有任意数量的 data 属性。

一个使用 list 保存用户数据的案例，如下：

```
<li data-id="1234" data-email="calvin@example.com" data-age="21">Calvin</li>
```

当然，这数据对于一个访客并没有多大的用处，因为这数据是看不见的。但是，创建 web 应用是非常有用。

想象在你应用有一个删除按钮：  

```
<button type="button" data-cmd="delete" data-id="1234">Delete</button>
```

所有你需要的数据，都已经准备好，只需发送给你的后台脚本了。不需要 rel 或 解释其他属性获得 IDs 和 actions。

Data URLs 让你的生活变得更简单。

> ### 2. 你可以保存什么数据？

**使用 data 属性，需要记住一点，data 属性不能保存对象。如果你真的想要保存对象，可通过对象序列化。**



> ### 3. 使用JavaScript读/写data属性

使用删除按钮作为例子，看看如何使用 JavaScript 访问数据。

```
// 这是我们的按钮
var button = document.getElementById('your-button-id');

// 取值
var cmd = button.getAttribute('data-cmd');
var id = button.getAttribute('data-id');

// 重新赋值
button.setAttribute('data-cmd', yourNewCmd);
button.setAttribute('data-id', yourNewId);
```

> ### 4 .使用jQuery读/写data属性

使用 jQuery 的 .attr() 方法：（或者可以使用 .data() 方法）

```
// 取值
var cmd = $("#your-button-id").attr('data-cmd'); // $("#your-button-id").data('data-cmd');
var id = $("#your-button-id").attr('data-id');


// 赋值
$("#your-button-id").attr('data-cmd', yourNewCmd).attr('data-id', yourNewId);
```

**注意，不要将上面的方式，与 jQuery 的 .data() 弄混了。尽管 data 属性和 .data() 的运作原理有些重叠，它们是两个完全不同的东西。**

**如果你对 .data() 不熟悉，那么就牢记住 .attr() 就好了。**



> ### 5、使用 dataset API

HTML5 有一个用于与这类型数据的 API。唉，IE10 和 低于IE10 并没有完全支持这一特性。

再次以按钮案例，让我们看看如何使用 dataset API 来设置 data 属性的：



```
// here's our button
var button = document.getElementById('your-button-id');

// Get the values

var cmd = button.dataset.cmd;
var id = button.dataset.id;

// Change the values
button.dataset.cmd = youNewCmd;
button.dataset.id = yourNewId;
```

这里没有 data - 前缀或破折号。类似于 CSS 属性工作在 JavaScript ，你将必须使用驼峰命名法。  
dataset API 转换每一个，你总是会在 HTML 使用 data-some-attribute-name，而 JavaScript 中是 dataset.someAttributeName。
