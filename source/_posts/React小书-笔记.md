---
title: React 小书-笔记
date: 2019-01-03 10:29:32
tags: 读书笔记
categories: 读书笔记
---
* react in 40

<!-- more -->

``` js
class Component {
  constructor (props = {}) {
    this.props = props
  }
  setState (state) {
    const oldEl = this.el
    this.state = state
    this.el = this.renderDOM()
    if (this.onStateChange) this.onStateChange(oldEl, this.el)
  }
  renderDOM () {
    this.el = createDOMFromString(this.render())
    if (this.onClick) {
      this.el.addEventListener('click', this.onClick.bind(this), false)
    }
    return this.el
  }
}
const createDOMFromString = (domString) => {
  const div = document.createElement('div')
  div.innerHTML = domString
  return div
}
const mount = (component, wrapper) => {
  wrapper.appendChild(component.renderDOM())
  component.onStateChange = (oldEl, newEl) => {
    wrapper.insertBefore(newEl, oldEl)
    wrapper.removeChild(oldEl)
  }
}

class LikeButton extends Component {
  constructor (props) {
    super(props)
    this.state = { isLiked: false }
  }
  onClick () {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }
  render () {
    return `
      <button class='like-btn' style="background-color: ${this.props.bgColor}">
        <span class='like-text'>
          ${this.state.isLiked ? '取消' : '点赞'}
        </span>
        <span>👍</span>
      </button>
    `
  }
}

class RedBlueButton extends Component {
  constructor (props) {
    super(props)
    this.state = {
      color: 'red'
    }
  }
  onClick () {
    this.setState({
      color: 'blue'
    })
  }
  render () {
    return `
      <div style='color: ${this.state.color};'>${this.state.color}</div>
    `
  }
}

const wrapper = document.querySelector('.wrapper')
mount(new LikeButton({ bgColor: 'red' }), wrapper)
mount(new LikeButton(), wrapper)
mount(new RedBlueButton(), wrapper)
```

* JSX：本质是js对象
* 如何用js对象表现一个DOM元素的结构
```js
<div class='box' id='content'>
  <div class='title'>Hello</div>
  <button>Click</button>
</div>
{
  tag: 'div',
  attrs: {className: 'box', id: 'content'},
  children: [
    {
      tag: 'div',
      arrts: {className: 'title'},
      children: ['Hello']
    },
    {
      tag: 'button',
      arrts: null,
      children: ['Click']
    }
  ]
}
```
* React.createElement：构建一个js对象来描述你HTML结构的消息，包括标签名，属性，子元素等
```js
render() {
  return(
    <div>
      <h1 className='title'>react</h1>
    </div>
  )
}
render() {
  return(
    React.createElement(
      'div',
      null,
      React.createElement(
        'h1',
        {className: 'title'},
        'react'
      )
    )
  )
}
```
* ReactDOM.render：把组件渲染并且构造DOM树，然后插入到页面某个特定元素上
* jsx（babel编译+react.js构造）--> JavaScript对象结构（ReactDOM.render）--> DOM元素 --> 插入页面
* 组件树
* defaultProps
* 有状态组件:业务逻辑  
  无状态组件:UI
* 函数式组件：只能接受props和提供render方法的类组件，不能使用state
```js
const HelloWorld = (props) => {
  const sayHi = () => console.log('hello world')
  return(
    <div onClick={sayHi}>hello world</div>
  )
}
```
* 状态提升：当某个状态被多个组件依赖或者影响的时候，把该状态提升到这些组件的最近公共父组件中去管理，用props传递数据或者函数来管理这种依赖或者影响的行为
  <!-- ![](statics/1.png) -->
* 组件的挂载：组件渲染并且构造DOM元素然后塞入页面的过程  
  construct-->component will mount -->render-->component did mount-->component will unmount  
  componentWillMount:Ajax、定时器  
  componentDidMount:动画  
  componentWillUnmount:清除定时器
* 组件的更新阶段  
  shouldComponentUpdate(nextProps, nextState):组件是否重新渲染  
  componentWillReveiveProps(nextProps):组件从父组件接收到新的props之前调用  
  componentWillUpdate  
  componentDidUpdate
