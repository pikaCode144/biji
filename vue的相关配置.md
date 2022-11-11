# npm安装

## 降低版本安装

​	npm的版本太高导致一些插件或者库安装不上，降低npm版本就可以

```cmd
npx -p npm@6 npm i --legacy-peer-deps
```

# axios 的使用

## 1.axios安装

```javascript
npm i axios
```

##2.main.js中的配置

```javascript
// 引入axios
import axios form 'axios';
// 将axios放入Vue原型上
Vue.prototype.$http = axios;
// 全局定义默认的baseURL
axios.defaults.baseURL = 'URL';
```

## 3.axios二次封装

​	在src中创建一个名为api的文件夹，api文件夹中创建一个request.js文件，在此文件进行axios的二次封装

```javascript
// 对于axios进行二次封装
import axios from 'axios';

// 1.利用axios对象的方法create，去创建一个axios实例
const requests = axios.create({
    // 配置对象
    // 基础路径，发送请求的时候，路径当中会出现api
    baseURL: 'http://gmall-h5-api.atguigu.cn',
    // 代表请求超时的时间5s
    timeout: 5000,
});

// 请求拦截器：在发送请求之前，请求拦截器可以监测到，可以在请求发出去之前做一些事情
requests.interceptors.request.use((config) => {
    // config：配置对象，对象里面有一个属性很重要，headers请求头
    return config;
})

// 响应拦截器
requests.interceptors.response.use((res) => {
    // 成功的回调函数：服务器相应数据回来以后，响应拦截器可以检测到，可以做到一些事情
    return res.data;
}, (error) => {
    // 相应失败的回调函数
    return Promise.reject(new Error('faile'));
})

// 对外暴露
export default requests;
```

## 4.API接口同意管理

​	在src中创建一个名为api的文件夹，api文件夹中创建一个index.js文件，在此文件进行axios发送请求

```javascript
// 当前这个模块：API进行统一管理
import requests from './request';

// 发请求：axios发请求返回结果Promise对象
export const reqCategoryList = () => {
    requests({
        url: '/api/product/getBaseCategoryList',
        method: 'GET'
    });
};
```

​	采用的是分别暴露，利用解构赋值的方法进行引入

```javascript
import { reqCategoryList } from './api';
reqCategoryList();
```



## 5.VueCompent中发出请求

```javascript
this.$http.post('/api/users/login',{
    params: {
        email: this.email.trim(),
        password: this.pwd.trim()
    }
})
    .then((response) => {
        console.log("访问接口成功");
        console.log(response);
    })
    .catch((error) => {
        console.log("访问接口失败");
        alert(error.message)
    });
```



# Vue-router的使用

##1.Vue-router安装

```javascript
npm i vue-router@3 // vue2用3版本 vue3用4版本
```

## 2.main.js中的配置

```javascript
// 引入路由器
import router from './router';
// 创建Vue实例
new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

## 3.router文件夹下的index.js

```javascript
// 引入vue
import Vue from "vue";
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 使用VueRouter插件
Vue.use(VueRouter);
// 引入组件
import UserLogin from '../pages/UserLogin'
// 创建一个路由器
export default new VueRouter({
    routes: [
        { path: '/', redirect: '/UserLogin'}, // redirect重定向
        { 
		   name: '',
            path: '/UserLogin',
            component: UserLogin,
            meta: { show: true } // 路由元信息
            children: [
        	    { path: '', component: ''}
		   ]
        },
    ]
})
```

## 4.路由导航

1. 声明式路由导航
2. 编程式路由导航

```javascript
// 声明式路由导航
<router-link to="/login">登录</router-link>
// 编程式路由导航
<button class="sui-btn btn-xlarge btn-danger" type="button" @click="goSearch">搜索</button>
methods: {
    goSearch() {
        this.$router.push("/search/:keyword")
    }
}
// :keyword用于params参数的占位符，这个参数必须要传，不传路由跳转之后的路径会出现问题。
// :keyword?同上，但是这个参数不是必须要传的，可以省略。
```

### 编程式路由导航传递参数：

```javascript
// 第一种：字符串形式
this.$router.push("/search/" + this.keyword + '?k=' + this.keyword.toUpperCase());
// 第二种：模板字符串
this.$router.push(`/search/${this.keyword}?k=${this.keyword.toUpperCase()}`);
// 第三种：对象形式
this.$router.push({
    name: 'search',
    params: {
        keyword: this.keyword
    },
    query: {
        k: this.keyword.toUpperCase()
    }
})
// 对象方式绑定路由一般在route声明name属性，其他方式用path绑定即可。
// 路由跳转传参的时候，对象的写法可以是name、path形式，但是需要注意的是：path这种写法不能与params参数一起使用,以下方法是错误的。
this.$router.push({
    path: '/search',
    params: {
        keyword: this.keyword
    },
    query: { 
        k: this.keyword.toUpperCase() 
    }
})
```

###路由传参props

```javascript
routes: [
    {
        path: "/search/:keyword?",
        component: Search,
        // 布尔值写法：params 只能传params参数
        // props:true,
        // 对象写法：额外的给路由组件传递一些props
        // props:(a:1,b:2),
        // 函数写法：可以params参数、query参数，通过props传递给路由组件
        props:($route) => ({keyword:$route.params.keyword,k:$route.query.k}),
    }
]
```

###路由出口

```html
<router-view></router-view>
```

###编程式路由导航出现的问题

​	编程式路由导航跳转到当前路由(参数不变)，多次执行会抛出NavigationDuplicated的警告报错。

​	声明式路由导航不会出现这种问题，因为底层给处理掉了

​	为什么会出现这个问题？

```javascript
// 因为this.$router.push()返回的是一个promise状态
let res = this.$router.push({
    name: 'search',
    params: {
    	keyword: this.keyword 
    },
    query: {
        k: this.keyword.toUpperCase()
    }           
})
console.log(res);
// Promise {<fulfilled>: {…}}
// [[Prototype]]: Promise
// [[PromiseState]]: "fulfilled"
// [[PromiseResult]]: Object
// promise会返回一个成功的回调或这一个失败的回调
// 因为参数不变，所以promise是失败的，所以返回的失败的回调就抛在了控制台上

