##关于TP5的模型关联，自己的理解

##### 1，模型关联有一对一关联和一对多关联，因为刚用，所以只知道一对一关联，下面记下自己对一对一关联的理解

官方用法

```
hasOne('关联模型名','外键名','主键名',['模型别名定义'],'join类型')
```

**一对一关联的用法**，'关联模型名'：就是本模型要关联到的那个模型 也就是模型GoodsList，'外键名'：就是相对于本模型GoodsOnline来说是外键，对于GoodsList来说是它的主键，'主键名'：相对于本模型GoodsOnline 来说 是模型里要关联GoodsList里的id 也就是goods_id。（简单说 就是通过模型GoodsOnline里的键goods_id去找GoodsList模型里id为goods_id的数据）

```
class  GoodsOnline  extends  Model
{
protected  $autoWriteTimestamp = 'timestamp';
public  function  productlist()
    {
        return  $this->hasOne('GoodsList','id','goods_id');
    }
}
```

关联之后 查数据？？？

这里用到了 关联预载入

看PHP代码

$list = GoodsOnlineModel::with(['productlist.onlinetype'])->where($where)->select();



因为我找了商品之后还要通过商品去找商品的分类 所以用了嵌套预载入

官方文档

```
$list = User::with('profile.phone')->select([1,2,3]);
foreach($list as $user){
    // 获取用户关联的phone模型
    dump($user->profile->phone);
}
```

其实就是在通过模型GoodsOnline关联到模型GoodsList之后，再通过模型GoodsList再去关联模型GoodsCategory 得出数据

然后在通过方法调用也就是with(['productlist.onlinetype'])，

GoodsList模型下关联GoodsCategory模型 方法onlinetype 代码

```
class GoodsList extends Model
{
	protected $autoWriteTimestamp = 'timestamp';

	public function onlinetype()
	{
		return $this->hasOne('GoodsCategory','id','category_id');
	}
   //商品收藏 一对一关联
	public function goodscollect(){
		return $this->hasOne('getGoodsListModel','goods_id','id');
	}

	//规格
	public function getspec()
	{
		return $this->hasMany('app\admin\model\GoodsSpec','goods_id','id');
	}

}
```


