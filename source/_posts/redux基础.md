<!--
 * @Author: kristennn 13949836783@163.com
 * @Date: 2022-05-17 17:11:52
 * @LastEditors: kristennn 13949836783@163.com
 * @LastEditTime: 2022-07-16 17:57:07
 * @FilePath: /blog/source/_posts/redux基础.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->

---

title: redux 基础
date: 2022-05-17 17:11:52
tags: []
categories: [前端, React]

---

# 一、什么是 redux

老生常谈之 Flux 与 Redux 思想 https://juejin.cn/post/6844903806644256782

React 是一个视图层框架，它主要负责的点是：数据 与 界面之间的交互渲染。
而在一个中大型项目中，涉及的组件多，组件之间的嵌套关系复杂，组件间数据的传递和使用就难以管理和维护。
一个数据可能有多个地方修改，导致有问题时很难排查。
数据层的不稳定性太高。

针对这个问题，facebook 团队先开发了 flux，一种应用程序中数据流的设计模式。基于这个设计思想，redux 诞生了。

# 二、redux 的基本思想-单向数据流

![](img.png)

为了保证数据的可维护性，将数据统一放在 store 中管理起来。
这个思想的核心是：管理数据的更新流程。
view 层可以通过接口从 store 中获取 state，view 层要更改数据，需通过构建 action，由 store 来更改数据。
这样的数据流向是单向的，数据的更新都有迹可循。整体可维护性提高。

主要流程如下：

页面行为 -> 创建 action -> dispatch 分发器 -> reducer 函数根据 action 返回 newState -> store 拿到 newState 更新数据 -> 页面通过 api 监听 store 更新时，更新页面数据

# 三、redux 的原则

### 3.1 单一数据源。只有一个 store

整个项目只有一个 store ，在大型项目中可以进行代码分割，但根节点只有一个 store

### 3.2 只有 store 能改变数据

State 的改变只能通过 store 本身，reducer 函数只是通过当前 state 和 action 计算出最新的 state，返回给 store。执行变更的是 store 本身

### 3.3 使用纯函数 reducer 进行状态修改

纯函数的定义如下：

- 相同的入参总会返回相同的出参（出参只由入参所决定）
- 没有副作用
  Reducer 是纯函数，只负责通过当前 state 和 action 计算出最新的 state，返回给 store

# 四、redux 基本使用

1. 引入 redux

```
npm install redux --save
```

2. 基本配置

```
import { createStore } from 'redux'
// reducers 纯函数
function reducers (state = { val: 0 }, action) {
	// 根据 action type 编写处理逻辑
	switch (action.type) {
		case 'add_val':
			// 注意先深拷贝，不能直接改变 state 的值
			const newState = JSON.parse(JSON.stringify(state))
			newState.val += action.value
			return newState
		default:
			return state
	}
}
// 传入 reducers 创建 store
const store = createStore(reducers)
// 获取状态
const value = store.getState().val
// 订阅 store 的变动，执行回调
store.subscribe(() => {
	console.log('store 更新啦')
})

// 创建 action，对象中需包含 type 字段，说明此次变动
const action = {
	type: 'add_val',
	value: 2
}
// 调用 dispatch 来更新状态
store.dispatch(action)
```
