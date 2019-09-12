---
layout: post
title: 'egg 集群定时任务调度'
date: 2019-09-05
categories: jekyll update
---

## 案例一

> 5 分钟调度一次

`/app/schedule/cluster.js`

```js
'use strict'

module.exports = {
  schedule: {
    interval: 5 * 60 * 1000,
    type: 'cluster'
  },

  async task(ctx) {
    // do something...
  }
}
```

`/agent.js`

```js
'use strict'

const sleep = s => new Promise(resolve => setTimeout(resolve, s))

module.exports = agent => {
  class ClusterStrategy extends agent.ScheduleStrategy {
    start() {
      try {
        ;(async () => {
          while (true) {
            const { interval } = this.schedule
            await sleep(interval)
            this.sendOne()
          }
        })()
      } catch (error) {
        ctx.logger.error(error)
      }
    }
  }

  agent.schedule.use('cluster', ClusterStrategy)
}
```

## 案例二

> 每天凌晨调度一次

`/app/schedule/cluster.js`

```js
'use strict'

module.exports = {
  schedule: {
    cron: '0 0 0 * * *',
    type: 'cluster'
  },

  async task(ctx) {
    // do something...
  }
}
```

`/agent.js`

```js
'use strict'

const parser = require('cron-parser')

const sleep = s => new Promise(resolve => setTimeout(resolve, s))

module.exports = agent => {
  class ClusterStrategy extends agent.ScheduleStrategy {
    start() {
      try {
        ;(async () => {
          const { cron } = this.schedule
          const interval = parser.parseExpression(cron)
          while (true) {
            const next = interval.next()
            const date = next.toString()
            const timestamp = new Date(date).getTime()
            await sleep(timestamp - Date.now())
            this.sendOne()
          }
        })()
      } catch (error) {
        ctx.logger.error(error)
      }
    }
  }

  agent.schedule.use('cluster', ClusterStrategy)
}
```
