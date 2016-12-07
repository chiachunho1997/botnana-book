# Javascript API

使用 nodejs 時：

    var botnana = require("botnana");

`botnana` 函式庫亦可在瀏覽器執行。

## Version API

範例：

    botnana.on_version(function (version) {
        console.log("version: " + version);        
    });
    botnana.version.get();

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

修改 configuration 內容並不會立刻儲存至設定檔，也不會影響到 motion 目前使用的參數。

### 儲存設定參數

儲存設定參數會立刻將設定值儲存至參數設定檔，但不會影響到各裝置目前使用的參數。

關機再開後系統會使用新的設定。

範例：要求儲存 configuration：

    botnana.config.save();

## Event API

Botnana Control 回傳資料的格式為

    tag1|value1|tag2|value2...

透過內建的 handler 處理，這些 tags 被轉成各種事件。可使用事件的 API 來處理這些事件。例如：

    botnana.motion.on_log(function (log) {
        console.log("log: " + log);
    });
    botnana.motion.on_error(function (err) {
        console.log("err: " + err);
    });
    botnana.motion.slave(1).on_homing_method(function (value) {
        console.log("result: " + result);
    });
    botnana.motion.slave(1).on_dout(function (dout, value) {
        console.log("dout " + aout + ": " + value );
    });

## Slave API

### 設定馬達驅動器參數

範例：設定馬達回原點的方式

    botnana.motion.slave(1).set({
      tag: "homing_method",
      value: 33
    });

或

    botnana.motion.slave(1).set_homing_method{33);

範例：取得馬達回原點的方式

    botnana.motion.slave(1).on_homing_method(function (value) {
        console.log("result: " + result);
    });
    botnana.motion.slave(1).get("homing_method");

### 設定及讀取 IO 點狀態

範例：數位及類比 IO 的輸出及輸入：

    botnana.motion.slave(1).on_dout(function (dout, value) {
        console.log("dout " + aout + ": " + value );
    });
    botnana.motion.slave(1).on_din(function (din, value) {
        console.log("din " + ain + ": " + value );
    });
    botnana.motion.slave(1).on_aout(function (aout, value) {
        console.log("aout " + aout + ": " + value );
    });
    botnana.motion.slave(1).on_ain(function (ain, value) {
        console.log("ain " + ain + ": " + value );
    });
    botnana.motion.slave(1).set_dout{1, true);
    botnana.motion.slave(1).get_dout(1);
    botnana.motion.slave(1).get_din(1);
    botnana.motion.slave(1).set_aout(1, 30);
    botnana.motion.slave(1).get_aout(1);
    botnana.motion.slave(1).get_ain(1);

範例：某些 slave 的 Analog IO 必須要輸出致能：

    botnana.motion.slave(1).disable_aout(5);
    botnana.motion.slave(1).enable_aout(5);
    botnana.motion.slave(1).disable_ain(2);
    botnana.motion.slave(1).enable_ain(2);

## Low-level Real-time Script API

Botnana Control 在其 real-time event loop 提供特殊的 Real-time script 來滿足更複雜的程式需求。以下為使用 Real-time script 設定 Slave 1 回歸原點方法的 Javascript 命令。一般使用者並不需要使用此一 API。

    botnana.motion.evaluate("1 33 homing-method!");

Realt-ime script 的指令集請見 [Real-time script API](./real-time-script-api.md)
