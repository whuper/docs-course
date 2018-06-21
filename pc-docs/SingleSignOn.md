
## 什么是单点登录

> 在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统

## 实现方式

请咨询后台开发人员

## 在网页中调用pc

#### 实现原理

windows注册表协议(`innosetup`打包工具,安装时候写入的`Ngarihealth` 协议)


#### 调用

##### HTML页面及a标签参数
新建`<a>`标签，使用`“Ngarihealth”`协议。传参，请在`<a>`标签中添加“data-request”属性，多个属性使用‘&’符连接。

给`<a>`标签添加click事件，通过**get**的方式请求，打开clz或者activeModule定义的模块的页面，并接收解析“data-request”中定义的其他参数。

#### 参数

##### 基础参数
- appkey
- appsecret 
- doctorId
- doctorMobile

##### 其他参数 
打开业务列表页 ：

- activeModule

打开申请页面 ：

- clz

病人信息参数

- idcard
- patientName
- patientType
- patientMobile


其中appKey、appsecret是对接的唯一标识，请找纳里健康的运维人员配置

启动示例：（两种，一种是到业务列表页，一种是到申请页面）

####　代码参考

> 以http get请求的方式,请求纳里医生pc监听在7009端口的一个服务器(URL地址http://127.0.0.1:7009),并传入参数,纳里医生pc根据接受到的参数(doctorId,clz,idcard,patientName等信息)打开对应的页面。
> 
> 启动需要时间，这里setTimeout延迟一秒或者更久等待客户端启动完成。


```html

		<body>
			<a class="pc-call am-btn am-btn-default am-btn-secondary" href="Ngarihealth:" data-request="&appkey=wanda&appsecret=ZWXxk3k7H4d0KaRn&clz=eh.bus.web.cloud.wizard.Wizard&orgName=&doctorName=&idcard=420100199310266133&patientName=%E5%AD%99%E9%A3%9E&patientType=1&patientMobile=18974776381&doctorId=441371413_90">云门诊</a>			
			
			<a class="pc-call am-btn am-btn-default am-btn-success"  href="Ngarihealth:" data-request="&appkey=Test&appsecret=H977NTnkmq68CDTT&activeModule=4&doctorId=15868186758&doctorMobile=15868186758">会诊</a>
			
			<a class="pc-call am-btn am-btn-default am-btn-success"  href="Ngarihealth:" data-request="&appkey=Test&appsecret=H977NTnkmq68CDTT&activeModule=4&clz=eh.bus.web.meet.MeetContentForm&meetClinicId=21550&doctorId=15868186758&doctorMobile=15868186758">会诊单</a>
		</body>

		<script type="text/javascript">
		$(document).ready(function(){
			var basic_url = 'http://127.0.0.1:7009/open?sourse=web';
		       $(".pc-call").bind("click",function(){
				var link_url = basic_url + $(this).data('request');
				alert(link_url);
				start(link_url);
			     })
			     
		     });
			function start(link_url){		
				//延迟1秒 等客户端启动后再次调用 打开窗口
				setTimeout(function(){
					$.jsonp({
					     url: link_url,
					     //data:para_data,
					     callbackParameter: "callback",
					     success:function(data){
					     console.log('suc');
					     },
					     error:function(e){
						console.log('err');
						start(link_url);
					     }
					 });			
						},1200)	
			}
		</script>
```

### 常见异常处理

1.appKey和appsecret无效
请联系运维部门的人进行相关配置。

2.因为其他情况无法自动登录，如何自动填充医生的账号
在参数中定义doctorMobile，这样登录界面会自动填充账号，输入密码即可。

3.点击启动无反应、启动了参数没有传过去
增加启动的延迟时间，可能程序启动慢，端口没监听上，无法实现传输，请设置setTimeout增加等待时长

## 启动过程小结


1. 安装纳里医生pc单点登录版本,完成以后会往操作系统的注册表写入一个 Ngarihealth协议

2. 在a链接里,使用用Ngarihealth协议而不是http协议,通过点击a连接就会启动纳里医生pc端了,pc端启动以后会在本地开启一个小的服务器,监听在7009端口

3. 为a链接增加data-request属性,把所有参数写到属性里,用&符号连接(参数列表参考请求参数表格)

4. 为a链接绑定click事件,点击以后,以http get请求的方式,请求纳里医生pc监听在7009端口的一个服务器(URL地址http://127.0.0.1:7009),并传入参数,纳里医生pc根据接受到的参数(doctorId,clz,idcard,patientName等信息)打开对应的页面;

## demo下载

[weblogin](/resourse/weblogin.zip "weblogin.zip")