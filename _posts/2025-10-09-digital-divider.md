数字可配置分频器

```verilog
module clk_divider(
    input clk_in,
    input scan_mode,
    input div_rst_n,
    input cnt_start,

    input       pcfg_cnt_start_sync_en,
    input       pcfg_div_rst_n_sync_en,
    input [3:0] pcfg_cnt_start_pos_sel,
    input [5:0] pcfg_cnt_num,
    input [5:0] pcfg_cnt_step,
    input [5:0] pcfg_cnt_out_h,
    input [5:0] pcfg_cnt_out_l,
    input [5:0] pcfg_cnt_mask_h,
    input [5:0] pcfg_cnt_mask_l,

    output clk_div_out
);

wire div_rst_n_sync_pre;
wire cnt_rst_n;
wire cnt_start_sync;
wire cnt_start_mux;
reg [7:0] cnt_start_pos_pipe;


always@(posedge clk_in )

endmodule

```
