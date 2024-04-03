# Day09 - DevOps x Observability 小結

大家好，我是伐伐伐伐木工

> 前面介紹了 Business Continuity、SLA、DR、Availability & Reliability 等概念，那它們之間有什麼關聯呢 ? 

## SLA、DR、Availability 和 Reliability：系統穩定性的關鍵因素
在自己過去的經驗中，SLA（Service Level Agreement）、DR（Disaster Recovery）、Availability（可用性）、Reliability（可靠性）和BCP（Business Continuity Plan），在確保系統的穩定性有密切關係
- **SLA（Service Level Agreement）**：
    - SLA 是服務提供商與客戶之間定義的正式承諾
    - SLO 是為了實現 SLA 的目標所設定的，SLI 用於衡量服務的性能指標
    - 可用性的指標，確保服務或是產品可以在特定時間提供所需要的功能
- **DR（Disaster Recovery）**
    - 目的是確保組織在意外發生時能夠以最快速的方式來恢復，以確保業務連續性(BC)
    - RTO 最大可允許中斷時間，RPO 數據損失可允許的最遠回溯時點
- **可用性（Availability）**
    - 正常運行時間的百分比，使用 SLA 來衡量
    - 高可用(High Availability)意味系統能持續提供服務，減少停機時間
- **可靠性(Reliability)**
    - 特定的時間內，系統正常執行成功的機率。使用 MTBF 來衡量
    - 透過監控與告警機制，提醒團隊系統異常並緊急處理，減少 downtime 時間
- **BCP（Business Continuity Plan）**
    - 減少意外與危險可能帶來的損失，確保企業能夠持續經營
    - 包含災難復原、備份策略、緊急回應計畫。


根據以上各點以及過去幾天分享文章的內容，我整理了以下重點資訊(偏維運)，方便大家更好理解 
![](https://share.balsamiq.com/c/bka8P6e5o3pwZtD4NBS7Gn.png)

備註 : 
- 藍色框+黃色數字為比賽的文章標題與第幾天
- 眼尖的朋友可以發現缺少監控，會再補齊 XD

下一篇要來開始進入重點可觀測性 Observability 的世界，如果有任何疑問或想法，歡迎留言提出討論 !

