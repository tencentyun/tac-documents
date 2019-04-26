# 获取设备推送 token


注册成功后，会返回设备 token，token 用于标识设备唯一性，同时也是 Messaging 服务维持与后台连接的唯一身份标识(APNS的token)。在 iOS SDK 中，您可以在接入 Messaging 服务时，通过重写 `AppDelegate` `application:didReceiveRemoteNotification` 方法来获取 token。

Objective-C 代码示例：
~~~
- (void) application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    [TACMessagingService defaultService].token ;
}
~~~

Swift 代码示例：
~~~
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
        TACMessagingService.default().token;
}
~~~

> 请注意请一定要在该函数中调用绑定代码，如果您没有注册成功 APNS 则不会存在变量`[TACMessagingService defaultService].token`


> 接入 Messaging 服务请参考 [这里](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E9%80%9A%E7%9F%A5%E6%8E%A8%E9%80%81%20Messaging%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/iOS%20%E6%96%87%E6%A1%A3/iOS%20%E4%BD%BF%E7%94%A8%E5%85%A5%E9%97%A8.md)
