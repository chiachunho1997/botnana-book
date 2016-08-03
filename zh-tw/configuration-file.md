## 建構檔

Botnana Control 使用 [Toml](https://github.com/toml-lang/toml) 格式作為其建構檔。檔案名稱為_motion.toml_。Botnana Control 支援不止是 EtherCAT 及 CANOpen，也要支援 CNC 控制器本身的需求，因此未使用 EhterCAT 的 ESI 檔或 CANopen 的 EDS 檔。

### [file] section

* _spec-version_ 欄位：列出使用的建構檔格式的版本，本文件目前的版本是 0.0.1。

### [[device]] section

可以有多個 devices，因此使用 `[[device]]`。
每個 device 有以下欄位。

* _protocol_ 欄位：本 Device 使用的 protocol，目前只支持 EtherCAT 以及 Local。Local 代表 master 上的裝置。

當 _protocol_ = "EtherCAT" 時，可以有以下欄位：

* _position_: Slave 的位置。
* _vendor-id_
* _vendor-name_
* _product-name_
* _product-code_
* _revision-no_
* _profile-no_: 指出裝置對應的 CANopen 的標準。目前只支援 401 和 402。

當 _profile-no_ = "402" 時，有以下欄位：

* _homing-method_
* _home-offset_
* _homing-speed-1_
* _homing_speed-2_
* _homing-acceleration_
* _profile-velocity_
* _profile-acceleration_
* _profile_deceleration_

### Example of a motion.toml

    [file]
      spec-version="0.0.1"
    [[device]]
      protocol = "EtherCAT"
      position = 1
      vendor-id = "#x1A05"
      vendor-name = "SYN-TEK Automation o., Ltd."
      product-name = "R1-EC5621 1-Axis Pulse Output Motoin Control Module"
      product-code = "#x00005621"
      revision-no = "#x00100101"
      profile-no="402"
      homing-method = 33
      home-offset = 0
      homing-speed_1 = 50
      homing-speed_2 = 5
      homing-acceleration = 8
      profile-velocity = 8000
      profile-acceleration = 9000
      profile-deceleration = 9000
