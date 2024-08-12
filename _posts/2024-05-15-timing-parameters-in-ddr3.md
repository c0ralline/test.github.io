# DDR3时序参数

```
读过程包括两部步：发送地址，读取数据
寻址过程包括 
①同时发出bank地址和row地址，称之为“ACTIVE”
②发送列地址和读命令，称为“READ”
·ACTIVE和READ两个命令的间隔就是tRCD，也就是发送行地址和列地址之间的间隔
·发送READ后tCL，获得DQ&DQS
·此时发送的DQ&DQS边沿几乎对齐的，不利于采样，在送到MC中后，会对DQS增加一定延时，使DQS的边沿和DQ数据中心对齐
```

|command|CKE|CS#|RAS#|CAS#|WE#|BA[2:0]|A[12:0]|
|---|---|---|---|---|---|---|---|
|ACTIVE|H|L|L|H|H|BA|ROW ADDRESS|
|READ|H|L|H|L|H|BA|COLUNM ADDRESS|

```
写过程与读过程类似
WRITE命令发出后tCWL，第一个数据DQ用DQS边沿采样进颗粒，此时的DQS已经和DQ中心对齐
```
|command|CKE|CS#|RAS#|CAS#|WE#|BA[2:0]|A[12:0]|
|---|---|---|---|---|---|---|---|
|ACTIVE|H|L|L|H|H|BA|ROW ADDRESS|
|WRITE|H|L|H|L|L|BA|COLUNM ADDRESS|

## CL (CAS [Column Address Strobe] Latency)

列地址选通延时，也就是读取延时，指memory controller发出读指令后，经过CL个周期，数据从DDR中读出，出现在传输线上。

在DDR3中通过MR0[6:4]对CL进行配置，可选范围为5~14个周期

## CWL (Column Write Latency)

列写入延时，也就是写入延时，指memory controller发出Write Command后，经过CWL个周期写入DDR中。

在DDR3中通过MR2[5:3]对CWL进行配置，可选范围为5~10个周期

## AL (Additive Latency)

附加延时，在DDR3中，通过设置MR1[4:3],AL可选配为三个值：0, CL-1, CL-2

## CWL&AL

总体写入延时WL=AL+CWL

## CL&CWL

CL和CWL影响tCK的配置，当ddr_speed增加时，为了保证数据的准确，CL和CWL通常也会随之增加。

---
```
refresh 刷新操作由mc发出，在总线上形成指令实现
· auto-refresh mc自动发出refresh命令
· 通过寄存器配置发送刷新指令
刷新的具体操作由颗粒自己完成
```
## tREFI (refresh interval time)

发送两个刷新命令的间隔时间，对于DDR，tREFI的典型值为7.8us，不受ddr_speed影响

两个刷新命令的最长间隔，是9个tREFI

## tREFW (refresh window)

在时间窗口内，必须把所有的行都刷新一次

## tREFC (refresh command)

执行一个刷新命令所需要的时间

## trefprd

refresh 操作所需时间周期
