# vue 笔记

### 一，路由带参数跳转

vue中路由跳转传参数有多种,自己常用的是下面的几种

* 通过`router-link`进行跳转

* 通过编程导航进行路由跳转 

1. ##### router-link

   ```
   
   <router-link 
       :to="{
           path: 'yourPath', 
           params: { 
               name: 'name', 
               dataObj: data
           },
           query: {
               name: 'name', 
               dataObj: data
           }
       }">
   </router-link>
   
    1. path -> 是要跳转的路由路径,也可以是路由文件里面配置的 name 值,两者都可以进行路由导航
    2. params -> 是要传送的参数,参数可以直接key:value形式传递
    3. query -> 是通过 url 来传递参数的同样是key:value形式传递
   
    // 2,3两点皆可传递
   ```

2. ##### $router方式跳转(编程式导航跳转)

   ```
   
   // 组件 a
   <template>
       <button @click="sendParams">传递</button>
   </template>
   <script>
     export default {
       name: '',
       data () {
         return {
           msg: 'test message'
         }
       },
       methods: {
         sendParams () {
           this.$router.push({
               path: 'yourPath', 
               name: '要跳转的路径的 name,在 router 文件夹下的 index.js 文件内找',
               params: { 
                   name: 'name', 
                   dataObj: this.msg
               }
               /*query: {
                   name: 'name', 
                   dataObj: this.msg
               }*/
           })
         }
       },
       computed: {
   
       },
       mounted () {
   
       }
     }
   </script>
   <style scoped></style>
   ```

   ```
   // 组件b
   <template>
       <h3>msg</h3>
   </template>
   <script>
     export default {
       name: '',
       data () {
         return {
           msg: ''
         }
       },
       methods: {
         getParams () {
           // 取到路由带过来的参数 
           let routerParams = this.$route.params.dataobj
           // 将数据放在当前组件的数据内
           this.msg = routerParams
         }
       },
       watch: {
       // 监测路由变化,只要变化了就调用获取路由参数方法将数据存储本组件即可
         '$route': 'getParams'
       }
     }
   </script>
   <style scoped></style>
   ```

## VUE 使用AXIOS 异步

安装：

```
$ npm install axios
```

**安装其他插件的时候，可以直接在 main.js 中引入并使用 Vue.use()来注册，但是 axios并不是vue插件，所以不能 使用Vue.use()，所以只能在每个需要发送请求的组件中即时引入。**

**为了解决这个问题，我们在引入 axios 之后，通过修改原型链，把axios挂载到vue的原型链上，来更方便的使用。**

```
//在main.js中挂载

import axios from 'axios'
Vue.prototype.$http = axios
```

#### 1，执行 GET 请求：

```
// 向具有指定ID的用户发出请求
$http.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
// 也可以通过 params 对象传递参数
$http.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

#### 2，执行 POST请求：

```
$http.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

#### 3，执行多个并发请求：

```
function getUserAccount() {
  return $http.get('/user/12345');
}
function getUserPermissions() {
  return $http.get('/user/12345/permissions');
}
axios.all([getUserAccount(), getUserPermissions()])
  .then($http.spread(function (acct, perms) {
    //两个请求现已完成
  }));
```

### 4，axios API

可以通过将相关配置传递给 axios 来进行请求。

1. **axios(config)**

```
// 发送一个 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```

    2. **axios(url[, config])**

```
// 发送一个 GET 请求 (GET请求是默认请求模式)
axios('/user/12345');
```

### 请求方法别名

为了方便起见，已经为所有支持的请求方法提供了别名。

- axios.request（config）
- axios.get（url [，config]）
- axios.delete（url [，config]）
- axios.head（url [，config]）
- axios.post（url [，data [，config]]）
- axios.put（url [，data [，config]]）
- axios.patch（url [，data [，config]]）

**注意**  
当使用别名方法时，不需要在config中指定url，method和data属性。

### 并发

帮助函数处理并发请求。

- axios.all（iterable）
- axios.spread（callback）

# * Vue中的组件通信

* **父组件传值给子组件**

  `父组件给子组件传值，使用props`

  ```
  //父组件：parent.vue
  <template>
      <div>
          <child :vals = "msg"></child>
      </div>
  </template>
  <script>
  import child from "./child";
  export default {
      data(){
          return {
              msg:"我是父组件的数据，将传给子组件"
          }
      },
      components:{
          child
      }
  }
  </script>

  
  //子组件：child.vue
  <template>
      <div>
          {{vals}}
      </div>
  </template>
  <script>
  export default {
        props:{ //父组件传值 可以是一个数组，对象
          vals:{
              type:String,//类型为字符窜
            default:"123" //可以设置默认值
          }
      },
  }
  </script>
  ```

