
# VUE中的一些问题

## 生命周期详解
啥也不说了，先掏出祖传的生命周期图看一波，这里涉及到一个叫钩子函数的概念，在我理解就是像钩子一样，我们可以在vue的不同生命周期挂载自己的函数<br>
![vue生命周期](https://cn.vuejs.org/images/lifecycle.png)
- beforeCreate 发生在实例刚创建，属性还没有计算，这里取不到$el,$date，实际开发过程中我们可以在这里添加一个loading函数。
- created 这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调，这里我们可以访$date,实际开发过程中在这结束loading，数据初始化。
- beforeMount 挂载之前被调用，这里可以访问到$el,$date但是在$el中所有双向绑定的数据均使用{{xxxx}}占位。
- mounted 挂载之后被调用，这里可以访问到正常的$el,$date。实际开发过程中在这发现后端查询，这里可以操作dom。
- beforeUpdate 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。实际开发过程中可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
- updated 这里数据已经被更新，DOM 已重新渲染和补丁也已经打好，实际开发过程中避免在此期间更改状态，因为这可能会导致更新无限循环。
- activated keep-alive组件独有的状态，组件激活时调用。
- deactivated keep-alive组件独有的状态，组件停用时调用。
- beforeDestroy 实例销毁之前调用。在这一步，实例仍然完全可用，实际开发过程中可以做删除确认操作。
- destroyed 实例销毁之后调用。在这一步，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。实际开发过程中可以做内容清空操作。

## 响应式原理
![vue响应式原理](https://cn.vuejs.org/images/data.png)
将一个普通的js对象传给vue实例的date选项时，vue会使用Object.defineProperty会便利对象的属性将它们转换为getter和setter，每个vue实例都有一个wacther对象，当组件渲染的时候，watcher会通过getter将属性记录为依赖，当依赖的setter被调用时，会通知watcher重新渲染组件。

Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。如果我们想在 DOM 状态更新后做点什么，可以在数据变化之后立即使用 `Vue.nextTick(callback) `。这样回调函数在 DOM 更新完成后就会调用。
