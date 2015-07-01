By utilizing ios deeplink, we can to do AD preview in real app, sample url link like follows: {urlScheme}://adpreview?adid={number}

CrystalExpress SDK will only handle adpreview url, and ignore others.
## How to enable app deeplink ?
1. First you need to register a URL scheme in you app, in Project > Info > URL Types, register a url scheme for your app to enable the deeplink.
    - [TODO] 補圖
2. Add the following code in AppDelegate.m to enable the crystalexpress adPreview function.

```objc
- (BOOL)application:(UIApplication *)application
                        openURL:(NSURL *)url
    sourceApplication:(NSString *)sourceApplication
                annotation:(id)annotation
{
        [I2WAPI handleDeepLinkWithUrl:url sourceApplication:sourceApplication];

        .....
        return YES;
}
```
