移动开发平台（MobileLine）使用起来非常容易，只需要简单的 4 步，您便可快速接入移动崩溃监测。接入后，您即可获得我们提供的各项能力，减少您在开发应用时的重复工作，提升开发效率。

## 准备工作

为了使用移动开发平台（MobileLine）iOS 版本的 SDK，您首先需要一个 iOS 工程，这个工程可以是您现有的工程，也可以是您新建的一个空的工程。

## 第一步：创建项目和应用


在使用我们的服务前，您必须先在 MobileLine 控制台上创建项目和应用。

> 如果您已经在 MobileLine 控制台上创建过了项目和应用，请跳过此步。


## 第二步：添加配置文件

> 如果您已经添加过配置文件，请跳过此步。

创建好应用后，您可以点击红框中的【下载配置】来下载该应用的配置文件的压缩包：

![](https://ws2.sinaimg.cn/large/006tNc79gy1fq0pubol92j31kw093gnw.jpg)

解压后将 tac_services_configurations.plist 文件集成进项目中。其中有一个  tac_services_configurations_unpackage.plist 文件，请将该文件放到您工程的根目录下面(**切记不要将改文件添加进工程中**)。 添加好配置文件后，继续点击【下一步】。


![](https://ws1.sinaimg.cn/large/006tNc79gy1forbnw3ijyj31bi11wnch.jpg)

>**注意：**
>请您按照图示来添加配置文件，`tac_service_configurations_unpackage.plist` 文件中包含了敏感信息，请不要打包到 apk 文件中，MobileLine SDK 也会对此进行检查，防止由于您误打包造成的敏感信息泄露。



## 第三步：集成 SDK

如果还没有 Podfile，请创建一个。

~~~
$ cd your-project directory
$ pod init
~~~

并在您的 Podfile 文件中添加移动开发平台（MobileLine）的私有源：

~~~
source "https://github.com/tencentyun/cocoapods-repo.git"
source "https://github.com/CocoaPods/Specs"
~~~

添加 Podfile 依赖：

```
pod 'TACMessaging'
```

并运行命令

~~~
pod update
~~~  

并且在工程配置和后台模式中打开推送,如下图所示：

![Project Settings](http://docs.developer.qq.com/xg/assets/iOSXGCap.jpg)

### 启动服务

移动推送 服务无需启动，到此您已经成功接入了 MobileLine 移动推送服务。

## 第四步 验证

**请先参考 [iOS 推送证书设置指南](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E9%80%9A%E7%9F%A5%E6%8E%A8%E9%80%81%20Messaging%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/iOS%20%E6%96%87%E6%A1%A3/iOS%E6%8E%A8%E9%80%81%E8%AF%81%E4%B9%A6%E8%AE%BE%E7%BD%AE%E6%8C%87%E5%8D%97.md) 设置开发和发布证书**

### 在控制台上推送消息

打开 [MobileLine 控制台](https://console.cloud.tencent.com/tac)，选择【创建推送】下的【通知栏消息】，并填写好 **通知标题** 和 **通知内容**，然后选择单选框中的【单个设备】，并将注册成功后回调时打印的设备唯一标识 token 信息拷贝到编辑框中，您也可以在推送时添加自定义参数，然后点击【确认推送】。

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fqmfgsejl4j31kw16uk69.jpg)

### 验证通知是否发送成功

推送通知栏消息成功后，若 Messaging SDK 接收到了通知，如果您的程序在前台则会调用 `AppDelegate` 的 `application:didReceiveRemoteNotification` 方法，您可以在该方法中调用如下方法打印日志：

```
// 收到通知栏消息后回调此接口。
- (void) application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
     NSLog(@"got messaging");
}
```

收到通知后终端将会输出日志：

~~~
2018-04-20 16:37:06.983857+0800 TACSamples[384:51189] got messaging
~~~

