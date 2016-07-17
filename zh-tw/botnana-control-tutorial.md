## Botnana Control 入門

Botnana A2 預設於開機時自動啟動動程科技的 Botnana Control P2P 軸控軟體。此一軸控軟體安裝於 `/opt/mapacode/botnana-control`。

因此，使用瀏覽器連上 http://192.168.7.2:3000 。可見到以下畫面。


若因某種原因此一軟體未正常啟動，可以下列方式手動起動。

    sudo su

password: temppwd

執行 EtherCAT master:

    /etc/init.d/ethercat start
    
執行 Botnana Control 的 Motion Server：

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/etherlab/lib
    /opt/mapacode/botnana-control/bin/motion-server

執行 Botnana Control 的 Web Server：

    /opt/mapacode/botnana-control/bin/hmi
