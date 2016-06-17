# EtherCAT master

霸蕉控制器作為 EtherCAT 主站時，由以下軟體構成：

* Debian Linux
* IgH EtherCAT Master for Linux
* Botnana Control: 針對 EtherCAT 以及工業物聯網運用開發的多軸控制軟體。

## 軸控軟體

軸控軟體分以下數個等級。

### 基本軸控、監控

* 系統掃掃與建構軟體，自動偵測 EtherCAT 從站。
* 軸控及監控軟體。支援 EtherCAT 馬達驅動器的 hm 及 pp 模式，可進行原點回歸及點到點運動。
* 非同動軸可達 1-32 軸以上。
* 支援 Panasonic、Yaskawa、Sanyo Denki, Coply 及 Delta 的 EtherCAT 馬達驅動器。
* 支援 Beckhoff 及 Delta 的類比及數位輸出入模組。

### 三軸同動軸控、監控

* Real-time extenstion (Xenomai)
* 支援 EtherCAT 馬達驅動器的 csp 模式，可進行三軸同動及直線圓弧補間。

### 三軸 CNC 控制器及四至六軸同動及多軸組控制器

* 工件程式解譯器
* CNC 人機界面

並可依客戶需求特製四至六軸同動、多軸組、雷射及相機觸發等特殊控制器。
