# 第三方接入纳里健康开发文档


## 申请appsecret

目前暂不开放对外自注册接口，联系纳里健康注册，发放appkey和appsecret，用于接入的唯一标识，妥善保管，请勿泄漏。appsecret一般是长度为16位的随机字符串。


## 接入地址

纳里健康提供对外的统一接入url地址
https://weixin.ngarihealth.com/weixin/page.html


## 参数

- appkey (必填)发放的第三方唯一标识- 
- idcard 身份证号码
- mobile (必填)手机号码
- patientName 姓名
- tid (必填)第三方系统的用户唯一标识- 
- signature (必填)签名



## 例子

    https://weixin.ngarihealth.com/weixin/page.html?appkey=xyapp&tid=6654&mobile=13584675644&patientName=张一天&idcard=330101199912249351&signature=c0c7d06a6b19afb7ed39eff51da11552


## 签名规则

所有参数，即使没有值，按照参数名称`升序`的方式，以key=value的方式，拼接成一个字符串。
上述参数列表由上至下已经是排好顺序的。拼接完成后得到字符串
`appkey=xyapp&idcard=330101199912249351&mobile=13584675644&patientName=张一天&tid=6654`
注意的是，即使patientName或者idcard没有值，也需要以key=的方式加入到签名字符串中，如
`appkey=xyapp&idcard=&mobile=13584675644&patientName=&tid=6654`
最后将`appsecret`和这一串字符串和拼接，appsecret在后面，假如appsecret为8ngsGrJhSuHGlkun，那么最终的签名字符串是
`appkey=xyapp&idcard=330101199912249351&mobile=13584675644&patientName=张一天&tid=66548ngsGrJhSuHGlkun`
再将此字符串做md5得到的就是最终签名3778deec0b07b749668b60b8df09802e


## GET还是POST

虽然接口兼容get和post，为了信息的安全性，还是建议使用POST方式请求该接口。参数以post的方式传递。


#### From

[http://gitlab.ngarihealth.com/document/document/wikis/web%E7%AC%AC%E4%B8%89%E6%96%B9%E6%8E%88%E6%9D%83%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3](http://gitlab.ngarihealth.com/document/document/wikis/web%E7%AC%AC%E4%B8%89%E6%96%B9%E6%8E%88%E6%9D%83%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3 "http://gitlab.ngarihealth.com/document/document/wikis/web%E7%AC%AC%E4%B8%89%E6%96%B9%E6%8E%88%E6%9D%83%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3")