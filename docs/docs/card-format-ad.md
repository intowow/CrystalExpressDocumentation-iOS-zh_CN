## 基本需求
針對非 TableView 的廣告整合, 由 App 控制廣告行為

## 1. 初始化 CECardADHelper
初始化 helper 並帶入廣告版位名稱
```objc
_adHelper = [[CECardADHelper alloc] initWithPlacement:@"STREAM"];
```

## 2. 要求廣告
```objc
    [_adHelper requestADonReady:^(ADView *adView) {
        // return ADView if request successfully, this is callback from main thread
        .....
    } onFailure:^(NSError *error) {
        // return error if request failed
        .....
    }];
```

## 3. 控制廣告行為 (出現/消失/播放/停止)
透過 CECardADHelper 要求廣告後會拿到廣告的 ADView, 需要 App 整合時於對的時機呼叫相對應的 API

1. 當廣告出現在用戶眼前時呼叫 `onShow`
2. 當廣告消失在用戶眼前時呼叫 `onHide`
3. 曝光廣告, 觸發視頻廣告播放時呼叫 `onStart`
4. 停止廣告播放時呼叫 `onStop`

```objc
// API in ADView
@interface ADView : UIView
/**
 *  call onShow while ADView is seen by user
 */
- (void)onShow;

/**
 *  call onHide while ADView is hide from user
 */
- (void)onHide;

/**
 *  call onStart to trigger ADView impression and video play
 */
- (void)onStart;

/**
 *  call onStop to stop ADView video play
 */
- (void)onStop;
@end
```

## 注意事項
1. 請定期重新要求廣告, 避免長時間因 App 端持有而曝光過期廣告
2. 正確控制廣告行為 <br>
   例如: `onShow` 應該是用戶看到廣告時才呼叫一次，而非加入 view hierarchy 時就呼叫 <br>
   `onShow` `onHide` 應該成對呼叫 <br>
   `onStart` `onStop` 應該成對呼叫

***
瞭解更多:

- [API reference](api-reference.md)
