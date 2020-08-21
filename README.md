PS_multiExec
(old name is multiExec_from_PS.ps1)

==

PS_multExec.ps1 is a script that executes commands in parallel in multiple processes from PowerShell.


## System requirements

* Windows 7/8/10 or Windows Server 2008/2012/2016
* PowerShell 5.0 or later


## Execution method

1. List the command strings you want to execute in any text file (cmdList.txt). Please fill in one command per line.

```
PS D:\PS_multiExec> Get-Content .\cmdList.txt
ping www.google.com
ping www.yahoo.co.jp
ping www.amazon.com
ping www.nikkei.co.jp
snmpwalk -v 2c -c public 10.15.10.254 .1.3.6.1.2.1.2
snmpwalk -v 2c -c public 10.15.10.254 .1.3.6.1.2.1.2.2.1.7
snmpwalk -v 2c -c public 10.15.10.254 .1.3.6.1.2.1.2.2.1.8
snmpwalk -v 2c -c public 10.15.10.254 .1.3.6.1.2.1.2.2.1.9
PS D:\PS_multiExec>
```


2. Start PowerShell and execute PS_multExec.ps1 in the following format.

```
> ./PS_multExec.ps1 <cmdListFile> `
                    [-maxProcessCount <Count>] `
                    [-startInterval <interval>] `
                    [-checkInterval <interval>] `
                    [-retryCount <count>] `
                    [-exitWait <true|false>] `
                    [-logDir <dir>] `
                    [-enable_log_stdout <true|false>] `
                    [-enable_log_stderr <true|false>]
```

### Options

|Options|Explanation|
|-------|-----------|
cmdListFile|List the command strings file you want to execute.
-maxProcessCount \<Count\>|Maximum number of concurrently executing processes.<br>(default = 4)
-startInterval \<interval\>|Wait time for each command execution.<br>(default = 0[sec])
-checkInterval \<interval\>|Check interval for the number of running processes.<br>(default = 4[sec])
-retryCount \<count\>|Maximum number of processes to wait for next command execution.<br>(default = 4)
-exitWait \<true\|false\>|Wait for the completion of command execution and execute the next command.<br>(default = false)
-logDir \<dir\>|Specifying the log output destination directory of stdout/stderr.<br>(default = "./log_stdout_stderr")
-enable_log_stdout \<true\|false\>|Enable stdout log output destination.<br>(default = false)
-enable_log_stderr \<true\|false\>|Enable stderr log output destination.<br>(default = false)

## Execution command example

```
PS D:\PS_multExec> .\PS_multExec.ps1 .\cmdList.txt 4 -enable_log_stdout true -enable_log_stderr true
$cmdListFileName = .\cmdList.txt
$maxProcessCount = 4
$startInterval = 0
$checkInterval = 4
$retryCount = 4
$exitWait = False
$logDir = .\log_multiexec
$enable_log_stdout = True
$enable_log_stderr = True
[1] : ping www.google.com
.\log_multiexec\line_1_stdout.txt
.\log_multiexec\line_1_stderr.txt
PID =  11840
[2] : ping www.yahoo.co.jp
.\log_multiexec\line_2_stdout.txt
.\log_multiexec\line_2_stderr.txt
PID =  10164
[3] : ping www.amazon.com
.\log_multiexec\line_3_stdout.txt
.\log_multiexec\line_3_stderr.txt
PID =  13756
[4] : ping www.nikkei.co.jp
.\log_multiexec\line_4_stdout.txt
.\log_multiexec\line_4_stderr.txt
PID =  8740
[5] : snmpwalk -v 1 -c public 10.15.10.254 .1.3.6.1.2.1.2
.\log_multiexec\line_5_stdout.txt
.\log_multiexec\line_5_stderr.txt
PID =  14248
[6] : snmpwalk -v 1 -c public 10.15.10.254 .1.3.6.1.2.1.2.2.1.7
.\log_multiexec\line_6_stdout.txt
.\log_multiexec\line_6_stderr.txt
PID =  5840
[7] : snmpwalk -v 1 -c public 10.15.10.254 .1.3.6.1.2.1.2.2.1.8
.\log_multiexec\line_7_stdout.txt
.\log_multiexec\line_7_stderr.txt
PID =  3716
[8] : snmpwalk -v 1 -c public 10.15.10.254 .1.3.6.1.2.1.2.2.1.9
.\log_multiexec\line_8_stdout.txt
.\log_multiexec\line_8_stderr.txt
PID =  12936
PS_multExec.ps1 is Done...
PS D:\PS_multExec>
```

