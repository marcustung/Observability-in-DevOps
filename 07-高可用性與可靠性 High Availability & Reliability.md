# Day07 - 高可用性與可靠性 High Availability & Reliability

大家好，我是伐伐伐伐木工

今天要跟大家分享在討論系統中常會聽到的概念可用性(High Available)與可靠性(Reliability)，本篇內容的重點如下
- 可用性與可靠性

## 可用性與可靠性
當我們在討論系統時，使用者通常只會遇到兩種情況，它「可以使用」或「無法使用」。「可以使用」是使用者順利完成他們的目標；不可用則是無法使用其功能，其背後可能是使用者自己的錯誤、系統 Bug 或是環境問題，例如網路瞬斷、設備異常或遭受攻擊。

接著回到主題，根據維基百科關於兩者的定義
> 可用性 : 系統在給定時間運行的概率，即設備實際運行的時間佔其應運行的總時間的百分比。
> 可靠性 : 系統在某個給定時間t內產生正確輸出的概率

試著透過翻譯蒟蒻翻譯如下
> 可用性 : 正常運行時間的百分比(成功)
> 可靠性 : 特定的時間內，系統正常執行成功的機率(失敗)

兩者都是屬於抽象的概念，為了更好理解可用性與可靠性，可以使用兩個指標來衡量
| Concept | Metric |Example|
| -------- | -------- | -------- |
| Availability     | 百分比     |99.90% |
| Reliability     | 平均無故障時間 (MTBF)     |20天10小時12分鐘 |

備註 : 
- 平均故障間隔時間(MTBF) : 總正常運行時間/故障數量。
- 平均修復時間(MTTR) : 總停機時間/故障數量

可靠性衡量系統正確運行的能力，包括避免數據損壞，而可用性衡量系統可用的頻率，即使它可能無法正常運行。

## 共同特性

可用性與可靠性都是服務重要的關鍵特性，兩者具有一些共用的特性，這些特性有助於確保系統的穩定性，以下是列出個人覺得兩者共同具備的重要特性
- Monitoring and Alerting 
- Automation 
- Redundancy 

監控機制跟告警機制(Monitoring and Alerting)，目的是當系統或服務有不穩定時可以在第一時間知道並進行處理，提高系統的穩定性與停機時間。自動化(Automation)可以加速故障恢復的時間，並減少在緊急問題人為錯誤的可能性。Redundancy 是什麼呢 ? 以下就針對 Redundancy 進行更多的說明。

#### 冗餘 Redundancy 
> 冗餘是刻意設置重複的零件或是功能，作為安全的緩衝。以確保如果一個服務組件發生故障，其他服務組件可以併行工作不影響其服務內容。

為了要提高系統可用性，可以先透過盤點方式了解系統架構中關鍵的組件，每個組件都會有其可用性(故障率)，如果單一組件可用性高(故障率低)，可以透過簡單的冗餘策略來提高可用性，例如在雲端環境中透過自動擴展(Scale)機制，確保單點發生故障時也能維持一定數量的 Instance。

例如，在雲端服務平台中設定規則，應用程式 Instance 的數量必須要同時維持 4 台。因此當雲端平台監控到某台機器已經關閉或是health check 沒回應時，會立即根據其配置建立副本應用程式，確保在最短停機時間並減少恢復可用性的手動干擾。

以上是關於高可用與可靠性的特點與解釋，如果有任何疑問或想法，歡迎留言提出討論 !

## 小結
- 可用性是正常運行時間的百分比，使用 SLA 來衡量 
- 可靠性是特定的時間內，系統正常執行成功的機率。使用 MTBF 來衡量
 
## 參考連結
[High availability](https://en.wikipedia.org/wiki/High_availability)

[Available . . . or not? That is the question—CRE life lessons](https://cloud.google.com/blog/products/gcp/available-or-not-that-is-the-question-cre-life-lessons)

[Reliability, availability and serviceability](https://en.wikipedia.org/wiki/Reliability,_availability_and_serviceability)

[Impact of Redundancy on Availability](https://rickhw.github.io/2020/04/22/SoftwareEngineering/Reliability-Engineering/)

[可靠性工程 (Reliability Engineering)](https://rickhw.github.io/2020/04/22/SoftwareEngineering/Reliability-Engineering/)





