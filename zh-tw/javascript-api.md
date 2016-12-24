# Javascript API

使用 nodejs 時：

    var botnana = require("botnana");

`botnana` 函式庫亦可在瀏覽器執行。

## 範例

以下範例使用 Websocket 連上 Botnana Control 後取得 Botnana Control 的版本：

    var botnana = require("botnana");

    botnana.on("version", function(version) {
        console.log("version: " + version);
    })

    botnana.on("ready", function() {
        botnana.version.get();
    });

    // Start communicating with Botnana Control at 'ws://192.168.7.2:3012'.
    // If the programm is running on a Botnana Control board, IP address should
    // be 'ws://localhost:3012'.
    botnana.start('ws://192.168.7.2:3012');

## Start 和 Ready

程式使用 `botnana.start(ip_address)` 連上位於 `ip_address` 的 Botnana Control。Ready 事件代表已經連上並建立基本資料，
可以開始處理之後函式內的工作。

    botnana.on("ready", function() {
        // 工作 1
        // 工作 2
        // ...
    });

    botnana.start("ws://192.168.7.2:3012");

## Data Event API

Botnana Control 回傳資料的格式為

    tag1|value1|tag2|value2...

經過函式 `botnana.handle_response(response)` 處理後，tags 被轉成事件。可使用資料事件 API 處理這些事件。例如：

    botnana.on("version", function(version) {
        console.log("version: " + version);
    })
    botnana.on("log", function (log) {
        console.log("log: " + log);
    });
    botnana.on("error", function (err) {
        console.log("err: " + err);
    });
    botnana.on("homing_method.3", function (value) {
        console.log("Homing method of slave 3 is " + result);
    });
    botnana.on("dout.2.1", function (value) {
        console.log("dout 1 of slave 2 is " + value);
    });

## Version API

範例：

    botnana.on("version", function (version) {
        console.log("version: " + version);        
    });
    botnana.on("ready", function() {
        botnana.version.get();
    })

## Configuration API

程式可以使用 Configuration API 來處理 configuration 檔。

### 修改設定參數

修改設定參數並不會立刻將設定值儲存至參數設定檔，也不會影響到各裝置目前使用的參數。

範例：修改 configuration 檔中 slave 1 的回歸原點方法。

    botnana.config.set_slave({
      position: 1,
      tag: "homing_method",
      value: 33
    });

修改 configuration 內容並不會立刻儲存至設定檔，也不會影響到 EtherCAT slaves 目前使用的參數。

### 儲存設定參數

儲存設定參數會立刻將設定值儲存至參數設定檔，但不會影響到各裝置目前使用的參數。

關機再開後系統會使用新的設定。

範例：要求儲存 configuration：

    botnana.config.save();

## Slave API

### 讀取 Slave 狀態

函式 `get()` 可以用來取得 Slave 的狀態。例如

        botnana.ethercat.slave(1).get();

因為經過函式 `botnana.handle_response(response)` 處理後，回傳的資訊會產生對應的事件，
可以使用 Event API 處理這些回傳的資料。

範例：取得位於第一個 Slave 位置的馬達驅動器回原點的方式

    botnana.on("homing_method.1", function (homing_method) {
        console.log("result: " + homing_method);
    });
    botnana.on("ready", function() {
        botnana.ethercat.slave(1).get();
    });

### 設定馬達驅動器參數

設定馬達驅動器參數的命令格式為

    botnana.ethercat.slave(i).set(tag, value);

範例：設定馬達回原點的方式

    botnana.ethercat.slave(1).set("homgin_method", 33);

### 清除馬達驅動器異警

    botnana.ethercat.slave(i).reset_fault();

### 設定及讀取 IO 點狀態

範例：數位及類比 IO 的輸出及輸入：

    botnana.on("dout.1.5", function (value) {
        console.log("dout 5 of slave 1 is " + value);
    });
    botnana.on("din.2.4", function (value) {
        console.log("din 4 of slave 5 is " + value);
    });
    botnana.on("aout.3.2", function (value) {
        console.log("aout 2 of slave 3 is " + value);
    });
    botnana.on("ain.4.2", function (value) {
        console.log("ain 2 of slave 2 is " + value );
    });
    botnana.on("ready", function() {
        botnana.ethercat.slave(1).set_dout{1, 1);
        botnana.ethercat.slave(3).set_aout(1, 30);
        botnana.ethercat.slave(1).get();
        botnana.ethercat.slave(2).get();
        botnana.ethercat.slave(3).get();
        botnana.ethercat.slave(3).get();
    });

範例：某些 slave 的 Analog IO 必須要輸出致能：

    botnana.ethercat.slave(1).disable_aout(5);
    botnana.ethercat.slave(1).enable_aout(5);
    botnana.ethercat.slave(1).disable_ain(2);
    botnana.ethercat.slave(1).enable_ain(2);

## Low-level Real-time Script API

Botnana Control 在其 real-time event loop 提供特殊的 Real-time script 來滿足更複雜的程式需求。以下為使用 Real-time script 設定 Slave 1 回歸原點方法的 Javascript 命令。一般使用者並不需要使用此一 API。

    botnana.motion.evaluate("1 33 homing-method!");

Realt-ime script 的指令集請見 [Real-time script API](./real-time-script-api.md)

## Hidden API

Hidden API 是一群用於 Javascript API 內部實作用的函式，在未來很可能會改變作法，不建議一般使用者使用。

### 取得 slaves 資訊

    botnana._.get_slaves();

以下範例取得 Slave 總數

    botnana.on("slaves", function(slaves) {
        let s = slaves.split(",");
        console.log("slave counts: " + s.length/2);
    })
    botnana._.get_slaves();
