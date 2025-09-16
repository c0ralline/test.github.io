# apb转apb

- 实现src clk时钟域的apb请求转到dst clk时钟域
- 包含两组完整的apb接口,src clk域的master接口和dst clk域的slave接口
- 使用异步fifo实现跨时钟域
- src_rst和dst_rst可同时复位两个fifo的两端
- 通过pready信号控制传输的节奏

```verilog
module apb2apb(

//src apb
input src_clk,src_rst_n,
input [31:0] src_pwdata,
input [15:0] src_paddr,
input src_psel,src_pwrite,src_penable,
output [31:0] src_prdata,
output src_pslverr,
output reg src_pready,

//dst apb
input dst_clk,dst_rst_n,
output [31:0] dst_pwdata,
output [15:0] dst_paddr,
output dst_psel,dst_pwrite,dst_penable,
input [31:0] dst_prdata,
input dst_pslverr,dst_pready

);

wire src2dst_wr_en,src2dst_rd_en;
wire dst2src_wr_en,dst2src_rd_en;
wire src2dst_full,src2dst_empty;
wire dst2src_full,dst2src_empty;

// rst sync
// any rst can reset both side of fifo
signal_synchronizer u_src_rst_n_sync(
    .clk(src_clk),
    .rst(src_rst_n&dst_rst_n),
    .rst_sync(src_rst_n_sync)
);

signal_synchronizer u_dst_rst_n_sync(
    .clk(dst_clk),
    .rst(src_rst_n&dst_rst_n),
    .rst_sync(dst_rst_n_sync)
);

// src2dst ctrl
// transmit paddr,pwdata,pwrite
assign src2dst_wr_en = src_psel && ~src_penable && ~src2dst_full ;//src domain setup stage
assign src2dst_rd_en = dst_pready && dst_penable;//dst domain

// dst2src ctrl
//transmit prdata
assign dst2src_wr_en = ~dst_pwrite && dst_pready && dst_penable ;//dst domain
assign dst2src_rd_en = ~src_pwrite && src_psel && src_penable && ~dst2src_empty ; //src domain

//src output
assign src_pslverr = 1'b0;
always@(posedge src_clk or negedge src_rst_n_sync)
    if(!src_rst_n_sync)
        src_pready<=1'b0;
    else if(src_pwrite)
        src_pready<=~src2dst_full;
    else if(!src_pwrite)
        src_pready<=~dst2src_empty;
    else
        src_pready<=1'b1;

//dst output
assign dst_psel = ~src2dst_empty;
always@(posedge dst_clk or negedge dst_rst_n_sync)
    if(~dst_rst_n_sync)
        dst_psel_d <= 1'b0;
    else
        dst_psel_d <= dst_psel;
assign dst_penable = dst_psel_d && dst_psel;

// fifo
async_fifo u_src2dst_fifo(
//write
    .wr_clk  (src_clk),
    .wr_rst_n(src_rst_n_sync),
    .wr_en   (src2dst_wr_en),
    .data_in ({src_pwrite,src_paddr,src_pwdata}),
//read
    .rd_clk  (dst_clk),
    .rd_rst_n(dst_rst_n_sync),
    .rd_en   (src2dst_rd_en),
    .data_out({dst_pwrite,dst_paddr,dst_pwdata}),
//control
    .full    (src2dst_full),
    .empty   (src2dst_empty)
);

async_fifo u_dst2src_fifo(
//write
    .wr_clk  (dst_clk),
    .wr_rst_n(dst_rst_n_sync),
    .wr_en   (dst2src_wr_en),
    .data_in (dst_prdata),
//read
    .rd_clk  (src_clk),
    .rd_rst_n(src_rst_n_sync),
    .rd_en   (dst2src_rd_en),
    .data_out(src_prdata),
//control
    .full    (dst2src_full),
    .empty   (dst2src_empty)
);

endmodule

```
