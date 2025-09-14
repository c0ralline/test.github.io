# 异步复位,同步释放

这是跨时钟域设计中，对复位reset信号的常见操作

当复位信号有效时(低有效则为0),直接复位，不做同步处理

当复位信号释放时(低有效则为1),用clk进行同步处理后，再释放

### 纯异步复位：复位信号可以立即生效，不受时钟控制，确保系统在任何时候都能被复位

- 容易收到毛刺影响
- release时容易产生毛刺

### 同步复位：复位信号的撤销(释放)与时钟同步，避免亚稳态问题

大部分lib中的DFF是带异步复位的，同步复位可能产生更多逻辑，占用资源

### 异步复位,同步释放兼具异步复位和同步复位的优点：

 - 确保复位立即生效，提高系统可靠性

 - 避免复位释放时的亚稳态问题

### 异步复位,同步释放的实现

- 采用两级寄存器，消除亚稳态
- 第一级的寄存器输入直接连接释放时候的值（若reset低有效则为常值1）

```verilog
module reset_sync (
    input clk,
    input async_reset_n,
    output sync_reset_n
);
    reg rst_reg1, rst_reg2;
    
    always @(posedge clk or negedge async_reset_n) begin
        if (!async_reset_n) begin
            rst_reg1 <= 1'b0;
            rst_reg2 <= 1'b0;
        end else begin
            rst_reg1 <= 1'b1;
            rst_reg2 <= rst_reg1;
        end
    end
    
    assign sync_reset_n = rst_reg2;
endmodule
```






