---
layout: post
title: '防抖函数与节流函数'
date: 2023-04-21
categories: jekyll update
---

```js
// 防抖函数
function debounce(fun, delay) {
  return function (...args) {
    clearTimeout(fun.timer)
    fun.timer = setTimeout(() => {
      fun.apply(this, args)
    }, delay)
  }
}

// 节流函数
function throttle(fun, delay) {
  return function (...args) {
    if (fun.timer) { return }
    fun.apply(this, args)
    fun.timer = setTimeout(() => {
      fun.timer = null
    }, delay)
  }
}

// 验证
function test(fn, delay, num) {
  while (num > 0) {
    const random = Math.round(Math.random() * delay)
    setTimeout(() => {
      console.log(random)
      fn(random)
    }, random)
    num--
  }
}

const DELAY = 1000 // 时间阈值
const NUM = 3 // 验证次数

test(debounce((last) => console.log('只有最后一次实际执行', last), DELAY), DELAY, NUM)
// test(throttle((first) => console.log('只有第一次实际执行', first), DELAY), DELAY, NUM)
```
