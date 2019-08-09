---
layout: post
title: "SPA 应用开启 History 模式"
date: 2019-02-11
categories: jekyll update
---

## nginx

### 根目录

> 部署在根目录的项目

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

### 子目录

> 未部署在根目录的项目

```nginx
if (!-e $request_filename) {
  rewrite ^/(.*) /index.html last;
  break;
}
```

或

```nginx
location / {
  try_files $uri $uri/ @router;
}

location @router {
  rewrite ^.*$ /index.html last;
}
```

## 其他后端配置例子

[Vue Router]

## client

### Vue

```js
const router = new VueRouter({
  mode: 'history',
})
```

> 如果应用服务在 ```/app/``` 下（非根目录）

```js
const router = new VueRouter({
  base: '/app/',
  mode: 'history',
})
```

### React

React Router 默认开启 browserHistory

> 如果应用服务在 ```/app/``` 下（非根目录）

``` js
<BrowserRouter basename='/app'></BrowserRouter>
```

### 参阅

+ [Vue Router]

+ [React Router]

[Vue Router]: https://router.vuejs.org/zh/guide/essentials/history-mode.html

[React Router]: https://reacttraining.com/react-router/web/guides/server-rendering
