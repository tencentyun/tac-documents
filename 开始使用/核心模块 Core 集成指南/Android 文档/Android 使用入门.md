
移动开发平台（MobileLine）使用起来非常容易，只需要简单的 4 步，您便可快速接入。接入后，您即可获得我们提供的各项能力，减少您在开发应用时的重复工作，提升开发效率。

## 准备工作

您首先需要一个 Android 工程，这个工程可以是您现有的工程，也可以是您新建的一个空的工程。

## 第一步：创建项目和应用

在使用我们的服务前，您必须先在 MobileLine 控制台上创建项目和应用。

> 如果您已经在 MobileLine 控制台上创建过了项目和应用，请跳过此步。

## 第二步：添加配置文件

在您创建好的应用上单击【下载配置】按钮来下载该应用的配置文件的压缩包：

![](http://tacimg-1253960454.file.myqcloud.com/guides/project/downloadConfig.gif)

解压该压缩包，您会得到 `tac_service_configurations.json` 和 `tac_service_configurations_unpackage.json` 两个文件，请您如图所示添加到您自己的工程中去。

![](https://main.qcloudimg.com/raw/2098031bcf22b6a32ac87066ed8a3278.jpg)

>**注意：**
>请您按照图示来添加配置文件，`tac_service_configurations_unpackage.json` 文件中包含了敏感信息，请不要打包到 APK 文件中，MobileLine SDK 也会对此进行检查，防止由于您误打包造成的敏感信息泄露。


## 第三步：集成 SDK

下表展示了 MobileLine 各种服务所对应的库

|功能|服务名称|Gradle 依赖项|
|:---|:---|:---|
|腾讯移动分析（MTA）|analytics|com.tencent.tac:tac-core|
|腾讯移动推送（信鸽）|messaging|com.tencent.tac:tac-messaging|
|腾讯崩溃服务（bugly）|crash|com.tencent.tac:tac-crash|
|腾讯计费（米大师）|payment|com.tencent.tac:tac-payment|
|移动存储（Storage）|storage|com.tencent.tac:tac-storage|
|登录与授权（Authorization）|authorization|com.tencent.tac:tac-authorization|


如果您想集成我们的各种服务，那么您只需要在您应用级 build.gradle 文件（通常是 app/build.gradle）中添加对应的服务依赖即可：

例如，您只想集成 `analytics` 服务，

```
dependencies {
	// 增加这行
	compile 'com.tencent.tac:tac-core:1.3.+'
}
```

如果您想集成 `messaging` 服务：

```
dependencies {
	// 增加这两行，其中 core 是所有其他模块的基础
	compile 'com.tencent.tac:tac-core:1.3.+'
	compile 'com.tencent.tac:tac-messaging:1.3.+'
}
```

如果您想同时集成 `messaging` 和 `crash` 服务：

```
dependencies {
	// 增加这三行，其中 core 是所有其他模块的基础
	compile 'com.tencent.tac:tac-core:1.3.+'
	compile 'com.tencent.tac:tac-messaging:1.3.+'
	compile 'com.tencent.tac:tac-crash:1.3.+'
}
```

此外，我们提供了 gradle 插件，帮您进一步降低集成成本，

```
apply plugin: 'com.tencent.tac.services'
```

> **注意：**
> 使用 Payment 计费等服务时还需要额外的配置，详情请参见各自服务的快速入门。

## 第四步：参考各个服务的快速入门

一些子服务可能还有其他的集成步骤，请参考各个服务的快速入门文档。

|功能|服务名称|入门指南|
|:---|:---|:---|
|腾讯移动分析（MTA）|analytics|[Analytics 快速入门](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E5%88%86%E6%9E%90%20Analytics%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Android%20%E6%96%87%E6%A1%A3/Android%20%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.md)|
|腾讯移动推送（信鸽）|messaging|[Messaging 快速入门](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E9%80%9A%E7%9F%A5%E6%8E%A8%E9%80%81%20Messaging%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Android%20%E6%96%87%E6%A1%A3/Android%20%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.md)|
|腾讯崩溃服务（bugly）|crash|[Crash 快速入门](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E5%B4%A9%E6%BA%83%E7%9B%91%E6%B5%8B%20Crash%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Android%20%E6%96%87%E6%A1%A3/Android%20%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.md)|
|腾讯计费（米大师）|payment|[Payment 快速入门](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Android/Android%20%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.md)|
|移动存储（Storage）|storage|[Storage 快速入门](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E5%AD%98%E5%82%A8%20Storage%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Android%20%E6%96%87%E6%A1%A3/Android%20%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.md)|
|微信 QQ 登录（Authorization）|authorization|[Authorization 快速入门](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%8E%88%E6%9D%83%20Authorization%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/Android%20%E6%96%87%E6%A1%A3/Android%20%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8.md)|
