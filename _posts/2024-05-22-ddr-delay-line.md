# delay line in ddrphy
label: ddr

## 为什么需要delay line？

### vt tracking

在实际电路使用过程中，会伴随电压V和温度T的变化drift

温度T上升时， 电子无规则布朗运动加强，电子迁移率降低，delay增大

电压V上升时，晶体管驱动电流增大，delay减小

## delay 如何实现？

SDL (Standard cell Delay Line)

CDL (Custom cell Delay Line)

一个SDL unit由一个反相器和三个与非门构成

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/6b7b6835-a9b1-46e0-8e7a-b1bb34321734" />

当en=0时候，信号经过此SDL延时单元

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/f85c22e8-314f-4c68-9d36-486138624d88" />

当en=1时，信号在此SDL单元结束，返回SDL0

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/92a63994-0844-4ae2-9940-4834f5d0b797" />


