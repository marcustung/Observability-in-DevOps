# Day18 - OpenTelemetry : 核心元件大解密 (1/2)

![https://ithelp.ithome.com.tw/upload/images/20231003/20162577JseNAg6Qph.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162577JseNAg6Qph.png)

> OpenTelemetry 看起來很厲害，背後是如何做到的呢 ?

OpenTelemetry 是一個開源工具、API 與 SDK 集合。用於雲原生 (Cloud Native) 應用程式、為服務和分散式系統，透過它可以協助收集與匯出遙測資料(Trace、Metrics和Logs)。使用這些數據以及對分散式系統的可觀測性，開發人員可以排除故障、進行 Debug、測試和監控應用程式。提高應用程式的可靠性和可擴展性。

今天我們將介紹 OpenTelemetry 的各核心元件。簡稱 OTel，在下面內容中使用 OTel 作為替代 (~~自以為專業~~)。


## 核心元件
          +------------------------------------------------------+
          |                   OpenTelemetry (OTEL)               |
          +------------------------------------------------------+
          |                                                      |
          | +-------------------------+  +---------------------+ |
          | |       Specifications    |  |   Implementations   | |
          | +-------------------------+  +---------------------+ |
          | | - OTel Specification    |  | - OTel SDKs         | |
          | |                         |  |                     | |
          | | - OTel Protocol         |  | - OTel Collector    | |
          | |                         |  |                     | |
          | | - Open Agent Management |  | - OTel API          | |
          | |   Protocol              |  |                     | |
          | |                         |  |                     | |
          | | - OTel Semantic         |  |                     | |
          | |   Conventions           |  |                     | |
          | +-------------------------+  +---------------------+ |
          |                                                      |
          +------------------------------------------------------+

### OpenTelemetry Specification : `規格`
OpenTelemetry Specification 是 OTel 的基礎，提供所有 OTel 標準所衍生的 API、SDK 和資料模型。

從 2019 開始至今，發展時間表
- 2019.09 Tracing stable
- 2021.11 Metrics stable
- 2023.04 Log stable

對於可觀測性重要的信號(Signals)是穩定支援的，想了解信號可以參考 [Day17 : 可觀測性信號的進化之旅](https://ithelp.ithome.com.tw/articles/10329784)

### OpenTelemetry SDKs : `程式語言的 API 實作`
提供基於 OTel 規範的客戶端工具。對於每種程式語言提供其 SDK，幫助開發人員更快速的收集、處理和導出遙測(Telemetry)資料，在不同信號中都有其成熟度等級。
![https://ithelp.ithome.com.tw/upload/images/20231003/20162577ct2r3Vxk5t.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162577ct2r3Vxk5t.png)

追蹤數據可以使用兩種方式產生，分別是 **自動(automatic)** 或 **手動(manual)**

`自動 : Auto Instrumentation`
SDK 會自動將信號或屬性欄位注入到應用程式中，例如上下文傳播和語義約定，不需要太多額外的程式碼來收集，透過這方式開發人員可以簡單、輕鬆地對應用程式進行分散式追蹤。

`手動 : Manual Instrumentation`
指為應用程式加上特定的程式碼，透過 OTel API 向應用程式添加可觀測性代碼的過程，可以更有效的滿足客製欄位上的需求，例如 : 新增自定義的屬性和事件內容。

程式語言支援細節可參考官方網站的 [Status and Releases](https://opentelemetry.io/docs/instrumentation/)

---
> **應用程式收集遙測數據資料後，下一步是將其數據傳送到某個地方**
---

以上是 OTel 相關核心的元件關於 spec 與 SDKs 的介紹，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
* OpenTelemetry (OTel) 是一個開源工具、API 與 SDK 集合，用於雲原生應用程式和分散式系統，幫助收集和匯出遙測資料。
* 核心元件包括 Specification（規格）、SDKs（程式語言的 API 實作）、Protocol（傳輸協定）、Collector（資料處理方式）、和 Semantic Conventions（通用數據定義方法）。

## 參考連結
[OpenTelemetry: A full guide](https://gethelios.dev/opentelemetry-a-full-guide)

[awesome-opentelemetry](https://bit.kevinslin.com/p/opentelemetry-in-2023?subscribe_prompt=free)

[What Are Semantic Conventions in OTEL?](https://www.logicmonitor.com/blog/what-are-semantic-conventions-in-otel)

[A beginner’s guide to OpenTelemetry](https://faun.pub/opentelemetry-d71d369c83d7)

[OpenTelemetry in 2023
](https://bit.kevinslin.com/p/opentelemetry-in-2023?subscribe_prompt=free)
