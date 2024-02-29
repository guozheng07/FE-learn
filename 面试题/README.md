# 导航守卫
导航守卫是一种特性，主要用于路由跳转时的权限控制。在Vue.js中，我们可以通过它来控制页面的访问权限，比如用户未登录时跳转到登录页面，用户登录后才能访问某些页面等。

Vue Router 提供的导航守卫主要有以下几种：
- 全局前置守卫：router.beforeEach
```
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  if (to.path === '/login') {
    next();
  } else {
    let token = localStorage.getItem('Authorization');
    if (token === null || token === '') {
      next('/login');
    } else {
      next();
    }
  }
})

```
- 全局解析守卫：router.beforeResolve
```
router.beforeResolve((to, from, next) => {
  // 确保要调用 next()，否则钩子就不会被 resolved。
  next()
})

```
- 全局后置钩子：router.afterEach
```
router.afterEach((to, from) => {
  // 这些钩子不会接受 next 函数，也不会改变导航本身
})
```
- 路由独享的守卫：beforeEnter
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // 这是路由独享的守卫
        next()
      }
    }
  ]
})
```
- 组件内的守卫：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave
```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    next()
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    next()
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    next()
  }
}
```
这些守卫都接收三个参数：
- to: 即将要进入的目标路由对象
- from: 当前导航正要离开的路由
- next: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

# 介绍redux，主要解决什么问题
# 文件上传如何做断点续传
# 表单可以跨域吗
# promise、async有什么区别
# 搜索请求如何处理（防抖）
# 搜索请求中文如何请求
# 介绍观察者模式
# 介绍中介者模式
# 观察者和订阅-发布的区别，各自用在哪里
# 介绍react优化
# 介绍http2.0
# 通过什么做到并发请求
# http1.1时如何复用tcp连接
# 介绍service worker
# 介绍css3中的position:sticky
# redux请求中间件如何处理并发
# 介绍Promise，异常捕获
# 介绍position属性（包括css3新增）
# 浏览器事件流向
# 介绍事件代理以及优缺点
# React组件中怎么做事件代理
# React组件事件代理的原理
# 介绍this各种情况
# 前端怎么控制管理路由
# 使用路由时出现问题如何解决
# React怎么做数据的检查和变化

# 面试题仓库
https://vue3js.cn/interview/