* ref
```js
componetDidMount() {
  this.input.focus()
}
<input ref={(input) => this.input = input} />
```
* props.children
```js
<Card>
  <h2>xxx</h2>
  <div>xxxx</div>
</Card>

class Card extends Component {
  render() {
    return (
      <div>
        {this.props.children}
      </div>
    )
  }
}
```
* dangerouslySetInnerHTML:动态设置元素的innerHTML
```js
<div className='xx'
  dangerouslySetInnerHTML = {{__html: '<h1>xxxxx</h1>'}}
/>
```
* style
```js
<h1 style={{fontSize: '12px', color: this.state.color}}>xxxx</h1>
```
* PropTypes:第三方库prop-types
```js
static propTypes = {
  comment: PropTypes.object.isRequired
}
```
* 高阶组件：就是一个函数，传给它一个组件，它返回一个新的组件，为了组件之间代码复用
```js
export default (WrappedComponent, name) => {
  class NewComponent extends Component {
    render() {
      return <WrappedComponent>
    }
  }

  return NewComponent
}
```
<!-- ![](statics/2.png) -->
* context
```js
class Index extends Component {
  // 必须声明
  static childContextTypes = {
    themeColor: PropTypes.string
  }

  getChildContext() {
    return {
      themeColor: this.state.themeColor
    }
  }
}

class Title extends Component {
  // 必须声明
  static childContextTypes = {
    themeColor: PropTypes.string
  }

  render() {
    return (
      <h1 style={{color: this.context.themeColor}}>xxx</h1>
    )
  }
}
```
* redux
* dispath
```js
let appState = { 
  title: {
    text: 'React.js
    color: 'red', 
  },
  content: {
    text: 'React.js color: 'blue'
  } 
}
function dispatch (action) { 
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT': 
      appState.title.text = action.text 
      break
    case 'UPDATE_TITLE_COLOR': 
      appState.title.color = action.color 
      break
    default: 
    break
  } 
}
dispatch({type: 'UPDATE_TITLE_TEXT', text: 'xxx'})
```
* store
```js
let appState = { 
  title: {
    text: 'React.js
    color: 'red', 
  },
  content: {
    text: 'React.js color: 'blue'
  } 
}
function stateChanger (state, action) { 
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT': 
      state.title.text = action.text 
      break
    case 'UPDATE_TITLE_COLOR': 
      state.title.color = action.color 
      break
    default: 
    break
  } 
}
function createStore (state, stateChanger) {
  const getState = () => state
  const dispatch = (action) => stateChanger(state, action) 
  return { getState, dispatch }
}
const store = createStore(appState, stateChanger)
renderApp(store.getState) // 首次渲染页面
store.dispatch({type: 'UPDATE_TITLE_TEXT', text: 'xxx'})
renderApp(store.getState) // 把新的数据渲染到页面
```
* 监听数据变化，重新渲染页面(观察者模式)
```js
function createStore(state, stateChanger) {
  const listeners = []
  const subscribe = (listener) => listeners.push(listener) 
  const getState = () => state
  const dispatch = (action) => {
    stateChanger(state, action)
    listeners.forEach((listener) => listener())
  }
  return {
    getState, // 获取共享状态
    dispatch, // 修改共享状态
    subscribe // 监听数据被修改了，并进行后续的操作，如重新渲染页面
  }
}
const store = createStore(appState, stateChanger) 
store.subscribe(() => renderApp(store.getState()))
renderApp(store.getState()) // 首次渲染页面
store.dispatch({ type: 'UPDATE_TITLE_TEXT', text: ''})
// 后面不管如何store.dispatch，都不需要重新调用renderApp
```
* 纯函数：函数的返回结果只依赖于它的参数，函数执行过程里面没有副作用
```js
// 依赖参数
const a = 1 
const foo = (b) => a + b 
foo(2) // => 3

const a = 1 
const foo=(x,b)=>x+b 
foo(1, 2) // => 3

const a = 1
const foo = (obj, b) => {
  return obj.x + b
}
const counter = {
  x: 1
}
foo(counter, 2) // => 3 
counter.x // => 1
// 有副作用，非纯函数
const a=1
const foo = (obj, b) => {
  obj.x = 2
  return obj.x + b
  }
const counter={x:1} 
foo(counter, 2) // => 4 
counter.x // => 2
```
* 共享结构的对象
```js
const obj={a:1,b:2} 
const obj2={...obj} //=>{a:1,b:2}
obj !== obj2 // true
```
* 性能优化
```js
function stateChanger(state, action) {
  switch (action.type) {
    case 'UPDATE_TITLE_TEXT':
      return { // 构建新对象并且返回
        ...state,
        title: {
          ...state.title,
          text: action.text
        }
      }
    case 'UPDATE_TITLE_COLOR':
      return { ...state, title: {
        ...state.title,
        color: action.color
      }
  }
  default:
  return state }
}
function createStore(state, stateChanger) {
  const listeners = []
  const subscribe = (listener) => listeners.push(listener)
  const getState = () => state
  const dispatch = (action) => {
    state = stateChanger(state, action) //覆盖原对象
    listeners.forEach((listener) => listener())
  }
  return {
    getState,
    dispatch,
    subscribe
  }
}
```
* reducer
```js
function createStore(reducer) {
  let state = null
  const listeners = []
  const subscribe = (listener) => listeners.push(listener) 
  const getState = () => state
  const dispatch = (action) => {
    state = reducer(state, action) 
    listeners.forEach((listener) => listener())
  }
  dispatch({}) // state
  return {
    getState,
    dispatch,
    subscribe
  }
}
function themeReducer(state, action) {
  if (!state) return {
    themeName: 'Red Theme',
    themeColor: 'red'
  }
  switch (action.type) {
    case 'UPATE_THEME_NAME':
      return { 
        ...state,
        themeName: action.themeName
      }
    case 'UPATE_THEME_COLOR':
      return { 
        ...state,
        themeColor: action.themeColor
      }
    default:
      return state
  }
}
const store = createStore(themeReducer)

```
* redux
```js
// 定义一个reducer
function reducer (state, action) { 
  /* 初始化 state switch case */
}
// 生成store
const store = createStore(reducer)
// 监听数据变化重新渲染页面
store.subscribe(() => renderApp(store.getState()))
// 首次渲染页面
renderApp(store.getState())
// 后面可以随意 dispatch，页面自动更新
store.dispatch(...)
```
* react-redux
