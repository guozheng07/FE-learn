从 Web 前端诞生的第一天起，工程师们对于 Web 性能优化的探索就没有停止。但优化的手段是否有效、哪里还有性能优化空间、优化后的结果如何量化等问题一直是困扰开发者的难题。

本文归纳了目前主流的Web性能指标的分析方式，希望能够作为开发者在Web性能分析方法上较为全面的参考。

“页面加载速度快”是一个相对并且模糊的概念，那我们该如何拆解这个目标，制定技术方案进行逐个优化，让用户更快感知网页内容呢？

# 1.有哪些关键的性能指标？
我们通常通过性能指标来衡量Web应用是否具有良好的加载体验，无论是 Chrome Devtools 还是 W3C 提供的 Navigation Timing API 都提供了许多参考指标。这些指标中哪些是需要重点关注，能够真实反映页面加载和渲染效率的呢？这里先回顾一下浏览器渲染机制，了解一下需要注意的关键节点。

## 1.1 浏览器渲染过程
![image](https://user-images.githubusercontent.com/42236890/230043365-e6c883f6-61af-4e87-aa20-8ae082d58f05.png)

1. **获取HTML文档**。 浏览器从磁盘或网络读取 HTML 的原始字节。
2. **解析HTML文档**。 解析 HTML 内容，加载<head>和<body>中的静态文件。
3. **构建 DOM 和 CSSOM，合并成渲染树**。将 HTML 转换成令牌，形成树模型。
4. **布局和绘制**。渲染树生成“盒模型”，获得视口内的确切位置和大小，将渲染树节点绘制为屏幕上的实际像素。
5. **完全加载**。开始加载异步资源，同时也会下载字体、图片等远程文件。
  
经过以上5个阶段，就完成了网页的加载和渲染。**有效的利用Web性能指标的关键就在于抓住1~5阶段的关键时间点**，以此找到相对应的性能优化策略。
  
## 1.2 网页加载的关键时间点
1. **首字节时间**：指浏览器接收到 HTML 文档第一个字节的时间。此时间点之前，浏览器需要经过 DNS解析、重定向(若有)、建立 TCP/SSL 连接、服务器响应等过程。
2. **DOM 构建完成时间**：HTML 解析器完成 DOM 树构建的时间。此时间点之前，浏览器需要经过同步静态资源加载、内联 JavaScript 脚本运行、HTML 解析器生成 DOM 树的过程。
3. **DOM Ready 时间**：CSS树和DOM树合并渲染树后，并执行完成同步 JavaScript 脚本的时间。此时间点之前，包含了DOM树构建的过程、CSS树构建的过程、以及同步 JavaScript 脚本执行的时间。
4. **首屏时间**：网页布局和绘制的完成，将用户设备视窗范围内的DOM节点渲染完成的时间。若首屏中包含异步请求才能完成渲染的内容，则需要包含等待异步请求和页面重绘的时间。
5. **页面完全加载时间**：所有处理完成，并且网页上的所有资源（图像等）都已下载完毕的时间。此时会触发浏览器 onload 事件。

以上这些时间点，该如何进行检查和使用呢？在 2012年以后 W3C 提供了 Navigation Timing API 指标接口，我们尝试找出与上述各时间点之间的对应关系。
  
## 1.3 W3C 提供的性能指标
试图找出性能指标和页面渲染过程的对应关系：

![image](https://user-images.githubusercontent.com/42236890/230045861-7419a7c8-087e-4b90-9d59-63fe645d49a9.png)

W3C提供了大量的性能指标，对能够衡量性能的关键时间，这里做了整理：
- 首字节时间：responseStart
- Dom 构建完成时间：domInteractive
- Dom Ready时间：DOMContentLoadedEventEnd
- 页面完全加载时间：loadEventStart
- 首屏时间：**无法体现**

Navigation Timing API 提供了文档构建过程中的多数性能指标，**对于传统的直出型页面，由于没有模块的异步加载页面过程，所以我们认为它的首次渲染时间即为DOM构建完成时间，页面渲染完毕即为页面完全加载时间**。

但是**对于 SPA（Single Page Application） 类型的页面，由于存在异步模块及远程数据的加载过程，需要找到一个能够衡量首次渲染和首屏时间的指标**，于是下面会对 SPA 模式下的网页渲染指标进行更深入的探索。

# 2.SPA渲染指标的探索与验证
SPA 页面与传统直出页面最大不同在于 **JS 对象与 DOM 结构之间的相互响应**。由于这种模式在 HTML 解释器在遍历主文档时只挂载一个<div id="app"> 节点，当 HTML 结构解析完毕时，真实的页面主体还未从 JS 对象转换成 DOM 插入页面。因此针对这种情况，需要单独分析与验证，我们先看下SPA 框架是什么时候完成首次渲染/首屏渲染的？如下图。

![image](https://user-images.githubusercontent.com/42236890/230046633-49f11e2d-2d11-4d6f-96a6-1b8a8e768d86.png)

## 2.1 如何计算 SPA 的首次渲染时间？
SPA 是何时将首屏DOM节点插入网页中的呢？以 React 为例，准备了一个简单的场景进行验证。
```
// App.js

const App = () => {
// ... 省略
  return (
    <div>
      <h1>要开始首次渲染了哦</h1>
      <input value={inputValue} onChange={e => setInputValue(e.target.value)} />
    </div>
  )
}
```
利用 Chrome DevTools for Performance，观察 React 在渲染过程中性能相关的时间节点。

![image](https://user-images.githubusercontent.com/42236890/230046969-e2931a34-5359-4deb-9424-b2f92bb3692e.png)

Chrome DevTools for Performance 我们从上到下关注 Network、Timings和Main 三栏。Network 记录了从页面开始加载后，资源请求的情况。Timings标记了一些重要的时间点如（DomContentLoaded、First Paint、Loaded）。Main展示了浏览器于何时会执行HTML解析与JavaScript文件解析。

页面开始加载后，浏览器按以下顺序执行页面解析构建与渲染：
- 请求所有相关资源
- React接管渲染，开始执行渲染
- 浏览器标记DCL时间
- 浏览器开始渲染
- 首屏时间
- 浏览器完成渲染，标记Loaded时间

可以看到 React 项目在 DCL（SPA框架逻辑执行结束） 后，浏览器进行了首次渲染。在真实场景的项目中，也进行了多次同样的验证，观察到的现象与前述一致。

由此我们得出结论：SPA 页面会在渲染树构建完成后执行 SPA 框架逻辑，将虚拟 DOM 转换成 HTML 插入页面，**首屏 DOM在 DCL 时间点完成插入，此时开始页面主体渲染**。
  
## 2.2 如何计算 SPA 的首屏时间？
根据上面的结论，DCL 代表了 JavaScript 框架执行逻辑插入页面主体 DOM，这也代表了页面主体开始渲染的时间。那对于用户来说，页面首屏渲染完成的时间点怎么计算呢？因此 Raptor 提出首屏时间（First Screen Time）的采集计算模型。

计算模型中将首屏时间定义为浏览器视窗范围内的 DOM 达到稳定状态的时间。为了监测这个稳定状态，核心算法主要使用接口：MutationObserver。

MutationObserver 实例能够监听 DOM 变化并执行回调。**当一定时间内没有再监听到首屏内 DOM 变化或首屏外mutation超过一定次数时，即可得出结果并停止监听**。

![image](https://user-images.githubusercontent.com/42236890/230048356-cabad3ed-c48b-4059-bbca-5b0d6dc66244.png)

此算法计算的首屏时间，在大部分场景下，与自定义打点的首屏时间是相近的，误差并不大，可以作为统计首屏时间的依据。

# 3.利用Web性能指标排查性能问题
我们已经掌握了5个 Web 页面要关注的关键性能指标，正确掌握它们的使用姿势，才能准确分析页面性能的问题所在，从而进行针对性的优化。那么不同阶段的耗时过长，分别代表了一些什么问题呢？

![image](https://user-images.githubusercontent.com/42236890/230048822-7f2da134-7bcf-421f-9c89-092218306663.png)

![image](https://user-images.githubusercontent.com/42236890/230049019-2954715b-bff7-4731-9a51-be1b84f640b5.png)

![image](https://user-images.githubusercontent.com/42236890/230049082-1c409635-63b5-4fa2-abd4-09cb4dc007e6.png)

依据上表，Web性能优化的关键在于尽快下载处理关键资源，同时消除非关键资源的阻塞，让用户花在网站上的大多数时间是在使用时等待响应，而不是等待资源的加载。