// 解决方法
// 接收返回的状态，但不输出，巧妙的解决办法。
this.$router.push({
    name: 'search',
    params: {
    	keyword: this.keyword 
    },
    query: {
        k: this.keyword.toUpperCase()
    }
}, () => { }, () => { }
)
```

###重写push|replace

```javascript
// index.js
// 先把VueRouter原型对象的push，先保存一份
let originPush = VueRouter.prototype.push; // 推送一个路由
let originReplace = VueRouter.prototype.replace; // 更改一个路由

// 重写push|replace，进行二次封装。
// 第一个参数：告诉原来push方法，你往哪里跳转（传递哪些参数）
// 第二个参数：成功的回调
// 第三个参数：失败的回调
// call||apply区别
// 相同点，都可以调用函数一次，都可以篡改函数的上下文一次
// 不同点，call与apply传递参数，call传递参数用逗号隔开，apply方法执行，传递数组
VueRouter.prototype.push = function (location, resolve, reject) {
    if (resolve && reject) {
        originPush.call(this,location,resolve,reject);
    } else {
        originPush.call(this,location, () => { }, () => { });
    }
}

VueRouter.prototype.replace = function (location, resolve, reject) {
    if (resolve && reject) {
        originreplace.call(this,location,resolve,reject);
    } else {
        originreplace.call(this,location, () => { }, () => { });
    }
}
```

# Vuex的使用

## 1.Vuex的安装

```javascript
npm i vuex@3
```

## 2.Vuex的配置

​	在src下创建一个store文件夹，里面创建index.js。

```javascript
import Vue from 'vue';
import Vuex from 'vuex';
// 使用Vuex
Vue.use(Vuex);

// state: 仓库存储数据的地方
const state = {};
// mutations:修改state的唯一手段
const mutations = {};
// actions:处理action，可以书写自己的业务逻辑，也可以处理异步
const actions = {};
// getters:理解为计算属性，用于简化仓库数据，让组件获取仓库的数据更加方便
const getters = {};

// 创建并暴露Store
export default new Vuex.Store({
    state,
    mutations,
    actions,
    getters
})
```

## 3.Vuex模块式开发的配置

```javascript
import Vue from 'vue';
import Vuex from 'vuex';
// 使用Vuex
Vue.use(Vuex);
// 引入小仓库
import home from './home';
import search from './search';
// 创建并暴露Store
export default new Vuex.Store({
    // 实现Vuex仓库模块式开发存储数据
    modules: {
        home,
        search
    }
})
```

​	在store文件夹下建小仓库home文件夹，里面包含index.js文件

```javascript
// home模块的小仓库
const actions = {};
const mutations = {};
const state = {};
const getters = {};
export default {
    actions,
    mutations,
    state,
    getters
}
```

​	其他模块化小仓库同上

# Vue-cli中的less

## 1.less的安装

```javascript
npm i less-loader
```

# nprogress进度条的使用

## 1.nprogress的安装

```javascript
npm i nprogress
```

## 2.nprogress的使用

​	在src/api/request.js中使用

```javascript
// 引入进度条
import nprogress from 'nprogress'; // start: 进度条开始 done: 进度条结束
// 引入进度条样式
import "nprogress/nprogress.css";
// 在请求拦截器里面开始
requests.interceptors.request.use((config) => {
    // 进度条开始
    nprogress.start();
    return config;
})
// 在响应拦截器里面结束
requests.interceptors.response.use((res) => {
    // 成功的回调函数：服务器相应数据回来以后，响应拦截器可以检测到，可以做到一些事情
    // 进度条结束
    nprogress.done();
    return res.data;
}, (error) => {
    // 相应失败的回调函数
    return Promise.reject(new Error('faile'));
})
```

​	进度条的样式默认为蓝色的，如果要改颜色可以去nprogress/nprogress.css改

# 前端pdf和excel导出

## 通过js-file-download插件

### 安装

```cmd
npm install js-file-download --S
```

### 使用

```javascript
import axios from 'axios'
import fileDownload from 'js-file-download'

axios.get('文件地址', {
    responseType: 'blob'	//  返回的数据类型
})
    .then(res => {
    fileDownload(res.data, this.fileName)	//  this.fileName 文件名
})
```

### 发请求

```html
<Button type="primary" @click="downloadReports()">
    <span class="iconfont">
    	下载报表
    </span>
</Button>
```

```javascript
export defult {
    data () {
        return {
            downloadUrl: ''
        }
    },
    methods: {
        downloadReports () {
            reqDownloadExcel().then(res => {	// reqDownloadExcel请求接口的函数
                const downloadPath = res.excel_path
                this.downloadUrl = downloadPath		// 将从后端获取的文件地址付给downloadUrl变量
            }).catch(err => console.log(err))
            console.log('获得地址', this.downloadUrl)
            axios.get(this.downloadUrl, { responseType: 'blob' })
            	.then(res => { fileDownload(res.data, '分析报表.xls') })
        }
    }
}
```
