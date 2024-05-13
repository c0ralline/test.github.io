# verilog supply

supply可以在module内定义使用，也可以作为全局变量，跨module使用

作用全局

```
supply1 vdd;
supply0 vss;

module test (x, w, y);

inst inst_name( .VDD(vdd), .VSS(vss));

endmodule

```verilog

作用于module中

```
module test (x, w, y);

supply1 vdd;
supply0 vss;


inst inst_name( .VDD(vdd), .VSS(vss));

endmodule

```verilog
