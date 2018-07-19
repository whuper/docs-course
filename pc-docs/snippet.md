

## `$create()`

创建新模块

``` javascript

		$create(clz,conf).then(function(m){
			me.modules[mid] = m;

			if(mid == "doctorForm"){
				//绑定rtcStarted事件,执行一些操作
				m.on("rtcStarted",function(){
					me.fireEvent("rtcStarted",me);
				});
			}
			
		})
		.fail(function(e){
			console.error(e)
		})
```

$create的第二个参数,可以给新模块 **传递参数** ,在新模块中可以直接以 `this.conf.**` 访问到

## refresh()

由于模块里的代码是在AngularJS上下文之外的地方运行的,需要我们手动调用`$apply()`方法来通知AngularJS更新

该方法被封装在了`refresh()`里



## destroy()

模块销毁时需要调用的方法

### ssdev.angular.widget.window.WindowStub()

打开新窗口

代码示例
```javascript

           var stub = new ssdev.angular.widget.window.WindowStub({
                cls: "/eh.rtc.ConfirmQuit",
                alwaysOnTop: true,
                width: 320,
                height: 180
            });
            stub.on("moduleLoaded", function (form) {
                
                form.on("quit", function () {

					//some code...

                })
                form.on("cancel", function () {
                    stub.close();
                });

				//调用子窗口模块的方法,并传入参数
				form.init(data);

            })
            stub.on("close", function () {
				//子窗口关闭的时候,要删除子窗口的的引用对象
                me.quitWindow = null;
            })

```


## openModule()

打开新窗口



