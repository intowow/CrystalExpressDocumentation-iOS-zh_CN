## 蓋屏廣告進階設定
### 使用者點擊廣告之後自動離開蓋屏廣告
- 當使用者點擊了廣告連結, 完成瀏覽點擊關閉之後, 也自動關閉蓋屏廣告進入內容頁
- 需要在呼叫 `showFromViewController: animated:` 之前設定

```objc
// set auto dismiss after user engage AD
[_CESplashAD setDismissAdAfterEngageAd:YES];

// need to set before this line
[_CESplashAD showFromViewController:topController animated:YES];
```
