# 异步fifo

```verilog
module async_fifo#(
    parameter FIFO_WIDTH = 16,
    parameter FIFO_DEPTH = 4
)(
    input  wr_clk,wr_rst_n,wr_en,
    input  rd_clk,rd_rst_n,rd_en,
    input  [FIFO_WIDTH-1:0] wr_data,
    output [FIFO_WIDTH-1:0] rd_data,

    output full,empty
);

localparam PNTR_WIDTH=$clog2(FIFO_DEPTH);
reg  [FIFO_WIDTH-1:0] fifo_mem [0:FIFO_DEPTH-1];

reg  [PNTR_WIDTH:0] wr_pntr;
reg  [PNTR_WIDTH:0] rd_pntr;
wire [PNTR_WIDTH:0] wr_pntr_grey;
wire [PNTR_WIDTH:0] rd_pntr_grey;
reg  [PNTR_WIDTH:0] wr_pntr_grey_d1, wr_pntr_grey_d1_sync;
reg  [PNTR_WIDTH:0] rd_pntr_grey_d1, rd_pntr_grey_d1_sync;

always @(posedge wr_clk) 
    if (wr_en && !wr_full) 
        fifo_mem[wr_pntr[PNTR_WIDTH-1:0]] <= wr_data;
assign rd_data = fifo_mem[rd_pntr[PNTR_WIDTH-1:0]];

always@(posedge wr_clk or negedge wr_rst_n)
    if(!wr_rst_n)
        wr_pntr <= {PNTR_WIDTH}1'b0;
    else if(wr_en && !full)
        wr_pntr <= wr_pntr + 1'b1;

always@(posedge rd_clk or negedge rd_rst_n)
    if(!rd_rst_n)
        rd_pntr <= {PNTR_WIDTH}1'b0;
    else if(rd_en && !empty)
        rd_pntr <= rd_pntr + 1'b1;

// empty
assign wr_pntr_grey = wr_pntr^(wr_pntr>>1);
always@(posedge rd_clk or negedge rd_rst_n)
    if(!rd_rst_n)
        wr_pntr_grey_d1 <= {PNTR_WIDTH}1'b0;
    else
        wr_pntr_grey_d1 <= wr_pntr_grey;
always@(posedge rd_clk or negedge rd_rst_n)
    if(!rd_rst_n)
        wr_pntr_grey_sync <= {PNTR_WIDTH}1'b0;
    else
        wr_pntr_grey_sync <= wr_pntr_grey_d1;
assign empty = wr_pntr_grey_sync == rd_pntr_grey;

// full
assign rd_pntr_grey = rd_pntr^(rd_pntr>>1);
always@(posedge wr_clk or negedge wr_rst_n)
    if(!wr_rst_n)
        rd_pntr_grey_d1 <= {PNTR_WIDTH}1'b0;
    else
        rd_pntr_grey_d1 <= rd_pntr_grey;
always@(posedge wr_clk or negedge wr_rst_n)
    if(!wr_rst_n)
        rd_pntr_grey_sync <= {PNTR_WIDTH}1'b0;
    else
        rd_pntr_grey_sync <= rd_pntr_grey_d1;
assign full = rd_pntr_grey_sync == {~wr_pntr_grey[PNTR_WIDTH:PNTR_WIDTH-1],wr_pntr_grey[PNTR_WIDTH-2:0]}

endmodule

```
