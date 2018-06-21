# 断点续传

## 说明

请求`POST`地址

```javasc
http://domain/ehealth-base/multipart/upload
```

注意提交该接口时候都应该用form方式提交 并且要设置form的

```html
<form enctype="multipart/form-data" ...>
  ...
</form>
```

或者直接采用`FormData`对象直接提交请求

```javascript
var formData = new FormData()
formData.append('catalog', 'foo')
formData.append('mode', 'bar')
formData.append('file', file)
...

var request = new XMLHttpRequest()
request.open("POST", "http://domain/ehealth-base/multipart/upload");
//request.setRequestHeader('X-Access-Token', token)//PC端登陆后cookie中已经写入可以忽略此步骤
request.onload = function () {//send成功后的callback
	if (this.status === 200){
        // do your staff
    }
}
request.send(formData)
```

## api

### 初始化上传

为了能确保大文件（比如大于100M）在网络等其他非可控因素影响下实现断点续传功能，客户端需要持久化保存上传的中间信息，建议存储到`localStorage`中，在用户选择文件后，通过js获得该文件的`md5`码，推荐结构

```json
{
    md5(file1): {
  		fileId: 123456
	},
  	md5(file2): {
    	fileId: 123457,
    	parts: {
    		1: '分片1的MD5值',
        	2: '分片2的MD5值'
      	}
    },
}
```



提交请求至`http://domain/ehealth-base/multipart/upload`

请求：

| fieldName | 必填   | 类型     | example   | 说明          |
| --------- | ---- | ------ | --------- | ----------- |
| option    | y    | string | initiate  | 固定值initiate |
| catalog   | y    | string | other-doc |             |
| mode      | n    | int    | 31        | 详见mode文档    |
| file      | y    | file   |           | File类型      |
| notes     | n    | string |           |             |
| fileId    | n    | int    | 123456    | 续传场景需要传入    |

> 注意如果传入fileId开启续传模式的话，catalog，mode参数就不需要传了，响应结果中还会有parts部分，告诉你已经传成功了哪些分片

响应：

```json
{
    fileId: 123456,					//客户端需要保存该ID，并且和该文件md5做好映射
  	suggestedChunkSize: 20971520,	//此例中为20M,分片的大小
  	parts: {}						//入参中如果有fileId还会有该property
}
```

### 开始上传

请求：

| fieldName | 必填   | 类型   | example | 说明             |
| --------- | ---- | ---- | ------- | -------------- |
| fileId    | y    | int  | 123456  | 第一个请求得到的fileId |
| part      | y    | int  | 1       | 分片索引           |
| file      | y    | file |         | File类型         |

响应：

```json
{
  	fileId: 123456,
    part: 1,
  	md5string: 'blablablablabla'
}
```

> 此结果需要持久化保存到客户端本地，即前面的localStorage中

### 完成上传

当客户端所有分片都已经上传完成，还需要告诉服务器已经上传完成，服务器才会将分片拼接成最终的文件

请求：

| fieldName | 必填   | 类型     | example                  | 说明             |
| --------- | ---- | ------ | ------------------------ | -------------- |
| option    | y    | string | complete                 | 固定值complete    |
| fileId    | y    | int    | 123456                   | 第一个请求得到的fileId |
| parts     | y    | string | "{1: 'md51', 2: 'md52'}" | 注意是字符串不是json对象 |

> 调用 JSON.stringify()方法将json对象转成string



## 其他

- UI部分需要客户端自己处理，建议用总分片数为总进度，每一个分片上传成功后走进度条。


- 断点续传真正的发挥地方是比如一个文件215M，分片大小为20M，那么分片数量为11，最后一个为15M，传了前面5个分片后因为网络原因或者电脑死机，那么该文件的上传情况因为存储在localStorage中，再次上传先对比文件的MD5值，就可以忽略已经传成功的分片，继续上传为上传完成的分片，实现断点续传


From

[http://gitlab.ngarihealth.com/document/document/wikis/multipart-upload](http://gitlab.ngarihealth.com/document/document/wikis/multipart-upload "http://gitlab.ngarihealth.com/document/document/wikis/multipart-upload")