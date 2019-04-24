## JQ 杂项

### jQuery实现原理

- jQuery构造函数
- jQuery实例对象  

  ```
  jQuery('#box');//得到jquery对象（实例）
  ```

> jquery对象只能用jquery的方法

- jQuery实例属性
  - length: 返回jQuery对象中匹配元素的个数
  - jquery: 当前jquery类库版本号（一般用于判断是否jquery对象）
  - 下标（索引）: DOM节点
- jQuery的别名：$
- 延迟代码执行：jQuery(document).ready(fn)
  - 此方法传入一个匿名函数;
  - 页面DOM渲染完成时执行，用来替代window.onload;
  - 简写方式:`$(function(){})`;
- 编写jquery代码只需两步  
  1. 选择元素  
  2. 操作元素

## 选择器

> 选择页面中的元素，得到jQuery实例对象

- ID选择器$(“#save”)

- 类选择器$(“.class”)

- 标签选择器$(“div”)

- 复合选择器 $(“div,span,p.myClass”)

- 属性选择器$(‘[id=box]’)

  - $(‘li[data-index]’):获取有data-index属性的所有元素
  - $(‘li[data-index!=10]’):data-index属性不等于10的元素,css目前未支持
  - $(‘li[data-index^=10]’):data-index属性以10开头的元素
  - $(‘li[data-index$=10]’):data-index属性以10结尾的元素
  - $(‘li[data-index*=10]’):data-index属性包含10的元素

- 表单选择器$(‘:input’)

  - :radio //匹配所有单选按钮

  - :checkbox //匹配所有复选按钮

  - :selected //获取已选择的option元素

  - :checked //匹配所有被选中的元素(复选框、单选框等，select中的option)

  - :submit //匹配所有提交按钮

  - :reset //匹配所有重置按钮

  - :button //匹配所有按钮

  - :text //匹配所有的单行文本框

  - :password //匹配所有密码框

- 可见性  

  :hidden //匹配所有不可见元素(display:none)，或者type为hidden的元素  

  :visible //匹配所有可见元素

  > 以上两个选择器配合is()方法通常用于判断，返回布尔值  
  > 
  > ```
  > if(box.is(':visible')){
  >     box.css('display','none');
  > }
  > ```

### 常用操作

- jquery对象与原生对象的转换

  - jquery转原生:
    - get(0)/[0]获取集合中第一个DOM节点
    - get()不传参得到集合中所有的dom节点
  - 原生转jquery: $(dom);

- 判断是否为jquery对象  

  ```
  var box = $('#box');
  if(box.jquery){
  
  }
  ```

- 判断一个jquery对象是否存在(是否能获取到元素)

  - length
  - 转成原生对象再判断

### 筛选

> 利用选择器得到得结果不一定是我们想要得最终结果，有时需要进一步筛选

### 基本筛选

- :odd/:even,:gt(n)/:lt(n) 筛选范围（索引支持负数）
- :contains(奥巴马) 筛选出包含“奥巴马”这三个字的元素

### 筛选方法

- first()/last(): 获取集合中第一个/最后一个元素
- eq(index|-index): 获取第N个元素,n支持负数（表示从后面查找）
- filter(expr|obj|ele|fn): 筛选出与指定表达式匹配的元素集合。这个方法用于缩小匹配* 的范围。用逗号分隔多个表达式
- map(fn): 将一组元素转换成其他数组（不论是否是元素数组）
- slice(start,[end]): 选取一个从start到end(不包括end)匹配的子集
- has(expr|ele): 保留包含特定后代的元素，去掉那些不含有指定后代的元素。
- not(expr|ele|fn): 删除与指定表达式匹配的元素

### 查找

> 利用当前元素去查找其他元素

- 查找子元素

  - find(expr|obj|ele): 查找后代元素
  - children([expr]): 取得匹配元素的所有子元素。(原生js:children)

- 查找父级元素

  - parent([expr]): 获取父元素
  - parents([expr]): 取得所有父级元素
  - closest(expr|obj|ele): 从元素本身开始，逐级向上级元素匹配，并返回最先匹配的元素
  - offsetParent(): 返回第一个有定位属性(absolute,relative,fixed)* 的父元素,如果没有定位父级，则返回html元素

- 查找兄弟元素

  - next([expr]): 返回下一个同辈元素 ==> nextElementSibling
  - prev([expr]): 获取前一个同辈元素 ==> previousElementSibling
  - nextAll([expr]): 获取当前元素之后所有的同辈元素
  - prevAll([expr]) 获取当前元素之前所有的同辈元素
  - siblings([expr]) 获取当前元素的所有兄弟元素（除自身以外的所有兄弟元素 = * prevAll + nextAll）

# DOM节点操作

### 增删改

- 创建jquery对象  

  ```
  $('<div/>');
  $('<div>生成一个div</div>');
  ```

- 元素添加

  - 内部添加（添加为子元素）

    - append(content|obj|ele|fn): 在元素内部最后面追加内容（后置）
    - prepend: 向元素内部增加内容（前置）

      > appendTo,prependTo

  - 外部插入内容（添加为兄弟元素）

    - after: 在元素后面插入内容
    - before: 在元素前面插入内容

      > insertAfter,insertBefore

> 如果页面上已经存在了要添加的元素，append,prepend,after,before会把元素移动到页面相应的位置

- 元素删除

  - remove(); 删除元素, 虽然元素从文档中删除了，但js内部依然保留对它引用
  - empty(); 清空内容

- 元素复制

  - clone([Event[,deepEvent]])
    - Event：（true 或 false）是否复制元素的行为，默认为false
    - deepEvent: （true 或 false）是否复制子元素的行为，默认为Even的值

### 盒模型属性

- .offset():获取匹配元素相对于根元素的偏移量  

  返回一个对象，包含当前元素的top,left值

- position():获取匹配元素相对(有定位属性)父元素的偏移量，如果没有定位父级，则相对于根元素(html)，返回一个对象，包含当前元素的top,left值。

- width(v) = width; //取值/赋值,当传入v时，相当于css(‘width’,v);

- innerWidth() = width + padding; <==> clientWidth

- outerWidth() = width + padding + border; <==> offsetWidth

- outerWidth(true) = width + padding + border + margin;

# 常用事件方法

- 鼠标事件

  - click([[data],fn]) //点击时触发 click = mousedown + mouseup
  - dblclick([[data],fn]) //双击事件 dblclick = 2*click
  - mousedown([[data],fn])
  - mouseup([[data],fn])
  - mousemove([[data],fn])
  - mouseout([[data],fn])
  - mouseover([[data],fn])
  - mouseenter([[data],fn]) //事件不会冒泡
  - mouseleave([[data],fn]) ) //事件不会冒泡

