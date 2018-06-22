## 实现原理

#### nodejs的http模块


- 在boot-nw定义了一个调用`node http`模块的方法`startServer()`,客户端启动以后,在portal.js执行`$server.start`运行,打开

- 在localhost的7009端口监听,接收 `get` 请求过来的参数,并处理


- 调用portal.js的 `openModule`方法和 `openapi()` 方法,打开相应的模块





