# Day13 - 可觀測性信號(Signals)的進化之旅

## Observability Signals
可觀測性目的是希望能更了解你的系統，為了更了解系統運行中的狀態跟行為，會從不同的角度、不同的時間和管道蒐集不同的遙測(Telemetry)數據資料，透過這些資料類型我們可以推斷出系統的情況，將這些稱之為信號，在可觀測性中常見的三個信號包括 Metrics、Logging 與 Tracing。

在各大研討會與文章都可以看到上述為可觀測性三支柱 (尤其是工具商 XD)，但如果要定義得更明確，實際執行上更為清楚個人應該是 **Metrics**、**Structured Log** 與 **Distributed Tracing**，以下針對三者做簡單說明

### Metrics : 指標 
* 用來衡量和監控系統性能的關鍵數據指標，通常是數字化的方式呈現(可聚合數字)。
* 這些數據通常用於即時監控系統，以確保它們在正常運行範圍內。
* 例如 : CPU使用率、記憶體使用率、請求速率、錯誤率等都可以通過指標來衡量

### Structured Log : 結構化日誌

> 跟 Logging 有什麼分別 ?

日誌通常會包含有用的資訊，當程式出現異常時方便開發人員進行盤查查找問題，可能包含事件的描述，發生的時間，嚴重類型與其他像是用戶ID、IP 等各種訊息。

傳統的日製設計上是非結構化的，可能會是以行為單位為了方便人類閱讀，例如

```
2023-03-16T12:02:00 - info: Request 1234 started from user 5678. GET /my/endpoint
2023-03-16T12:02:00 - info: User 5678 authenticated - name foo
2023-03-16T12:02:01 - info: Processing request 1234
2023-03-16T12:02:02 - info: Request 1234 finished with status code 200. It took 2 seconds
```
結構化日誌是在原有的資訊中使用 key/value 加在其資訊中，方便機器進行解析的動作，將上面日誌換成結構化格式

```
timestamp=2023-03-16T12:02:00 level="info" message="Request started" requestId="1234" userId="5678" path="/my/endpoint" httpMethod="GET"
timestamp=2023-03-16T12:02:00 level="info" message="User authenticated" requestId="1234" userId="5678" userName="foo" userPlan="professional"
timestamp=2023-03-16T12:02:01 level="info" message="Processing request" requestId="1234" rateLimited="false"
timestamp=2023-03-16T12:02:02 level="info" message="Request finished" requestId="1234" durationMs="2000" statusCode="200"
```
結構化日誌是可觀測性除錯的基礎。這些日誌更容易被日誌系統取得，團隊也可以透過日誌系統任意的標準搜尋相關資訊。

* 日誌包含有關離散事件的結構化或可讀的詳細資訊。用於提供請求的細節與上下文。
* 日誌通常被用來瞭解系統中特定事件的發生情況，以及在事件發生時提供上下文。
* 例如，錯誤日誌、訪問日誌、應用程式日誌等都是常見的日誌類型。


### Distributed Tracing : 分散式追蹤
* 用於追蹤複雜分佈式系統中請求的過程，以便了解請求從一個元件到另一個元件的傳遞情況。
* 它有助於識別性能問題和瓶頸，並提供有關請求流程的詳細信息。
* 通常，追蹤由一個唯一的標識符（例如 traceID）來關聯相關事件。

![https://ithelp.ithome.com.tw/upload/images/20230928/20162577qK9dAf8DWq.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162577qK9dAf8DWq.png)

分散式追蹤的偵測有兩個主要目的：上下文傳播(context propagation)和跨度映射(span mapping)。上下文傳播是透過使用可與 HTTP 用戶端和伺服器整合的程式庫完成。在這一部分中，可以使用 OpenTelemetry API/SDK、OpenTracing 和 OpenCensus 等專案、工具和技術。



## 三者關聯性 
![https://ithelp.ithome.com.tw/upload/images/20230928/20162577ZzvxKhE85b.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162577ZzvxKhE85b.png)

如果你是對可觀測性略有研究的朋友，相信一定看過上面這張圖， Peter Bourgon 在 2017 年參加完分散式追蹤研討會後所繪製的圖，說明 Metrics, tracing 及 logging 訊號彼此的關聯性與重要性。並在可觀測性中發揮不同且互補的作用。

![https://ithelp.ithome.com.tw/upload/images/20230928/201625776DaEM1DiIu.png](https://ithelp.ithome.com.tw/upload/images/20230928/201625776DaEM1DiIu.png)
這句話聽起來好像有道理但又似乎有些模糊，這裡來舉個範例讓大家更容易理解，如上圖所示，
- 收到伺服器錯誤過多的警告提示，超過了我們所設定的 SLO 標準。
- Mertics : 從錯誤計數器來看，發現是由於請求量變高導致伺服器回傳 501 比例變高。
- Logs : 導航到 log 日誌，看來錯誤來自許多後面的內部微服務。
- Trace : 透過 traceID 相同的 requestID 可以導航到 Trace，明確地找到多個服務中哪個緩慢的原因。

透過三個信號的結合，團隊才能清楚的確切地知道哪個服務或流程導致了問題，並進行了更多挖掘可能的問題動作。

## 超越「三大支柱」: Continuous Profiling 
> These three pillars continue to be critically important. But it’s important not to be confined by the “three pillars” paradigm and to choose the right telemetry data for your needs. 
> from logz.io
> 
OS : 只有賽亞人才能超越超級賽亞人 XDDD

關於可觀察性的討論通常會提到「可觀察性三大支柱」，這兩年開始越來越多人開始討論 **連續性分析（Continuous Profiling）** 作為一種新的可觀測性信號
- 它是實時監控應用程式性能和效率的方法，與靜態性能分析不同，它是在應用程式運行時持續進行的。
- OpenTelemetry 已開始整合連續性分析，提供全面性能的可觀測性
- 透過連續性分析幫助優化性能、排查問題和提高系統穩定性

關於更多的 Continuous Profiling 資訊，可以參考 OpenObservability Talks 的介紹
[![Yes](https://img.youtube.com/vi/G02g63oI0IA/0.jpg)](https://www.youtube.com/watch?v=G02g63oI0IA)

以上是今天的分享，如果有任何疑問或想法，歡迎留言提出討論 !
*OS : 越來越長篇字數已爆，不知道有沒有休刊 OR 暫停連載的選項 XDDD*

## 小結
- 可觀測性三個重要的 Signals，分別是 Metrics（指標）、Structured Log（結構化日誌）和 Distributed Tracing（分散式追蹤）。
- 連續性分析（Continuous Profiling）是可觀測性新的趨勢，提供運行時間控應用性能的能力。

## 參考連結
[Correlating Signals Efficiently in Modern Observability](https://www.bwplotka.dev/2021/correlations-exemplars/)

[Observability Signals](https://github.com/cncf/tag-observability/blob/main/whitepaper.md#observability-signals)

[OpenTelemetry Roadmap and Latest Updates](https://horovits.medium.com/opentelemetry-roadmap-and-latest-updates-a389144f3812)

[5 Key Observability Trends for 2022](https://horovits.medium.com/5-key-observability-trends-for-2022-dd8346854c79)

[Continuous Profiling: A New Observability Signal](https://logz.io/blog/continuous-profiling-new-observability-signal-in-opentelemetry/)
