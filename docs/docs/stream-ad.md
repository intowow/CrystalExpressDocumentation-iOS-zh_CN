## 基本需求
- 此信息流廣告是基於 UITableViewController 類別而設計

## 1. 初始化 CEStreamADHelper
- 我們提供 CEStreamADHelper 類別讓信息流廣告整合變得更容易, 透過 CEStreamADHelper, 您可以要求並管理信息流廣告
- 在物件生成階段初始化 CEStreamADHelper (例如. `viewDidLoad`)
```objc
- (void)viewDidLoad
{
    .....

    // prepare your tableview data source
    [self prepareDataSource];

    //
    [self setupStreamADHelper];
   .....
}

#pragma mark - stream ADHelper
- (void)setupStreamADHelper
{
    _adHelper = [CETableViewADHelper helperWithTableView:self.tableView viewController:self placement:@"STREAM"];

    // @optional
    // You can assign a AD width for stream AD
    [_adHelper setAdWidth:310];

    // start request AD
    [_adHelper loadAd];
}

```

## 2. 更新 viewController 出現/隱藏
- 當 viewController 出現在使用者眼前時, 我們需要呼叫 onShow 讓廣告啟動播放 (譬如 `viewDidAppear`)
- 當 viewController 消失在使用者眼前時, 我們需要呼叫 onHide 讓廣告停止播放 (譬如 `viewDidDisappear`)

```objc
- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    [_adHelper onShow];
}

- (void)viewDidDisappear:(BOOL)animated
{
    [super viewDidDisappear:animated];
    [_adHelper onHide];
}
```

## 3. 取代 `UITableView` 呼叫的方法
- 將下列 `UITableView` 的方法改使用 CrystalExpressSDK category 等同的方法

例如原本使用:

```objc
[self.tableView selectRowAtIndexPath:myIndexPath];
```

改為

```objc
[self.tableView ce_selectRowAtIndexPath:myIndexPath];
```

這些方法就像一般的 `UITableView` 方法, 但會根據插入的廣告來調整 `NSIndexPath`

`**重要**` 這些方法置換是非常重要的, 如果你跳過了這個步驟, 內容欄位的位置可能會出現錯誤

| 原始方法                                          | 替換方法                                             |
|---------------------------------------------------|------------------------------------------------------|
| setDelegate:                                      | ce_setDelegate:                                      |
| delegate                                          | ce_delegate                                          |
| setDataSource:                                    | ce_setDataSource:                                    |
| dataSource                                        | ce_dataSource                                        |
| reloadData                                        | ce_reloadData                                        |
| rectForRowAtIndexPath:                            | ce_rectForRowAtIndexPath:                            |
| indexPathForRowAtPoint:                           | ce_indexPathForRowAtPoint:                           |
| indexPathForCell:                                 | ce_indexPathForCell:                                 |
| indexPathsForRowsInRect:                          | ce_indexPathsForRowsInRect:                          |
| cellForRowAtIndexPath:                            | ce_cellForRowAtIndexPath:                            |
| visibleCells                                      | ce_visibleCells                                      |
| indexPathsForVisibleRows:                         | ce_indexPathsForVisibleRows:                         |
| scrollToRowAtIndexPath:atScrollPosition:animated: | ce_scrollToRowAtIndexPath:atScrollPosition:animated: |
| beginUpdates                                      | ce_beginUpdates                                      |
| endUpdates                                        | ce_endUpdates                                        |
| insertSections:withRowAnimation:                  | ce_insertSections:withRowAnimation:                  |
| deleteSections:withRowAnimation:                  | ce_deleteSections:withRowAnimation:                  |
| reloadSections:withRowAnimation:                  | ce_reloadSections:withRowAnimation:                  |
| moveSection:toSection:                            | ce_moveSection:toSection:                            |
| insertRowsAtIndexPaths:withRowAnimation:          | ce_insertRowsAtIndexPaths:withRowAnimation:          |
| deleteRowsAtIndexPaths:withRowAnimation:          | ce_deleteRowsAtIndexPaths:withRowAnimation:          |
| reloadRowsAtIndexPaths:withRowAnimation:          | ce_reloadRowsAtIndexPaths:withRowAnimation:          |
| moveRowAtIndexPath:toIndexPath:                   | ce_moveRowAtIndexPath:toIndexPath:                   |
| indexPathForSelectedRow:                          | ce_indexPathForSelectedRow:                          |
| indexPathsForSelectedRows:                        | ce_indexPathsForSelectedRows:                        |
| selectRowAtIndexPath:animated:scrollPosition:     | ce_selectRowAtIndexPath:animated:scrollPosition:     |
| deselectRowAtIndexPath:animated:                  | ce_deselectRowAtIndexPath:animated:                  |
| dequeueReusableCellWithIdentifier:forIndexPath:   | ce_dequeueReusableCellWithIdentifier:forIndexPath:   |

## 4. 更新 tableView 資料源
tableView 定期更新資料源的機制是相當常見的 (例如. 下拉更新). 當資料源被更新時, 同時 streamADHelper 也要清除之前已讀取被暫存的廣告
```objc
- (void)refresh
{
    [self.pullToRefreshView startLoading];
    [self prepareDataSources];
    [_adHelper cleanAds];
    [self.tableView reloadData];
    [self.pullToRefreshView finishLoading];
}

- (void)pullToRefreshViewDidStartLoading:(SSPullToRefreshView *)view
{
    [self refresh];
}
```
***
瞭解更多:

- [API reference](api-reference.md)
- [中英術語對照](https://github.com/roylo/CrystalExpressDocumentation-iOS-zh_CN/blob/master/terminology.md)
