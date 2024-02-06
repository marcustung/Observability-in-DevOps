# Day05 - Disaster Recovery 

大家好，我是伐伐伐伐木工

今天要與大家分享的主題是災難復原(DR)，本篇內容的重點如下
- 什麼是災難復原 (Disaster Recovery)

## 災難復原 Disaster Recovery 

![https://ithelp.ithome.com.tw/upload/images/20230920/20162577Q1SoiFJj3R.png](https://ithelp.ithome.com.tw/upload/images/20230920/20162577Q1SoiFJj3R.png)
photo credit https://www.techtarget.com/searchdisasterrecovery/definition/Business-Continuity-and-Disaster-Recovery-BCDR

災難復原是業務連續(BC)的一部分。業務連續性是指企業有應對風險、自動調整和快速反應的能力，以保證企業業務的連續運轉。為企業重要應用和流程提供業務連續性應該包括以下三個方面。
1.	高可用性（High availability）: 指提供在本地故障情況下，能繼續訪問應用的能力。無論這個故障是業務流程、物理設施，還是IT軟硬體故障。
2.	連續操作（Continuous operations）: 指當所有設備無故障時保持業務連續運行的能力。用戶不需要僅僅因為正常的備份或維護而需要停止應用的能力。
3.	災難復原（Disaster Recovery）: 指當災難破壞生產中心時，在不同的地點恢複數據的能力。

簡單來說，BCP的目標只有一個，那就是確定並減少危險可能帶來的損失，有效地保障業務的連續性。
今天所要介紹的是第三部分災難復原 DR 為出發，規劃當災難發生時所要進行的動作。
在災難復原中 **RTO** 與 **RPO** 是很常聽到的概念，兩者分別是

#### RTO : **R**ecovery **T**ime **O**bjectives
- 當災難發生時恢復應用程式與數據所需要花費的時間
- 設定這指標目的是減少停機的時間。
- 手動恢復流程與自動恢復流程不同，會影響恢復時間長短。

#### RPO : **R**ecovery **P**oint **O**bjectives
- 當災難發生時允許遺(丟)失多少資料，白話就是接受噴掉多久時間的資料量
- 設定這指標目的是最小化數據丟失。
- 備份或是數據保護量頻率越高，丟失的數據量就會越少。

可能聽起來有些抽象沒那麼好理解，透過以下示意圖可以更清楚瞭解 RTO 與 RPO 兩者的差異

![https://ithelp.ithome.com.tw/upload/images/20230920/201625775Uz8lYoBfE.png](https://ithelp.ithome.com.tw/upload/images/20230920/201625775Uz8lYoBfE.png)

讓我們舉個實例看看其應用

前情提要
- 指標 : RTO 與 RPO 分別是 4 小時
- 備份時間 : 系統固定 4 小時進行備份，分別是 6:00、10:00、14:00 與 18:00

> 問題 : 如果 13:05 發生系統中斷時，RPO 會是多少 ? 

當下午 13:05 發生意外中斷，最後數據備份時間是 10:00，因此 RPO 為 3小時5分鐘，資料庫恢復備份與重新佈署應用程式，4小時候系統可以正常恢復運作。

## SLA 與 RTO , RPO 關係

前一篇文章中，我們提到透過 SLA 可以清楚的知道服務的可用性與服務內容與補償方案，幫助企業評估是否可以承受其風險，在災難復原的 SLA ，主要的指標是 **RTO** 與 **RPO**，兩者是災難復原 DR 中重要的衡量指標，以時間單位為衡量值越低越理想。

![](https://www.zerto.com/wp-content/uploads/brizy/imgs/From-SLA-MTD-MTDL-to-RTO-RPO-diagram-798x538x0x0x798x538x1671219975.png)

在制定 RTO 與 RPO 之前需要考慮到 MTD(最大可容忍時間) 與 MTDL(最大可容忍數據丟失) 兩個業務連續性(BC)指標，並在設定 RTO 與 RPO 指標時，需小於 SLA 所設定的 MTD 與 MTDL，才有機會達到目標，其關係可以參考上圖。


以上是今天的分享。下一篇要來探討災難復原計畫 (Disaster Recovery Plan)，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
- 災難復原 (DR) 目的是確保組織在意外發生時能夠以最快速的方式來恢復，以確保業務連續性(BC)
- RTO 最大可允許中斷時間，RPO 數據損失可允許的最遠回溯時點

## 參考連結
[Disaster Recovery (DR)](https://www.zerto.com/resources/essential-guides/disaster-recovery-guide/)

[Risk Assessment, BIA, SLAs, RTOs, and RPOs: What’s the Link? MTD and MTDL](https://www.zerto.com/blog/business-it-resilience/risk-assessment-bia-slas-rtos-and-rpos-whats-the-link-mtd-and-mtdl/)

[Business continuity management](https://learn.microsoft.com/en-us/azure/reliability/business-continuity-management-program)
