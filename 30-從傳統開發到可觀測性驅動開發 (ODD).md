# Day30 - 從傳統開發到可觀測性驅動開發 (ODD)

![https://ithelp.ithome.com.tw/upload/images/20231016/20162577oElLFd6gbt.jpg](https://ithelp.ithome.com.tw/upload/images/20231016/20162577oElLFd6gbt.jpg)

> 傳統的軟體開發方法如何面對現代化系統複雜的挑戰 ? 或者，有沒有一種方法能夠建構更可靠、高性能的應用程式 ?

## Observability Maturity Model : 可觀測性成熟度
監控 (Monitor) 做為團隊了解系統可用性與效能這方式已存在數十年，傳統的監控工具有助於捕捉一組特定的指標，解決**已知的未知問題**，可觀測性是監控的再進化，透過蒐集可觀測性重要的指標像是 metrics、trace、log 及 profiling 等信號資訊，希望解決**未知的未知問題**。前面在可觀測性工具的整合與挑戰文章也提到了工具在整合時所遭遇的問題，超過一半以上公司使用 5 種以上工具，數據上的整合或是維運人員在不同工具的 context switch 都會是個潛在的問題。
> 解決這些問題，就是實踐可觀測性了嗎 ?

![https://ithelp.ithome.com.tw/upload/images/20231016/20162577lhvEv6D0MP.png](https://ithelp.ithome.com.tw/upload/images/20231016/20162577lhvEv6D0MP.png)


在 [Observability Maturity Model](https://dzone.com/refcardz/observability-maturity-model?fromrel=true) 文章中定義了可觀測性演化的四個不同的層級，每個層級各有不同的缺點、挑戰與說明事項，還有如何影響系統的可靠性 (Reliability)。這四個層級分別是 **Monitoring**、**Observability**、**Causal Observability** 與 **Proactive Observability with AIOps**。透過表格整理四個層級各自重點與目的如下

| 等級 | 主要目的 | 摘要 | 
| -------- | -------- | -------- | 
| Monitoring      | 確保各個組件如預期運作     | - 追蹤 IT 系統中各個元件的基本運作狀況<BR>- 查看事件；觸發警報和通知<BR>- 告訴您出了問題 <BR>    |
| Observability | 確定系統不工作的原因 |- 透過觀察系統的輸出來深入了解系統行為<BR>- 重點關注從指標、日誌和跟踪中推斷出的結果，並結合現有的監控數據<BR>- 提供基線數據以幫助調查出了什麼問題以及原因 | 
| Causal Observability | 尋找事件的原因並確定其對整個系統的影響 | - 提供更全面的見解，幫助確定導致問題的原因<BR> - 基於 1 級和 2 級基礎構建，並增加了跟踪 IT 堆疊中拓撲隨時間變化的能力<BR> - 生成廣泛的相關信息，有助於減少識別問題所所需的時間，問題為何發生、何時開始以及哪些其他領域受到影響 | 
| Proactive Observability with AIOps | 分析大量數據，自動回應事件，並防止異常成為問題 | - 使用 AI 和 ML 尋找大量資料中的模式<BR>- 將 AI/ML 與 1-3 級資料結合，提供整個堆疊中最全面的分析<BR>- 及早檢測異常並發出足夠的警告以防止故障 | 

隨著成熟度等級越高，團隊在處理問題上就可以更快地找到故障異常的服務，更快速的找到問題根本原因，提供客戶更好的體驗與提高系統可靠性 (Reliability)，縮短 MTTR 的時間。


## Observability Driven Development : 可觀測性驅動開發 

#### 什麼是可觀測性驅動開發 ODD ? 
![https://ithelp.ithome.com.tw/upload/images/20231016/20162577hKv97NTg1B.png](https://ithelp.ithome.com.tw/upload/images/20231016/20162577hKv97NTg1B.png)

權威調查機構 Gartner 每年會定期公布技術趨勢，在 2022 年 新興技術成熟度公布有三項與可觀測性相關的項目分別是
- OpenTelemetry : 
    - 是由一系列規範、工具、API和SDK組成的可觀察能力框架，進而支援開源儀器及實現軟體的可觀察性。
    - 到達成熟需要的時間 (預估) : 2 ~ 5 年
- 數據可觀測性 （Data observability）:
    - 是指透過持續監測、追蹤、警報、分析和故障排除的記錄，來了解組織的資料圖景、資料工作流程和資料基礎設施等健康狀況的能力。
    - 到達成熟需要的時間 (預估) : 5 ~ 10 年
- 可觀測性驅動開發 (Observability-driven development，ODD) : 
    - 一種軟體工程實踐，透過設計可觀察的系統，為系統的狀態和行為提供細粒度的可見性與背景。
    - 到達成熟需要的時間 (預估) : 5 ~ 10 年

在 2023 年也把可觀測性技術列為戰略技術策略趨勢之一。

過去曾經聽過測試驅動開發 TDD (Test-Driven design)、領域驅動開發 DDD (Domain-driven design)、死線驅動開發 DDD (Deadline-driven design)，那麼驅動開發這個詞究竟是怎麼來的呢 ? 「驅動開發」詞源自於 1989 年 Rebeccah Wirf-Brock 的[責任驅動設計](https://en.wikipedia.org/wiki/Responsibility-driven_design) (Responsibility-driven design)。

可觀測性的目的是讓團隊可以更了解系統運行的狀況，那麼可觀測性驅動開發是甚麼呢 ? 
> 可觀察性驅動開發 (ODD) 是一種將可觀察性需求整合到軟體開發生命週期(SDLC)早期階段的方法。

ODD 可觀測性驅動開發是一個新的術語，強調在整個軟體開發週期中加入可觀測性的需求，像是可觀測性重要信號日誌、指標和追蹤。這種收集跟分析是在應用程式的開發過程中完成的，而不是在其中一個階段像是上線後或是當問題發生後才加入。

![https://ithelp.ithome.com.tw/upload/images/20231016/20162577b9cCdxfOrn.png](https://ithelp.ithome.com.tw/upload/images/20231016/20162577b9cCdxfOrn.png)


就像 TDD 測試驅動開發強調在寫程式碼前要先寫測試案例，以提高程式碼的保護與品質。ODD 在建立可觀測性系統方面也是如此，可觀察性驅動開發意味著開發人員在編寫程式碼之前考慮可觀察性信號或是監控方法，適用於元件級別或是整個系統。利用工具和實踐開發人員來觀察系統的狀況和行為，讓團隊可以更了解系統，包含可能的弱點或是潛在問題。

TDD 在測試(Test)和設計(Design)之間創建了反饋(feedback) 循環，而 ODD 擴展了反饋循環，確保功能按預期運行，改進部署流程(Deploy)並向規劃(Plan)提供反饋。

當功能或是程式上線之後，就可以透過遙測數據分析程式上線後，程式有按照預期執行嗎 ? 還有什麼看起來很奇怪的嗎 ? 適時地放入麵包屑，以便未來當問題發生時可以有效的追朔到問題的源頭。

#### ODD 加入 SDLC 階段可能的挑戰 ? 
前面提到可觀測性驅動開發左移觀念，以下列出在各 SDLC 階段可能會遇到的問題

![https://ithelp.ithome.com.tw/upload/images/20231016/20162577pz4cBNBffB.png](https://ithelp.ithome.com.tw/upload/images/20231016/20162577pz4cBNBffB.png)

- 設計 (Design) 
    * 在設計階段確定系統的運作方式和功能 (outcome)，可能是系統或是業務 KPI 相關問題。
    * 確定要持續監控和追蹤的關鍵數據點或指標，例如 MTTR、平均回應時間等。
    * 透過公用程式碼或 AOP 方式，方便統一從容器、服務等收集遙測數據。

- 開發 (Development) 
    * 為儀器數據提供上下文，使原始數據有意義。
    * 使用統一的上下文標準規範，降低蒐集與開發人員手動處理
    * 使用 OpenTelemetry 作為遙測數據收集的標準 (10篇文章會有 12 篇推薦你使用 XDDD)

- 建置與部署 (Build & Deployment)
    - 遵循「[可觀察性即程式碼](https://www.techtarget.com/searchitoperations/tip/Observability-as-code-is-key-to-the-cloud-operating-model)」的實踐。
    - 通過部署管道 pipleline 實現可觀察性的自動化。

- 維運 (Operate) 
    - 實踐可觀察性使您能夠進行準確的 debugging 和主動檢測，提高生產系統的效率。
    - DevOps 團隊應專注於主動監控標準最佳實務，將營運期間的觀察結果回饋給開發團隊。


當開發人員 Day1 在開發程式時就建立營運思維，在系統營運上就有機會更快的解決跟發現可能的問題，讓使用者更滿意。

## 小結
- 觀察性驅動開發（ODD）強調在整個軟體開發生命週期中加入可觀察性方面的需要。鼓勵從早期階段就將可觀察性所需的活動左移。

## 參考連結

[Observability-Driven Development: From Development to DevOps](https://www.simform.com/blog/observability-driven-development/)

[Resilient software delivery through observability-driven development](https://www.ey.com/en_in/cybersecurity/resilient-software-delivery-through-observability-driven-development)

[Observability-driven development](https://developer.ibm.com/articles/observability-driven-development/)

[Observability Maturity Model](https://dzone.com/refcardz/observability-maturity-model?fromrel=true)

[Moving to Observability Driven Development](https://dzone.com/articles/moving-towards-observability-deriven-development)

[Gartner 公布 2022 新兴技术成熟度曲线，这些技术趋势最值得关注](https://www.infoq.cn/article/5Bp8BIJd5DSgFzaRjzAT)

[What Observability-Driven Development Is Not](https://www.honeycomb.io/blog/observability-driven-development)


