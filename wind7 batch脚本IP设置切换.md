title: wind7批处理设置IP（BATCH脚本）
date: 2013-01-21 00:43
Tags: wind7, ip, 批处理
category: wind7,batch
---
说明：在x.x.x.x处填上自己的IP地址。保存成“.bat”文件，以管理员身份运行即可。 代码如下：

```batch

@echo off
:main
cls
echo 请按提示操作...
echo.
echo 1 STI-HUST
echo 2 Dian-HUST-711
echo 3 DHCP
echo 4 Exit
echo.
set /p choice= Input a number:
echo.
if "%choice%"=="1" goto ip_STI
if "%choice%"=="2" goto ip_Dian711
if "%choice%"=="3" goto ip_DHCP
if "%choice%"=="4" goto ip_Exit
goto main
:ip_STI
echo IP自动设置中...
echo.
echo 更新IP及子网掩码
netsh interface ip set address name="本地连接" source=static addr=x.x.x.x mask=x.x.x.x gateway=x.x.x.x gwmetric=1
echo 更新DNS服务器
netsh interface ip set dns name="本地连接" source=static addr=x.x.x.x register=PRIMARY
netsh interface ip add dns name="本地连接" addr=x.x.x.x
echo 设置完成
pause
exit
if errorlevel 2 goto main
if errorlevel 1 goto end

:ip_Dian711
echo IP自动设置中...
echo.
echo 更新IP及子网掩码
netsh interface ip set address name="本地连接" source=static addr=x.x.x.x mask=x.x.x.x gateway=x.x.x.x gwmetric=1
echo 更新DNS服务器
netsh interface ip set dns name="本地连接" source=static addr=x.x.x.x register=PRIMARY
netsh interface ip add dns name="本地连接" addr=x.x.x.x
echo 设置完成
pause
exit
if errorlevel 2 goto main
if errorlevel 1 goto end

:ip_DHCP
netsh interface ip set address name="本地连接" source=dhcp
netsh interface ip set dns name="本地连接" source=dhcp
netsh interface ip set wins name="本地连接" source=dhcp
echo 设置完成
pause
exit
if errorlevel 2 goto main
if errorlevel 1 goto end

:ip_Exit
exit

```
