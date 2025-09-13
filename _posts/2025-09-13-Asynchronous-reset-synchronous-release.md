# 异步复位,同步释放

这是跨时钟域设计中，对复位reset信号的常见操作

当复位信号有效时(低有效则为0),直接复位，不做同步处理

当复位信号释放时(低有效则为1),用clk进行同步处理后，再释放

```verilog
wire clk, rst_n;
reg  rst_n_d, rst_n_sync;

always@(posedge clk or negedge rst_n)
    if(rst_n)begin
        rst_n_d<=1'b0;
        rst_n_sync<=1'b0;end
    else begin
        rst_n_d<=rst_n;
        rst_n_sync<=rst_n_d;end
```