## stdout/stderr output result file example

```
PS D:\PS_multiExec> Get-ChildItem .\log_stdout_stderr\


    ディレクトリ: D:\PS_multiExec\log_stdout_stderr


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        2020/08/21     15:44              0 line_1_stderr.txt
-a----        2020/08/21     15:44            486 line_1_stdout.txt
-a----        2020/08/21     15:44              0 line_2_stderr.txt
-a----        2020/08/21     15:44            484 line_2_stdout.txt
-a----        2020/08/21     15:44              0 line_3_stderr.txt
-a----        2020/08/21     15:44            487 line_3_stdout.txt
-a----        2020/08/21     15:44              0 line_4_stderr.txt
-a----        2020/08/21     15:44            482 line_4_stdout.txt
-a----        2020/08/21     15:44              0 line_5_stderr.txt
-a----        2020/08/21     15:44          12200 line_5_stdout.txt
-a----        2020/08/21     15:44              0 line_6_stderr.txt
-a----        2020/08/21     15:44            682 line_6_stdout.txt
-a----        2020/08/21     15:44              0 line_7_stderr.txt
-a----        2020/08/21     15:44            685 line_7_stdout.txt
-a----        2020/08/21     15:44              0 line_8_stderr.txt
-a----        2020/08/21     15:44            878 line_8_stdout.txt


PS D:PS_Work\PS_multiExec>
```

```
PS D:\PS_multiExec> Get-Content .\log_stdout_stderr\line_1_stdout.txt

www.google.com [172.217.26.36]に ping を送信しています 32 バイトのデータ:
172.217.26.36 からの応答: バイト数 =32 時間 =20ms TTL=117
172.217.26.36 からの応答: バイト数 =32 時間 =22ms TTL=117
172.217.26.36 からの応答: バイト数 =32 時間 =21ms TTL=117
172.217.26.36 からの応答: バイト数 =32 時間 =23ms TTL=117

172.217.26.36 の ping 統計:
    パケット数: 送信 = 4、受信 = 4、損失 = 0 (0% の損失)、
ラウンド トリップの概算時間 (ミリ秒):
    最小 = 20ms、最大 = 23ms、平均 = 21ms
PS D:\PS_multiExec>
```

```
PS D:\PS_multiExec> Get-Content .\log_stdout_stderr\line_8_stdout.txt
IF-MIB::ifLastChange.1 = Timeticks: (7567) 0:01:15.67
IF-MIB::ifLastChange.52 = Timeticks: (7830) 0:01:18.30
IF-MIB::ifLastChange.99 = Timeticks: (8161) 0:01:21.61
IF-MIB::ifLastChange.10101 = Timeticks: (1015025) 2:49:10.25
IF-MIB::ifLastChange.10102 = Timeticks: (8077) 0:01:20.77
IF-MIB::ifLastChange.10103 = Timeticks: (1379245) 3:49:52.45
IF-MIB::ifLastChange.10104 = Timeticks: (8461) 0:01:24.61
IF-MIB::ifLastChange.10105 = Timeticks: (7777) 0:01:17.77
IF-MIB::ifLastChange.10106 = Timeticks: (7777) 0:01:17.77
IF-MIB::ifLastChange.10107 = Timeticks: (7777) 0:01:17.77
IF-MIB::ifLastChange.10108 = Timeticks: (7777) 0:01:17.77
IF-MIB::ifLastChange.10109 = Timeticks: (7777) 0:01:17.77
IF-MIB::ifLastChange.10110 = Timeticks: (7778) 0:01:17.78
IF-MIB::ifLastChange.10501 = Timeticks: (0) 0:00:00.00
IF-MIB::ifLastChange.20567 = Timeticks: (7649) 0:01:16.49
PS D:\PS_multiExec>
```

```
PS D:\PS_multiExec> Get-Content .\log_stdout_stderr\line_8_stderr.txt
PS D:\PS_multiExec>
```


## LICENSE

[MIT](https://github.com/NobuyukiInoue/multiExec_from_PowerShell/blob/master/LICENSE)
