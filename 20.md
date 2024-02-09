# Day20 - OpenTelemetry : Demo 專案快速入門  (1/2)

![https://ithelp.ithome.com.tw/upload/images/20231005/201625778AjEZI7twL.jpg](https://ithelp.ithome.com.tw/upload/images/20231005/201625778AjEZI7twL.jpg)

前面兩天介紹關於 OpenTelemetry 重要的功能，身為一位專業的村民還是需要看到程式碼會比較有安全感，今天我們就透過 Demo 專案來體驗 OpenTelemetry 帶來的幫助
 
## 不明覺厲的 OpenTelemetry Demo 專案
在 OpenTelemetry 官網有提供 [**OpenTelemetry Astronomy Shop**](https://github.com/open-telemetry/opentelemetry-demo) 範例專案，它是一個基於微服務的分散式系統，模擬了線上商店的操作，以下是關於這專案的重點介紹

- 提供用於 Demo OpenTelemetry 儀器和可觀測性的分散式系統的實際案例與架構介紹。
- 包含 15 種不同的服務，使用 10 多種不同的程式語言，以模擬網路商店操作行為。
- 為供應商、工具作者和開發者建立一個基礎，以擴展他們的 OpenTelemetry 整合。
- 它使用負載產生器（Locust）不斷向前端發送模仿真實使用者購物流程的請求
- 該專案使用 Jaeger 與 Grafana 等工具來診斷異常問題

#### 快速開始
> 在開始之前的前置作業，需要先在電腦上安裝 [Docker](https://docs.docker.com/engine/install/)。

Clone Demo repo
`git clone https://github.com/open-elemetry/opentelemetry-demo.git`

移駕到該 repo 目錄
`cd opentelemetry-demo/`

執行 Docker Compose 指令
`docker compose up --no-build`

開啟瀏覽器輸入網址 http://localhost:8080/ ，看到下列圖片就代表網站啟動成功

![https://ithelp.ithome.com.tw/upload/images/20231005/20162577t7KSdBnBP7.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577t7KSdBnBP7.png)

#### 工具
* 網上商店：http://localhost:8080/
* Grafana：http://localhost:8080/grafana/
* 功能標誌 UI：http://localhost:8080/feature/
* 負載產生器 UI：http://localhost:8080/loadgen/
* Jaeger 使用者介面：http://localhost:8080/jaeger/ui/

### 應用程式架構
![https://ithelp.ithome.com.tw/upload/images/20231005/20162577KYdvxfN310.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577KYdvxfN310.png)

重點摘要
* 15 個以上服務
* 10 種以上程式語言開發
* http + grpc
* 使用負載產生器（Locust）模仿真實使用者購物流程的請求

### 分散式重要觀念

OpenTelemetry 中基本的對像是**事件（event）**。事件只是⼀個時間戳和⼀組屬性。⼤多數屬性對單個事件來說並不獨特。相反，它們是⼀組事件所共有的。例如，http.target 屬性與作為 HTTP 請求的每個事件相關。如果在每個事件上反覆記錄這些屬性，效率會很低。相反，我們把這些屬性拉出到圍繞事件的封裝中，在那裡它們可以被寫入⼀次。我們把這些封裝稱為`上下文（context）`。

有兩種類型的上下文**靜態**和**動態**，如下圖所示
![https://ithelp.ithome.com.tw/upload/images/20231005/20162577wDux6UDqSK.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577wDux6UDqSK.png)
* **靜態上下文** : 定義了事件發⽣的物理位置。在 OpenTelemetry 中，這些靜態屬性被稱為資源。⼀旦程序啟動，這些資源屬性的值通常不會改變。
* **動態上下文** : 定義了事件參與的活動操作。這個操作層⾯的上下文被稱為**跨度（span）**。每次操作執⾏時，這些屬性的值都會改變。

![https://ithelp.ithome.com.tw/upload/images/20231005/20162577sD6HOmJ7rD.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577sD6HOmJ7rD.png)

`span` 是我們描述因果關係的⽅式。TraceID、SpanID 和 ParentID，這三個屬性是 OpenTelemetry 的基礎。

| 屬性 | 類型 | 描述 | 範例 | 
| -------- | -------- | -------- | -------- |
| traceid     | 16字節數組     | 識別整個交易  | bf92f3577b34da6a3ce929d0e0e4736    |
| spanid | 8 字節數組 | 識別當前操作 | 00f067aa0ba902b7 | 
| parentid | 8 字節數組 | 識別⽗操作 | 3ce929d0e0e4736 | 

通過添加這些屬性，我們所有的事件現在可以被組織成⼀個圖。這種類型的圖被稱為追踪（trace）。透過 TraceID 索引，可以找到該請求中所有的 Log 資訊。

### 模擬異常 : 讓他爆 
![https://ithelp.ithome.com.tw/upload/images/20231005/201625776kU2pkru9g.png](https://ithelp.ithome.com.tw/upload/images/20231005/201625776kU2pkru9g.png)

在這範例專案中，有提供模擬故障的功能可以到 [功能標誌設定頁](http://localhost:8080/feature/) 啟用需要失敗的功能設定，將所有的設定改為 `true` (讓他爆)

範例專案有負載產生器（Locust）不斷發送模仿真實使用者購物流程的請求，加上 1/10 的錯誤發生機率，我們等待 5~10 分鐘再來觀看可能有哪些錯誤。


### Trace : 使用 Jaeger 定位問題 
[Jaeger](https://github.com/jaegertracing/jaeger) 是 CNCF 畢業的專案，用於分散式系統監控和故障排除的開源工具，透過視覺化的工具追蹤性能、定位故障點、優化性能，提升系統穩定性。

開啟 [Jaeger UI](http://localhost:8080/jaeger/ui/) 畫面，在 Tag 輸入 error = trur，按下 Find trace 可以看到部分請求有 error 的狀況發生  

![https://ithelp.ithome.com.tw/upload/images/20231005/201625772bfMXSbso5.png](https://ithelp.ithome.com.tw/upload/images/20231005/201625772bfMXSbso5.png)

點擊軌跡，您可以查看該請求經過的服務列表、執行順序以及每個不同服務所花費的時間。這可以幫助您定位問題。

以下是正常的請求，當按下購物車後的請求，可以看到該發送請求(Request) 後與 9 個服務 (span) 關聯與執行順序，其每個 span 的 Service Name 與時間長短。

![https://ithelp.ithome.com.tw/upload/images/20231005/20162577E57cHHpuxq.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577E57cHHpuxq.png)

錯誤請求時，點擊進去則可以看到哪一段發生異常，這範例是我們先使用設定頁讓 adservice 功能一定機率異常，遇到時發生的錯誤，在點擊異常紅色的 span 則可以看到像是 error message 等 debug 需要的資訊

![https://ithelp.ithome.com.tw/upload/images/20231005/20162577kwwSqOfUia.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577kwwSqOfUia.png)


### Metrics : 使用 Grafana 查看 OTel Collector 狀況

範例專案可以開啟 Grafana 查看內建的 Dashboard 儀表板資訊，了解遙測數據的收集狀況，目前有四個部分
* Process Metrics
* Traces Pipeline
* Metrics Pipeline
* Prometheus Scraping

**OpenTelemetry Collector Dashboard**
![https://ithelp.ithome.com.tw/upload/images/20231005/20162577tgoPnvaiNE.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577tgoPnvaiNE.png)

透過此 Dashboard 可以了解可觀測性管道 Receivers、Processors、Exporters 收集/發送數據的狀況 (資料流)以及每個管道的導出比例。

**Opentelemetry Collector Data Flow**
觀看可觀測性信號遙測數據的資訊，看到來源有 http 與 grpc，
Trace 
![https://ithelp.ithome.com.tw/upload/images/20231005/20162577N5DbwlhnSd.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577N5DbwlhnSd.png)

Prometheus Scrape
![https://ithelp.ithome.com.tw/upload/images/20231005/20162577KLGMPsOzs5.png](https://ithelp.ithome.com.tw/upload/images/20231005/20162577KLGMPsOzs5.png)

> 反思 : 沒有這些工具，要如何快速定位問題呢

以上是針對 OpenTelemetry 專案簡單的介紹，對開發人員帶來的幫助是使用 Jaeger 和 Grafana 進行故障排除和監控，有效的縮短異常事件處理的時間。下一篇我們再繼續介紹是如何做到的。

## 參考連結
[OpenTelemetry in Action](https://medium.com/@magstherdev/opentelemetry-in-action-fc61263c852)

[Development](https://opentelemetry.io/docs/demo/development/)

[opentelemetry-demo](https://github.com/open-telemetry/opentelemetry-demo/)
