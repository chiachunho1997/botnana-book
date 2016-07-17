## NodeJS 入門

Botnana 使用 Javascript 做為其控制語言。執行環境則為 [NodeJS](https://nodejs.org/)。採用 Javascript 及 NodeJS 的理由如下：

* 人才：Javascript 是瀏覽器的主要程式語言。互聯網的發展為社會培養了大量的 Javascript 工程師。
* 函式庫：大量的 Javascript 函式庫在網上隨手可得，能加速應用程式的開發。
* 性能：Google, Microsoft 和 Mozilla 在瀏覽器上的競爭，使得 Javascript 成為性能最優越的互動式程式語言。
* 工業物聯網：NodeJS 使得 Javascript 成為唯一可在瀏覽器及伺服器執行的程式語言。也使得 Javascript 成為整合雲端及設備端的工業物聯網的當然選擇。

執行 nodejs：

    debian@arm:~$ node
    >

按 `Ctrl-D` 可離開 nodejs 環境。

以下 nodejs 程式取得並印出類比輸入 AIN0 的值。
    
    var fs = require('fs');

    fs.readFile('/sys/bus/iio/devices/iio:device0/in_voltage0_raw', function (err, data) {
        if (err) throw err;
        data.toString('ascii');
        var value = parseInt(data);
        console.log(value);
    });

將以上程式存於檔案 `test.js` 中，並以下列方式執行，或直接在 nodejs 的互動式執行環境中執行：

    debian@arm:~$ node test.js
    21

