# Day17 - OpenTelemetry : 在收集遙測數據中加上一點新標準

![https://ithelp.ithome.com.tw/upload/images/20231002/20162577fYvzVpZtFn.jpg](https://ithelp.ithome.com.tw/upload/images/20231002/20162577fYvzVpZtFn.jpg)

> 為什麼我們需要分散式追蹤 ? 日誌 (Log) 跟指標 (Metrics) 不夠用嗎

## 為什麼需要分散式追蹤
隨者科技的進步，測量系統指標和發送警報也變得更加複雜；許多組織每天處理數百甚至數千萬個請求，以 Uber 為例內部系統有 [4,000 個微服務](https://www.cncf.io/case-studies/uber/?uclick_id=97814446-5c17-4d09-be33-518c69ee17fd)彼此關聯。

如果你的架構圖像是下面圖示，一個請求可能會經過數十或是數百個網路節點。架構切分這麼細的情況下，這使得了解請求所經過的路徑變得相當困難，如果你只有日誌跟指標，要進行故障排除也是相當複雜的。在問題發生時，對開發或是運維團隊來說，需要了解的是怎麼找到**根本問題**(Root Cause)及**經過哪些服務**。

![https://ithelp.ithome.com.tw/upload/images/20231002/20162577hgDtNTJ3Mm.png](https://ithelp.ithome.com.tw/upload/images/20231002/20162577hgDtNTJ3Mm.png)

> 分散式追蹤可協助您查看整個請求 (Request) 期間服務之間的交互，並讓您深入了解系統中請求的整個生命週期。它幫助我們發現應用程式中的錯誤、瓶頸和效能問題。

意味著當使用者透過瀏覽器發送請求到伺服器應用程式的那一刻起，我們可以可看到整個請求的過程。

(追蹤)資料(以跨度的形式)產生遙測資料，可以幫助了解延遲或錯誤是什麼，以及為什麼會發生還有對整個請求的影響。

![https://ithelp.ithome.com.tw/upload/images/20231002/20162577YFYbD4Nepg.png](https://ithelp.ithome.com.tw/upload/images/20231002/20162577YFYbD4Nepg.png)


## 什麼是 OpenTelemetry
近期很多在看很多 Open Source 專案都用相似方式來在介紹，為了跟上這股潮流我們也不能輸，以下參考官網說明

**OpenTelemetry 是什麼**
> OpenTelemetry 是一個雲端原生運算基金會(CNCF)專案。
> OpenTelemetry 是一個可觀察性框架和工具包，旨在創建和管理遙測數據，例如追蹤、指標和日誌。
> OpenTelemetry 與供應商和工具無關，這意味著它可以與各種可觀察性後端一起使用，包括 Jaeger和 Prometheus 等開源工具以及商業產品。

**OpenTelemetry 不是什麼**
> OpenTelemetry 不是像 Jaeger、Prometheus 或商業供應商那樣的可觀察性後端。
> OpenTelemetry 專注於遙測資料的產生、收集、管理和匯出。該資料的儲存和視覺化有意留給其他工具。



## 使命、願景和工程價值
了解其設計初衷跟方向，才會知道為什麼這樣設計與其它想要試圖解決的問題

#### 使命(Mission) : 無所不在的高品質、可攜式遙測

#### 願景：有效的可觀測性世界
OpenTelemetry 的願景是實現一個有效的可觀測性世界。這個願景的核心思想是，有效的可觀測性需要高品質的遙測技術，以及使其成為可能的高性能、一致的儀器。在這樣的世界中，遙測應該是[容易的](https://github.com/open-telemetry/community/blob/main/mission-vision-values.md#telemetry-should-be-easy)、[通用的](https://github.com/open-telemetry/community/blob/main/mission-vision-values.md#telemetry-should-be-universal)、[與供應商無關的](https://github.com/open-telemetry/community/blob/main/mission-vision-values.md#telemetry-should-be-vendor-neutral)、[鬆散耦合的](https://github.com/open-telemetry/community/blob/main/mission-vision-values.md#telemetry-should-be-loosely-coupled)、[內建的](https://github.com/open-telemetry/community/blob/main/mission-vision-values.md#telemetry-should-be-built-in)。

#### 工程價值：相容性、穩定性、 彈性和效能
OpenTelemetry 重視以下四個關鍵價值：

- **相容性**
OpenTelemetry致力於相容性，這意味著遵循規範並實現互通性非常重要。我們希望OpenTelemetry能夠跨語言和組件一致，並保持供應商中立。
- **穩定性**
API穩定性和向後相容性對於OpenTelemetry的程式庫來說至關重要。我們不會引入新概念，除非確信廣泛子集需要它們。
- **彈性**
OpenTelemetry強調技術彈性，即使在資源稀缺或其他環境挑戰下，也能適應並繼續運作。我們設計OpenTelemetry能夠優雅地應對應用程式行為異常，並持續收集遙測訊號。
- **效能**
高效能是OpenTelemetry的要求，我們不希望用戶在高品質遙測和高效能應用程式之間做出選擇。OpenTelemetry確保高效能，並且不接受主機應用程式中的意外干擾。

## 分散式技術追蹤的演進
![https://ithelp.ithome.com.tw/upload/images/20231002/20162577sCJsTOUIVD.png](https://ithelp.ithome.com.tw/upload/images/20231002/20162577sCJsTOUIVD.png)
在過去為了嘗試解決分散式追蹤 (Distributed Tracing) 問題，有多種不同的 Open Source 專案，以下是簡單的整理 ([圖片來源](https://speakerdeck.com/ido_kara_deru/construction-and-operation-of-observability-platform-using-istio))
- 2010 年 : Google 發表一篇名為 Dapper 的論文，描述分散式追蹤系統的運作。
- 2016 年 : CNCF 發表 OpenTracing 專案目的是為了分散式追蹤提供標準化。
- 2017 年 : Google 再次創新，推出 OpenCensus 作為分散式追蹤和監控的 library
- 2019 年 :
    -  OpenCensus 及 OpenTracing 進行整合，OpenTelemetry 成為新一代的遙測數據蒐集標準。
    -  OpenTelemetry 實作 W3C Distributed Tracing 所定義的規範與標準

## 核心元件
OpenTelemetry (aka OTel) 在可觀測性的領域中扮演越來越重要的角色，近幾年已成為 CNCF 中第二個活耀的 Open source 專案，受歡迎程度僅次於 Kubernetes，它的貢獻者遍布所有主要的可觀測性供應商，其協議在可觀測性供應商中幾乎被普遍採用。有興趣可以參考 [CNCF Dev Status](https://all.devstats.cncf.io/d/1/activity-repository-groups?orgId=1&var-period=h24&var-repogroups=All)。

OpenTelemetry 核心分為兩個部分，分別是**規範**與**實作**

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

由於資料內容過多~~體力不支~~，下一章節再繼續介紹核心的元件 XDDD

## 小結
`小結呼應主題搭配梗圖`
![https://ithelp.ithome.com.tw/upload/images/20231002/20162577f9xqUEfRhW.png](https://ithelp.ithome.com.tw/upload/images/20231002/20162577f9xqUEfRhW.png)

## 參考連結
[What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)

[Observability Whitepaper](https://github.com/cncf/tag-observability/blob/main/whitepaper.md#traces)

[OpenTelemetry mission, vision, and values](https://github.com/open-telemetry/community/blob/main/mission-vision-values.md#telemetry-should-be-built-in)

[OpenTelemetry Up and Running](https://medium.com/@magstherdev/opentelemetry-up-and-running-b4c58eaf8c05)

[OpenTelemetry in 2023](https://bit.kevinslin.com/p/opentelemetry-in-2023?utm_source=profile&utm_medium=reader2)
