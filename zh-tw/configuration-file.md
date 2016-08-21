## 建構檔

Botnana Control 使用 [Toml](https://github.com/toml-lang/toml) 格式作為其建構檔。檔案名稱為 _motion.toml_。Botnana Control 的建構檔不止需要支 EtherCAT 及 CANopen，也要支援 CNC 控制器本身的需求，因此未使用 EtherCAT 的 ESI 檔或 CANopen 的 EDS 檔。

### file section

* _spec_version_ 欄位：列出使用的建構檔格式的版本，本文件目前的版本是 0.0.1。

### server section

* _address_ 欄位：motion server 的 websocket 位置。不寫時預設的位置為 192.168.7.2:3012。

### device section

可以有多個 devices，因此使用 `[[device]]`。每個 device 有以下欄位。

* _protocol_ 欄位：本 Device 使用的 protocol，目前只支持 EtherCAT。本欄位不寫時代表 EtherCAT 裝置。

當 _protocol_ = "EtherCAT" 時，可以有以下欄位：

* _position_: Slave 的位置。
* _vendor_id_
* _product_code_
* _homing_method_
* _home_offset_
* _homing_speed_1_
* _homing_speed_2_
* _homing_acceleration_
* _profile_velocity_
* _profile_acceleration_
* _profile_deceleration_

### Example of a motion.toml

    [file]
      spec_version = "0.0.1"
    [server]
      address = "192.168.7.2:3012"
    [[device]]
      protocol = "EtherCAT"
      position = 1
      vendor_id = 6661
      product_code = 22049
      homing_method = 33
      home_offset = 0
      homing_speed_1 = 50
      homing_speed_2 = 5
      homing_acceleration = 8
      profile_velocity = 8000
      profile_acceleration = 9000
      profile_deceleration = 9000
