## 基本要求
- CrystalExpress 需要 iOS 7.0 或是以上版本

## SDK 整合之前事項
- 確認您有跟 Intowow 取得 CrystalExpress.plist 的檔案, 檔案內容會像以下 xml (Crystal_Id 會不同)
    - 如果您的 app 沒有申請過 `Crystal_Id`, 請跟 Intowow 申請

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Crystal_Id</key>
  <string>2a317bd038f742a082ee284b478a8e37</string>
</dict>
</plist>
```

## 安裝 SDK
### 使用 Cocoapods
- 我們強烈建議您使用 Cocoapods 來整合 CrystalExpress SDK
- 將下列的代碼加入到 Podfile 中
```
pod "CrystalExpressSDK", '~> 1.2'
```
- 執行 `pod update` 或 `pod install`
- 打開 pod 產生好的 {yourprojectname}.xcworkspace, CrystalExpressSDK 已經安裝完成
- 可參考 [sample project](https://github.com/roylo/CrystalExpressSample)

### 手動安裝 SDK
1. 下載 SDK
    - [CrystalExpressSDK-1.2.3](http://intowow-demo.oss-cn-beijing.aliyuncs.com/ios_manual_sdk%2FCrystalExpressSDK-CN-1.2.3.zip)
2. 打開 xcode 中的 project 設定頁面, Build Phases > Link Binary With Libraries, 加入 CrystalExpressSDK-x.x.x.a
3. 將 zip 檔中的 header 檔加入 project 中
4. 確認以下的 frameworks 都已加入 Build Phases
    - Securty.framework
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
6. 將下列的檔案加入 project 中
    - CrystalExpress.plist
7. 安裝完成, 您可以開始使用 CrystalExpress SDK library
