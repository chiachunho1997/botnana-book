# Javascript API

使用 nodejs 時：

    var botnana = require("botnana");

`botnana` 函式庫亦可在瀏覽器執行。

## Version API

範例：

    function callback(result, err) {
        if(err) {
            console.log(err);
        } else {
            console.log("result: " + result);
        }
    }
    botnana.version.get(callback);

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

    botnana.motion.slave(1).get("homing_method", function (result, err) {
        if(err) {
            console.log(err);
        } else {
            console.log("result: " + result);
        }
    });
 
 範例：馬達的參數眾多，可以以下法一次存取多個參數，以避免 callback hell：

     botnana.motion.slave(1).get(["homing_method", "home_offset"], function (result, err) {
         if(err) {
             console.log(err);
         } else {
             console.log("homing_method: " + result.homing_method);
             console.log("home_offset: " + result.home_offset);
         }
     });

### 設定及讀取 IO 點狀態

範例：數位及類比 IO 的輸出及輸入：

    function callback(result, err) {
        if(err) {
            console.log(err);
        } else {
            console.log("value: " + result)
        }
    }

    botnana.motion.slave(1).set_dout{1, true);
    botnana.motion.slave(1).get_dout(1, callback);
    botnana.motion.slave(1).get_din(1, callback);
    botnana.motion.slave(1).set_aout(1, 30);
    botnana.motion.slave(1).get_aout(1, callback);
    botnana.motion.slave(1).get_ain(1, callback);

 範例：可以以下法一次存取多個 IO 點，以避免 callback hell：

     botnana.motion.slave(1).get_dout([1, 3, 4], function (result, err) {
         if(err) {
             console.log(err);
         } else {
             console.log("Digital output 1: " + result[1]);
             console.log("Digital output 3: " + result[3]);
             console.log("Digital output 4: " + result[4]);
         }
     });

範例：某些 slave 的 Analog IO 必須要輸出致能：

    botnana.motion.slave(1).disable_aout(5);
    botnana.motion.slave(1).enable_aout(5);
    botnana.motion.slave(1).disable_ain(2);
    botnana.motion.slave(1).enable_ain(2);
