# 20240229
1.浏览器中的异常捕获
- window.onerror
- unhandledrejection：当 Promise 被 reject 且没有 reject 处理器的时候，会触发 unhandledrejection 事件；这可能发生在 window 下，但也可能发生在 Worker 中。这对于调试和为意外情况提供后备错误处理非常有用。
```
// 将有关未处理的 Promise rejection 信息打印到控制台
window.addEventListener("unhandledrejection", (event) => {
  console.warn(`UNHANDLED PROMISE REJECTION: ${event.reason}`);
});

```

2.Vue中的异常捕获
- app.config.errorHandler：为应用内抛出的未捕获错误指定一个全局处理函数。
  - 错误处理器接收三个参数：错误对象、触发该错误的组件实例和一个指出错误来源类型信息的字符串。
  - 可以从下面这些来源中捕获错误：组件渲染器、事件处理器、生命周期钩子、setup() 函数、侦听器、自定义指令钩子、过渡 (Transition) 钩子。
- app.config.warnHandler：为 Vue 的运行时警告指定一个自定义处理函数。
  - 警告处理器将接受警告信息作为其第一个参数，来源组件实例为第二个参数，以及组件追踪字符串作为第三个参数。
  - 可以用于过滤筛选特定的警告信息，降低控制台输出的冗余。所有的 Vue 警告都需要在开发阶段得到解决，因此仅建议在调试期间选取部分特定警告，并且应该在调试完成之后立刻移除。
- errorCaptured：在捕获了后代组件传递的错误时调用。
  - 这个钩子带有三个实参：错误对象、触发该错误的组件实例，以及一个说明错误来源类型的信息字符串。
  - 错误可以从以下几个来源中捕获：组件渲染器、事件处理器、生命周期钩子、setup() 函数、侦听器、自定义指令钩子、过渡 (Transition) 钩子。

3.深入->前端所有错误捕获方法和上报方案
- js运行时错误：window.onerror与window.addEventListener('error')捕获
- 定时器内部函数抛出错误：window.onerror与window.addEventListener('error')捕获
- 资源加载错误：window.addEventListener('error')捕获
- iframe内部异常：window.onerror捕获
- 未处理的promise错误处理方式：window.addEventListener('unhandledrejection')捕获
- fetch与xhr错误的捕获：改写它们的原生方法，在触发错误时进行自动化的捕获和上报
- script error：script 标签添加 crossOrigin 属性 + 添加跨域 HTTP 响应头，通过 window.onerror 捕获跨域脚本的报错信息
- React错误处理：componentDidCatch
- Vue错误处理：Vue.config.errorHandler

![image](https://github.com/guozheng07/FE-learn/assets/42236890/fc17fcd6-cec1-4288-aaaf-98db924ff017)