- 键盘事件

  - keydown([[data],fn]) //键盘按下时触发
  - keypress([[data],fn])//字符按键
  - keyup([[data],fn]) //键盘弹起时触发

- 表单事件

  - blur([[data],fn]) //失去焦点时触发
  - focus([[data],fn]) //获得焦点
  - change([[data],fn]) //值改变并失去焦点时触发
  - submit([[data],fn])

- 其他事件

  - resize([[data],fn]) //元素大小改变时触发
  - scroll([[data],fn]) //滚动时触发

### jquery事件绑定与移除

on(type,[[selecetor] ] ,fn )

- selecetor 把本来绑定的selecetor的事件委托给他的父级

- 事件命名空间 自定义事件（对事件加以细分）

  格式： 事件类型 . 自定义名字

- 一次性绑定多个事件 事件之间以空格隔开

- 支持自定义事件的绑定 

  $(ele).on('xxxxx',function(){})

  触发自定义事件：$(ele).trigger('xxxx');

- off 清除绑定事件 

1. off('click'); 清除当前元素的点击事件

2. off(); 清除当前元素的所有事件

3. off('click mouseover')  一次性清除多个事件 事件之间以空格隔开

4. off ('click.xxxx'); 清除命名空间事件

### 其他事件方法

- hover(enter[,leave])

  - enter:鼠标移入时执行
  - leave:鼠标移出时执行

  > hover方法内部使用mouseenter + mouseleave来实现效果

- trigger(type): 手动触发事件（即使事件没有发生，也能执行事件处理函数）

- triggerHandler(type): 这个方法会触发指定的事件类型上所有绑定的处理函数。但不会执行浏览器默认行为，也不会产生事件冒泡

- 阻止浏览器默认行为  

  event.preventDefault();

- 阻止事件传播  

  event.stopPropagation();

- 两者一起阻止：  

  return false;



## jQuery动画

### 基本动画效果

- 显示隐藏：show()/hide()

  - hide(duration)通过改变元素的高度、宽度、和不透明度，直到这三个属性值到0
  - show(duration)通过改变元素的高度、宽度、和不透明度，直至内容完全可见

- 滑动（通过改变高度）

  - slideDown([speed,callback])：
    1. 显示元素
    2. 不断改变高度，直到样式内设定的值
  - slideUp([speed,callback])：
    1. 不断改变高度，直到0
    2. 隐藏元素
  - slideToggle([speed,callback])  

    当元素隐藏时调用slideDown()，当元素显示时调用slideUp()

- 淡入淡出（通过改变不透明度）

  - fadeIn:  

    1)显示元素  

    2)不断改变透明度直到1

  - fadeOut:  

    1)不断改变透明度直到0  

    2)隐藏元素

  - fadeToggle([speed,callback])

  - fadeTo([[speed],opacity,[fn]]) 不断改变透明度opacity，直到设定的值，并在动画完成后可选地触发一个回调函数。

> PS：jQuery动画由三种预设速度slow,normal,fast（600，400，200）

### 自定义动画

- animate (params,[speed],[fn])

- :animated  

  获取正在执行动画的元素，一般与is()方法配合使用，用于判断元素是否处于动画状态

### 动画队列

- 一个元素上的动画：

  - 当animate中存在多个属性时，动画同时发生
  - 当同一个元素链式调用animate时，动画是按顺序发生(队列)

- 不同元素上的动画：

  - 默认情况下，动画同时发生
  - 回调函数内的动画等到当前动画执行完后才接着执行

- stop([clearQueue],[jumpToEnd])  

  不加参数：停止当前元素所有《正在运行》的动画。

  - clearQueue:值为true时，清除队列
  - jumpToEnd:值为true时，跳到当前动画的最后一帧

- delay(duration)  

  设置一个延时来推迟执行队列中之后的动画。

  - duration:延迟的时间



## ajax

### jQuery的ajax方法

- $.ajax(settings)

  - type:请求类型，默认GET
  - url:数据请求地址（API地址）
  - data:发送到服务器的数据对象，格式：{Key:value}。
  - success:请求成功时回调函数。
  - dataType:设定返回数据的格式，json, jsonp, text(默认), html, xml, script
  - async：是否为异步请求，默认true

- $.get(url,[data],[fn],[dataType]) // type:’get’

- $.post(url,[data],[fn],[dataType]) // type:’post’

- $.getJSON(url,[data],[fn]) // type:’get’, dataType:’json’

- $.getScript(url,[callback]) // type:’get’, dataType:’script’

- load(url,[data],[callback]) 载入远程 HTML 文件代码并插入页面中。
