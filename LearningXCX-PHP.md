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