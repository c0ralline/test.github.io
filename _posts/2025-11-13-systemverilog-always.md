always,always_comb,always_ff,always_latch

always为verilog中最常使用的关键词之一，用法多样，搭配不同敏感列表和语法块，可被实现为组合逻辑，ff和latch

sv中为避免预期之外的实现方式，设立了always_comb,always_ff,always_lath三个新关键字

使用专用的always块可以提高代码可读性，减少错误，并让工具更好地进行逻辑检查和优化。

#### always_comb
```
1.不需要设置敏感列表
2.只允许使用阻塞赋值(=)
3.不允许生成锁存器
```
#### always_ff
```
1.时钟边沿敏感
2.只允许使用非阻塞赋值(<=)
```
#### always_latch
```
1.不需要设置敏感列表
2.只允许使用阻塞赋值(=)
```
