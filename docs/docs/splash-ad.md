## Init SplashADHelper
We provided a helper class to make integration more easier, via SplashADHelper, you can easily request Splash ADs.
```objc
// init splash helper
_splashHelper = [[SplashADHelper alloc] init];

// set helper delegate
[_splashHelper setDelegate:self];
```

## Request Splash AD
Request splash AD is really simple, and you may controll the splash AD type while request.
```objc
// request AD with placement and mode
[_splashHelper requestSplashADWithPlacement:@"OPEN_SPLASH" mode:CE_SPLASH_MODE_SINGLE_OFFER];
```
- `placement` is an unique string to indicate this AD space
- `mode` can configure the splash AD type, we suggest to use `CE_SPLASH_MODE_SINGLE_OFFER` here
    - For more detail, please refer to the [API reference]()

## SplashADHelper delegate
SplashADHelper will call delegate function and return a ready `SplashADInterfaceViewController` for you to present.
```objc
#pragma mark - SplashADHelperDelegate

- (void)SplashADDidReceiveAd:(NSArray *)ad viewController:(SplashADInterfaceViewController *)vc
{
    // set delegate to receive splash AD viewcontroller events (Optional)
    [vc setDelegate:self];

    UIViewController *topController = [UIApplication sharedApplication].keyWindow.rootViewController;
    while (topController.presentedViewController) {
        topController = topController.presentedViewController;
    }

    [topController presentViewController:vc animated:YES completion:^{
        // you can prepare app content here
        ...
    }];
}

- (void)SplashADDidFailToReceiveAdWithError:(NSError *)error viewController:(SplashADInterfaceViewController *)vc
{
    NSLog(@"fail to request OPEN_SPLASH, reason:%@", error);
}
```

## SplashAD ViewController delegate (Optional)
You can hook `SplashADInterfaceViewController` related event for your needs.
```objc
#pragma mark - SplashADViewControllerDelegate
// splash ad viewcontroller is going to dismiss from screen
- (void)SplashAdWillDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller is goint to present on screen
- (void)SplashAdWillPresentScreen:(SplashADInterfaceViewController *)vc
{

}

// splash ad viewcontroller did dismiss from screen
- (void)SplashAdDidDismissScreen:(SplashADInterfaceViewController *)vc
{
}

// splash ad viewcontroller did present to screen
- (void)SplashAdDidPresentScreen:(SplashADInterfaceViewController *)vc
{

}
```
**Notice** There are both portrait and landscape Splash AD support in our SDK, make sure your app support that kind of rotation before you serving ADs.
***
More information

- [API reference]()
