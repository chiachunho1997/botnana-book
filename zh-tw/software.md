# 軟體規格

霸蕉控制器出廠時預裝以下開源軟體，

* Debian Linux
* IgH EtherCAT Master for Linux
* nodejs

以及動程科技自行開發的

* Botnana Control P2P: 針對 EtherCAT 以及工業物聯網運用開發的多軸控制軟體。

## 軸控軟體

軸控軟體 Botnana Control 分三個等級。Botnana A2 出廠時預裝的是等級一的 Botnana Control P2P。

所有等級的 Botnana Control 都支援以下規格：

* EtherCAT 非同動軸可達 1-32 軸以上。
* 支援 Panasonic 及 Delta 的 EtherCAT 馬達驅動器，其他廠牌的驅動器正開發中。
* 支援 Delta 的類比及數位輸出入模組。未來將支援 Beckhoff 及其他廠牌的 EtherCAT 模組。

### 等級一：Botnana Control P2P，點到點軸控、監控

* 系統掃描與建構軟體，自動偵測 EtherCAT 從站。
* 軸控及監控軟體。支援 EtherCAT 馬達驅動器的 hm 及 pp 模式，可進行原點回歸及點到點運動。

### 等級二：Botnana Control Profiling：三軸同動軸控、監控

* 包含等級一 Botnana Control P2P 全部功能。
* Real-time extenstion (Xenomai)
* 支援 Modbus。
* 可進行三軸同動及直線圓弧補間。補間支援具 pp 模式的馬達驅動器。

### 等級三：Botnana Control CNC：CNC 控制器，三、四軸至六軸同動及多軸組控制器

* 包含等級二 Botnana Control Profiling 全部功能。
* 工件程式解譯器
* CNC 人機界面

並可依客戶需求特製四至六軸同動、多軸組、雷射及相機觸發等特殊控制器。
