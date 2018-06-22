## WindowStub.js

文件路径 ssdev.angular.widget.window.WindowStub.js

主窗口以外的窗口,都通过此文件的`show`方法,创建并打开新的窗口,传递一些全局变量

#### show方法代码
```javascript
  	show: function () {
        var me = this, win = me.win, cls = me.bootCls;
        if (!win) {
            var winc = me.winConf;

            var gui = require('nw.gui');
            win = gui.Window.open('index.html?clz=' + cls, winc);
            win.on("loaded", function () {
                win.window['$AppContext'].urt = $AppContext.urt;
                win.window['$AppContext'].notLogonCallback = $AppContext.notLogonCallback;
                win.window['$AppContext'].serverDate = $AppContext.serverDate;
                win.window['$AppContext'].exInfo = $AppContext.exInfo;
                win.window['$AppContext'].tid = $AppContext.tid;
                win.window['$env'].getHuanSetting=$env.getHuanSetting;
            });
            win.on("closed", function () {
                win.removeAllListeners();
                me.win = null;
                me.fireEvent("close")
            });
            win.on("hide", function () {
                me.fireEvent("hide");
            });
            win.on("moduleLoaded", function (m) {
                me.module = m;
                win.window["module"]=m;
                me.fireEvent("moduleLoaded", m)
            })
            win.on("moduleLoadError", function (e) {
                console.error(e);
            })
            if (!winc.fullscreen) {
                win.on("maximize", function () {
                    win.restore()
                });
            }
            me.win = win;
        }
        else {
            win.show();
            win.focus();
        }
    }
```

**通过show方法打开新的窗口,加载window.js和相应的控制器文件和模版**

该文件的一些对窗口操作的方法,和window.js重复了



## boot-nw.js

主框架文件,封装了常用的对数组,对象,cookie,继承,异步加载文件和创建新模块等操作的方法


## window.js

文件路径 /ssdev.angular.widget.css.window

该文件是主窗口和其他新打开的窗口都会继承的文件,可以理解为一个自定义的`窗口`,包括顶部的标题和工具栏,边框,弹框,loading框,进度条等一些共用的方法



### 方法

- close 关闭窗口
- hide	隐藏窗口
- show	显示窗口
- resizeTo	改变窗口大小
- showMask	显示loading框,传false代表隐藏loading框
- showModal	显示提示信息弹狂,可传入,标题,提示内容,按钮,并绑定相应的事件
- showTip	在窗口中央显示一个提示框,默认3秒钟自动隐藏
- showErrorMsg	在窗口顶部显示一个提示信息,不会自动隐藏
- bindData		改变窗口的标题,图标等信息

以上方法都可以在个模块的js代码中通过` $windows.**` 调用到



## angularInit

文件路径 `/ssdev/angular`

在index.js中,加载完angular.js之后加载的第一个文件

手动绑定dom到angualr应用,并引入动态加载控制器所需要的一些服务

## Container

文件路径 `/ssdev.bootstrap.container`


混入 `Observable.js`(观察者模式,实现事件绑定和触发的一种设计模式)

虽然该文件的目录名叫bootstap,但它和前端框架的boostrap没有关系

该方法是手动启动angulajs应用的方法

```javascript

	angular.bootstrap(element, [modules], [config]);
	
	//element(必需)。要启动angular的根节点，一般为document，也可以是其他的的html的dom。
	//modules(数组，可选)。依赖的模块。
	//conifg(object,可选)。配置项，目前只有strictDi一个可配置项，默认为false,是否开启辅助debug。

```

## AngularContainer

文件路径 `/ssdev.angular`

继承自`bootstrap/Container.js`,根据访问的页面动态加载controller


## Observable.js

文件路径 `/ssdev.utils`

该文件使用设计模式中的观察者模式,实现自定义事件

在其他文件中通过`混入(mixins)`的方式,在实例模块上使用`on(绑定),un(解绑),fireEvent(触发)`等方法了

## RTMClient.js

对websocket 常用操作的封装

## RTCSession.js

对webrtc常用操作的封装

## ServiceSupport.js

封装了请求服务接口,以便在模块中使用`me.servive.***`的方式请求

