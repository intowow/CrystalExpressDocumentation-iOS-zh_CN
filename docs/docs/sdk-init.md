## Import header

Import the header files by pasting this code into your AppDelegate :
```objc
#import "I2WAPI.h"
```

## Initialize the SDK
In your application delegate's `application:didFinishLaunchingWithOptions:` method, start the CrystalExpress SDK:
```objc
#import "I2WAPI.h"

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // init SDK
    [I2WAPI initWithVerboseLog:YES isTestMode:NO];
    return YES;
}

```
- Set `initWithVerboseLog:(BOOL)` will enable/disable the debug log in console
- Set `isTestMode:(BOOL)` will control whether to init the SDK in [TEST]() mode

