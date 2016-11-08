title: 单片机EUSART串口通信
date: 2012/07/07 02:50
Tags: EUSART,串口通信,单片机,pic16f883,双向数据传输
category: muc,EUSART
---

在两个单片机之间建立串口通信。
说明：我们使用数码管显示接收到的数据，数据是对方的按键编号。


####硬件连接图：
<img src="http://static.oschina.net/uploads/space/2012/0707/023011_3lvH_185037.jpg">
<font color="red">有一点至关重要，就是两个单片机要共地。发送和接收引脚在两个单片机上交叉相连。</font>

这里采用的是异步发送和接收。


####发送原理图：
<img src="http://static.oschina.net/uploads/space/2012/0707/023450_K8gD_185037.png">


####EUSART接收图：
<img src="http://static.oschina.net/uploads/space/2012/0707/023526_qup7_185037.png">


####时序图:
<img src="http://static.oschina.net/uploads/space/2012/0707/023709_somN_185037.png">
<img src="http://static.oschina.net/uploads/space/2012/0707/023722_L6fH_185037.png">

####软件流程图：
<img src="http://static.oschina.net/uploads/space/2012/0707/024706_zhJY_185037.png">

####软件源码：

```c

#include<p16f883.inc>
 
    __CONFIG    _CONFIG1, _LVP_OFF & _FCMEN_ON & _IESO_OFF & _BOR_OFF & _CPD_OFF & _CP_OFF & _MCLRE_ON & _PWRTE_ON & _WDT_OFF & _INTRC_OSC_NOCLKOUT
    __CONFIG    _CONFIG2, _WRT_OFF & _BOR21V
 
udata_shr
counter res 1
counter1 res 1 ;扫描按键变量
counter2 res 1 ;延时程序微调参数
key_state res 1 ;按键状态
keynum res 1 ;按键标号
swap res 1 ;确认按键转换值
keypress res 1
keypressbak res 1
keyrelease res 1
LED1 res 1
LED2 res 1
LED3 res 1
LED4 res 1
 
    UDATA
counter3 res 1
counter4 res 1
sign res 1
 
 
reset code 0x0000
pagesel start
goto start
 
;int_vector code 0x0004
code
start
banksel ANSEL ;设置PORTA为数字模式
clrf ANSEL
banksel ANSELH ;设置PORTB为数字模式
clrf ANSELH
banksel TRISB ;设置PORTB为输入模式
movlw b'11111111'
movwf TRISB
banksel WPUB ;设置PORTB弱上拉
movlw b'11111111'
movwf WPUB
banksel OPTION_REG
movlw b'01000101' ;TMR0 64分频
movwf OPTION_REG
banksel T1CON
movlw b'10010001' ;打开TMR1，设置1:8预分频，内部时钟源1:4分频
movwf T1CON
banksel TRISA ;设置PORTA<3:0>为输出，接数码管的共阴极
movlw b'11100000'
movwf TRISA
banksel PORTA
clrf PORTA
banksel TRISC
movlw b'00000000' ;设置PORTC为输出，接8段数码管
movwf TRISC
clrf counter1
;***************************************
;串口发送配置
banksel TXSTA
movlw b'10100100'
movwf TXSTA
banksel BAUDCTL
movlw b'00000001'
movwf BAUDCTL
banksel SPBRG ;设置波特率
movlw d'25'
movwf SPBRG
banksel RCSTA
movlw b'10010000'
movwf RCSTA
 
;***************************************
loop
movlw HIGH Table1
movwf PCLATH
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
movf counter1,0
call Table1
banksel PORTB
movwf PORTB
movf PORTB,0
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
 
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto case1
 
incf counter1,1
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
movf counter1,0
call Table1
banksel PORTB
movwf PORTB
movf PORTB,0 ;读取I/O状态
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto case2
 
incf counter1,1
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
movf counter1,0
call Table1
banksel PORTB
movwf PORTB
movf PORTB,0 ;读取I/O状态
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto case3
 
incf counter1,1
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
movf counter1,0
call Table1
banksel PORTB
movwf PORTB
movf PORTB,0 ;读取I/O状态
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto case4
goto continue
 
case1
btfsc key_state,4
goto key2
movlw d'1'
movwf keynum
call DealKeyPress
goto continue
key2
btfsc key_state,2
goto key3
movlw d'2'
movwf keynum
call DealKeyPress
goto continue
key3
btfsc key_state,1
goto key4
movlw d'3'
movwf keynum
call DealKeyPress
goto continue
key4
btfsc key_state,0
goto continue
movlw d'4'
movwf keynum
call DealKeyPress
goto continue
 
 
 
 
case2
;-------------------------------------------------
;下面代码实现K10\K8\K5的按键处理
btfsc key_state,2
goto key8
movlw d'10'
movwf keynum
call DealKeyPress
goto continue
;------------------------------------
;处理K8
key8
btfsc key_state,1
goto key5
movlw d'8'
movwf keynum
call DealKeyPress
goto continue
;------------------------------------
;处理K5
key5
btfsc key_state,0
goto case3
movlw d'5'
movwf keynum
call DealKeyPress
goto continue
 
case3
;----------------------------------
;处理K6/K9
btfsc key_state,1
goto key6
movlw d'9'
movwf keynum
call DealKeyPress
goto continue
key6
btfsc key_state,0
goto case4
movlw d'6'
movwf keynum
call DealKeyPress
goto continue
case4
;-----------------------------------------
;处理K7
btfsc key_state,0
goto continue
movlw d'7'
movwf keynum
call DealKeyPress
 
continue
banksel PIR1
btfss PIR1,RCIF ;检测有无收到数据
goto lOOP2
call receive
lOOP2
call display
clrf counter1
goto loop
 
 
;-----------------------------------
;按键去抖，约8mS
delay
movlw d'4'
movwf counter2
LOOP2
banksel TMR0
clrf TMR0
LOOP1
banksel INTCON
btfss INTCON,T0IF
goto LOOP1
bcf INTCON,T0IF
decfsz counter2,1
goto LOOP2
return
 
delay2
incfsz counter3,1
goto delay2
return
 
;--------------------------------
;按键处理程序
;
DealKeyPress
clrf LED1
clrf LED2
clrf LED3
; clrf LED4
call delay
 
banksel TMR1H
clrf TMR1H
banksel TMR1L
clrf TMR1L
clrf keypress
presstime
banksel PIR1
btfss PIR1,TMR1IF
goto next
bcf PIR1,TMR1IF
incf keypress
movlw d'2'
subwf keypress,0
banksel STATUS
btfsc STATUS,C
goto longpress
next
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
banksel PORTB
movwf PORTB
movf PORTB,0 ;读取I/O状态
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto presstime
call delay
 
banksel TMR1H
clrf TMR1H
banksel TMR1L
clrf TMR1L
clrf keyrelease
releasetime
banksel PIR1
btfss PIR1,TMR1IF
goto next1
bcf PIR1,TMR1IF
incf keyrelease
movlw d'1'
subwf keyrelease,0
banksel STATUS
btfsc STATUS,C
goto click
next1
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
banksel PORTB
movwf PORTB
movf PORTB,0 ;读取I/O状态
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto over
goto releasetime
over
call delay
banksel TMR1H
clrf TMR1H
banksel TMR1L
clrf TMR1L
clrf keypress
presstime1
banksel PIR1
btfss PIR1,TMR1IF
goto next2
bcf PIR1,TMR1IF
incf keypress
movlw d'1'
subwf keypress,0
banksel STATUS
btfsc STATUS,C
goto click
next2
movf counter1,0
call Table1
banksel TRISB
movwf TRISB
banksel PORTB
movwf PORTB
movf PORTB,0 ;读取I/O状态
movwf key_state
movlw b'11001000'
iorwf key_state,1
movf counter1,0
call Table1
xorwf key_state,0
movwf swap
comf swap,1
incfsz swap,1
goto presstime1
movf keynum,0
movwf LED1
goto back
longpress ;长按计数清零
goto back
click
movf keynum,0
movwf LED1
back
;*********************************
;发送按键编号
banksel PIR1
lOOP
btfss PIR1,TXIF
goto lOOP
movf LED1,0
banksel TXREG
movwf TXREG
banksel TXSTA 
lOOP1
btfss TXSTA,TRMT
goto lOOP1
 
;*********************************
return
 
;--------------------------------
;显示数码管
;
display
banksel PORTA
movlw b'11111110'
movwf PORTA
movf LED1,0
call Table3
banksel PORTC
movwf PORTC
movwf counter
btfss counter,6
goto lOOP5
banksel PORTA
bsf PORTA,4
goto next4
lOOP5
banksel PORTA
bcf PORTA,4
next4
call delay2
 
banksel PORTA
movlw b'11110111'
movwf PORTA
movf LED4,0
call Table3
banksel PORTC
movwf PORTC
movwf counter
btfss counter,6
goto lOOP6
banksel PORTA
bsf PORTA,4
goto next5
lOOP6
banksel PORTA
bcf PORTA,4
next5
call delay2
return
 
receive
;*********************************
;接收按键编号
banksel RCREG
movf RCREG,0
movwf LED4
return
;*********************************
 
;----------------------
;Table真值表
;
Table1 ;PORTB、TRISB扫描配置信息
    ADDWF   PCL,f
    RETLW   B'11111111' 
    RETLW   B'11101111'
    RETLW   B'11111011'
    RETLW   B'11111101'

Table3 ;PORTC设置，数码管真值表
    ADDWF   PCL,f
    RETLW   B'00111111' ;0
    RETLW   B'00000110' ;1
    RETLW   B'01011011' ;2
    RETLW   B'01001111' ;3
    RETLW   B'01100110' ;4
    RETLW   B'01101101' ;5
    RETLW   B'01111101' ;6
    RETLW   B'00000111' ;7
    RETLW   B'01111111' ;8
    RETLW   B'01101111' ;9
    RETLW   B'00111111' ;0
end
```

按键检测前面有博文讲解，数码管程序比较简单。老师要求我们用中断实现，不过由于我们之前的实验总在中断那里出问题我们就没有用中断。
