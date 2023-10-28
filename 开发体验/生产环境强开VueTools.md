https://blog.eh5.me/force-enable-vue-devtools/

1. 找到 Vue 根实例的挂载元素，例如id名称为app的div元素
2. 在浏览器console面板中执行如下代码（以下代码可以适用于https://www.bilibili.com/）
```
// Vue 实例是挂载在元素的 `__vue__` 属性上的
app = document.querySelector('#app').__vue__

// 获取此实例的构造函数
Vue = app.constructor

// 获取 `Vue` 基类，只有基类上有 `Vue.config` 属性
while (Vue.super) { Vue = Vue.super }

// 尝试打印 Vue.config
console.log(Vue.config)

Vue.config.devtools = true // 开启devtools
__VUE_DEVTOOLS_GLOBAL_HOOK__.Vue = Vue

```
3. 关闭并重新打开“开发者工具”面板