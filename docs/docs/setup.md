## 基本要求
- CrystalExpress 需要 iOS 7.0 或是以上版本

## SDK 整合之前事項
- 如果您的 app 沒有申請過 `Crystal_Id`, 請跟 Intowow 申請

## 安裝 SDK
### 使用 Cocoapods
- 我們強烈建議您使用 Cocoapods 來整合 CrystalExpress SDK
- 將下列的代碼加入到 Podfile 中
```
pod "CrystalExpressSDK-CN", '~> 1.6'
```
- 執行 `pod update` 或 `pod install`
- 打開 pod 產生好的 {yourprojectname}.xcworkspace, CrystalExpressSDK 已經安裝完成
- 可參考 [sample project](https://github.com/ytli1204/CrystalExpressCNSample)

### 手動安裝 SDK
1. 下載 SDK
    - [CrystalExpressSDK-1.8](http://s3.cn-north-1.amazonaws.com.cn/intowow-sdk/ios/manual/cn/CrystalExpressSDK-CN-1.8.0.zip)
2. 打開 xcode 中的 project 設定頁面, Build Phases > Link Binary With Libraries, 加入 CrystalExpressSDK-x.x.x.a
3. 將 zip 檔中的 header 檔加入 project 中
4. 確認以下的 frameworks 都已加入 Build Phases
    - Security.framework
    - CFNetwork.framework
    - MessageUI.framework
    - MobileCoreServices.framework
    - SystemConfiguration.framework
    - AdSupport.framework
    - libz.dylib
    - libc++.dylib
    - CoreTelephony.framework
    - CoreMedia.framework
    - libsqlite3.dylib
    - AVFoundation.framework
    - libicucore.dylib
5. 在 TARGETS -> Build Settings -> Linking -> Other Linker Flags 加入 `-ObjC`
6. 安裝完成, 您可以開始使用 CrystalExpress SDK library

## 設定允許 Http 連線
- 在 xcode7 (iOS9) 之後, 會阻擋 http 連線, 需要在 Info.plist 中加入忽略的網址清單
- 目前許多廣告的點擊連結都還是 http 連線, 我們無法事先預知需要排除的 domain, 因此需要允許任意的 http 連線

```xml
<key>NSAppTransportSecurity</key>
<dict>
     <key>NSAllowsArbitraryLoads</key>
     <true/>
</dict>
```

##設定第三方 APP 的白名單
- 在xcode7(iOS9 SDK)之後，若是需要開啟第三方APP時，APP需要在Info.plist中將第三方APP的url schemes列為白名單，才可以正常檢查是否安裝或是開啟網頁．例如希望以Facebook APP 開啟 fb://profile/1493535890869． 你可以在以下連結得到更多細節 https://developer.apple.com/videos/wwdc/2015/?id=703.
```xml
<key>LSApplicationQueriesSchemes</key>
<array>
		<string>fb</string>
		<string>twitter</string>
		<string>youtube</string>
		<string>...</string>
</array>
```
