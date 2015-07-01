In AppDelegate.m, register a background task will allow CrystalExpress SDK able to fetch ADs while app enter background.
```objc
- (void)applicationDidEnterBackground:(UIApplication *)application
{
    // register bgTask while enter background mode
    __block UIBackgroundTaskIdentifier bgTask = [application beginBackgroundTaskWithExpirationHandler:^{
        [application endBackgroundTask:bgTask];
        bgTask = UIBackgroundTaskInvalid;
    }];
}
```
