---
title: koa-api
date: 2021-11-19 15:44:07
tags:
---

### koa安装

```bash
yarn add koa // 安装koa
yarn add --dev @types/koa // 安装 k oa TS提示
```

### 安装TS

```bash
npm install typescript -g 
```

### 安装运行TS工具

```bash
npm i ts-node-dev -g
```



### TS初始化

```bash
tsc --init
```

### hello world

```typescript
// ./server.ts
import Koa from 'koa'

const app = new Koa()

app.use(async(ctx) =>{
    ctx.body = 'hello world'
})

app.listen(3000)
```

### 运行

```bash
ts-node-dev server.ts
```

### app.use()

使用中间件

```javascript
app.use(async (ctx, next) => {
  console.log(ctx)
});
```

### app.on()

监听某个事件

```javascript
app.on('error', err => {
  log.error('server error', err)
});
```

### request.idempotent

检查请求是否是幂等的。

> 幂等: 任意多次执行所产生的影响均与一次执行的影响相同

在`http` 请求中 `get`是幂等的 `post`、`put`等不幂等



