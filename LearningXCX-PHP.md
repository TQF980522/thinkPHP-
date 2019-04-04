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


