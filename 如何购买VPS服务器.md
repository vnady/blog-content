title: 如何购买Digitalocean的 VPS 主机
date: 2014-08-30
category: vps

开始就决定选择KVM完全虚拟机，准备搭建一个VPN爬墙，然后在上面建一些网站来积累技术。

####一、查论坛评论、测评：找出三个比较合适的（便宜、快、有优惠） 
> 对比  [<font color="blue">digitalocean</font>](https://www.digitalocean.com)  [<font color="blue">ramnode</font>](https://www.ramnode.com)  [<font color="blue">buyvm</font>](http://www.buyvm.net)  的价格和配置:   
> $5/m 512MB-Memory 1core-Processor 20GB-SSD Disk 1TB-Transfer（目前未统计流量，超了也不扣钱）   
> www.ramnode.com   
> 5$/m 256MB 1core 20GB-SSD 1000GB-Transfer 或 100Mbps   
> www.buyvm.net   
> 3.6$/m 256MB 1core 30GB-SSD 1000GB-Transfer   

####二、ping 服务器IP、下载100M文件测试 
> 综合价格和性能，最终决定选择 digitalocean

####三、注册购买 digitalocean 云主机
> ![ERROR!](http://static.oschina.net/uploads/space/2014/0824/235820_APVl_185037.png)
> digitalocean云主机是按小时计费的，会从你的账户里实时扣费。每小时0.007美元。 
> 选择服务器类型、镜像版本、服务器地域（建议国内用旧金山的），Create Droplet之后会受到一封写着 ip 和 root 密码的Email。可以开始折腾咯！

> 可以进入web模拟终端，建议使用putty SSH远程登录服务来操作。

* 购买链接，新用户注册即可获得10美元账户余额（等于免费使用两个月）.
[<font color="blue">https://www.digitalocean.com/?refcode=14b3eb194704</font>](https://www.digitalocean.com/?refcode=14b3eb194704 "优惠链接") 
