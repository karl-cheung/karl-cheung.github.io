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
// image2array.js
// 来自孩子王-许祥成
export default function(url, callback) {
  const image = new Image(url)
  image.onload = function() {
    try {
      let { naturalWidth: width, naturalHeight: height } = image
      const ratio = Math.min(500 / width, 500 / height)
      const cWidth = ~~(ratio * width)
      const cHeight = ~~(ratio * height)
      const canvas = document.createElement('canvas')
      const ctx = canvas.getContext('2d')
      canvas.width = cWidth
      canvas.height = cHeight
      ctx.drawImage(image, 0, 0, width, height, 0, 0, cWidth, cHeight)
      const data = ctx.getImageData(0, 0, cWidth, cHeight)
      callback(null, data)
    } catch (e) {
      callback(e)
    }
  }
  image.onerror = function() {
    callback('image load fail')
  }
  image.src = url
}
```

```js
import jsQR from 'jsqr'
import image2array from '../../../utils/image2array'

function handleFileChange(e) {
  const { files } = e.target
  if (files.length === 0) return
  const reader = new FileReader()
  const file = files[0]
  // 休眠等待 image2array 计算，数据流格式转化至 Uint8ClampedArray
  await new Promise((res) => { setTimeout(() => { res() }, 100) })
  const imageUrl = URL.createObjectURL(file)
  try {
    image2array(imageUrl, (err, info) => {
      if (!err) return
      const { data, width, height } = info
      const binaryData = jsQR(data, width, height)
      if (binaryData && binaryData.data) {
        const link = binaryData.data
        console.log(link)
      }
    })
  } catch (error) {
    console.log(error)
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
