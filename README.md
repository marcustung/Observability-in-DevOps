# Observability in DevOps

## 前言
在國外，可觀測性 (Observability) 這個詞已經成為熱門話題引起熱烈討論，在 CNCF (Cloud Native Computing Foundation) landscape 中有一個區域專門介紹可觀測性 (Observability) 相關工具與技術。 在國外研討會中，發現越來越多人探討如何透過可觀測性來實現 DevOps 更高效率。

近幾年，台灣也開始看到越來越多相關文章與研討會分享，分享有關可觀測性的觀念與實踐經驗。在吸收這些知識與寶貴知識後，對這主題有更深入的了解與認識。因此希望自己可以藉由 ITHome 鐵人賽的機會，整理並分享自己關於可觀測性的理解與小小心得。希望藉由透過這次不要臉分享，可以讓對這主題也有興趣的朋友們能夠更快了解可觀測性的重要觀念，一起入坑。

## 目錄
- [第 01 天 : 起心動念](https://github.com/marcustung/Observability-in-DevOps/blob/main/01-%E5%89%8D%E8%A8%80.md)
- [第 02 天 : Observability in DevOps](https://github.com/marcustung/Observability-in-DevOps/blob/main/02-Observability%20in%20DevOps.md)
- [第 03 天 : Business Continuity](https://github.com/marcustung/Observability-in-DevOps/blob/main/03-Business%20Continuity.md)
- [第 04 天 : Service Level Agreement](https://github.com/marcustung/Observability-in-DevOps/blob/main/04-Service%20Level%20Agreement.md)
- [第 05 天 : Disaster Recovery](https://github.com/marcustung/Observability-in-DevOps/blob/main/05-Disaster%20Recovery.md)
- [第 06 天 : 災難復原計畫 Disaster Recovery Plan](https://github.com/marcustung/Observability-in-DevOps/blob/main/06-%E7%81%BD%E9%9B%A3%E5%BE%A9%E5%8E%9F%E8%A8%88%E7%95%AB%20Disaster%20Recovery%20Plan.md)
- [第 07 天 : 高可用性與可靠性 High Availability & Reliability](https://github.com/marcustung/Observability-in-DevOps/blob/main/07-%E9%AB%98%E5%8F%AF%E7%94%A8%E6%80%A7%E8%88%87%E5%8F%AF%E9%9D%A0%E6%80%A7%20High%20Availability%20%26%20Reliability.md)
- [第 08 天 : 監控與指標分析 Monitor](https://github.com/marcustung/Observability-in-DevOps/blob/main/08-%E7%9B%A3%E6%8E%A7%E8%88%87%E6%8C%87%E6%A8%99%E5%88%86%E6%9E%90%20Monitor.md)
- [第 09 天 : DevOps x Observability 小結](https://github.com/marcustung/Observability-in-DevOps/blob/main/09-DevOps%20x%20Observability%20%E5%B0%8F%E7%B5%90.md)
- [第 10 天 : 可觀測性 Observability](https://github.com/marcustung/Observability-in-DevOps/blob/main/10-%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%20Observability.md)
- [第 11 天 : 解析監控和可觀測性：從哨塔到全景地圖](https://github.com/marcustung/Observability-in-DevOps/blob/main/11-%E8%A7%A3%E6%9E%90%E7%9B%A3%E6%8E%A7%E5%92%8C%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%EF%BC%9A%E5%BE%9E%E5%93%A8%E5%A1%94%E5%88%B0%E5%85%A8%E6%99%AF%E5%9C%B0%E5%9C%96.md)
- [第 12 天 : 可觀察性的演進史：從控制理論到重新定義](https://github.com/marcustung/Observability-in-DevOps/blob/main/12-%E5%8F%AF%E8%A7%80%E5%AF%9F%E6%80%A7%E7%9A%84%E6%BC%94%E9%80%B2%E5%8F%B2%EF%BC%9A%E5%BE%9E%E6%8E%A7%E5%88%B6%E7%90%86%E8%AB%96%E5%88%B0%E9%87%8D%E6%96%B0%E5%AE%9A%E7%BE%A9.md)
- [第 13 天 : 可觀測性信號(Signals)的進化之旅](https://github.com/marcustung/Observability-in-DevOps/blob/main/13-%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%E4%BF%A1%E8%99%9F(Signals)%E7%9A%84%E9%80%B2%E5%8C%96%E4%B9%8B%E6%97%85.md)
- [第 14 天 : 可觀測性管道：解析現代數據收集與分析](https://github.com/marcustung/Observability-in-DevOps/blob/main/14-%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%E7%AE%A1%E9%81%93%EF%BC%9A%E8%A7%A3%E6%9E%90%E7%8F%BE%E4%BB%A3%E6%95%B8%E6%93%9A%E6%94%B6%E9%9B%86%E8%88%87%E5%88%86%E6%9E%90.md)
- [第 15 天 : 開賽至今的回顧與反思](https://github.com/marcustung/Observability-in-DevOps/blob/main/15-%E9%96%8B%E8%B3%BD%E8%87%B3%E4%BB%8A%E7%9A%84%E5%9B%9E%E9%A1%A7%E8%88%87%E5%8F%8D%E6%80%9D.md)
- [第 16 天 : 可觀測性與它的工具小夥伴們](https://github.com/marcustung/Observability-in-DevOps/blob/main/16-%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%E8%88%87%E5%AE%83%E7%9A%84%E5%B7%A5%E5%85%B7%E5%B0%8F%E5%A4%A5%E4%BC%B4%E5%80%91.md)
- [第 17 天 : OpenTelemetry : 在收集遙測數據中加上一點新標準](https://github.com/marcustung/Observability-in-DevOps/blob/main/17-OpenTelemetry%20%E5%9C%A8%E6%94%B6%E9%9B%86%E9%81%99%E6%B8%AC%E6%95%B8%E6%93%9A%E4%B8%AD%E5%8A%A0%E4%B8%8A%E4%B8%80%E9%BB%9E%E6%96%B0%E6%A8%99%E6%BA%96.md)
- [第 18 天 : OpenTelemetry : 核心元件大解密 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/18-OpenTelemetry%20%E6%A0%B8%E5%BF%83%E5%85%83%E4%BB%B6%E5%A4%A7%E8%A7%A3%E5%AF%86%20(1).md)
- [第 19 天 : OpenTelemetry : 核心元件大解密 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/19-OpenTelemetry%20%E6%A0%B8%E5%BF%83%E5%85%83%E4%BB%B6%E5%A4%A7%E8%A7%A3%E5%AF%86%20(2).md)
- [第 20 天 : OpenTelemetry : Demo 專案快速入門 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/20-OpenTelemetry%20Demo%20%E5%B0%88%E6%A1%88%E5%BF%AB%E9%80%9F%E5%85%A5%E9%96%80(1).md)
- [第 21 天 : OpenTelemetry : Demo 專案快速入門 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/21-OpenTelemetry%20Demo%20%E5%B0%88%E6%A1%88%E5%BF%AB%E9%80%9F%E5%85%A5%E9%96%80(2).md)
- [第 22 天 : 可觀測性摘要 cheetsheet 及小結](https://github.com/marcustung/Observability-in-DevOps/blob/main/22-%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%E6%91%98%E8%A6%81%20cheetsheet%20%E5%8F%8A%E5%B0%8F%E7%B5%90.md)
- [第 23 天 : 可觀測性工具的整合與挑戰](https://github.com/marcustung/Observability-in-DevOps/blob/main/23-%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%E5%B7%A5%E5%85%B7%E7%9A%84%E6%95%B4%E5%90%88%E8%88%87%E6%8C%91%E6%88%B0.md)
- [第 24 天 : Grafana Cloud : 雲端監控的未來](https://github.com/marcustung/Observability-in-DevOps/blob/main/24-Grafana%20Cloud%20%E9%9B%B2%E7%AB%AF%E7%9B%A3%E6%8E%A7%E7%9A%84%E6%9C%AA%E4%BE%86.md)
- [第 25 天 : Grafana Cloud : 蒐集應用程式遙測數據 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/25-Grafana%20Cloud%20%E8%92%90%E9%9B%86%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E9%81%99%E6%B8%AC%E6%95%B8%E6%93%9A%20(1).md)
- [第 26 天 : Grafana Cloud : 蒐集應用程式遙測數據 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/26-Grafana%20Cloud%20%E8%92%90%E9%9B%86%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E9%81%99%E6%B8%AC%E6%95%B8%E6%93%9A%20(2).md)
- [第 27 天 : Grafana Cloud : 使用 Grafana Pyroscope 優化應用程式性能](https://github.com/marcustung/Observability-in-DevOps/blob/main/27-Grafana%20Cloud%20%E4%BD%BF%E7%94%A8%20Grafana%20Pyroscope%20%E5%84%AA%E5%8C%96%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E6%80%A7%E8%83%BD.md)
- [第 28 天 : Grafana 事件管理 : 從告警到修復的問題排除流程 (1/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/28-Grafana%20%E4%BA%8B%E4%BB%B6%E7%AE%A1%E7%90%86%20%E5%BE%9E%E5%91%8A%E8%AD%A6%E5%88%B0%E4%BF%AE%E5%BE%A9%E7%9A%84%E5%95%8F%E9%A1%8C%E6%8E%92%E9%99%A4%E6%B5%81%E7%A8%8B%20(1).md)
- [第 29 天 : Grafana 事件管理 : 從告警到修復的問題排除流程 (2/2)](https://github.com/marcustung/Observability-in-DevOps/blob/main/29-Grafana%20%E4%BA%8B%E4%BB%B6%E7%AE%A1%E7%90%86%20%E5%BE%9E%E5%91%8A%E8%AD%A6%E5%88%B0%E4%BF%AE%E5%BE%A9%E7%9A%84%E5%95%8F%E9%A1%8C%E6%8E%92%E9%99%A4%E6%B5%81%E7%A8%8B%20(2).md)
- [第 30 天 : 從傳統開發到可觀測性驅動開發 (O.D.D)](https://github.com/marcustung/Observability-in-DevOps/blob/main/30-%E5%BE%9E%E5%82%B3%E7%B5%B1%E9%96%8B%E7%99%BC%E5%88%B0%E5%8F%AF%E8%A7%80%E6%B8%AC%E6%80%A7%E9%A9%85%E5%8B%95%E9%96%8B%E7%99%BC%20(ODD).md)
- [第 31 天 : 30 天之後 ?](https://github.com/marcustung/Observability-in-DevOps/blob/main/31-30%20%E5%A4%A9%E4%B9%8B%E5%BE%8C.md)

作者 : Marcus Tung

- [第 1~30 天 電子書下載](https://github.com/marcustung/Observability-in-DevOps/blob/main/Observability101.pdf)

## 2023 iThome 鐵人賽
- [鐵人賽文章 : Observability 101](https://ithelp.ithome.com.tw/users/20162577/ironman/6448) 
- [2023 鐵人賽得獎名單](https://ithelp.ithome.com.tw/2023ironman/reward)

## 關於我
- Linkedin : [MarcusTung](https://www.linkedin.com/in/marcus-tung-tw/)
- 部落格 : [m@rcus 的學習筆記](https://marcus116.blogspot.tw/)
- fb 粉絲團 : [m@rcus 的學習筆記](https://www.facebook.com/marcustung.tech)
- 鐵人檔案 : [Marcus (lumberjack)](https://ithelp.ithome.com.tw/users/20162577/ironman)
