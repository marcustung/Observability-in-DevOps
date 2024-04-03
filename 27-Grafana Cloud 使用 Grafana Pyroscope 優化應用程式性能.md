# Day27 - Grafana Cloud : 使用 Grafana Pyroscope 優化應用程式性能

![https://ithelp.ithome.com.tw/upload/images/20231012/201625772TdbZYXqUr.jpg](https://ithelp.ithome.com.tw/upload/images/20231012/201625772TdbZYXqUr.jpg)

## 什麼是 Grafana Pyroscope 

在 Day13 - 可觀測性信號 Observability Signals 中提到常見的三個信號分別是 Metrics、Log 與 Trace，應用程式的性能分析 Profiling 也是在追查問題的 root cause 重點項目之一，Grafana Cloud 提供解決方案名為 Grafana Pyrscope。是由 Plare 與 Pyrscope 兩個開源專案合併而成，目的是為了提供開源社群大規模的分析線上應用程式資訊，以了解程式碼在上線之後資源的使用狀況，幫助開發人員在問題定位或是效能優化。

## 如何進行分析
![https://ithelp.ithome.com.tw/upload/images/20231012/201625778LWWiAIDMB.png](https://ithelp.ithome.com.tw/upload/images/20231012/201625778LWWiAIDMB.png)
在官網有提供分析步驟，說明如下
- 應用程式 : 負責收集數據動作，在 Grafana 中可以透過 Grafana Agent 或是應用程式加上 language 的 sdks 蒐集 profiling 資訊
- Pyroscope : 負責資料儲存與查詢
- Grafana : 負責資料的可視化與查詢，可透過 Dashboard 火焰圖、直方圖提供不同維度的視覺分析數據。
- 優化 : 透過上述資訊，可以識別應用程式中更慢或是消耗記憶體的部分，以提供更快的應用程式或找到 OOM 問題。

## 過去在 Windows 環境是怎麼做
> 如何在正式環境找到應用程式的性能瓶頸 ?

這一直是個複雜的任務，需要了解不同的應用程式組件像是前端、後端、資料庫或外取各自的瓶頸，才有機會制定其優化策略達到提高性能目的，在  .NET 應用程式優化部分以自己維運經驗可以透過 [Debug Diagnostic](https://www.microsoft.com/en-us/download/details.aspx?id=58210) 或是 [WinDbg](https://learn.microsoft.com/zh-tw/windows-hardware/drivers/debugger/) 來追查應用程式性能問題
![https://ithelp.ithome.com.tw/upload/images/20231012/20162577ljqJ6WYixn.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577ljqJ6WYixn.png)
Debug Diagnostic

![https://ithelp.ithome.com.tw/upload/images/20231012/20162577BQ5YW0MJmN.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577BQ5YW0MJmN.png)
WinDbg

以 Debug Diagnostic 為例，所需要步驟大概如下
- 收集應用程式資訊 : 將執行中的應用程式 Memory Dump 下來，可以選擇在執行當下或是特定規則，例如 CPU 超過 60% 時進行 Dump 動作，Dump 資訊會是一個 dump file
- 工具分析 : 開啟工具選擇下載的 dump file，按下分析按鈕，工具會針對 Dump 檔案內資訊進行分析 
- 產生報表 : 分析結果透過報表方式呈現，例如網站 hand 住 CPU 100%、OOM (out of memort)、Memory leak，方便開發者可以了解網站當下使用 Memory/Thread/Callstack 等線上程式執行狀況資訊。以及彙整這些資訊後分析可能的問題與建議
![https://ithelp.ithome.com.tw/upload/images/20231012/20162577LP4ClBptdh.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577LP4ClBptdh.png)

但這些在問題處理上可能都是屬於落後指標，意味者在處理上可能會是問題發生後才有機會發現及後續處理，舉例來說發現 CPU 使用量飆高，團隊來建立規則達到特定 % 後，下一次發生時觸發其規則時自動 Dump 檔案，團隊取得檔案進行後續分析當下程式執行狀況。

透過即時與不斷的蒐集線上應用程式資訊，可以更有效的定位可能發生的問題並針對問題進行調整，也可縮短 MTTR(Mean time to recovery) 平均修復的時間。

### 蒐集資訊方式
![https://ithelp.ithome.com.tw/upload/images/20231012/20162577ZHbf9bhKg3.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577ZHbf9bhKg3.png)

在蒐集遙測數據方面，有兩種方式
- Pyrscope SDKs : 在應用程式中使用各自程式語言的 SDKs，收集即時的 Profiling data 到 Grafana Cloud 
- Grafana Agent : 架設 Grafana Agent 定期從中收集分析數據

在 Grafana Pyroscope 中有提供不同程式語言的範例，目前有以下幾種
![https://ithelp.ithome.com.tw/upload/images/20231012/20162577hkorDGs4r7.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577hkorDGs4r7.png)
補充 : Github supported lanagauges 有提到 PHP (phpspy)

自己對於 .NET 比較熟悉，接著使用該程式語言進行範例說明，如果是要將既有的 .NET 專案加入 Profiling 可以參考官方說明 : [連結](https://grafana.com/docs/pyroscope/v1.1.x/configure-client/language-sdks/dotnet/)

以下是直接使用範例程式進行步驟說明
 
* **Clone 範例專案**
> https://github.com/grafana/pyroscope.git
* **產生 Pyrscope config**
到 Grafana Stack 頁面，按下 Send Profiles 
![https://ithelp.ithome.com.tw/upload/images/20231012/20162577QksfW8cL1o.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577QksfW8cL1o.png)
根據 Token Name 設定有效日期，Generate API Token 
![https://ithelp.ithome.com.tw/upload/images/20231012/20162577J4y0PhfDh6.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577J4y0PhfDh6.png)

* **修改 API Token 設定**
將剛剛產好的 User Name 與 API Token 複製到應用程式設定中的 `PYROSCOPE_BASIC_AUTH_USER` 與 `PYROSCOPE_BASIC_AUTH_PASSWORD`
* **執行應用程式**
到 sample 專案底下 Fast-slow 目錄，執行下列語法
> docker-compose up

![https://ithelp.ithome.com.tw/upload/images/20231012/20162577Nr1lLEe5bh.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577Nr1lLEe5bh.png)

透過以上方式即可在 Grafaca Cloud 看到應用程式 profiling 資訊。

另外在官網也有提供 [Pyroscope Demo](https://demo.pyroscope.io/?query=load-generator.inuse_objects%7B%7D&rightQuery=load-generator.inuse_objects%7B%7D&leftQuery=load-generator.inuse_objects%7B%7D)，有興趣可以看到更多資訊感受其強大威力。

![https://ithelp.ithome.com.tw/upload/images/20231012/20162577U6F3KAaJEZ.png](https://ithelp.ithome.com.tw/upload/images/20231012/20162577U6F3KAaJEZ.png)


## 參考連結
[documentation](https://grafana.com/docs/pyroscope/latest/)

[Pyroscope Language SDKs](https://grafana.com/docs/pyroscope/next/configure-client/language-sdks/)

[pyroscope](https://github.com/grafana/pyroscope)

