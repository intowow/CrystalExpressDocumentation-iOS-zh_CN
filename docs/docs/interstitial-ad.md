## 觸發時機
從文章內文頁返回頻道列表⾴時

## 支援廣告格式
插頁蓋屏支援的廣告格式為 **蓋屏系列**

## 整合方式
- 可參考[範例程式](https://github.com/roylo/CrystalExpressCNSample/blob/5e4ac9cb1e44021cea7d7d4bae4fc8fb0dba36a2/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoStreamSectionViewController.m#L239)

### 整合細節說明
- 當使用者閱讀完文章頁退回到頻道列表頁後彈出插頁蓋屏
- 在範例程式中, `DemoDetailContentViewController` 為文章內容頁, `DemoStreamSectionViewController` 為頻道列表頁
- 在 `DemoDetailContentViewController` 偵測到使用者按了返回鍵, 在退出 viewController 動畫完成後呼叫頻道列表頁 viewController 的 requestInterstitialSplashAD 方法
    - [觀看程式碼](https://github.com/roylo/CrystalExpressCNSample/blob/5e4ac9cb1e44021cea7d7d4bae4fc8fb0dba36a2/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoDetailContentViewController.m#L148)
```objc
#pragma mark - DemoDetailContentViewController.m
- (void)pressBackBtn:(id)sender
{
    [CATransaction begin];
    [CATransaction setCompletionBlock:^{
        [_delegate requestInterstitialSplashAD];
    }];

    [self.navigationController popViewControllerAnimated:YES];

    [CATransaction commit];
}
```

- 在`DemoStreamSectionViewController` 中實作 requestInterstitialSplashAD 方法
    - [觀看程式碼](https://github.com/roylo/CrystalExpressCNSample/blob/5e4ac9cb1e44021cea7d7d4bae4fc8fb0dba36a2/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoStreamSectionViewController.m#L233)
- 實作 CESplashADDelegate 方法
    - [觀看程式碼](https://github.com/roylo/CrystalExpressCNSample/blob/5e4ac9cb1e44021cea7d7d4bae4fc8fb0dba36a2/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoStreamSectionViewController.m#L236)
```objc
#pragma mark - DemoStreamSectionViewController.m
- (instancetype)init
{
    self = [super init];
    if (self) {
        ....
        _CEInterstitialSplash = [[CESplashAD alloc] initWithPlacement:@"INTERSTITIAL_SPLASH" delegate:self];
    }
    return self;
}

- (void)requestInterstitialSplashAD
{
    [_CEInterstitialSplash loadAd];
}

#pragma mark - CESplashADDelegate
- (void)CESplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    [_CEInterstitialSplash showFromViewController:self animated:YES];
}

- (void)CESplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog("load interstitial ad fail, error:%@", error);
}

- (void)CESplashAdWillDismissScreen:(SplashADInterfaceViewController *)vc
{
}

- (void)CESplashAdWillPresentScreen:(SplashADInterfaceViewController *)vc
{
}

- (void)CESplashAdDidDismissScreen:(SplashADInterfaceViewController *)vc
{
}

- (void)CESplashAdDidPresentScreen:(SplashADInterfaceViewController *)vc
{
}
```