* **子组件到父组件的通信**

  `使用 $emit(eventname,option) 触发事件,`

  参数一：自定义事件名称，写法,小写或者用-连接，如event,event-name 不能写驼峰写法(eventName)

  子组件给父组件传值，可以在子组件中使用$emit触发事件的值传出去,父组件通过事件监听，获取数据

  但是，这里有一个问题，

  1、究竟是由子组件内部主动传数据给父组件，由父组件监听接收（由子组件中操作决定什么时候传值）

  2、还是通过父组件决定子组件什么时候传值给父组件，然后再监听接收 （由父组件中操作决定什么时候传值）

  两种情况都有

  2.1 : $emit事件触发,通过子组件内部的事件触发自定义事件$emit

  2.2 : $emit事件触发， 可以通过父组件操作子组件 (ref)的事件来触发 自定义事件$emit

  第一种情况：

  ```
  //父组件：parent.vue
  <template>
      <div>
          <child  v-on:childevent='wathChildEvent'></child>
          <div>子组件的数据为：{{msg}}</div>
      </div>
  </template>
  <script>
  import child from "./child";
  export default {
      data(){
          return{
              msg:""
          }
      },
      components:{
          child
      },
      methods:{
          wathChildEvent:function(vals){//直接监听 又子组件触发的事件，参数为子组件的传来的数据
              console.log(vals);//结果：这是子组件的数据，将有子组件操作触发传给父组件
              this.msg = vlas;
          } 
      }
  }
  </script>
  
  //子组件：child.vue
  <template>
      <div>
         <input type="button" value="子组件触发" @click="target">
      </div>
  </template>
  <script>
  export default {
      data(){
              return {
              texts:'这是子组件的数据，将有子组件操作触发传给父组件'
              }
      },
      methods:{
          target:function(){ //有子组件的事件触发 自定义事件childevent
              this.$emit('childevent',this.texts);//触发一个在子组件中声明的事件 childEvnet
          }
      },
  }
  </script>
  ```

  第二种情况

  ```
  //父组件：parent.vue
  <template>
      <div>
          <child  v-on:childevent='wathChildEvent' ref="childcomp"></child>
          <input type="button" @click="parentEnvet" value="父组件触发" />
          <div>子组件的数据为：{{msg}}</div>
      </div>
  </template>
  <script>
  import child from "./child";
  export default {
      data(){
          return{
              msg:""
          }
      },
      components:{
          child
      },
      methods:{
          wathChildEvent:function(vals){//直接监听 又子组件触发的事件，参数为子组件的传来的数据
              console.log(vals);//这是子组件的数据，将有子组件操作触发传给父组件
              this.msg = vlas;
          },
          parentEnvet:function(){
              this.$refs['childcomp'].target(); //通过refs属性获取子组件实例，又父组件操作子组件的方法触发事件$meit
          }
      }
  }
  </script>
  
  //子组件：child.vue
  <template>
      <div>
        <!-- dothing..... -->
      </div>
  </template>
  <script>
  export default {
      data(){
              return {
              texts:'这是子组件的数据，将有子组件操作触发传给父组件'
              }
      },
      methods:{
          target:function(){ //又子组件的事件触发 自定义事件childevent
              this.$emit('childevent',this.texts);//触发一个在子组件中声明的事件 childEvnet
          }
      },
  }
  </script>
  ```

  `将两者情况对比，区别就在于$emit 自定义事件的触发是有父组件还是子组件去触发`

  `第一种，是在子组件中定义一个click点击事件来触发自定义事件$emit,然后在父组件监听`

  `第二种，是在父组件中第一一个click点击事件，在组件中通过refs获取实例方法来直接触发事件,然后在父组件中监听`

* **兄弟组件之间的通信**

   1.兄弟之间传递数据通过事件

   2.创建一个新Vue的实例，让各个兄弟共用同一个事件机制。（关键点）

  3.传递数据方，通过事件触发$emit(方法名，传递的数据)。

   4.接收数据方，在mounted()钩子函数(挂载实例)中 触发事件$on(方法名，callback(接收数据的数据))，此时callback函数中的this已经发生了改变，可以使用箭头函数。

  例子：

  ```
  //建立一个空的Vue实例,将通信事件挂载在该实例上
  //emptyVue.js
  import Vue from 'vue'
  export default new Vue()
  
  //兄弟组件a:childa.vue
  <template>
      <div>
          <span>A组件->{{msg}}</span>
          <input type="button" value="把a组件数据传给b" @click ="send">
      </div>
  </template>
  <script>
  import vmson from "./emptyVue"
  export default {
      data(){
          return {
              msg:"我是a组件的数据"
          }
      },
      methods:{
          send:function(){
              vmson.$emit("aevent",this.msg)
          }
      }
  }
  </script>

  
  //兄弟组件b:childb.vue
  <template>
       <div>
          <span>b组件,a传的的数据为->{{msg}}</span>
      </div>
  </template>
  <script>
  import vmson from "./emptyVue"
  export default {
   data(){
          return {
              msg:""
          }
      },
      mounted(){
          vmson.$on("aevent",(val)=>{//监听事件aevent，回调函数要使用箭头函数;
              console.log(val);//打印结果：我是a组件的数据
              this.msg = val;
          })
      }
  }
  </script>
  

  //父组件：parent.vue
  <template>
      <div>
          <childa><childa>
          <childb></childb>
      </div>
  </template>
  <script>
  import childa from "./childa";
  import childb from "./childb";
  export default {
      data(){
          return{
              msg:""
          }
      },
      components:{
          childa,
          childb
      },
    
  }
  </script>
  ```
