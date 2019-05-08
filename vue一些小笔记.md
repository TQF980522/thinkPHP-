# vue 笔记



### 一，路由带参数跳转

vue中路由跳转传参数有多种,自己常用的是下面的几种

*  通过`router-link`进行跳转

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


