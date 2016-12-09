# Javascript API

使用 nodejs 時：

    var botnana = require("botnana");

`botnana` 函式庫亦可在瀏覽器執行。

## 連線

以下範例使用 Websocket 連上 Botnana Control 後取得 Botnana Control 的版本：

    var WebSocket = require('ws');

    var ws = new WebSocket('ws://192.168.7.2:3012');
    // ws = new WebSocket('ws://localhost:3012');

    ws.on('message', function(data, flags) {
        console.log(data);
        botnana.handle_response(data);
    });

    ws.on('open', function () {
        botnana.version.get(ws);
    });

    botnana.on("version", function(version) {
        console.log("version: " + version);
    })


## Event API

Botnana Control 回傳資料的格式為

    tag1|value1|tag2|value2...

經過函式 `botnana.handle_response(response)` 處理後，tags 被轉成事件。可使用事件 API 處理這些事件。例如：

    botnana.on("version", function(version) {
        console.log("version: " + version);
    })
    botnana.motion.on("log", function (log) {
        console.log("log: " + log);
    });
    botnana.motion.on("error", function (err) {
        console.log("err: " + err);
    });
    botnana.motion.slave(1).on("homing_method", function (value) {
        console.log("result: " + result);
    });
    botnana.motion.slave(1).on("dout", function (dout, value) {
        console.log("dout " + aout + ": " + value );
    });

## Version API

範例：

    botnana.on("version", function (version) {
        console.log("version: " + version);        
    });
    botnana.version.get(ws);

## Configuration API

程式可以使用 Configuration API 來處理 configuration 檔。

### 修改設定參數

修改設定參數並不會立刻將設定值儲存至參數設定檔，也不會影響到各裝置目前使用的參數。

範例：修改 configuration 檔中 slave 1 的回歸原點方法。

    botnana.config.set_slave(ws, {
      position: 1,
      tag: "homing_method",
      value: 33
    });

修改 configuration 內容並不會立刻儲存至設定檔，也不會影響到 motion 目前使用的參數。

### 儲存設定參數

儲存設定參數會立刻將設定值儲存至參數設定檔，但不會影響到各裝置目前使用的參數。

關機再開後系統會使用新的設定。

範例：要求儲存 configuration：

    botnana.config.save(ws);

## Slave API

### 設定馬達驅動器參數

範例：設定馬達回原點的方式

    botnana.motion.slave(1).set(ws, {
      tag: "homing_method",
      value: 33
    });

或

    botnana.motion.slave(1).set_homing_method{ws, 33);

範例：取得馬達回原點的方式

    botnana.motion.slave(1).on("homing_method", function (value) {
        console.log("result: " + result);
    });
    botnana.motion.slave(1).get(ws, "homing_method");

### 清除馬達驅動器異警

    botnana.motion.slave(i).reset_fault(ws);

### 設定及讀取 IO 點狀態

範例：數位及類比 IO 的輸出及輸入：

    botnana.motion.slave(1).on("dout", function (dout, value) {
        console.log("dout " + aout + ": " + value );
    });
    botnana.motion.slave(1).on("din", function (din, value) {
        console.log("din " + ain + ": " + value );
    });
    botnana.motion.slave(1).on("aout", function (aout, value) {
        console.log("aout " + aout + ": " + value );
    });
    botnana.motion.slave(1).on("ain", function (ain, value) {
        console.log("ain " + ain + ": " + value );
    });
    botnana.motion.slave(1).set_dout{ws, 1, true);
    botnana.motion.slave(1).get_dout(ws, 1);
    botnana.motion.slave(1).get_din(ws, 1);
    botnana.motion.slave(1).set_aout(ws, 1, 30);
    botnana.motion.slave(1).get_aout(ws, 1);
    botnana.motion.slave(1).get_ain(ws, 1);

範例：某些 slave 的 Analog IO 必須要輸出致能：

    botnana.motion.slave(1).disable_aout(ws, 5);
    botnana.motion.slave(1).enable_aout(ws, 5);
    botnana.motion.slave(1).disable_ain(ws, 2);
    botnana.motion.slave(1).enable_ain(ws, 2);

## Low-level Real-time Script API

Botnana Control 在其 real-time event loop 提供特殊的 Real-time script 來滿足更複雜的程式需求。以下為使用 Real-time script 設定 Slave 1 回歸原點方法的 Javascript 命令。一般使用者並不需要使用此一 API。

    botnana.motion.evaluate(ws, "1 33 homing-method!");

Realt-ime script 的指令集請見 [Real-time script API](./real-time-script-api.md)
