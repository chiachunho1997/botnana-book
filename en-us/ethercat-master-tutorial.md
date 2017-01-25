## EtherCAT Master Tutorial

### Advises for self-taught EtherCAT

_Botnana A2_ comes preinstalled with _IgH EtherCAT Master for Linux. But software development using EtherCAT master is not a simple task. Therefore, Botnana A2 comes with a built in solution _Botnana Control_.

Following tips and sites are for in case you do not want ot rely on _Botnana Control_, want to teach yourself EtherCAT, or even build your own contorl system.

* Read _IgH EtherCAT Master_ manual. The end of this chapter will provide a quick guide for using _IgH EtherCAT Master_.
* Understand what _CANopen_ is. Websites for reference: [CANopen Basics](http://www.canopensolutions.com/english/about_canopen/about_canopen.shtml), read up on _object dictionary_, _PDO_, and _SDO_ concepts.
* EtherCAT Master uses _CANopen over EtherCAT_ to conduct IO and motor/ drive controller. CANopen standard used are _DSP401_ and _DSP402_. The two documents can be found on the web. You can also download Copley or Elmo's motor drive manual for reference.
* When controlling motors, we recommend your motor drive's state machine. The state machine descrives everything from power suppply to operation enabled, and the _Control Word_.
* Then understand the several operation mode of your motor when it's operation enabled. The most basic ones are _pp_ and _hm_ mode. We recommend starting from _pp_ mode.
    * _pp_: Point to point motion. This mode does not require immediate OS support.
    * _hm_: Home seeking operation. 又依有沒有 limit switches 及回歸原點的方向分成多數子模式。This mode does not require real-time OS support.

Another way to learn is through Botnana Control. Botnana Control is expected to launch basic _Botnana Control P2P_, supporting _pp_ and _hm_ operation, in 2016-11-30. And in 2016-11-30, release an advanced version _Botnana Control Profiling_, supporting 即時作業系統及直線圓弧插補功能.

### Launch, check status, and stop EtherCAT Master
By default Botnana Control will automatically launch EtherCAT master. The following are steps for manual start and stop:

IgH EtherCAT Master is installed under `/opt/etherlab` directory.

Following are basic operations:

    service ethercat start
    service ethercat status
    service ethercat stop

If your wish to automatically launch EtherCAT master:

    update-rc.d ethercat defaults

###check the status of EtherCAT and slaves

TODO, Please check IgH EtherCAT master's manual.
