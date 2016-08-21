## Botnana Control 入門

Botnana A2 預設於開機時自動啟動動程科技的 Botnana Control P2P 軸控軟體。此一軸控軟體安裝於 `/opt/mapacode/botnana-control`。

因此，使用瀏覽器連上 http://192.168.7.2:3000 。可見到以下畫面。

如果 Botnana A2 上未安裝 Botnana Control P2P，可以以下列方式安裝：

    dpkg -i botnana-control_0.0.1-1_armhf.deb

解安裝請執行

    dpkg -r botnana-control

### Configuration

請參考 [建構檔](./configuration-file.md) 章節，若無必要，請勿手動修改建構檔。
