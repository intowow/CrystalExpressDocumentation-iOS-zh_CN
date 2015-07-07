## 觸發時機
從文章內文頁返回頻道列表⾴時

## 支援廣告格式
插頁蓋屏支援的廣告格式為 **蓋屏系列**

## 整合方式
- 可參考[範例程式](https://github.com/roylo/CrystalExpressCNSample/blob/4d5143ed1251c91aec6ba9dc19d86aef2e7ed1fb/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoStreamSectionViewController.m#L239)

### 整合細節說明
- 當使用者閱讀完文章頁退回到頻道列表頁後彈出插頁蓋屏
- 在範例程式中, `DemoDetailContentViewController` 為文章內容頁, `DemoStreamSectionViewController` 為頻道列表頁
- 在 `DemoDetailContentViewController` 偵測到使用者按了返回鍵, 在退出 viewController 動畫完成後呼叫頻道列表頁 viewController 的 requestInterstitialSplashAD 方法
    - [觀看程式碼](https://github.com/roylo/CrystalExpressCNSample/blob/4d5143ed1251c91aec6ba9dc19d86aef2e7ed1fb/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoDetailContentViewController.m#L153)
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
    - [觀看程式碼](https://github.com/roylo/CrystalExpressCNSample/blob/4d5143ed1251c91aec6ba9dc19d86aef2e7ed1fb/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoStreamSectionViewController.m#L239)
- 實作 SplashADHelperDelegate 方法
    - [觀看程式碼](https://github.com/roylo/CrystalExpressCNSample/blob/4d5143ed1251c91aec6ba9dc19d86aef2e7ed1fb/CrystalExpressAppCN/CrystalExpressAppCN/ViewController/DemoStreamSectionViewController.m#L245)
```objc
#pragma mark - DemoStreamSectionViewController.m
- (void)requestInterstitialSplashAD
{
    [_interstitialSplashHelper requestSplashADWithPlacement:@"INTERSTITIAL_SPLASH" mode:CE_SPLASH_MODE_SINGLE_OFFER];
}

#pragma mark - SplashADHelperDelegate
- (void)SplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    [self presentViewController:vc animated:YES completion:nil];
}

- (void)SplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog("request interstitial AD fail, error:%@", error);
}
```
