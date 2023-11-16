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