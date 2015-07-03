## 觸發時機
開機蓋屏的觸發時機為每次開啟 App, 都有機會顯示開機蓋屏

## 支援廣告格式
開機蓋屏支援的廣告格式為 **蓋屏系列**

## 整合方式
- 在 AppDelegate.m, App 從背景進入前景的階段去要求蓋屏廣告
    - 可參考範例程式 [AppDelegate.m](https://github.com/roylo/CrystalExpressCNSample/blob/master/CrystalExpressAppCN/CrystalExpressAppCN/AppDelegate.m)

### 整合細節說明
- App 重新啟動時, 首先初始化 SDK, 並設定應該要求開機蓋屏廣告
```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    .....

    [I2WAPI initWithVerboseLog:YES isTestMode:NO];
    _shouldRequestOpenSplash = YES;

    .....
    return YES;
}
```

- App 進入前景時, 設定應該要求開機蓋屏廣告
```objc
- (void)applicationWillEnterForeground:(UIApplication *)application
{
    _shouldRequestOpenSplash = YES;
    [I2WAPI refreshI2WAds];
}
```

- `applicationWillResignActive:` 和 `application:openURL:sourceApplication:annotation:` 不需呼叫開機蓋屏
```objc
- (void)applicationWillResignActive:(UIApplication *)application
{
    _shouldRequestOpenSplash = NO;
    .....
}

#pragma mark - deeplinking
- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation
{
    .....
    _shouldRequestOpenSplash = NO;
    return YES;
}
```

- 統一在 `applicationDidBecomeActive:` 時要求開機蓋屏廣告
- 若此時沒有呼叫開機蓋屏, 則準備 App 的內容 viewController
```objc
- (void)applicationDidBecomeActive:(UIApplication *)application
{
    BOOL hasReqOpenSplash = [self requestOpenSplash];
    if (!hasReqOpenSplash) {
        [self prepareContentViewController];
    }
}
```

- 先確定目前 UI 上沒有蓋屏廣告, 才要求開機蓋屏廣告
```objc
- (BOOL)requestOpenSplash
{
    if (_shouldRequestOpenSplash == NO) {
        return NO;
    } else {
        _shouldRequestOpenSplash = NO;
    }

    BOOL isShowingSplashAd = NO;
    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
        if ([topController isKindOfClass:[SplashADInterfaceViewController class]]) {
            isShowingSplashAd = YES;
            return NO;
        }
    }

    [_openSplashHelper requestSplashADWithPlacement:@"OPEN_SPLASH" mode:CE_SPLASH_MODE_SINGLE_OFFER];
    return YES;
}
```

- 設定蓋屏廣告的 Delegate 函式
    - 成功收到蓋屏廣告的 ViewController, 顯示在 UI 上並開始準備 App 內容 viewController
    - SDK 回應要求蓋屏廣告失敗, 準備 App 內容 viewController
```objc
#pragma mark - SplashADHelperDelegate
- (void)SplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    [vc setDelegate:self];

    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [topController presentViewController:vc animated:YES completion:^{
        [self prepareContentViewController];
    }];
}

- (void)SplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);
    [self prepareContentViewController];
}
```
