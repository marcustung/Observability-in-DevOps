# Day02 - Observability in DevOps

大家好，我是伐伐伐伐木工 

今天要與大家分享 DevOps & Observability，本篇內容的重點如下
- 淺談 Observability 可觀測性 
- 探討 Devops、Observability 與 Business Contiunity 的關聯性 

---

## 淺談 Observability (可觀測性)

先來談談什麼是 Observability 可觀測性，根據 CNCF（Cloud Native Computing Foundation）的定義

> Observability is a system property that defines the degree to which the system can generate actionable insights. It allows users to understand a system’s state from these external outputs and take (corrective) action. 
> 
> Observable systems yield meaningful, actionable data to their operators, allowing them to achieve favorable outcomes (faster incident response, increased developer productivity) and less toil and downtime.
> 
> Consequently, how observable a system is will significantly impact its operating and development costs.

重點摘要
- 系統應具備 <u>可被觀測</u> (Observable) 的能力
- 透過可觀測性的工具可以了解 CPU、Memory、硬碟、API 回應時間、錯誤等資訊。
- 更快的回應速度、增加開發者的生產力、減少停機時間
- 系統的可觀測性程度將影響運營與開發成本

備註 : 這裡先可觀測性做簡單介紹，後面會再針對可觀測性做詳細說明

#### 想要解決問題
- 保持系統穩定運作，減少故障的影響及停機的時間
- 開發與運維同仁可以更高效的運營、提高開發效率

## 淺談 Devops 

我們來談談 DevOps，根據維基百科 Wiki 的定義

> DevOps（Development和Operations）是一種重視「軟體開發人員（Dev）」和「IT運維技術人員（Ops）」之間溝通合作的文化、運動或慣例。通過自動化「軟體交付」和「架構變更」的流程，來使得構建、測試、發布軟體能夠更加地快捷、頻繁和可靠

#### 想要解決什麼問題
- 實現更快速的軟體交付，更高品質的軟體
- 團隊間更好的協作和溝通

## Devops 與 Observability 關聯

> DevOps 與 Observability 兩者的關聯是什麼呢 ? 

Observability 是 Devops 其中的一部分。Observability 透過遙測數據收集，讓系統潛在問題有機會被更快速的發現並解決。DevOps 實現更快速的軟體交付，讓團隊間更高效。透過 Observability與 DevOps 的協作，提高軟體開發與運營的效率，確保系統可靠性。

背後目的是實踐**業務連續性**(Business Contiunity)，業務連續性目標是無論發生什麼故障或是事件，系統都能夠持續運行提供7x24的服務。系統為了達到業務連續性，需要具備可靠性(Reliability)、高可用(High Availability)及穩定性(Stability)。

以上是今天的分享。下一篇要來探討業務連續性(Business Contiunity)，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
- Observability（可觀測性）透過遙測數據的蒐集，幫助我們迅速識別和解決問題，提高系統的穩定性。
- DevOps 和 Observability 共同有助於實現業務連續性(Business Continuity)，確保系統可靠運行，並提高開發和運營的效率。


---

## 參考連結
[Observability](https://glossary.cncf.io/observability/) 

[DevOps](https://zh.wikipedia.org/zh-tw/DevOps) 

[What is DevOps Observability (Importance & Best Practices)](https://www.browserstack.com/guide/observability-devops) 

[What Is Business Continuity ?](https://www.zerto.com/resources/a-to-zerto/business-continuity/) 
