---
layout: post
title: 'hammerjs 在 react 中的使用'
date: 2019-08-12
categories: jekyll update
---

## 使用

```js
import React from 'react'
import Hammer from 'hammerjs'

class HammerReact extends React.Component {
  state = { container: React.createRef() }

  componentDidMount() {
    const { container: { current } } = this.state
    this.listener = new Hammer(current)
    // 给监听的元素对象添加长按(press)事件
    this.listener.on('press', (e) => {
      // Do some thing...
    })
  }

  componentWillUnmount() {
    this.listener.destroy() // 销毁
  }

  render() {
    <div ref={container}>我是 div</div>
  }
}

ReactDOM.render(<HammerReact />, document.getElementById('root'))
```

## 附注

> 在添加的手势元素是动态渲染的情况下，通过事件委托的方式在父级元素处理。

```js
this.listener.on('press', (e) => {
  const { target: { nodeName } } = e
  if (nodeName === 'DIV') {
    // Do some thing...
  }
})
```
