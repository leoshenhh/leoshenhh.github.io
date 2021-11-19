---
title: express-api
date: 2021-11-08 14:42:46
tags:
---

## express()

创建一个express应用

```javascript
const express = require('express')
const app = express()
app.listen(8888)
```

### express.json()

监听 `request.on('data')` ,如果请求的 `Content-Type` 为  `application/json ` 则将结果( `chunk` )解析为`json`对象，放入 `request.body`  里

```javascript
app.use(express.json())
app.use((request,response,next) => {
    console.log(request.body)
})
```

若不使用express.json()中间件,则获取 **请求体** 的方法为

```javascript
app.use((request,response,next) => {
    request.on('data',(chunk) => {
        console.log(chunk.toString())
    })
})
```

### express.static(root)

指定`root`文件夹（目录名自定义，一般为`public`）为提供静态资源的根目录

```javascript
app.use(express.static('public'))
```

在`public`目录下新建`index.html`这个文件，并请求

```javascript
$.get('localhost:8888/index.html')
```

如果`public` 文件夹有请求的文件，则返回该文件，没有则执行`next()`进入下一个中间件

### express.row()

监听 `request.on('data')` ,如果请求的 `Content-Type` 为  `application/octet-stream ` 则将结果( `chunk` )解析为`buffer`，放入 `request.body`  

```javascript
app.use(express.row())
```

### express.text()

监听 `request.on('data')` ,如果请求的 `Content-Type` 为  `text/plain ` 则将结果( `chunk` )解析为`string`，放入 `request.body`  

```javascript
app.use(express.text())
```



### express.urlencoded()

监听 `request.on('data')` ,如果请求的 `Content-Type` 为  `application/x-www-form-urlencoded` 则将结果( `chunk` )解析为**对象**，放入 `request.body`  

```javascript
app.use(express.urlencoded())
```



## app.set()

设置一个任意`value`，可以通过`name`来获取到

```
app.set('name','shen')
app.get('name') // shen
```

可以设置以下特殊值：

+ `case sensitive routing`	设置对请求路径大小写敏感

```javascript
app.set('case sensitive routing',false) // 一般放在第一个中间件之前
```

+ `views`

设置视图文件夹 , 用于`res.render()`

```javascript
app.set('views','xxx')
```

+ `view engine`

设置视图渲染引擎(需要安装该第三库)

```javascript
app.set('view engine', 'pug')
```

### app.get()

+ 如果参数是一个字符串`name`则获取`app.set(name)`的值

```javascript
app.get('name')
```

+ 如果参数是路径和回调函数`(path,callback)`,则响应`get`请求,执行`callbakc`中间件

```javascript
app.get('/xxx',(req,res,next)=>{
	console.log(res)
})
```

+ `app.post()` 、`app.put()` 、`app.delete` 等用法同`app.get()`

### app.all()

无论是什么请求都响应

```javascript
app.all('/xxx', function (req, res, next) {
	console.log(res)
})
```



### app.render()

如果设置了`views`和`view engine` ，则去`views`设置的目录寻找该文件，并使用设置的`view engine`解析

```javascript
app.render('xxx', { name: 'Tobi' })
```

+ 可以传入变量`{ name: 'Tobi' }`供引擎模板使用

### app.locals

设置`app`局部变量

```javascript
app.locals.title = '啦啦啦'
app.get('/xxx',(req,res,next)=>{
    console.log(req.app.local.title)
})
```

### req.app

把`express`的实例挂在了`req`,在中间件中可以通过`req`来访问`app`

```javascript
// mymiddleware.js
module.exports = function (req, res,next) {
  console.log(req.app.local.title)
}
```

### req.params

获取路径传参的参数

```javascript
app.get('/users/:id',(req,res,next)=>{
	console.log(req.params)
})
// 请求 '/users/1' 打印出: {id: 1}

```

### req.query

获取查询参数

```javascript
app.get('/users',(req,res,next)=>{
    console.log(req.query)
})
// 请求 '/users?name=shen&id=1' 打印出 { name: shen,id: 1 }
```

### req.xhr

用来区分`ajax`请求，还是普通请求

```javascript
app.get('/xxx',(req,res,next)=>{
    console.log(req.xhr) // 如果是true 则为 ajax 请求
})
```

### req.get()

获取请求头的值

```javascript
app.get('/xxx',(req,res,next)=>{
    console.log(req.get('Content-Type'))
})
```

### req.param()

根据属性名获取 **查询参数/路径传参** 的值

```javascript
app.get('/xxx',(req,res,next)=>{
    console.log(req.param('name'))
})
// 请求 '/users?name=shen&id=1' 打印出 shen
```

### res.append()

设置一个响应头 （只能一个一个添加）

```javascript
res.append('Set-Cookie', 'foo=bar; Path=/; HttpOnly')
```

### res.set()

设置响应头（可以添加对象）

```javascript
res.set({
  'Content-Type': 'text/plain',
  'Content-Length': '123',
  ETag: '12345'
})
```

### res.status()

设置响应状态码

```javascript
res.status(404)
```

### res.format()

根据请求头的`Accept`格式，返回不同的内容

```javascript
res.format({
  'text/plain': function () { // 如果请求头的Accept 为 'text/plain'
    res.send('hey') 
  },

  'text/html': function () { // 如果请求头的Accept 为 'text/html'
    res.send('<p>hey</p>')
  },

  'application/json': function () { // 如果请求头的Accept 为 'application/json'
    res.send({ message: 'hey' })
  },

  default: function () { // 如果请求头的Accept 不为以上任意一个
    // log the request and respond with 406
    res.status(406).send('Not Acceptable')
  }
})
```

### res.get()

获取请求头

```javascript
res.get('Content-Type')
// => "text/plain"
```

### router.get()

`router` 可以看成一个小型的`app`,只有路由功能

```javascript
const express = require('express')
const app = express.router()

router.get('/',(req,res,next)=>{
    res.send('/users')
})

router.get('/:id',(req,res,next)=>{
    res.send('/users/:id')
})

router.get('/:id/edit',(req,res,next)=>{
    res.send('/users/:id/edit')
})
```

把上面的`router` 挂载到`app`,实现子路由

```javascript
const usersRouter = require('./usersRouter')
app.use('/users',usersRputer)
```

### express-generator

使用`express-generator`生成默认`express`项目

+ 下载`express-generator`

```bash
npm install -g express-generator
```

+ 查看帮助

```bash
express --help
```

+ 生成一个 以`ejs`为模板引擎，`demo`为项目名的 `express`项目

```bash
express --view=ejs demo
```

+ 下载依赖并运行

```bash
cd demo
npm install
npm run start
```

