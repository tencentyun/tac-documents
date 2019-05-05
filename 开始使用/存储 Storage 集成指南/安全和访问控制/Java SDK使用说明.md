## 获取 SDK

[Java SDK 下载>>](https://github.com/tencentyun/qcloud-cos-sts-sdk)

## 查看示例

请查看 `src/test` 下的 java 文件，里面描述了如何调用 SDK。

## 使用方法

#### 1. 在 java 工程的 pom.xml 文件中集成依赖：

```
<dependency>
   <groupId>com.qcloud</groupId>
   <artifactId>qcloud-java-sdk</artifactId>
   <version>2.0.6</version>
   <scope>compile</scope>
</dependency>
```

#### 2. 拷贝 `src/main` 下的 `StorageSts.java` 到您的工程中，调用代码如下：

```
TreeMap<String, Object> config = new TreeMap<String, Object>();

// 您的 SecretID
config.put("SecretId", "xxx");
// 您的 SecretKey
config.put("SecretKey", "xxx");
// 临时密钥有效时长，单位是秒，如果没有设置，默认是30分钟
config.put("durationInSeconds", 1800);

JSONObject credential = StorageSts.getCredential(config);
```

## 返回结果

成功的话，可以拿到包含密钥的 JSON 文本：

```
{"code":0,"message":"","codeDesc":"Success","data":{"credentials":{"sessionToken":"2a0c0ead3e6b8eed9608899eb74f2458812208ab30001","tmpSecretId":"AKIDBSrMaeFD0ZAECKuBzohnjAhJ53XNCE2F","tmpSecretKey":"UC7YjMrIlcuFgoWGwnrHwsMBrQrpUwYI"},"expiredTime":1526288317}}
```

## 自定义策略

默认情况下返回的密钥可以访问所有 cos 下的资源。如果您希望精确控制密钥的访问级别，您可以通过以下方式设置 policy：

```
config.put("policy", "your-policy");
```

关于策略描述和数据安全的最佳实践，请参见 [数据安全性最佳实践](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E5%AD%98%E5%82%A8%20Storage%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E5%AE%89%E5%85%A8%E5%92%8C%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6/%E6%95%B0%E6%8D%AE%E5%AE%89%E5%85%A8%E6%80%A7%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5.md)。
