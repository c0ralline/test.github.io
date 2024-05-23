# ps
label:linux,ps

使用ps(process status)查看linux系统中现存的进程

1.直接输入ps，查看当前terminal提交的进程

```ps ```

2.查看进程详细信息

```ps -aux ```

-a：列出所有进程，等同于-e

关于进程状态：

``` 
S: Sleep 休眠中
R: Run 正在运行
Z：Zombie 僵尸进程
+：前台进程
l: 多线程进程
s：session leader，表示包含子进程
---
Ss：处于Sleep且为session leader的进程
S+：处于Sleep且在前台的进程
Sl+： 处于Sleep且在前台的多线程进程
R+： 正在运行的前天进程
Ssl： 处于Sleep且为session leader的多线程进程
```
