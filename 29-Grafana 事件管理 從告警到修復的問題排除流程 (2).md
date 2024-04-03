# Day29 - Grafana 事件管理 : 從告警到修復的問題排除流程 (2/2)

## 過去怎麼做
1. 口而相傳，緣分到了就會修好
2. 各自通靈
3. (文件)定義流程 + 系統

## Grafana Incident Response & Management (IRM)
![https://ithelp.ithome.com.tw/upload/images/20231016/201625779oZRa27rax.png](https://ithelp.ithome.com.tw/upload/images/20231016/201625779oZRa27rax.png)
Grafana 解決方案 : Grafana IRM

* Grafana IRM = Incident Response  & Management
* 回應 (Response) :
    * Grafana Alter：這部分涉及設定警報和通知系統，以便在系統或應用程式出現問題時能夠及時通知相關團隊成員。這有助於快速回應問題並採取必要的措施。
    * Grafana Oncall：Grafana Oncall是一個關鍵元件，它確定了誰在特定的時間段內負責解決問題。這確保了團隊中的成員在需要待命時都自己知道處理問題。
    * Grafana Incident：Grafana Incident模組用於記錄和追蹤發生的問題和事件。它可幫助團隊建立問題的歷史記錄，以便記錄更好地了解問題的模式和趨勢。
* 管理 (Management) : using Grafana Incident
    * 問題協作：管理團隊成員之間的協作，確保每個人都知道問題的狀態和處理進度。
    * 問題解決：提供工具和資源，幫助團隊成員更輕鬆解決問題，包括文件、故障排除指南和知識庫(KM)。
    * 問題分析：幫助團隊分析問題的根本原因 Root cause。

由什麼所組成
![https://ithelp.ithome.com.tw/upload/images/20231016/20162577ZTXOlvxLfp.png](https://ithelp.ithome.com.tw/upload/images/20231016/20162577ZTXOlvxLfp.png)



