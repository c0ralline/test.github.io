# apb转apb

- 实现src clk时钟域的apb请求转到dst clk时钟域
- 包含两组完整的apb接口,src clk域的master接口和dst clk域的slave接口
- 使用异步fifo实现跨时钟域

```verilog
module apb2apb(

//src apb
input src_clk,src_rst_n,
input [31:0] src_pwdata,
input [15:0] src_paddr,
input src_psel,src_pwrite,src_penable,
output [31:0] src_prdata,
output src_pslverr,src_pready,

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
wire dst2src_full,dst2src_enpty;

// rst sync

// src2dst ctrl
assign src2dst_wr_en = src_psel & src_penable ;
assign src2dst_rd_en = !src2dst_empty;

// dst2src ctrl
assign dst2src_wr_en =  ;
assign dst2src_rd_en =  ;

// fifo
async_fifo src2dst_fifo(
//write
    .wr_clk  (src_pclk),
    .wr_rst_n(src_rst_n_sync),
    .wr_en   (src2dst_wr_en),
    .data_in ({src_pwrite,src_paddr,src_pwdata}),
//read
    .rd_clk  (dst_pclk),
    .rd_rst_n(dst_rst_n_sync),
    .rd_en   (src2dst_rd_en),
    .data_out({dst_pwrite,dst_paddr,dst_pwdata}),
//control
    .full    (src2dst_full),
    .empty   (src2dst_empty)
);

async_fifo dst2src_fifo(
//write
    .wr_clk  (dst_pclk),
    .wr_rst_n(dst_rst_n_sync),
    .wr_en   (dst2src_wr_en),
    .data_in (dst_prdata),
//read
    .rd_clk  (src_pclk),
    .rd_rst_n(src_rst_n_sync),
    .rd_en   (dst2src_rd_en),
    .data_out(src_prdata),
//control
    .full    (dst2src_full),
    .empty   (dst2src_enpty)
);

endmodule

```
