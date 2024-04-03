# Day04 - Service Level Agreement

大家好，我是伐伐伐伐木工

> 如何判斷一個系統或服務的服務質量有多好 ? 

今天要與大家分享 SLA，本篇內容的重點如下
- 什麼是 SLA (Service Level Agreement)
- SLA, SLO, SLI 三者的關係

## 服務級別協議
服務級別協議 SLA (Service Level Agreement) 是服務提供者對於其服務水準的承諾。明確定義了衡量服務的標準跟方法，讓客戶或是使用者可以清楚知道當未達到服務水平時的處罰與補救措施。

常見的 SLA 會包含下列項目
- 服務內容 
- 可用性 
- 賠償條款
- 服務標準
 
聽起來有點抽象，為了讓大家可以更清楚理解，我們來看看 AWS EC2 SLA 作為範例，[連結](https://d1.awsstatic.com/legal/AmazonComputeServiceLevelAgreement/Amazon_Compute_Service_Level_Agreement_Chinese%20Traditional%20(TW)_2022-05-25.pdf)
- 服務內容 ![https://ithelp.ithome.com.tw/upload/images/20230919/201625775Jxc02Aw1p.png](https://ithelp.ithome.com.tw/upload/images/20230919/201625775Jxc02Aw1p.png)
- 可用性 : 
    - **99.99%** 
- 賠償條款![https://ithelp.ithome.com.tw/upload/images/20230919/20162577JsnWEfHv4o.png](https://ithelp.ithome.com.tw/upload/images/20230919/20162577JsnWEfHv4o.png)
- 排除事項![https://ithelp.ithome.com.tw/upload/images/20230919/20162577CgCfDadJ9J.png](https://ithelp.ithome.com.tw/upload/images/20230919/20162577CgCfDadJ9J.png)
- 提供[技術支援](https://aws.amazon.com/tw/premiumsupport/?nc1=f_dr)服務內容

如果有興趣也可以參考其他雲端廠商 SLA 協議，參考連結
- [Azure SLA](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services?lang=1)
- [GCP SLA](https://cloud.google.com/terms/sla) 

#### 可用性
SLA 是可用性中重要的衡量指標，服務提供商與使用者會透過此指標來客觀的評估服務的水準，服務正常運行為 UpTime，反之，當服務發生不可用或是中斷時間稱之 Downtime。其指標越高代表服務越可靠，停機時間越短。

![https://ithelp.ithome.com.tw/upload/images/20230919/20162577kxgo1jrgvq.png](https://ithelp.ithome.com.tw/upload/images/20230919/20162577kxgo1jrgvq.png)
photo credit :[Why 99.99% uptime or SLA is bad](https://andrewmatveychuk.com/why-99-99-uptime-or-sla-is-bad/)
     
接著我們來看可用性時間，下表為換算 SLA 中幾個 9 與 UpTime 及 Downtime 時間關係

| SLA  | Downtime  | UpTime |
| -------- | -------- | -------- |
| 99.999% | 26 seconds | 729 hours, 59 minsutes, 34 second |
| 99.99% | 4 minutes, 22 seconds| 729 hours, 59 minsutes, 34 seconds |
| 99.95% | 21 minutes, 54 seconds| 729 hours, 38 minsutes, 6 seconds | 
| 99.90% | 43 minutes, 49 seconds| 729 hours, 16 minsutes, 12 seconds |
| 99.5% | 3 hours, 39 minutes, 8 seconds | 726 hours, 21 minutes| 
| 99.0% | 7 hours, 18 minutes, 17 seconds | 722 hours, 42 minutes

如果您所使用的系統服務提供的 SLA 為 99.99%，聽起來穩定，但對於服務提供商來說就是很大的挑戰，從上表可以得知，99.99% 意味著當系統發生異常時 5 分鐘之內必須修復好恢復使用，這其中還包含發現問題到修復完畢，然後系統重新上線完成的時間(壓力好大先睡一覺在說?)。

#### SLA 與 SLO
SLA 是對客戶的協議(Agreenents)，是系統服務對外面使用者的承諾，對內而言 RD 所開發的服務需要設定目標來滿足 SLA，SLA 則和 SLO 與 SLI 有密切關係 

> SLI (Service Level Indicator) : 衡量服務性能可靠性的指標

常見的 SLI 指標如下
- Availability、Latency
- Error Rate
- Throughput、MTTR(平均修復時間)

> SLO (Service Level Objective) : 為 SLI 設定可靠性的目標

在內部設定 SLOs 指標時會相較 SLA 更為嚴格。舉例來說
SLA 定義回應時間是 300ms，則在內部 SLO 目標則不會超過 200ms, 才更有機會達標。
![https://ithelp.ithome.com.tw/upload/images/20230919/201625772HUEsCMO0U.png](https://ithelp.ithome.com.tw/upload/images/20230919/201625772HUEsCMO0U.png)

photo credit : [The Art of SLOs](https://sre.google/resources/practices-and-processes/art-of-slos/)

## 如何計算 SLA 
前面介紹了 SLA 基本概念，還有 SLA、SLO、SLI 三者的關係，接著探討 SLA 是如何計算出來的。假設服務架構如下，服務中有應用程式(Web)與中間應用程式(Application)和 SQL 數據庫(SQL)三個組件所組成。
![https://ithelp.ithome.com.tw/upload/images/20230919/201625779Vjxu9ToUT.png](https://ithelp.ithome.com.tw/upload/images/20230919/201625779Vjxu9ToUT.png)
單獨來看，各自的可用性分別是
- Web : 99.95%
- Application : 99.9%
- SQL : 99.95%

整體服務的可用性是多少呢 ?

> 整體 SLA = Web SLA * Aplication SLA * SQL SLA 
>   99.95% * 99.9% * 99.99% = 99.84% 

最終答案是 **99.8%**，根據 [uptime.is](https://uptime.is/) 的計算，允許 downtime 的時間為
![https://ithelp.ithome.com.tw/upload/images/20230919/201625774cDQrBnb8x.png](https://ithelp.ithome.com.tw/upload/images/20230919/201625774cDQrBnb8x.png)
photo credit : https://uptime.is/

假設對外服務的承諾 SLA 是 99.9%，那麼就要思考架構上要如何優化，才有辦法從 99.8% 變為承諾的 99.9%。要注意的是，SLA 目標設定越高所需要花費及投入的成本更高，如果身為管理者，希望減少停機的時間，勢必要投入額外的基礎建設資源或是故障移轉機制，就需要額外思考其額外多出的成本是否值得投入。

如果想了解更多SLA、SLO、SLI 三者關係，個人推薦觀看下面影片，可以有更清楚的理解
[![Yes](https://img.youtube.com/vi/2alcBvMBRzw/0.jpg)](https://www.youtube.com/watch?v=2alcBvMBRzw)
以上是今天關於 SLA 的內容分享，下一篇要來探討 DR(Disaster Recovery)，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
- SLA 是服務提供商與客戶之間定義的正式承諾
- SLO 是為了實現 SLA 的目標所設定的，SLI 用於衡量服務的性能指標

## 參考連結
[服務級別協定](https://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E7%BA%A7%E5%88%AB%E5%8D%8F%E8%AE%AE)

[軟體技術架構如何正確與商業需求快速對齊：談 MAU 換算至 RPS，SLA 回推至 SLI](https://blog.gcos.me/post/2020-03-11_how-software-architecture-meet-business-require-mau-sla-by-rps-slo/)

[Cloud SLAs punish, not compensate](https://journal.uptimeinstitute.com/cloud-slas-punish-not-compensate/)

[Why 99.99% uptime or SLA is bad](https://andrewmatveychuk.com/why-99-99-uptime-or-sla-is-bad/)

[How do you calculate the compound Service Level Agreement (SLA) for cloud services?](https://devops.stackexchange.com/questions/711/how-do-you-calculate-the-compound-service-level-agreement-sla-for-cloud-servic)

[The Art of SLOs](https://sre.google/resources/practices-and-processes/art-of-slos/)

[SLA Vs SLO: Tutorial & Examples](https://www.squadcast.com/sre-best-practices/sla-vs-slo)
