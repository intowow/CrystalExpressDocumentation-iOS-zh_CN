在 AppDelegate.m 中, 註冊背景抓取將允許當 iOS 不定期背景喚醒 app 時, CrystalExpress SDK 可以下載廣告素材

## 如何啟用 app 背景抓取功能?
1. 在專案設定中, Target -> Capabilities -> 設定 Background Modes to ON, 勾選 "Background Fetch"
    - [TODO] 補圖
2. 在 AppDelegate.m 加入下方代碼:

```objc
- (void)application:(UIApplication *)application performFetchWithCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
{
    [I2WAPI triggerBackgroundFetchOnSuccess:^{
        completionHandler(UIBackgroundFetchResultNewData);
    } onFail:^{
        completionHandler(UIBackgroundFetchResultFailed);
    } onNoData:^{
        completionHandler(UIBackgroundFetchResultNoData);
    }];

    ......
}
```
