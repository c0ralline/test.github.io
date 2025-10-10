```verilog
module clk_glitch_filter(
    input clk_in,
    input flt_en,
    output clk_out
);
    wire clk_in_d1,clk_in_d2,clk_in_d3,clk_in_d4,clk_in_d5,clk_in_d6,clk_in_d7,clk_in_d8,clk_in_d9;
    wire clk_in_nand3_1,clk_in_nand3_2;
    wire clk_in_nand3_1_inv,clk_in_nand3_1_buf;
    
    // 延迟链
    // nand(Z,A,B) Z=~(A&B)
    nand nand1(clk_in_d1, clk_in, flt_en);
    nand nand2(clk_in_d2, clk_in_d1, flt_en);
    nand nand3(clk_in_d3, clk_in_d2, flt_en);
    nand nand4(clk_in_d4, clk_in_d3, flt_en);
    nand nand5(clk_in_d5, clk_in_d4, flt_en);
    nand nand6(clk_in_d6, clk_in_d5, flt_en);
    nand nand7(clk_in_d7, clk_in_d6, flt_en);
    nand nand8(clk_in_d8, clk_in_d7, flt_en);
    nand nand9(clk_in_d9, clk_in_d8, flt_en);

    //nand
    nand nand3_1(clk_in_nand3_1, clk_in, clk_in_d4,clk_in_d8);
    nand nand3_2(clk_in_nand3_2, clk_in_d1, clk_in_d5,clk_in_d9);

    //inv
    not not1(clk_in_nand3_1_inv,clk_in_nand3_1);
    not not2(clk_in_nand3_1_buf,clk_in_nand3_1_inv);
    
    //rs触发器
    nand nand1_rs(rs_q,clk_in_nand3_1_buf,rs_qn);
    nand nand2_rs(rs_qn,clk_in_nand3_2,rs_q);

    //output
    not not3(clk_out,rs_q);
    

endmodule

```
