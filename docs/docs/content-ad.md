## 基本需求
- 內文廣告是針對 scrollView 類別頁面所設計

## 1. 初始化 CEContentADHelper
- 我們提供了 CEContentADHelper 來管理內文廣告, 透過 CEContentADHelper 可以要求/管理內文廣告
- 當頁面在加載內容時, 插入一個 `UIView *adWrapperView` 作為廣告的位置, 並於要求廣告時傳入
    - 此 `adWrapperView` 需指定 origin.y 當作廣告位置, 並設定寬度, 要求回來的廣告會置中在 `adWrapperView` 中
- 初始化 CEContentADHelper 時, 帶入廣告版位名稱, scrollView, 和代表文章的 unique id

```objc
- (void)loadContentWithId:(NSString *)contentId
{
    // setup page content
      .....

    // content AD
    _adWrapperView = [[UIView alloc] initWithFrame:CGRectMake(0, scrollHeight, self.view.bounds.size.width, 0)];

    // setup page content below AD
      .....

    // you might need to set ScrollView content Size
    [_scrollView setContentSize:CGSizeMake(self.view.bounds.size.width, scrollHeight)];

    // setup content AD
    [self setupContentAdWithAdView:_adWrapperView contentId:contentId];
}

- (void)setupContentAdWithAdView:(UIView *)adView contentId:(NSString *)contentId
{
    _contentADHelper = [CEContentADHelper helperWithPlacement:@"CONTENT" scrollView:_scrollView contentId:contentId];

    // request AD
    [_contentADHelper loadAdInView:adView];
}
```

## 2. 更新 viewController 出現/隱藏
- 當 viewController 出現在使用者眼前時, 我們需要呼叫 onShow 讓廣告啟動播放 (譬如 `viewDidAppear`)
- 當 viewController 消失在使用者眼前時, 我們需要呼叫 onHide 讓廣告停止播放 (譬如 `viewDidDisappear`)

```objc
- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    [_contentADHelper onShow];
    .....
}

- (void)viewDidDisappear:(BOOL)animated
{
    [super viewDidDisappear:animated];
    [_contentADHelper onHide];
    .....
}
```
***
瞭解更多:

- [API reference](api-reference.md)
- [中英術語對照](https://github.com/roylo/CrystalExpressDocumentation-iOS-zh_CN/blob/master/terminology.md)
