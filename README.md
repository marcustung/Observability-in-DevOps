# Observability in DevOps

## 文章簡介
在國外，可觀測性 (Observability) 這個詞已經成為熱門話題引起熱烈討論，在 CNCF (Cloud Native Computing Foundation) landscape 中有一個區域專門介紹可觀測性 (Observability) 相關工具與技術。 在國外研討會中，發現越來越多人探討如何透過可觀測性來實現 DevOps 更高效率。

近幾年，台灣也開始看到越來越多相關文章與研討會分享，分享有關可觀測性的觀念與實踐經驗。在吸收這些知識與寶貴知識後，對這主題有更深入的了解與認識。因此希望自己可以藉由 ITHome 鐵人賽的機會，整理並分享自己關於可觀測性的理解與小小心得。希望藉由透過這次不要臉分享，可以讓對這主題也有興趣的朋友們能夠更快了解可觀測性的重要觀念，一起入坑。

## 目錄
- [第 01 天 : 起心動念](https://github.com/marcustung/Observability-in-DevOps/blob/main/01.md)
- [第 02 天 : Observability in DevOps](https://github.com/marcustung/Observability-in-DevOps/blob/main/02.md)
- [第 03 天 : Business Continuity](https://github.com/marcustung/Observability-in-DevOps/blob/main/03.md)
- [第 04 天 : Service Level Agreement](https://github.com/marcustung/Observability-in-DevOps/blob/main/04.md)
- [第 06 天 : 災難復原計畫 Disaster Recovery Plan](https://github.com/marcustung/Observability-in-DevOps/blob/main/06.md)
- [第 07 天 : 高可用性與可靠性 High Availability & Reliability](https://github.com/marcustung/Observability-in-DevOps/blob/main/07.md)
- [第 08 天 : 監控與指標分析 Monitor](https://github.com/marcustung/Observability-in-DevOps/blob/main/08.md)
- [第 09 天 : DevOps x Observability 小結](https://github.com/marcustung/Observability-in-DevOps/blob/main/09.md)
- [第 10 天 : 可觀測性 Observability](https://github.com/marcustung/Observability-in-DevOps/blob/main/10.md)
- [第 11 天 : 解析監控和可觀測性：從哨塔到全景地圖](https://github.com/marcustung/Observability-in-DevOps/blob/main/11.md)
- [第 12 天 : 可觀察性的演進史：從控制理論到重新定義](https://github.com/marcustung/Observability-in-DevOps/blob/main/12.md)
- [第 13 天 : 可觀測性信號(Signals)的進化之旅](https://github.com/marcustung/Observability-in-DevOps/blob/main/13.md)
- [第 14 天 : 可觀測性管道：解析現代數據收集與分析](https://github.com/marcustung/Observability-in-DevOps/blob/main/14.md)
- [第 15 天 : 開賽至今的回顧與反思](https://github.com/marcustung/Observability-in-DevOps/blob/main/15.md])
- [第 16 天 : 可觀測性與它的工具小夥伴們](https://github.com/marcustung/Observability-in-DevOps/blob/main/16.md])
- [第 17 天 : OpenTelemetry : 在收集遙測數據中加上一點新標準](https://github.com/marcustung/Observability-in-DevOps/blob/main/17.md)
- [第 18 天 : OpenTelemetry : 核心元件大解密 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/18.md)
- [第 19 天 : OpenTelemetry : 核心元件大解密 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/19.md)
- [第 20 天 : OpenTelemetry : Demo 專案快速入門 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/20.md)
- [第 21 天 : OpenTelemetry : Demo 專案快速入門 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/21.md)
- [第 22 天 : 可觀測性摘要 cheetsheet 及小結](https://github.com/marcustung/Observability-in-DevOps/blob/main/22.md)
- [第 23 天 : 可觀測性工具的整合與挑戰](https://github.com/marcustung/Observability-in-DevOps/blob/main/23.md)
- [第 24 天 : Grafana Cloud : 雲端監控的未來](https://github.com/marcustung/Observability-in-DevOps/blob/main/24.md)
- [第 25 天 : Grafana Cloud : 蒐集應用程式遙測數據](https://github.com/marcustung/Observability-in-DevOps/blob/main/25.md)
- [第 26 天 : Grafana Cloud : 蒐集應用程式遙測數據 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/26.md)
- [第 27 天 : Grafana Cloud : 使用 Grafana Pyroscope 優化應用程式性能](https://github.com/marcustung/Observability-in-DevOps/blob/main/27.md)
- [第 28 天 : Grafana 事件管理 : 從告警到修復的問題排除流程 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/28.md)
- [第 29 天 : Grafana 事件管理 : 從告警到修復的問題排除流程 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/29.md)
- [第 30 天 : 從傳統開發到可觀測性驅動開發 (ODD)](https://github.com/marcustung/Observability-in-DevOps/blob/main/30.md)
- [第 31 天 : 30 天之後 ?](https://github.com/marcustung/Observability-in-DevOps/blob/main/31.md)

作者 : Marcus Tung

## 獲獎紀錄
[2023 ITHome 鐵人賽 DevOps 組佳作【Observability 101】](https://ithelp.ithome.com.tw/2023ironman/reward)

## 與我聯絡
- 部落格 : [m@rcus 的學習筆記](https://marcus116.blogspot.tw/)
- 粉絲團 : [m@rcus 的學習筆記](https://www.facebook.com/marcustung.tech)
