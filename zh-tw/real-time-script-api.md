# Real-time Script API

Botnana Control 在其 real-time event loop 中使用了 Forth VM 以滿足更複雜的程式需求。透過 Forth 執行的命令會立刻影響裝置的行為。
一般使用者並不需要使用此一 API。

## 指令集

除了標準的 Forth 指令，Botnana Control 增加了以下 Forth 指令集。

### Host primitives

* `#din ( -- n )`	Digital input count 
* `#dout ( -- n )`	Digital output count 
* `dout@( n -- t=on )`	Read digital output
* `dout! ( t=on n -- )` Write digital output 
* `din@ ( n -- t=on )`	Read digital input 
* `time-msec ( -- n )`	Current time in milliseconds 

### EtherCAT queries

* `.slave ( n -- )`     Print information of slave n 
* `.slave-diff ( n -- )`	Print information difference of slave n 
* `list-slaves ( -- )`	Scan slaves 

### EtherCAT IO primitives

* `ec-dout@ ( channel n -- t=on )`	Get DOUT from EtherCAT slave n
* `ec-dout! ( t=on channel n -- )`	Set DOUT of EtherCAT slave n
* `ec-din@ ( channel n -- t=on )`	Get DIN from EtherCAT slave n 
* `-ec-aout ( channel n )`	Disable AOUT of EtherCAT slave n 
* `+ec-aout ( channel n )`	Enable AOUT of EtherCAT slave n 
* `ec-aout@ ( channel n -- value )`	Get AOUT from EtherCAT slave n 
* `ec-aout! ( value channel n -- )`	Set AOUT of EtherCAT slave n 
* `-ec-ain ( channel n )`	Disable AIN of EtherCAT slave n 
* `+ec-ain ( channel n )`	Enable AIN of EtherCAT slave n 
* `ec-ain@ ( channel n -- value )`	Get AIN from EtherCAT slave n 

### EtherCAT Drive primitives

* `op-mode! ( n mode -- )`	Set operation mode of slave n
* `pds-goal! ( goal n -- )`	Set PDS goal of slave n
* `reset-fault ( n -- )`	Reset fault for slave n
* `go ( n -- )`     Set point for slave n
* `jog ( n position -- )`	Jog slave n to position 
* `target-reached? ( n -- t=reached )`	Has slave n reached its target position?
* `home-offset! ( n offset -- )`	Set home offset of slave n 
* `homing-acceleration! ( n acceleration -- )`	Set homing acceleration of slave n 
* `homing-method! ( n method -- )`	Set homing method of slave n
* `homing-speed-1! ( n speed -- )`	Set homing speed 1 of slave n
* `homing-speed-2! ( n speed -- )`	Set homing speed 2 of slave n
* `profile-acceleration! ( n acceleration -- )`	Set profile acceleration of slave n
* `profile-deceleration! ( n deceleration -- )`	Set profile deceleration of slave n
* `profile-velocity! ( n velocity -- )`	Set profile velocity of slave n

### Internal testing primitives

* `tester-chkusb ( -- )`	Test USB memory stick 
* `tester-chkusd ( -- )`	Test microSD
* `-tester ( -- )`	Disable all tester outputs 
* `+tester ( -- )`	Enable all tester outputs, 
* `tester-high ( -- )`	Set all tester outputs to high 
* `tester-low ( -- )`	Set all tester outputs to 0V 
