# 数字IC设计流程中的工具

## vcs

## verdi

## spyglass

synopsys，用于评估RTL代码质量，检查语法规则

① spyglass lint

② spyglass cdc

## ccd 

**Conformal Constrain Designer**，cadence，用于验证时序约束的正确性，检查跨时钟域（CDC）信号是否正确

① ccd lint 检查rtl代码的语法是否符合规则

② ccd cdc 检查跨时钟域路径(cdc path)是否经过处理
主要包括三类检查
  - cdc_checks
  - clock_conv
  - reset

在清理cdc错误时，可通过两种方法：
  - 编辑静态信号列表
  - 编辑waive file

## lec

**Logic Equivalence Check**， cadence， 逻辑等效检查，相当于synopsys的formality 

形式验证不通过比较具体的激励和输出结果验证一致性，而是通过使用符号变量作为输入，解析输出表达式进行比较。

在项目后期进行ECO时，需要使用lec辅助验证rtl与netlist之间的一致性。

具体流程为： 
① 分别手动修改RTL,综合网表(syn netlist)和后端网表(pr netlist)

② 将三者两两比较，确定一致性
  - rtl **vs** syn netlist
  - rtl **vs** pr netlist
  - syn netlist **vs** pr netlist
