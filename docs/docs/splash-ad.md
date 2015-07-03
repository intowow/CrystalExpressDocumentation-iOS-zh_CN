## 基本需求
- App 必須支援直向與橫向兩種轉向

## 初始化 SplashADHelper
我們提供了一個 SplashADHelper 類別讓整合蓋屏廣告更簡單, 透過 SplashADHelper, 您可以簡單的要求蓋屏廣告
```objc
// init splash helper
_splashHelper = [[SplashADHelper alloc] init];

// set helper delegate
[_splashHelper setDelegate:self];
```

## 要求蓋屏廣告
要求蓋屏廣告只需要帶著廣告版位名稱與模式, 透過 SplashADHelper 呼叫即可
```objc
// request AD with placement and mode
// You might need to change @"OPEN_SPLASH" to correct placement name
[_splashHelper requestSplashADWithPlacement:@"OPEN_SPLASH" mode:CE_SPLASH_MODE_SINGLE_OFFER];
```
- `placement` 廣告版位名稱是一個獨一無二的字串用來代表廣告版位
- `mode` Splash AD 模式可以控制回傳回來的蓋屏廣告種類, 我們建議使用 `CE_SPLASH_MODE_SINGLE_OFFER`
    - 其他模式請參考 [API reference]()

## SplashADHelper 回調
SplashADHelper 會呼叫 delegate 函式並且返回準備好的 `SplashADInterfaceViewController`
```objc
#pragma mark - SplashADHelperDelegate

- (void)SplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    // set delegate to receive splash AD viewcontroller events (Optional)
    [vc setDelegate:self];

    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [topController presentViewController:vc animated:YES completion:^{
        // you can prepare app content here
        ...
    }];
}
```
若是呼叫蓋屏廣告失敗, 則會呼叫此函式並提供錯誤的訊息
```objc
- (void)SplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);
}
```

## SplashAD ViewController delegate (Optional)
您可以對 `SplashADInterfaceViewController` 註冊 delegate 來收取相關的事件回調
```objc
#pragma mark - SplashADViewControllerDelegate
// splash ad viewcontroller is going to dismiss from screen
- (void)SplashAdWillDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller is goint to present on screen
- (void)SplashAdWillPresentScreen:(SplashADInterfaceViewController *)vc
{

}

// splash ad viewcontroller did dismiss from screen
- (void)SplashAdDidDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller did present to screen
- (void)SplashAdDidPresentScreen:(SplashADInterfaceViewController *)vc
{

}
```
**注意** 蓋屏廣告支援直向和橫向兩種廣告, 在整合之前請確認您的 app 對於兩種轉向都有支援

## 常見的蓋屏廣告版位
- [開機蓋屏](open-splash-ad.md)
- [插頁蓋屏](interstitial-ad.md)

***
瞭解更多:

- [API reference](api-reference.md)
- [中英術語對照](https://github.com/roylo/CrystalExpressDocumentation-iOS-zh_CN/blob/master/terminology.md)
