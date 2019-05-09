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

    2. **axios(url[, config])**

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
