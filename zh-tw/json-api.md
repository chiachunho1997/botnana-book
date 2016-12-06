# JSON API

程式可以使用 JSON 格式和 Botnana Control 溝通。此一方法適用於各種支援 JSON 格式且具有 Websocket 函式庫的語言，例如：

* Java
* C#
* C++
* Python
* Ruby
* Go

以下程式語言雖然支援 JSON，但建議使用 Botnana Control 另行提供的 APIs：

* [Javascript API](./javascript-api.md)

## Version API

程式可以使用 Version API 取得 Botnana Control 的版本。

    {
      spec_version: "0.0.1",
      target: "version",
      command: "get"
    }

會回傳以下 JSON：

    {
      version: "1.0.0"
    }

## Configuration API

程式可以使用 Configuration API 來處理參數設定檔。參數檔的設定，在重開機或重新讀取參數檔後生效。

### 修改設定參數

修改設定參數並不會立刻將設定值儲存至參數設定檔，也不會影響到各裝置目前使用的參數。

範例：修改 slave 1 的回歸原點方法。

    {
      spec_version: "0.0.1",
      target: "config",
      command: "set_slave",
      arguments: {
        position: 1,
        tag: "homing_method",
        value: 33
      }
    }

### 儲存設定參數

儲存設定參數會立刻將設定值儲存至參數設定檔，但不會影響到各裝置目前使用的參數。

關機再開後系統會使用新的設定。

範例：要求儲存 configuration：

    {
      spec_version: "0.0.1",
      target: "config",
      command: "save"
    }

## Slave API

### 設定馬達驅動器參數

和設定檔的 API 不同，此法設定的參數會立即生效。

馬達驅動器部份目前提供以下參數：

* `homing_method`
* `home_offset`
* `homing_speed_1`
* `homing_speed_2`
* `homing_acceleration`
* `profile_velocity`
* `profile_acceleration`
* `profile_deceleration`

使者用可以使用 set 命令設定這些參數。

    {
      spec_version: "0.0.1",
      target: "slave",
      command: "set",
      arguments: {
        position: 1,
        tag: "homing_method",
        value: 33
      }
    }

使用者可以使用 get 取得多筆參數。

    {
      spec_version: "0.0.1",
      target: "slave",
      command: "get",
      arguments: {
        position: 1,
        tags: ["homing_method", "home_offset"]
      }
    }

回傳資料範例為，

    {
      homing_method: 33,
      home_offset: 20
    }

### 設定及讀取 IO 點狀態

TODO

## Motion planning API

TODO

## Low-level Real-time Script API

Botnana Control 在其 real-time event loop 提供特殊的 Real-time script 來滿足更複雜的程式需求。以下為 Real-time script 設定 Slave 1 回歸原點方法的 JSON 命令。注意 `target` 為 `motion`。對 Realt-time script 更詳細的描述請見 [Real-time script API](./real-time-script-api.md)。
一般使用者並不需要使用此一 API。

    {
      spec_version: "0.0.1",
      target: "motion",
      command: "evaluate",
      arguments: {
        script: "1 33 homing-method!"        
      }
    }

Real-time script 若回傳資料，格式為

    {
      result: "tag1|value1|tag2|value2..."
    }

Realt-ime script 的指令集請見 [Real-time script API](./real-time-script-api.md)
