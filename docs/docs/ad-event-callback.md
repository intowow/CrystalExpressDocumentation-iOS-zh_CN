CrystalExpress SDK 提供廣告事件的回調, 當廣告曝光與點擊時回調 App, 使用方法如下：

## 1. 宣告實作 Delegate 方法
```objc
#import "I2WAPI.h"

// implement I2WADEventDelegate method
@interface AppDelegate() <I2WADEventDelegate>
```

## 2. 初始化 I2WAPI 之後, 設定廣告回調代理人
```objc
// init I2WAPI
[I2WAPI initWithVerboseLog:NO isTestMode:NO];

// set delegate to self
[I2WAPI setAdEventDelegate:self];
```

## 3. 實作廣告回調函式
- 廣告回調時, 會帶著廣告的 id 提供辨識
- 要注意回調的方法不一定是從主線程回調, 若是需要操作 UI, 請在主線程操作

```objc
#pragma mark - adEvent delegate
- (void)onAdClick:(NSString *)adId
{
    dispatch_async(dispatch_get_main_queue(), ^{
        NSLog(@"ad[%@] click!", adId);
    });
}

- (void)onAdImpression:(NSString *)adId
{
    dispatch_async(dispatch_get_main_queue(), ^{
        NSLog(@"ad[%@] impression!", adId);
    });
}
```
