## contextParam.js

#### $env.setServerContextUrl()方法设置客户端的服务地址

	$env.setServerContextUrl("http://ssltest.ngarihealth.com:8280/ehealth-base-test/");


`$env`是在全局对象`global`下增加的一个对象,里面包含了很多全部的方法

#### 其他地址

- http://ngaribata.ngarihealth.com:8280/ehealth-base-test/ 		测试
- http://base.whhealth.gov.cn/ehealth-base-wuhan/
- http://ngaribata.ngarihealth.com:8480/ehealth-base-devtest/ 	开发测试
- https://ssltest.ngarihealth.com/ehealth-base-feature5/  		开发环境
- https://ssltest.ngarihealth.com/ehealth-base-prerelease/		预发布环境
- http://ehealth.easygroup.net.cn/ehealth-base/	 				正式

## config.json

	{
	    "exTitle": "纳里医生",						//窗口标题前缀
	    "basicUrlIcon": "eh/portal/images/icons/",	//图标的根目录
	    "dirIcon": "ngari/",						//正在使用的图标,目前每个目录下有三个图标
	    "mainVersion": "2017122908",				//客户端主程序的版本号,通过此字段判断,是否下载更新
	    "nemoVersion": "2017122908"					//小鱼的版本号,作用同上
	}
