# Day10 - 可觀測性 Observability

大家好，我是伐伐伐伐木工

今天要與大家分享 Observability，本篇內容的重點如下
- 淺談 Observability 可觀測性 
- 討論可觀測性想解決什麼問題

## 可觀測性是甚麼 ? 
終於要進入主題可觀測性，可觀測性 Observability 簡稱 **o11y**

可觀測性一詞是 1960 年由工程師 Rudolf E. Kálmán 創造，發展至今在不同的領域意味不同的描述。他在所發表的論文中介紹一個他稱為可觀測性的特徵用於描述數學控制系統，在控制理論中，可觀測性被定義如下
> Observability is defined as a measure of how well internal states of a system can be inferred from knowledge of its external outputs.

從對系統外部輸出的資訊推斷系統內部狀態的能力，這裡的資訊指的是收集到的遙測(Telemetry)資料。
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*N5A-_B2Mn4ZTbwfvvV3ulQ.png)

看過很多關於可觀測性的定義，自己最喜歡是 Lightstep 提供的定義
> Obsevability is defined as the ability to measure the internal state of a system only by its outputs

自己對於其理解是 measure 蒐集 log、metrics、trace 等遙測數據。internal state 是系統發生什麼問題，為甚麼發生及如何修復。outputs 是哪裡有問題、甚麼是慢的、需要做甚麼來提高性能。

要使系統具備可被觀測性，必須滿足以下
- 了解應用程式的內部運作原理
- 使用外部工具觀察來了解內部運作原理和系統狀態
- 了解內部運作狀態時，無須交付任何自訂程式來處理

## 為什麼可觀測性這麼重要 : 解決了哪些問題？
隨著科技技術的不斷創新，軟體架構和基礎設施的演進速度也在快速變化。以下（但不僅限於此）是企業過去在開發系統或應用程式時可能經歷的幾個階段：

- 第一階段：**單一應用程式架構**

在這個階段，應用程式的架構設計相對簡單，商業需求還不是很複雜，因此架構通常不會對模組進行太多的切割。應用程式運行在單一執行緒（Process）中處理單一請求。測試和問題排除相對簡單。部署通常在虛擬機器（VM）或機房（IDC）中提供給使用者使用。

- 第二階段：**雲端技術的崛起**

隨著雲端技術的出現，企業開始嘗試將應用程式部署到雲端基礎設施即服務（IaaS）上，或結合公有雲和私有雲的優勢。它們可能在私有雲中處理機密資料，並在公有雲服務上處理其他資訊。開發人員在應用程式開發和整合方面有更多的機會與雲端服務整合。這一階段也涉及選擇在雲端上使用適合的服務。

- 第三階段：**雲端技術的成熟**

隨著雲端技術的逐漸成熟和穩定，企業可以利用越來越多的雲端基礎設施服務和新服務。架構設計變得更加複雜，分散式系統架構成為許多討論的焦點。資料庫從機房遷移到雲端上，並使用像RDS、DynamoDB、Redis 等雲端資料庫服務，使雲端開發和管理更加便捷。架構也變得更加靈活，以滿足企業需求。同時，容器化技術（如Docker）的成熟也開始引入新的可能性。

- 第四階段：**微服務和容器化的興起**

軟體架構從單體式(Monolithic)演變為微服務(Microservices)、無伺服器（serverless）和服務網格（service meshes）等各種可能的組合。主要目標是減少服務之間的相依性，提高擴展能力與故障時的隔離與可用性。

帶來了好處也衍伸了一些有趣的議題，服務越切越細、分散式系統其也帶來複雜性，監控與測試變得更為複雜，如何在問題發生的第一時間定位異常的服務也變得更重要，在 Google 文章中 [DevOps measurement: Monitoring and observability](https://cloud.google.com/architecture/devops/devops-measurement-monitoring-and-observability) 提到要在監控和可觀測性方面做得出色，您的團隊應該具備以下特點：
- 報告系統整體健康狀況（我的系統是否運作正常？我的系統是否有足夠的可用資源？）。
- 報告顧客體驗到的系統狀態（我的客戶是否知道如果我的系統故障並且遭遇不佳體驗？）。
- 監控關鍵的業務和系統指標。
- 提供工具，幫助您在生產環境中了解和調試您的系統。
- 提供工具，用於查找您先前不知道的信息（即，您可以識別未知的未知）。
- 存取有助於追蹤、了解和診斷生產環境中基礎設施問題的工具和數據，包括服務之間的互動。

上述內容聽起來很抽象，白話點背後是希望可以回答以下問題
- 請求經過哪些服務 ? 系統中哪個部分 loading 最大
- 當服務發生緩慢時，哪裡慢(Bottlenecks)以及可能原因是什麼
- 當服務無法使用時，錯誤及可能異常的原因是什麼
- 當使用者反映操作timeout，但在 Dashboard 上顯示平均請求都很快，要如何找到其可能緩慢原因

目的是希望工程團隊可以理解並解釋系統的現況(增加系統透明度，而不是靠通靈)，當系統出現問題時，可以第一時間了解爆炸範圍，進行緊急問題的處理已加速恢復的時間。且無須發布新的程式碼，提高系統的穩定性和可用性，也是現代化應用程式非常重要的特性之一。

有想了解更多關於可觀測性的內容可以參考 CNCF 的白皮書，Git 上面的是完整版，Google 文件的是眾多大神在討論時的版本，可以更了解細節，對其有興趣的朋友可以參考看看。

可觀測性白皮書 : 
* [Github 版本](https://docs.google.com/document/d/1eoxBe-tkQclixeNmKXcyCMmaF5w1Kh1rBDdLs0-cFsA/edit)
* [草稿版本](https://github.com/cncf/tag-observability/blob/main/whitepaper.md#what-is-observability)


以上是今天的分享。下一篇要來探討可觀測性與監控的差異，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
- 可觀測性是一種能力，允許工程團隊深入了解系統運作，並快速識別問題，不需事先定義具體的屬性或模式。
- 可觀測性成為確保系統可用性和穩定性的關鍵，透過監控、報告、和工具，團隊能夠有效解決問題，提高可用性與可靠性。

## 參考連結
[DevOps measurement: Monitoring and observability](https://cloud.google.com/architecture/devops/devops-measurement-monitoring-and-observability)

[What is observability and why is it important?](https://www.ibm.com/resources/automate/observability-basics)

[What is DevOps Observability (Importance & Best Practices)](https://www.browserstack.com/guide/observability-devops)
