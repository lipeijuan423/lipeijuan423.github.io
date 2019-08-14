---
title: 状态管理器-flux+redux+mobx+vuex
date: 2019-08-14 09:57:44
tags:
---
### 为什么会出现状态管理器
> 框架实现需要组件，组件之间需要共享状态，层级太深或嵌套复杂的组件，往往不好传递状态
> 解决方法： 把组件之间需要共享的状态抽取出来，遵循特定的约定，统一来管理，让状态的变化可以预测。
> 简单实现： 只需要一个提取的共享状态，action直接改变里面的值,store也能直接修改，问题在于随处都能改变。组件不允许直接修改属于 store 实例的 state，组件必须通过 action 来改变 state，也就是说，组件里面应该执行 action 来分发 (dispatch) 事件通知 store 去改变。这样约定的好处是，我们能够记录所有 store 中发生的 state 改变，同时实现能做到记录变更 (mutation)、保存状态快照、历史回滚/时光旅行的先进的调试工具。
### Flux架构思想
![flux基本流程图](/static/状态管理/flux.png)
> 基本概念：
- view: 视图层
- action: 视图层发出的信息
- dispatcher(派发器)： 接收action，执行回调函数，唯一可以改变状态的地方
- store （数据层/仓库）： 用来存放应用的状态，一旦发生变动，需要提醒view更新页面
> 特点：
- 单向流动
> 流程： 用户访问view->点击（某种操作）发出action->dispatcher收到action,改变状态->store发出一个事件，改变view
### redux
redux不止用在react,还能用在其他框架
![redux基本流程图](/static/状态管理/redux.png)
> 基本概念
- view
- action
- store
  - dispatcher: store.dispatch()是 View 发出 Action 的唯一方法
  - middleware: 是对dispatcher的增强
  - reducer(oldstate, action)： 修改状态的方法，传入参数是旧状态和行为，返回一个新的状态对象
  - state： 状态值
> 对比Flux
- Flux 中 Store 是各自为战的，每个 Store 只对对应的 View 负责，每次更新都只通知对应的View
- redux: Redux 中各子 Reducer 都是由根 Reducer 统一管理的，每个子 Reducer 的变化都要经过根 Reducer 的整合
- Redux有三大原则： 单一数据源; State 是只读的; 使用纯函数来执行修改
- Flux 的数据源可以是多个;Flux 的 State 可以随便改; Flux 执行修改的不一定是纯函数。
> 中间件 - 是对dispatcher的增强
- middleware 最优秀的特性就是可以被链式组合
项目中都会有同步和异步的操作
同步流程就是： 用户发出Action，Store.dispatch派发action,Reducer立即计算State
异步流程就是： 用户发出Action, Store.dispatch派发action, Middleware处理异步api,Reducer立即计算State

- 处理异步流
- redux-thunk
Redux 本身只会处理同步的简单对象 action，但可以通过 redux-thunk 拦截处理函数（function）类型的 action，通过回调来控制触发普通 action，从而达到异步的目的
```js
// 使用
const createFetchDataAction = function(id) {
    return function(dispatch, getState) {
        // 开始请求，dispatch 一个 FETCH_DATA_START action
        dispatch({
            type: FETCH_DATA_START, 
            payload: id
        })
        api.fetchData(id) 
            .then(response => {
                // 请求成功，dispatch 一个 FETCH_DATA_SUCCESS action
                dispatch({
                    type: FETCH_DATA_SUCCESS,
                    payload: response
                })
            })
            .catch(error => {
                // 请求失败，dispatch 一个 FETCH_DATA_FAILED action   
                dispatch({
                    type: FETCH_DATA_FAILED,
                    payload: error
                })
            }) 
    }
}

//reducer
const reducer = function(oldState, action) {
    switch(action.type) {
    case FETCH_DATA_START : 
        // 处理 loading 等
    case FETCH_DATA_SUCCESS : 
        // 更新 store 等
    case FETCH_DATA_FAILED : 
        // 提示异常
    }
}

```

