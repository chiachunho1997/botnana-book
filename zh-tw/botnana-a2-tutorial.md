## Botnana A2 入門

--------------------
### 登入

使用 micro USB 連結到電腦的 USB port，再以 ssh 登入，

    ssh 192.168.7.2

使用者名稱為 debian，password 為 temppwd。

如果有必要切換使用者為 root，可執行

    sudo su

--------------------
### 7 個類比輸入 (ADC)

Botnana A2 在出廠時開放 AM3357 的七個類比輸入給使用者。若使用者有其他安排，可以參考 TI 的文件修改 Device tree。

[TI's Linux Core ADC User's Guide](http://processors.wiki.ti.com/index.php/Linux_Core_ADC_User's_Guide)

這七個 ADC 可以透過檔案系統存取，注意 ADC 可接受的電壓範圍為 0 - 1.8V。請勿使用超過 1.8V 的電壓：

    debian@arm:~$ ls -al /sys/bus/iio/devices/iio\:device0/
    total 0
    drwxr-xr-x 5 root root    0 Mar 26 06:19 .
    drwxr-xr-x 4 root root    0 Mar 26 06:19 ..
    drwxr-xr-x 2 root root    0 Mar 26 06:19 buffer
    -r--r--r-- 1 root root 4096 Mar 26 06:19 dev
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage0_raw
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage1_raw
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage2_raw
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage3_raw
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage4_raw
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage5_raw
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 in_voltage6_raw
    -r--r--r-- 1 root root 4096 Mar 26 06:19 name
    drwxr-xr-x 2 root root    0 Mar 26 06:19 power
    drwxr-xr-x 2 root root    0 Mar 26 06:19 scan_elements
    lrwxrwxrwx 1 root root    0 Mar 26 06:19 subsystem -> ../../../../../bus/iio
    -rw-r--r-- 1 root root 4096 Mar 26 06:19 uevent

TI AM3357 的 ADC 支援單擊模式 (One-shot Mode) 及連續模式 (Continuous Mode)。Botnana A2 出廠的預設為單擊模式。使用以下命令取得單擊模式下的類比輸入值。

    debian@arm:~$ cat /sys/bus/iio/devices/iio\:device0/in_voltage4_raw
    21

至於如何使用連續模式，請參考 [TI's Linux Core ADC User's Guide](http://processors.wiki.ti.com/index.php/Linux_Core_ADC_User's_Guide)。
