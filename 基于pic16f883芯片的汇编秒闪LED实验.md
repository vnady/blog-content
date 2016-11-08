title: 基于pic16f883芯片的汇编秒闪LED实验
date: 2012/06/30 03:21
Tags: PIC16F883,LED,闪灯,汇编,定时器
category: MCU,singlechip
---
器材：Microchip公司的控制芯片pic16f883、LED灯、电阻、导线、程序下载器、代码编译仿真软件MPLABV8.8
###电路连接原理图：
<img src="http://static.oschina.net/uploads/space/2012/0630/025650_uZO3_185037.png">
###软件流程图：
<img src="http://static.oschina.net/uploads/space/2012/0630/031143_AvWA_185037.png">
本实验使用TIMER1模块 16位计数器溢出检测来完成周期性延时0.5S。
###数据计算
计算方法如下：
计数频率：4MHz * 1/4 * 1/8 = 1/8 MHz
则计数周期为：8uS
由于代码中设置每延迟0.5S，LED引脚电位取反来切换状态。所以要使计数器累计计数0.5S：
计数器次数= 0.5S/8uS = 62500次
TMR1是16为计数器，所以计数起始值设为65536-62500 = 3036 （0x0BDC）。这样子仿真LED闪烁周期为：1.000036 S

###微调&校准周期
于是将计数起点调整为0x0BDF，并且在循环里加入6个NOP（之前测试执行一条命令的周期为1uS），LED闪烁周期准确的为1 S。

实现代码如下：
```

#include<p16f883.inc>

UDATA_SHR
COUNTER1 RES 1
COUNTER2 RES 1
RESET_VECTOR CODE 0X0000
NOP
GOTO MAIN
MAIN
BANKSEL ANSEL                ;设置PORTA为数字信号I/O
CLRF ANSEL
BANKSEL TRISA
MOVLW B'00000000' ;PORTA全设为输出
MOVWF TRISA
BANKSEL T1CON
MOVLW B'10110101' ;配置timer1 ，1:8预分频（时钟信号进入TIMER1时有一个1:4分频）
MOVWF T1CON
BANKSEL OSCCON
MOVLW B'11101100' ;时钟4M
MOVWF OSCCON
Delay ;延时，等待时钟振荡器稳定
INCFSZ COUNTER1,1
GOTO Delay
INCFSZ COUNTER2,1
GOTO Delay
BANKSEL PORTA

LOOP
COMF PORTA,1 ;PORTA I/O取反
CALL LOOP1 ;计数器计数，延时
GOTO LOOP

LOOP1
BANKSEL TMR1H        ;TMR1设置计数初值
MOVLW 0X0B
MOVWF TMR1H
BANKSEL TMR1L
MOVLW 0XDF
MOVWF TMR1L
BANKSEL PIR1
LOOP2 ;TMR1溢出检测
BTFSS PIR1,0
GOTO LOOP2
BCF PIR1,0
NOP
NOP
NOP
NOP
NOP
NOP
RETURN
END

```
