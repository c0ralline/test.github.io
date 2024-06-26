tag: sta, lib, model

---

## model

静态时序分析中不同的模型，用于模拟不同标准单元的delay

### NLDM (non-linear-delay-model)

非线性延时模型

原理：

NLDM对cell的输入和输出分别建模，输入端定义为driver model，用电压源模拟，输出端定义为receiver model，用电容模拟。

通过查找表的方式，根据输入transition和输出负载capacities获得延时，对于未在二位查找表中出现的数据，采用线性插值的方式计算对应delay

应用：常见于65nm以上的工艺节点

### CCS (Composite Current Source)

复合电流源模型

原理： driver model为电流源模型，receiver model使用两个电容模拟

### NLDM & CCS

区别：
1. NLDM简单，快速; CCS复杂，精确
2. CCS模型中增加了噪声相关的参数

---
## Parameter

### timing derate

timing derate用于指定timing path的缩放因子

通常在worst-case下进行set-up时序检查，以验证最差情况下的data-path是否符合要求。

但wc下capture-clock-path也变差，难以检查到最差data-path，最好capure-clock-path的“真”最差情况。

所以对capure-clock-path增加一个小于1的derate，减小capure-clock-path-delay
