# Day19 - OpenTelemetry : 核心元件大解密 (2/2)

![https://ithelp.ithome.com.tw/upload/images/20231004/20162577Ig2o0FkDPZ.jpg](https://ithelp.ithome.com.tw/upload/images/20231004/20162577Ig2o0FkDPZ.jpg)
上篇介紹負責蒐集應用程式遙測數據的元件，這篇將介紹後續處理的原件 OTel Collector

## 核心元件 (2/2)
          +------------------------------------------------------+
          |                   OpenTelemetry (OTEL)                |
          +------------------------------------------------------+
          |                                                      |
          | +-------------------------+  +---------------------+ |
          | |       Specifications    |  |   Implementations  | |
          | +-------------------------+  +---------------------+ |
          | | - OTel Specification    |  | - OTel SDKs        | |
          | |                         |  |                     | |
          | | - OTel Protocol         |  | - OTel Collector   | |
          | |                         |  |                     | |
          | | - Open Agent Management |  | - OTel API          | |
          | |   Protocol              |  |                     | |
          | |                         |  |                     | |
          | | - OTel Semantic         |  |                     | |
          | |   Conventions           |  |                     | |
          | +-------------------------+  +---------------------+ |
          |                                                      |
          +------------------------------------------------------+

### OpenTelemetry Protocol (OTLP): `傳輸可觀測性數據協定`
> 開放遙測協定 (OTLP) 規格描述了遙測源、採集器等中間節點和遙測後端之間遙測資料的編碼、傳輸和交付機制 
> [name=open-telemetry/opentelemetry-proto]

OTLP 協定描述如何編碼和傳輸遙測數據，白話意思是可以在接收、處理或導出 OTEL 資料的任何服務上實現。每種語言 SDK 都提供一個 OTLP 匯出器，您可以將其配置為透過 OTLP 匯出資料。然後 OpenTelemetry SDK 將事件轉換為 OTLP 資料。

### OpenTelemetry Collector: `接收、處理和匯出遙測資料的方式`
![https://ithelp.ithome.com.tw/upload/images/20231004/201625779SlV5luVdS.png](https://ithelp.ithome.com.tw/upload/images/20231004/201625779SlV5luVdS.png)

OTel Collector 是 OTel 重要核心元件之一，用於收集、轉換處理並發送遙測資數據，與供應商無關(vendor-agnostic)。也可依據團隊收集數據資料的需求進行採樣(sampling)設定。

#### 組成
OTel Collector 由以下組件組成
- Receivers : 接收器接受指定格式的數據，將其轉換為內部格式，並將其傳遞給適用管道中定義的處理器和匯出器
- Processors : 轉換/過濾/加工/發送數據
- Exporters : 將資料傳送到下游(一個或多個)目的地
- Connectors : 依據其需求，可以將多個管道彙總一起
- Extensions : 提供處理遙測數據之外的附加功能，例如基本驗證、執行狀況檢查等

各組件搭配一起成為可觀測性管道([Observability Pipleline](https://ithelp.ithome.com.tw/articles/10330480))，可以讓使用者從不同來源 (例如OpenTelemetry SDK、代理或導出器) 收集遙測數據資料，並在收集資料過程中進行轉化加工處理，再將處理後的資料傳送到任何目的地 (destinations)。

OpenTelemetry 不負責儲存後端或呈現結果。如果需要儲存或可視化，您可以參考 Awesome OpenTelemetry 中的存儲解決方案。

#### 不負責後端及呈現結果
另外提醒，OpenTelemetry 不提供儲存後端及呈現結果，
如果需要儲存或可視化，您可以參考 [Awesome OpenTelemetry 中的存儲解決方案](https://github.com/magsther/awesome-opentelemetry#storage)。

#### 彈性與生態系
OpenTelemetry 提供了一種跨各種程式語言、平台和雲端供應商檢測、收集和匯出遙測資料的標準方法。這種標準化可確保在不同環境和系統中以一致的方式收集和報告遙測數據，減少整合上的成本並使其更易於使用，使用上也具備彈性可因不同需求來搭配不同的Receivers、Processors 及 Exporters。
* [支援的接收器列表](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver)
* [支援的處理器列表](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor)
* [支持的出口商名單](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter)

![https://ithelp.ithome.com.tw/upload/images/20231004/201625770aQ5ZEC9ZV.png](https://ithelp.ithome.com.tw/upload/images/20231004/201625770aQ5ZEC9ZV.png)

另外可以參考 [OpenTelemetry 生態系統](https://opentelemetry.io/ecosystem/registry/)，了解更多支援 OTel 的工具、追蹤器實作、實用程式和其他有用的項目。

### OpenTelemetry Semantic Conventions : `通用數據定義方法`
提供了一種定義如何在系統中的不同元件之間建構和交換資料的方法。透過使用語意約定，各種工具和服務可以一致地解釋和處理資料。*白話就是定義了可觀測性資料的一組通用屬性。它們涵蓋了廣泛的領域，包括雲端資源、資料庫、異常和系統。*

以上是 OTel 相關核心的元件介紹與分享，如果有任何疑問或想法，歡迎留言提出討論 !

## 參考連結
[OpenTelemetry: A full guide](https://gethelios.dev/opentelemetry-a-full-guide)

[awesome-opentelemetry](https://bit.kevinslin.com/p/opentelemetry-in-2023?subscribe_prompt=free)

[What Are Semantic Conventions in OTEL?](https://www.logicmonitor.com/blog/what-are-semantic-conventions-in-otel)

[A beginner’s guide to OpenTelemetry](https://faun.pub/opentelemetry-d71d369c83d7)

[OpenTelemetry in 2023
](https://bit.kevinslin.com/p/opentelemetry-in-2023?subscribe_prompt=free)
