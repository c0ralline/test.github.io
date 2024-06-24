tag：ddr，ddrphy

## 在DDRPHY设计中，PUB和PPC有什么异同？

PPC(programmable phy controller) 是DDRPHY的控制器，在PPC的控制下，DDRPHY可以完成
1. PHY initialization
2. DRAM initialization
3. training
4. Command

PPC一边以DFI接口和MC互连，一边以PPI接口和DDRPHY互连。

PPC中包含BUS MATRIX模块。

PUB(phy utility block)
