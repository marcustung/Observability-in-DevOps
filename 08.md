# Day08 - 監控與指標分析 Monitor

大家好，我是伐伐伐伐木工

今天要與大家分享監控 Monitor，本篇內容的重點如下
- 監控的基本概念
- 主要的監控指標
- 監控框架：USE、RED、Golden Signals

## 什麼是監控
監控可以幫助團隊觀察系統的效能並偵測已知的故障，有效的監控包含三個步驟
- 預先定義 "指標"
- 部署程式來收集指標
- 在儀表板中顯示這些指標

然而，監測也有其局限性。為了進行監控，您必須知道要追蹤哪些指標和日誌。如果您的團隊沒有預測到問題，則可能會錯過關鍵的生產故障和其他問題。

### 監控類型
在現代分散式系統中，常見監控的元件有基礎建設、應用程式、資料庫、網路、資料流等，而我們想要的指標會因為監控類型而不同，例如
- 基礎設施 : 正常運作時、CPU使用率、記憶體使用率
- 應用程式效能監控 : 吞吐量、錯誤率、延遲
- 資料庫監控 : 連線數、查詢效能
- 網路監控 : 往返時間、TCP、連線遺失


### 監控指標(Framework)
![](https://hackmd.io/_uploads/r1TAFPj1a.png)

#### USE
由  Brendan Gregg 提出[文章](https://www.brendangregg.com/usemethod.html)，USE 方法是基於三種度量類型和處理複雜系統的策略，其縮寫代表意義如下
- 使用率 (Utilization) : 資源繁忙的時間百分比，例如「一個磁碟以 90% 的利用率運作」
- 飽和度 (Saturation) : 與工作資源量相關，例如「CPU 的平均運行佇列長度為 4」
- 錯誤 (Errors): 錯誤事件計數，例如「此網路介面發生了 50 次遲到衝突」

核心概念
> 對於每個資源，檢查使用率、飽和度和錯誤。

#### RED
2015年，Grafana 的 Tom Wilkie 談到了監控微服務的RED方法。Tom 建議不要監視每個資源的使用率、飽和度和錯誤，而是對於每個資源，監控
- 速率 (Rate) : 您的服務每秒處理的請求數
- 錯誤 (Errors): 每秒失敗的請求數
- 持續時間 (Duration): 每個請求所花費的時間量的分佈

核心概念
> 透過使用 RED 方法，公司將更了解客戶的滿意度，並將幫助您建立有意義的警報並衡量 SLA

#### Golden 
來自 Google SRE Book : [The Four Golden Signals](https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals)
> 監控的四個黃金訊號是延遲、流量、錯誤和飽和度。如果您只能衡量使用者導向的系統的四個指標，請專注於這四個指標。
- 延遲 (Latency): 處理請求所需的時間
- 流量 (Traffic): 對您的系統有多少需求
- 錯誤 (Errors): 失敗請求的比率
- 飽和度 (Saturation): 您的服務有多“完整”

以上是今天的分享，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
- 監控是一個策略和工具的組合，用於實時追蹤和評估資訊系統的性能和健康狀態
- 監控監控框架：USE、RED、Golden Signals

## 參考連結
[4 SRE Golden Signals (What they are and why they matter)](https://www.blameless.com/blog/4-sre-golden-signals-what-they-are-and-why-they-matter)

[USE vs RED vs The Four Golden Signals](https://faun.pub/use-vs-red-vs-the-four-golden-signals-50655e93fad7)



