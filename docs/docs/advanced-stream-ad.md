## 信息流廣告進階設定

### 信息流廣告播放控制

`CETableViewADHelper.h` 提供了三個 API 可以控制信息流廣告播放/停止

1. 取消廣告自動播放, 取消之後所有信息流廣告出現都會先是圖片 (視頻會展示影片封面)
2. 開始廣告播放
3. 停止廣告播放

```objc
// first init CETableViewADHelper
_adHelper = [CETableViewADHelper helperWithTableView:self.tableView viewController:self placement:@"STREAM"];

// 1. disable AD auto play
[_adHelper setAutoPlay:NO];

// 2. start AD play at NSIndexPath
[_adHelper startAdAtIndexPath:indexPath];

// 3. stop AD play at NSIndexPath
[_adHelper stopAdAtIndexPath:indexPath];
```

### 信息流廣告 UI 設定
`CETableViewADHelper.h` 提供下列 API 調整信息流廣告的 UI

1. 設定信息流廣告寬度
2. 設定信息流廣告上下的邊界高度
3. 設定信息流廣告背景顏色
4. 客製化信息流廣告 UI

```objc
/**
 *  @optional
 *  this is an optional method to set stream AD's width
 *  stream AD's height will be adjusted to keep the original creative ratio from width
 *
 *  @param width stream AD width
 */
- (void)setAdWidth:(float)width;

/**
 *  @optional
 *  this is an optiona; method to set stream AD's vertical margin between cells
 *
 *  @param verticalMargin the margin height
 */
- (void)setAdVerticalMargin:(float)verticalMargin;

/**
 *  @optional
 *  this is an optional method to set stream AD's background color
 *
 *  @param bgColor background color
 */
- (void)setAdBackgroundColor:(UIColor *)bgColor;

/**
 *  @optional
 *  this is an optional method to customized adCell view after ADView is set in UITableViewCell
 *
 *  @param customizedAdCellBlock customized code block
 */
- (void)setAdCellCustomizedBlock:(void (^)(UITableViewCell *adCell))customizedAdCellBlock;
```

