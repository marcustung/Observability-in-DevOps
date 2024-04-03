# Day24 - Grafana Cloud : 雲端監控的未來

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577gv67moFEvN.jpg](https://ithelp.ithome.com.tw/upload/images/20231009/20162577gv67moFEvN.jpg)

## Grafana 起源
在 GrafanaCON 2023 keynote 開幕創辦人 Torkel Ödegaard 回顧一切是如何開始的故事。

Grafana 起源來自兩個偉大的開源專案 `Graphite` 和 `Kibana`，Graphite 能查詢分析指標並繪製圖表，但圖表無法互動僅是 png 圖片，在編輯查詢與建置圖表上是複雜的。Kibana 是 open source 專案，提供可視化儀表板並儲存在 Elasticsearch 日誌中，2013 年誕生後改變集中式日誌與可視化儀表板的遊戲規則。

![https://ithelp.ithome.com.tw/upload/images/20231009/201625775K2na1mm6s.png](https://ithelp.ithome.com.tw/upload/images/20231009/201625775K2na1mm6s.png)

> Torkel : 如果 Kibana 可以查詢 Graphite 並加上 Metrics 呢 ?
 
~~爭什麼爭，摻在一起做撒尿牛丸不就好了嗎~~ ? 

但 Kibana 專注於 Elasticsearch 所提供的數據資料，並不支援其他數據來源，因此 Torkel 將 kibana 專案 fork 下來並完全專注於指標與時間序列，核心概念為 `Make querying easier` ，加上自己想法調整完後初版在 2014.1 在 github 推出初版

![https://ithelp.ithome.com.tw/upload/images/20231009/2016257772GQnmNysl.png](https://ithelp.ithome.com.tw/upload/images/20231009/2016257772GQnmNysl.png)

當時已經有很多 Graphite Dashboard 解決方案，作者認為推出後並不會發生什麼，但 Grafana 乾淨好看的用戶介面，快速查詢與好用的編輯查詢介面支持，讓儀表板可以重複使用，造成在監控社群上廣大的好評。

作者也分享當時的 Logo 想法 (都有動物 XDDDD
![https://ithelp.ithome.com.tw/upload/images/20231009/20162577lsZfsbQh2m.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577lsZfsbQh2m.png)

更多關於 Grafana 的歷史與重大里程碑可以看影片
[![Yes](https://img.youtube.com/vi/-gdz8re9qLs/0.jpg)](https://www.youtube.com/watch?v=-gdz8re9qLs)

## Grafana Cloud 是什麼

> Grafana is an open source interactive data-visualization platform

Grafana 是一個開放源碼的監控視覺化工具，提供不同的時間序資料庫來源，用於監控和可視化指標數據。

Grafana Cloud 是 Grafana Labs 所提供的雲端可觀測性平台，讓開發人員可以收集、儲存、視覺化應用程式的 Log、Metrics、Trace，並發送告警警報。並與100 多個外部數據來源集成，包括 AWS CloudWatch、Elasticsearch、Graphite、和 InfluxDB。它與 Grafana K6 整合進行效能測試、Grafana Incident 和 Grafana OnCall 以管理事件議題處理與回應流程。

#### Core Stack : LGTM
核心專案由`LGTM`，分別是 Loki、Grafana、Tempo 與 Mimir，以下針對核心專案做簡短說明其用途
* **Logs** : 
    * 由高效能日誌聚合和儲存系統 Grafana Loki 專案負責
    * 集中日誌且與 Prometheus 指標一致，且 Loki 使用 LogQL 查詢日誌
* **Grafana Dashboard** : 
    * Grafana 是每個 Grafana Cloud stack 的中心
    * 提供查詢、視覺化、發出警報並了解您的指標，並顯示在儀表板上。 
* **Grafana Tempo** : 
    * 提供易於使用、高度可擴展且經濟高效的分散式追蹤系統
    * Tempo 依賴 Grafana 內的深度整合，能夠在指標、日誌和追蹤之間無縫切換
* **Metrics** : 
    * Grafana Mimir 專案負責
    * 並具有高可用性、多租戶、持久儲存以及長時間內極快的查詢效能。
* **Grafana Agent** : 
    * 輕量級收集器，用於將遙測資料傳送到 Grafana Cloud

備註 : 以上皆為 Open Source 專案，如果要自架也可以。

#### Pricing 費用
分 FREE、Cloud Pro 及 Cloud Advanced  三種，其支援功能與限制如下

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577rFWRCUgpsi.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577rFWRCUgpsi.png)


## 起手式 : 註冊 Grafana Cloud 帳號
要開始使用 Grafana Cloud，需要先在官方網站註冊，點擊註冊頁面 [SIGN UP](https://grafana.com/auth/sign-up/create-user) ，可選擇透過其他帳號登入，或是建立新的帳號名稱

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577hbcQmCCnMX.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577hbcQmCCnMX.png)

選擇地區及輸入名稱

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577N8yB64rgHD.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577N8yB64rgHD.png)

按下 Finish setup，進行相關資料建立動作

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577EaBfR1Q2vR.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577EaBfR1Q2vR.png)

建立完成後，可以看到目前資源的資訊 (Free account 前 15 天免費，可以好好利用 XD)

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577nZvBM0cKjm.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577nZvBM0cKjm.png)

按下右上方 Connect data，可以選擇將資料進行整合到 Grafana Cloud 在 Dashboard 上呈現

![https://ithelp.ithome.com.tw/upload/images/20231009/20162577IEy07dlyMS.png](https://ithelp.ithome.com.tw/upload/images/20231009/20162577IEy07dlyMS.png)


以上是今天的分享，明天來繼續介紹如何與應用程式整合將資料上傳到 Grafana Cloud !

## 小結
* Grafana 是一個開放源碼的監控視覺化工具，提供不同的時間序資料庫來源，用於監控和可視化指標數據。
* Grafana Cloud 是雲端可觀測性平台，讓開發人員可以收集、儲存、視覺化應用程式的 Log、Metrics、Trace，並發送告警警報。

## 參考連結
[Grafana Cloud](https://grafana.com/products/cloud/)

[Grafana Cloud 介紹](https://grafana.com/products/cloud/features/#cloud-traces?pg=blog&plcmt=body-txt)


