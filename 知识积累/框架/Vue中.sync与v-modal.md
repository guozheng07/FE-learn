# Vue 中的双向绑定
Vue 中的 props 数据是单向流动的，即每次父组件更新时，子组件中的所有 props 都会刷新为最新的值。如果在子组件中修改 props，Vue 会向你发出一个警告，且并不能通过修改子组件的数据来更改父组件的数据。

如果有需求，我们可以在父组件中，子组件的标签上声明一个监听事件，在子组件想要修改数据时使用 $emit 触发事件并传入新的值，通知父组件进行修改。这就可以实现某种程度上的双向绑定。真正的双向绑定会带来维护上的问题，因为子组件可以变更父组件，且在父组件和子组件两侧都没有明显的变更来源。

**Vue 团队推荐以 update:myPropName 的模式触发事件取代直接修改的操作。这就用到了 .sync 修饰符。**

# Vue 中的 .sync 修饰符
自定义组件中，v-bind 命令的 .sync 修饰符同 v-model 一样，其实本质上都是 Vue 的语法糖，用于实现父子组件间接的数据双向绑定。需要注意的一点是，Vue3 中已经不再有 .sync 修饰符了，新的 v-model 取代了 Vue2 中的 v-model 和 .sync 修饰符。这里讨论的是 Vue2 中 的 .sync 修饰符。

一言以蔽之，**v-bind 命令的.sync 修饰符实质就是父组件监听子组件更新某个 props 的请求的缩写语法，一种语法糖。**
```
<Child v-bind:title.sync="title" /> 
等价于
<Child 
	v-bind:title="title" 
	v-on:update:title="title = $event" 
/>
与之配合，在子组件中，需要添加下面这段代码来通知父组件对这个 prop 重新赋值：
this.$emit('update:title', newTitle)
```
这一个 $emit 可以通过绑定事件触发，也可以使用 watch 监听等方式来触发。

此外，**当我们用一个对象 obj 同时设置多个 prop 的时候，也可以将这个 .sync 修饰符和 v-bind 配合使用**：
```
<Son v-bind.sync="obj"></Son>

data(){ 
  return { 
  	obj:{
      read: true, 
      name:'', 
      title:'', 
      length:''
    } 
  } 
},
```
这样会把 obj 对象中的每一个 property (如 title) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器。

# .sync 使用注意事项
需要注意，**带有 .sync 修饰符的 v-bind 命令不能和表达式一起使用**。例如 :title.sync="doc.title + '!'" 是无效的。你只能提供你想要绑定的 property 名，类似 v-model。

另外还需要注意**将 v-bind.sync 用在一个字面量的对象上**，例如 v-bind.sync="{ title: doc.title }"，**是无法正常工作的**，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。

# .sync 与 v-model 的比较
.sync 从功能上看和 v-model 十分相似，都是为了实现数据的“双向绑定”，本质上，也都不是真正的双向绑定，而是语法糖，这是他们的相同之处。

**相比较之下，.sync 更加灵活，它可以在一个组件内给多个 prop 使用，而 v-model 在一个组件中只能有一个 prop，在 Vue2 中是这样**。（需要注意，**Vue3中 v-model 已经可以给多个 prop 使用了**）。

**从语法的内容来看，v-model 绑定的值和触发的事件是较为固定的，根据不同类型的特定组件有对应的绑定值和事件**，比如 input 组件，select 组件等表单组件，日期时间选择组件，颜色选择器组件等，这些组件所绑定的值使用 v-model 比较合适。

**其他情况下，需要父子组件之间数据相互更新，还是使用 .sync。.sync 针对更多的是各种的状态变更，在父子组件之间互相传递，是一种 update 操作**。

