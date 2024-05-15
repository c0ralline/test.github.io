# DDR3时序参数

## CL (CAS [Column Address Strobe] Latency)

列地址选通延时，也就是读取延时，指memory controller发出读指令后，经过CL个周期，数据从DDR中读出，出现在传输线上。

## CWL (Column Write Latency)

列写入延时，也就是写入延时，指memory controller发出写地址后，经过CWL个周期写入DDR中。

## CL&CWL

CL和CWL影响tCK的配置，当ddr_speed增加时，为了保证数据的准确，CL和CWL通常也会随之增加。
