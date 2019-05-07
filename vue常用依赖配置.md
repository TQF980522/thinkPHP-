# vue常用依赖配置



## **vue之vue-cookies**

安装：

`npm install vue-cookies --save`

使用：

`import Vue from 'Vue' `

`import VueCookies from 'vue-cookies'`

` Vue.use(VueCookies)`

Api:

- 设置 cookie：

  `this.$cookies.set(keyName, time)   //return this`

- 获取cookie:

  `this.$cookies.get(keyName)       // return value`

- 删除 cookie:

  `this.$cookies.remove(keyName)   // return  false or true , warning： next version return this； use isKey(keyname) return true/false,please`

- 查看一个cookie是否存在（通过`keyName`）

  `this.$cookies.isKey(keyName)        // return false or true`

- 获取所有cookie名称

  `this.$cookies.keys()  // return a array`


