---
title: Vuejs 和 reactjs 生命周期对比
user: Richard Chen
date: 2019-07-11 20:03:12
updated: 2019-07-11 20:03:12
comments: 1
categories: 
tags: []
---

## 创建阶段

* vuejs

| 名称 | 说明 |
|------|-----|
| beforeCreate | 在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。 |
| created | 在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。|
| beforeMount | 在挂载开始之前被调用：相关的 render 函数首次被调用。 |
| mounted | el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted |
| beforeDestroy | 实例销毁之前调用。在这一步，实例仍然完全可用。 |
| destroyed | Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 |

* react

| 名称 | 说明 |
|-------|------|
| constructor | 调用super(props) |
| componentWillMount | calling setState() synchronously in this method will not trigger an extra rendering. only lifecycle hook called on server rendering. |
| render | will not be invoked if shouldComponentUpdate() returns false |
| componentDidMount | Calling setState() will trigger an extra rendering |
| componentWillUnmount | Perform any necessary cleanup in this method |

## 更新阶段

* vuejs

| 名称 | 说明 |
|------|-----|
| beforeUpdate | 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。|
| updated | 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。 |
| activated | keep-alive 组件激活时调用。 |
| deactivated | keep-alive 组件停用时调用。 |
| errorCaptured | 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。 |

* react

| 名称 | 说明 |
|------|-----|
| componentWillReceiveProps | receives new props. doesn’t call during mounting. setState() generally doesn’t trigger |
| shouldComponentUpdate | 性能优化的关键。Defaults to true. Returning false does not prevent child components from re-rendering when their state changes. |
| componentWillUpdate |  perform preparation before an update occurs. not called for the initial render. 不能做会触发组件更新的操作（死循环） |
| componentDidUpdate | not called for the initial render.opportunity to operate on the DOM when the component has been updated. |
| componentDidCatch | 错误处理 |
