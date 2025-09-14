# 配置寄存器模块代码模板

- 采用apb接口配置寄存器
- 可配置可读可写寄存器(rw_reg)和只读寄存器(ro_reg)

```verilog
module config_register(

//apb ports
  input pclk,prst_n,
  input [31:0] pwdata,
  input [15:0] paddr,
  input psel,pwrite,penable,
  output [31:0] prdata,
  output pslverr,pready,

//rw_regs
  output cfg_rw_reg0,

//ro_regs
  input cfg_ro_reg0
);

reg rw_reg0;
localparam rw_reg0_addr=1;
localparam ro_reg0_addr=2;

//ctrl signal
assign pslverr=1'b0;
assign pready=1'b1;
assign cfg_rw_reg0=rw_reg0;

//write regs
always@(posedge pclk or negedge prst_n)
  if(!prst_n)
    rw_reg0<=1'b0;
  else if(psel && pwrite && penable && paddr==rw_reg0_addr)
    rw_reg0<=pwdata[0];

//read regs
always@(posedge pclk or negedge prst_n)
  if(!prst_n)
    prdata<=32'b0;
  else case(paddr)
    rw_reg0_addr: prdata<={31'b0,rw_reg0};
    ro_reg0_addr: prdata<={31'b0,cfg_ro_reg0};
  endcase


endmodule
```
