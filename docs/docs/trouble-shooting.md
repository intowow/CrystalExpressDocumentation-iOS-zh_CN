While in debug, initialize I2WAPI with verbose log may print debug messages.

## Error/Crash in console
### Crystal_id is not correct
- Your console log has the following line.
    - `error:[Request failed: not found (404)], please reverify your crystal_id is set correct`
- This means your crystal_id is not correct, correct it in CrystalExpress.plist

### [__NSArrayI enumFromString:]: unrecognized selector
- Your app crash like this:
```
[__NSArrayI enumFromString:]: unrecognized selector sent to instance 0x78e37970
```
- If you crash on log like this, add -ObjC in TARGETS -> Build Settings -> Linking -> Other Linker Flags

### Crash on UIApplicationInvalidInterfaceOrientation
- Your app crash on this:
```
Terminating app due to uncaught exception 'UIApplicationInvalidInterfaceOrientation', reason: 'Supported orientations has no common orientation with the application, and [SOSplashADViewController shouldAutorotate] is returning YES'
```
- If you encounter the above exception, it means your app doesn't support the rotation which Splash AD need to display.
- Please reverify the app supported orientation in your project setting.
