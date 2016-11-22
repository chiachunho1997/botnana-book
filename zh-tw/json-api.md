# JSON API

程式可以使用 JSON 格式和 Botnana Control 溝通。此一方法適用於各種支援 JSON 格式且具有 Websocket 函式庫的語言，例如：

* Java
* C++ 可使用 JSON API 或 [C API](./c-api.md)
* Ruby

以下程式語言雖然支援 JSON，但建議使用 Botnana Control 另行提供的 API：

* [C API](./c-api.md)
* [Javascript API](./javascript-api.md)
* [Python API](./python-api.md)
* [Rust API](./rust-api.md)

## Configuration API

程式可以使用 Configuration API 來處理參數設定檔。

### 修改設定參數

修改設定參數並不會立刻將設定值儲存至參數設定檔，也不會影響到各裝置目前使用的參數。

範例：修改 slave 1 的回歸原點方法。

    {
      spec_version: "0.0.1",
      target: "config",
      command: "set_device",
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

## Forth Script API

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