## 送審前請檢查以下事項
### SDK 功能相關
1. [App 有註冊背景工作](register-background-task.md)
2. [App 有註冊背景抓取](register-background-fetch.md)
3. App 有深連結, 並且將 URL scheme 告知 Intowow
    - [啟用廣告預覽功能](ad-preview.md)

### 廣告整合相關
1. 初始化 SDK 關閉偵錯訊息與測試模式<br>
`[I2WAPI initWithVerboseLog:NO isTestMode:NO];`
2. 使用正確的 Crystal_id, 非測試用 Crystal_id
