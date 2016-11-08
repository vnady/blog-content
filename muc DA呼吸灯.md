title: PIC16F883和TLC5615（DA）数模转化实验，呼吸灯
date: 2012/07/06 00:19
Tags: pic16f883,DAC,TLC5615,呼吸灯，数模转换
category: muc,DAC
---

器材：PIC16F883控制芯片、TLC5615、LED灯一个、电阻一个（1K）、PICkit2下载器
实验说明，本实验采用I/O模拟方法实现数模转换，转换结果输出到LED引脚。LED呈现呼吸似的缓慢闪烁。


####实验电路图：
<img src="http://static.oschina.net/uploads/space/2012/0706/000802_06G5_185037.png">
RC3连接SCLK、RC5连接Din、RC2连接CS。
输入DA的数字从0加到1024（DA为10位），再从1024减到0。中间最亮的时候做了一点儿延时。
LED灯接一个电阻到地，另一端接DA的模拟输出引脚。


####实验中比较重要的时序图：
<img src="http://static.oschina.net/uploads/space/2012/0706/001439_hNfo_185037.png">


根据这个时序图，我周期性的给引脚的电位置1和置0。

####源代码如下：
```c

#include<htc.h>
#define uchar unsigned char
#define uint unsigned int
#define CLK RC3
#define DATA_IN RC5
#define CS RC2
void delay(uint x)
{
    uint a,b;
    while(x--)
    {
        b = 0x00ff;
        while(b--)
        {
            a = 0x00ff;
            while(a--);
        }
    }
}
void main()
{
    uint i,temp,k=0,kp;
    TRISC=0x00;
    void delay(uint x);
    while(1)
    {
        i = 12;
        CS = 0;
        CLK = 0;
        kp = k;
        kp<<=4;
        while(i--)
        {
            temp=kp&0x8000;
            if(temp!=0)
            {
                DATA_IN = 1;
            }
            else
            {
                DATA_IN = 0;
            }       
            CLK  = 1;
            kp<< = 1;
            CLK = 0;
        }
        CS  = 1;
        CLK = 0;
        k++;
        if(k==0x0fff)
        {
            delay(1);
            while(k--)
            {
                i = 12;
                CS = 0;
                CLK = 0;
                kp = k;
                kp<<=4;
                while(i--)
                {
                    temp = kp&0x8000;
                    if(temp!=0)
                    {                         
                        DATA_IN = 1;
                    }                    
                    else 
                    {
                        DATA_IN = 0;
                    }                     
                    CLK  = 1;                     
                    kp<< = 1;                     
                    CLK = 0;                 
                }                                 
                CS  = 1;                 
                CLK = 0;             
            }                     
            k = 0;         
        }     
    } 
}
```
可以在每完成一个数据的输入后来一个延时来减慢LED灯的亮灭速度。看起来就是呼吸的效果。
