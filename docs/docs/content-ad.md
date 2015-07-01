## Requirements
- Content AD is designed for scrollView page.
- Pair with streamView, not 1 content page.

## Init ContentADHelper
Pair ContentADHelper with your streams, so initialize it in your stream viewController (or the class you init/manage detail page viewControllers), then assign it to detail page viewController.
```objc
- (instancetype)init
{
    self = [super init];
    if (self) {
        .....

        // replace @"CONTENT" with your own placement name
        _contentADHelper = [[ContentADHelper alloc] initWithPlacement:@"CONTENT"];

        // set pulldown animation delegate to the right detail page viewController
        __weak typeof(self) weakSelf = self;
        [_contentADHelper setOnPullDownAnimation:^(UIView *view) {
            [[weakSelf.contentVCs objectAtIndex:weakSelf.curIndex] onPullDownAnimationWithAD:view];
        }];

        // preroll to preapre 1 content AD in advance
        [_contentADHelper preroll];

        .....
    }
    return self;
}
```

Assign contentADHelper to detail page viewController.
```objc
DemoContentViewController *newContentVC = [[DemoContentViewController alloc] initWithADHelper:_contentADHelper];
```

## Request Content AD
Request content AD while you load detail page content with article unique id.
```objc
- (void)loadContentWithId:(NSString *)articleId
{
    .....
    // You may want a wrapper view on adView to add AD margin
    _adWrapperView = [[UIView alloc] init];
    _contentADView = [_contentADHelper requestADWithContentId:articleId];
    [_contentADHelper setScrollOffsetWithKey:articleId offset:currentScrollOffset];
    if (_contentADView) {
        CGFloat horMargin = (self.view.bounds.size.width - (_contentADView.bounds.size.width + 2*AD_MARGIN))/2.0f;
        [_adWrapperView setFrame:CGRectMake(horMargin, currentScrollOffset, _contentADView.bounds.size.width + 2*AD_MARGIN, _contentADView.bounds.size.height + 2*AD_MARGIN)];
        horMargin = (_adWrapperView.bounds.size.width - _contentADView.bounds.size.width)/2.0f;
        [_contentADView setFrame:CGRectMake(horMargin, AD_MARGIN, _contentADView.bounds.size.width, _contentADView.bounds.size.height)];
        [_adWrapperView addSubview:_contentADView];
        _adOffset = currentScrollOffset;
        currentScrollOffset += _adWrapperView.bounds.size.height;
        [_scrollView addSubview:_adWrapperView];

    } else {
        // add default offset if there's no content AD
        currentScrollOffset += 10;
    }

    // Here's the view under content AD
    _theViewUnderContentAd = [[UIView alloc] init];
    .....
}
```

## onPullDownAnimation
- `onPullDownAnimationWithAD:` will only be called with a specific AD format (Card-Video-PullDown)
- When the AD is clicked by user, the engage module will extend from the AD view's bottom. Therefore, scrollView should update the views under content AD for the animation.
```objc
- (void)onPullDownAnimationWithAD:(UIView *)adView
{
    if (adView == _articleADView) {
        CGRect frame = [_adWrapperView frame];
        frame.size.height = adView.bounds.size.height + 2*AD_MARGIN;
        [_adWrapperView setFrame:frame];

        frame = [_theViewUnderContentAd frame];
        frame.origin.y = _adOffset + _adWrapperView.bounds.size.height;
        [_theViewUnderContentAd setFrame:frame];

        CGFloat finalContentOffset = _adOffset + adView.bounds.size.height + _theViewUnderContentAd.bounds.size.height;
        [_scrollView setContentSize:CGSizeMake(self.view.bounds.size.width, finalContentOffset)];
    }
}
```

## Hook viewController & tableView events
### Update scrollView state to allow helper check AD start/stop
```objc
- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    [_contentADHelper checkAdStartWithKey:_articleId ScrollViewBounds:[_scrollView bounds]];
    .....
}

- (void)viewDidDisappear:(BOOL)animated
{
    [super viewDidDisappear:animated];
    // give CGRectZero to force content AD stop
    [_contentADHelper checkAdStartWithKey:_articleId ScrollViewBounds:CGRectZero];
    .....
}

- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    if (decelerate == NO) {
        [_contentADHelper checkAdStartWithKey:_articleId ScrollViewBounds:[scrollView bounds]];
    }
}

- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView
{
    [_contentADHelper checkAdStartWithKey:_articleId ScrollViewBounds:[scrollView bounds]];
}
```

### Update scrollView visible bounds
```objc
- (void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    [_contentADHelper updateScrollViewBounds:[scrollView bounds] withKey:_articleId];
}
```
***
More information

- [API reference]()
