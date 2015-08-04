## 基本需求
- App 必須支援直向與橫向兩種轉向

## [網頁效果預覽](https://s3.cn-north-1.amazonaws.com.cn/intowow-common/preview/SPLASH2_VIDEO_GENERAL_P_ICLICK.html)

## 1. 宣告類別實作 CESplashADDelegate
首先在您的類別實作 CESplashADDelegate 代理
```objc
@interface AppDelegate() <CESplashADDelegate>
```

## 2. 初始化 CESplashAD
- 我們提供了一個 CESplashAD 類別讓整合蓋屏廣告更簡單, 透過 CESplashAD, 您可以簡單的要求蓋屏廣告。
- 初始化時告知帶入廣告版位名稱, 並將代理人指向自己的類別
    - `placement` 廣告版位名稱是一個獨一無二的字串用來代表廣告版位
```objc
// init splash helper
_CEOpenSplashAD = [[CESplashAD alloc] initWithPlacement:@"OPEN_SPLASH" delegate:self];
```

## 3. 要求蓋屏廣告
```objc
// request AD
[_CEOpenSplashAD loadAd];
```

## 4. CESplashAD 回調
CESplashAD 會呼叫 delegate 函式並且返回準備好的 `SplashADInterfaceViewController`
```objc
#pragma mark - CESplashADDelegate
- (void)CESplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [_CEOpenSplashAD showFromViewController:topController animated:YES];
}
```
若是呼叫蓋屏廣告失敗, 則會呼叫此函式並提供錯誤的訊息
```objc
- (void)CESplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);

    // direct to your app view controller
    // ....
}
```

您可以選擇實作下列函式來得到額外的事件回調
```objc
// splash ad viewcontroller is going to dismiss from screen
- (void)CESplashAdWillDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller is goint to present on screen
- (void)CESplashAdWillPresentScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller did dismiss from screen
- (void)CESplashAdDidDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller did present to screen
- (void)CESplashAdDidPresentScreen:(SplashADInterfaceViewController *)vc
{
    // prepare your app view controller
    // ...
}
```
**注意** 蓋屏廣告支援直向和橫向兩種廣告, 在整合之前請確認您的 app 對於兩種轉向都有支援

## 常見的蓋屏廣告版位
- [開機大屏](open-splash-ad.md)
- [插頁蓋屏](interstitial-ad.md)

***
瞭解更多:

- [API reference](api-reference.md)
- [中英術語對照](https://github.com/roylo/CrystalExpressDocumentation-iOS-zh_CN/blob/master/terminology.md)
