# Day06 - 災難復原計畫 Disaster Recovery Plan

大家好，我是伐伐伐伐木工

今天要與大家分享災難復原計畫 DRP，本篇內容的重點如下
- 淺談難復原計畫 DRP 

## 災難復原計畫 Disaster Recovery Plan
災難復原計畫 DRP (Disaster Recovery Plan)是確保業務連續性的關鍵，這計畫將有助於當災難發生時，核心業務可以迅速且可靠的恢復正常運作。有效的 DRP 可能會包含下列要素
- **業務影響分析 (BIA)** : 檢視當災難發生時哪些核心業務是至關重要的，並依據分析與識別後的結果設定優先權。
- **RTO 與 RPO** : 透過 BIA 分析後可以了解核心業務功能的排序跟影響，制定各核心業務的 RPO 與 RTO 時間點，內部或是非關鍵功能可能 RTO/RPO 時間會較長。
- **恢復策略 (Recovery Strategies)** : 制定可靠的恢復策略，需要考慮的點有網路、基礎建設、資料庫、應用程式及測試內容。以確保需要在特定時間點恢復時需要依賴那些重要元素或工具。
- **測試與演練 (Testing and Training)** : 透過分析與計畫定期的練習，模擬當意外發生時DRP 計畫啟動到測試完成系統恢復正常
![https://ithelp.ithome.com.tw/upload/images/20230921/20162577Dyp0gA375p.png](https://ithelp.ithome.com.tw/upload/images/20230921/20162577Dyp0gA375p.png)
> 迷之聲 : 我是誰、我在哪、我要去哪裡
## 現況盤點
以下是可能會遇到的問題
> Q1 : 萬事起頭難，如何開始 ? 

A : 先了解你的業務與IT資源使用，包含業務運作、IT 基礎建設、應用程式、雲端服務商或是數據庫等等，列出您目前擁有的所有資源跟系統。

範例 
- 線上電子購物平台
- 使用 AWS 雲端服務，使用虛擬伺服器、數據庫、存儲和負載平衡器
- 應用程式包括網站前端、購物車、付款處理和庫存管理
- 數據庫存放在 IDC 機房，包含產品目錄、客戶訂單和用戶資訊

盤點後可能如下，將服務拆成不同類別，並依據既有服務可以拆分為

| Service |Name | Description |
| -------- | -------- | -------- |
| Network     | 網路設定     | AWS Elastic Ips、S3、Subnet、NAT Gateway、Route 53、Load balance     |
| Application Server | 網站應用程式 | ECS、EC2、Service、Launch Template、Auto Scaling Group |
| Database | 數據資料 | 產品目錄、客戶訂單和用戶資訊 |

就資料 (Data) 與非資料 (Network & Application) 層面拆開來看分析

#### Network & Application Server
- 盤點在 Network 與 Application 使用服務資源，支援自動化腳本程度、支援跨區服務
- 各服務恢復所需要的時間
- IP 費用，開啟白名單時需要同時建立 DR Site IP

#### Database
災難復原是恢復數據的能力，同步與備援機制如下
- 即時同步 : 透過 MS SQL Always on 機制，將數據即時同步到其他資料庫中。
- 備份 : 每天備份資料壓縮做備援 Backup，運維同仁會將備援檔案備份存放另一個 IDC

資料庫數據和日誌的容量整理如下，包括完整備份、數據傳輸時間估計、還原預估等 

另外還需考慮
- 資料庫每天成長大小，ex : +1G/Day
- IDC 到雲端服務頻寬大小
- IDC upload Database full backup 時間
- 建置雲端機器設定與同步地端DB時間

透過現有狀況盤點後，可以得到系統恢復的真實時間，例如，上述盤點完光資料庫部分還原需要 6 小時，而產品服務的期待時間是 2 小時，因此我們要思考如何數據步驟是否有其他更快的方式進行還原，ex : 數據庫進行拆分。

> Q2 : 如何進行 BIA (業務影響分析) 

A : 進行業務影響分析(Business Impact Analysis)，確定哪些業務功能對您的業務至關重要，以及它們的優先順序。這有助於確定災難發生時應首先恢復的部分。

| 順序 | 業務目標 | 功能 | 服務 | 
| -------- | -------- | -------- | -------- |
| 1     | 登入     | 功能A、功能B     | 主站、登入 API     |
| 2     | 目錄     | 功能C、功能D     | 目錄 API     |
| 3     | 訂單     | 功能E、功能F     | API     |
| 4     | 其他     |    |    |

除了找到商業上重要的功能外，還要了解對應的服務資訊，以計算每個服務和數據恢復的關鍵時間。

>Q3 : 如何計算 Application 恢復時間

A : 盤點每個功能，在按下部署按鈕後到完成所需的步驟和時間。應用程式上線的總時間可以表示為：

AP上線總時間 = Script + 機器請求 + 移交機器 + Provisioned + Health Check

* Script：是執行Terraform 所需要的預估時間(無法控制的時間)
* 機器請求：從Auto Scaling Group向AWS請求EC2 instance所需要花費的時間(無法控制的時間)
* 移交機器：從AWS建立EC2 instance到執行Provisioned所需要的時間(無法控制的時間)
* Provisioned：執行Provisioned Script的所需時間
* Health Check：從Provisioned執行後到服務正式上線的心跳檢查

計算整個應用程式恢復所需的時間，包括腳本執行、機器請求、機器建立、Provisioned 腳本執行和健康檢查等步驟。這有助於確定可以控制的和無法控制的恢復時間因素。 

> Q4 : RTO 與 RPO 時間如何定義 ? 

A : 可以參考前一篇文章對於其定義與介紹 [Day5 DR](https://ithelp.ithome.com.tw/articles/10323040)

## 小結
- 災難復原計畫 (DRP) 是一項策略，在確保在災難性事件發生時，業務可以快速、可靠地恢復正常運作。
- DRP 包括業務影響分析BIA、RTO與RPO目標設定、恢復策略制定、測試與演練等，以確保業務在災難時能夠順利復原。
## 參考連結
[Disaster Recovery Plan (DRP)](https://cio-wiki.org/wiki/Disaster_Recovery_Plan_(DRP))

[Business Continuity Plan (BCP) vs. Disaster Recovery Plan (DRP): What Are the Key Differences?](https://www.zerto.com/blog/disaster-recovery/bcp-vs-dr-plans-what-are-the-key-differences/)
