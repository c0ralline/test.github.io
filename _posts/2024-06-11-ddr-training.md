## zq-calibration

## write leveling

DDRPHY与DDR颗粒之间的连线可以分为两种： CK/ADDR/CMD 和 DATA/DQS

这两类连线在功能上存在差异
 - CK/ADDR/CMD 信号由DDRPHY发送给各个颗粒，不同颗粒使用同一组的CK/ADDR/CMD信号
 - DATA/DQS 信号在各个颗粒之间存在差距，每个颗粒和DDRPHY之间存在独立的数据线

导致两类连线在布线上存在差异
 - CK/ADDR/CMD采用fly-by布线
 - DATA/DQS采用星形布局

这就导致write过程中，CK/ADDR/CMD到达不同颗粒的时间有先后，而DATA/DQS到不同颗粒延时差距不大

通常表现为DQS早于CK的上升沿到达颗粒

因此，通过给DQS信号增加delay，使DQS与CK边沿对其
