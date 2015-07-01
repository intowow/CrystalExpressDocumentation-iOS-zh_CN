在 AppDelegate.m 中, 註冊背景工作將允許 CrystalExpress SDK 可以在 app 進入背景時有短暫的時間可以下載廣告素材
```objc
- (void)applicationDidEnterBackground:(UIApplication *)application
{
    // register bgTask while enter background mode
    __block UIBackgroundTaskIdentifier bgTask = [application beginBackgroundTaskWithExpirationHandler:^{
        [application endBackgroundTask:bgTask];
        bgTask = UIBackgroundTaskInvalid;
    }];
}
```