<div style="display:none">
// 核心代码
  const thunk = ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState);
    }
    return next(action);
  };
</div>
- react-promise
代码简单不少，then、catch 之类的被中间件自行处理了，代码简单不少，不过要处理 Loading 啥的，还需要写额外的代码

```js
// 使用
const FETCH_DATA = 'FETCH_DATA'
//action creator
const getData = function(id) {
    return {
        type: FETCH_DATA,
        payload: api.fetchData(id) // 直接将 promise 作为 payload
    }
}
//reducer
const reducer = function(oldState, action) {
    switch(action.type) {
    case FETCH_DATA: 
        if (action.status === 'success') {
             // 更新 store 等处理
        } else {
                // 提示异常
        }
    }
}
```
核心原理：如果 action 或 action.payload 是 Promise 类型则将其 resolve，触发当前 action 的拷贝，并将 payload 设置为 promise的 成功／失败结果
```js
// 核心原理
export default function promiseMiddleware({ dispatch }) {
  return next => action => {
    if (!isFSA(action))  {// 判断是否是标准的 flux action
      return isPromise(action)
        ? action.then(dispatch)
        : next(action);
    }

    return isPromise(action.payload)
      ? action.payload.then(
          result => dispatch({ ...action, payload: result }),
          error => {
            dispatch({ ...action, payload: error, error: true });
            return Promise.reject(error);
          }
        )
      : next(action);
  };
}
```
- redux-saga
- redux-observable
> 扩展插件
-  react-redux 
Redux将React组件分为容器型组件和展示型组件，容器型组件一般通过connect函数生成，它订阅了全局状态的变化，通过mapStateToProps函数，可以对全局状态进行过滤，而展示型组件不直接从global state获取数据，其数据来源于父组件
#### redux使用
#### redux原理简析
### vuex
![vuex流程图](/static/状态管理/vuex.jpg)
> 基本概念
- view
- store: 全局Store
 - getter: 类似vue的computed,输出时，可以筛选state的值，但不是改变state
 - modules: 将多个模块合为一个
- action： 可操作异步 
 - store.dispatch('increment') 触发某个action,action里面写异步操作，store.commit('increment') 
 - 也能在函数内部操作异步，再请求state.commit()
- mutation: 事件类型 (type) 和 一个 回调函数 (handler)；调用store.commit('increment')； 同步操作
> 辅助函数
- mapState
- mapAction
- mapMutation
> 对比
Redux： view——>actions——>reducer——>state变化——>view变化（同步异步一样）

Vuex： view——>commit——>mutations——>state变化——>view变化（同步操作） view——>dispatch——>actions——>mutations——>state变化——>view变化（异步操作）
#### vuex使用
### DVA
### MobX了解
![vuex流程图](/static/状态管理/mobx.png)
> 统一维护公共的应用状态，以统一并且可控的方式更新状态，状态更新后，View跟着更新
> MobX背后的哲学很简单：任何源自应用状态的东西都应该自动地获得 -- 状态只要一变，其他用到状态的地方就都跟着自动变。
- 一个store只管理一个state,可以直接修改state
### 四者区别
- Flux，redux, vuex都属于flux架构思想的实现；mobx是另一种选择
- Redux有三大原则： 单一数据源; State 是只读的; 使用纯函数来执行修改
- Flux 的数据源可以是多个;Flux 的 State 可以随便改; Flux 执行修改的不一定是纯函数。
- Vuex在全局拥有一个State存放数据，通过Mutation和Action更改数据
- vuex是基于flux和redux实现，为vue定制；redux可以用在各个框架中，只是思想和react的单一数据源吻合，在react用得比较多。
- mobx是一个store只管理一个state,可以直接修改state
- 参考链接： 
- https://zhuanlan.zhihu.com/p/53599723
- redux相关插件： https://juejin.im/post/59e6cd68f265da43163c2821