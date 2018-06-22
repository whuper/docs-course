

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
                })
            })
            stub.on("close", function () {
                me.quitWindow = null;
            })

```


## openModule()

打开新窗口



