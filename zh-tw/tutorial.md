# 入門教學 (Tutorial)
## Botnana Control

    sudo su

password: temppwd

Start the EtherLab EtherCAT master:

    sudo su
    /etc/init.d/ethercat start

EtherLab's is install under `/opt/etherlab`.

Environment variable LD_LIBRARY_PATH

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/etherlab/lib
    ./motion_server
