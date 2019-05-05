## 获取 SDK

[Nodejs SDK 下载>>](https://github.com/tencentyun/qcloud-cos-sts-sdk)

## 查看示例

请查看 `test` 下的 `sts_demo`，里面描述了如何调用 SDK。

## 使用方法

#### 1. npm install:

```
npm i qcloud-cos-sts --save
```

#### 2. 调用代码如下：

```
var sts = require('tac-storage-sts');

var options = {
	// 您的 secretId
 	secretId: 'xxx',
	// 您的 secretKey
	secretKey: 'xxx',
	// 临时密钥有效时长，单位是秒
	durationInSeconds: 1800
};


sts.getCredential(options, function(data) {
    console.info(data)
});

```

## 返回结果

成功的话，可以拿到包含密钥的 JSON 文本：

```
{"code":0,"message":"","codeDesc":"Success","data":{"credentials":{"sessionToken":"2a0c0ead3e6b8eed9608899eb74f2458812208ab30001","tmpSecretId":"AKIDBSrMaeFD0ZAECKuBzohnjAhJ53XNCE2F","tmpSecretKey":"UC7YjMrIlcuFgoWGwnrHwsMBrQrpUwYI"},"expiredTime":1526288317}}
```


## 自定义策略

默认情况下返回的密钥可以访问所有 cos 下的资源，如果你希望精确控制密钥的访问级别，例如访问某个路径下的资源，您可以通过以下方式设置 policy：

```
var options = {
	...,
	policy: 'your-policy'
};
```

关于策略描述和数据安全的最佳实践，请参见 [数据安全性最佳实践](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E5%AD%98%E5%82%A8%20Storage%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E5%AE%89%E5%85%A8%E5%92%8C%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6/%E6%95%B0%E6%8D%AE%E5%AE%89%E5%85%A8%E6%80%A7%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5.md)。
