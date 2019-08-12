---
layout: post
title: '二维码与链接相互转化'
date: 2019-08-12
categories: jekyll update
---

## 二维码识别链接

```html
<input type="file" accept="image/*" onChange="{handleFileChange}" />
```

```js
import jsQR from 'jsqr'
const image = require('get-image-data')

function handleFileChange(e) {
  const { files } = e.target
  if (files.length === 0) return
  const reader = new FileReader()
  const file = files[0]
  reader.readAsDataURL(file)
  reader.onloadend = () => {
    const { result } = reader
    image(result, (err, info) => {
      if (!err) return
      const { data, width, height } = info
      const binaryData = jsQR(data, width, height)
      if (binaryData && binaryData.data) {
        const link = binaryData.data
        console.log(link)
      }
    })
  }
}
```

## 链接生成二维码

```js
import QRCode from 'qrcode'

const link = 'https://vincheung.github.io'

QRCode.toDataURL(link, {}, (err, url) => {
  if (err) return
  console.log(url)
}
```
