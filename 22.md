# Day22 - 可觀測性摘要 cheetsheet 及小結

以下列出這幾天可觀測性文章相關重點文字整理

* **可觀測性 Observability**
    - 可觀察性是指您能夠從系統提供的資料中了解系統的內部狀態，並且您可以探索該資料來回答有關發生了什麼以及為什麼發生的任何問題。
* **監控和可觀測性差異**
    - 監控是告訴你什麼沒有工作。
    - 可觀察性是告訴你為什麼它不工作。
* **可觀察性的演進史**
    * 從 1980年代的控制理論演變到現今的高度結構化和詳細的可觀察性過程
* **可觀測性信號 Observability Signals**
    - 可觀測性三個重要的 Signals，分別是 Metrics（指標）、Structured Log（結構化日誌）和 Distributed Tracing（分散式追蹤）。
    - 連續性分析（Continuous Profiling）是可觀測性新的趨勢，提供運行時間控應用性能的能力。
* **可觀測性管道 Observability Pipleline**
    - 可觀測性管道四個階段 : 收集(collection)、攝取(ingestion)、儲存(storage)和查詢(query)
    - 三個重要組件 : Agent、Collectors、Pipelines
* **可觀測性工具**
    - Cloud Native Projects (原生雲專案)
    - Observability & Analysis 專區清單整理

文字理解幫助還是有限，因此自己整理圖形化的 cheatsheet 關係圖，方便大家更好的理解彼此依賴關係

![https://ithelp.ithome.com.tw/upload/images/20231007/20162577WqX1EPyaaP.jpg](https://ithelp.ithome.com.tw/upload/images/20231007/20162577WqX1EPyaaP.jpg)

邁入鐵人賽第 22 天也開始慢慢接近尾聲，如第一天提到 Observability in Devops 與可觀測性 (Observability) 內容，也簡單介紹可觀測性中扮演重要元素的 OpenTelemetry 遙測數據收集新的標準，接著就是要進入工具實踐介紹的章節，也希望自己可已順利完賽 !



