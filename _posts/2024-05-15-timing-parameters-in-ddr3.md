# DDR3时序参数

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
