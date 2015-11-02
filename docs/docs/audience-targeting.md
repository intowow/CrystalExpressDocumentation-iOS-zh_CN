## 使用定向投放
要開啟廣告定向投放, 請透過以下 API 設定用戶資訊

```objc
// I2WAPI.h
/**
 *  @brief set audience targeting tags
 *
 *  @param tags set of tag strings
 */
+ (void)setAudienceTargetingUserTags:(NSSet *)tags;
```

- `NSSet *tags` 為包含用戶資訊字串的集合, 必須和後台廣告設定符合以達到定向投放
- 當設定用戶資訊之後, CrystalExpressSDK 將預先下載並且展示符合用戶資訊的廣告
- SDK 將保存上次的設定的用戶資訊在本地端, 不需每次重複設定

### 最佳作法
- 於程式中儘早設置用戶資訊, SDK 將有更多時間準備定向廣告
- 避免在執行奇中頻繁改變用戶資訊, 這將降低預下載廣告的利用率
