# 自建brook服务器教程

**2019.4.10：第一次发布自建brook服务器教程。这是一种新的翻墙方式，根据网友反馈，速度比v2ray和ssr要快！值得尝试。**

**自建brook教程很简单，整个教程分三步**：

第一步：购买VPS服务器

第二步：一键部署VPS服务器

第三步：一键加速VPS服务器

**【前言】**

Brook是一款新兴的代理软件，其版本横垮Windows、安卓、iOS、MacOS、Linux等多个系统平台，功能类似于我们经常使用的Shadowsocks/ShadowsocksR。Brook 的目标是简单易用、傻瓜化、速度快（新协议）。通过在服务器端安装Brook服务器端，同时在本地设备中使用Brook客户端，两者成功连接之后，可以为我们提供科学上网服务。如果你想在SS/SSR/V2ray之外，尝试一种新的代理软件，那么Brook是一个不错的选择！

**第一步：购买VPS服务器**

VPS服务器需要选择国外的，首选国际知名的vultr，速度不错、稳定且性价比高，按小时计费，能够随时开通和删除服务器，新服务器即是新ip。

vultr注册地址： [https://www.vultr.com/?ref=7887711-4F](https://www.vultr.com/?ref=7887711-4F) （vultr在2019年1月的最新活动，针对新用户，直接送50美元！全球15个服务器位置可选，kvm框架。）

虽然是英文界面，但是现在的浏览器都有网页翻译功能，鼠标点击右键，选择网页翻译即可翻译成中文。

注册并邮件激活账号，充值后即可购买服务器。充值方式是支付宝或paypal，使用paypal有银行卡（包括信用卡）即可。paypal注册地址：[https://www.paypal.com](https://www.paypal.com) （paypal是国际知名的第三方支付服务商，注册一下账号，绑定银行卡即可购买国外商品）

2.5美元/月的服务器配置信息：单核 512M内存 20G SSD硬盘 带宽峰值100M 500G流量/月 \(**仅提供ipv6 ip**\)

3.5美元/月的服务器配置信息：单核 512M内存 20G SSD硬盘 带宽峰值100M 500G流量/月 \(**推荐**\)

5美元/月的服务器配置信息： 单核 1G内存 25G SSD硬盘 带宽峰值100M 1000G流量/月 \(**推荐**\)

10美元/月的服务器配置信息： 单核 2G内存 40G SSD硬盘 带宽峰值100M 2000G流量/月

20美元/月的服务器配置信息： 2cpu 4G内存 60G SSD硬盘 带宽峰值100M 3000G流量/月

40美元/月的服务器配置信息： 4cpu 8G内存 100G SSD硬盘 带宽峰值100M 4000G流量/月

**注意：2.5美元套餐只提供ipv6，如果你用不了ipv6，那么你可以买3.5美元的套餐。另外，并非所有地区都有3.5美元的套餐，需要自己去看。由于资源的短缺，有的地区有时候有3.5美元的套餐，有时候没有。**

**vultr实际上是折算成小时来计费的，比如服务器是5美元1个月，那么每小时收费为5/30/24=0.0069美元 会自动从账号中扣费，只要保证账号有钱即可。如果你部署的服务器实测后速度不理想，你可以把它删掉（destroy），重新换个地区的服务器来部署，方便且实用。因为新的服务器就是新的ip，所以当ip被墙时这个方法很有用。当ip被墙时，为了保证新开的服务器ip和原先的ip不一样，先开新服务器，开好后再删除旧服务器即可。**

计费从你开通服务器开始算的，不管你有没有使用，即使服务器处于关机状态仍然会计费，如果你没有开通服务器就不算。比如你今天早上开通了服务器，但你有事情，晚上才部署，那么这段时间是会计费的。同理，如果你早上删掉服务器，第二天才开通新的服务器，那么这段时间是不会计费的。在账号的Billing选项里可以看到账户余额。

温馨提醒：同样的服务器位置，不同的宽带类型和地区所搭建的账号的翻墙速度会不同，这与中国电信、中国联通、中国移动国际出口带宽和线路不同有关，所以以实测为准。可以先选定一个服务器位置来按照教程进行搭建，熟悉搭建方法，当账号搭建完成并进行了bbr加速后，测试下速度自己是否满意，如果满意那就用这个服务器位置的服务器。如果速度不太满意，就一次性开几台不同的服务器位置的服务器，然后按照同样的方法来进行搭建并测试，选择最优的，之后把其它的服务器删掉，按小时计费测试成本可以忽略。

**账号充值如图**：

![](https://raw.githubusercontent.com/Alvin9999/pac2/master/pp100.png)

![](https://raw.githubusercontent.com/Alvin9999/pac2/master/pp101.png)

**开通服务器步骤如图**：

![](https://raw.githubusercontent.com/Alvin9999/crp_up/master/pac教程01.png)

![](https://raw.githubusercontent.com/Alvin9999/crp_up/master/pac教程02.png)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/Debian1.png)

## 一键部署brook脚本支持的系统有：CentOS 6+ / Debian 6+ / Ubuntu 14.04 +， 本教程演示选择系统Debian 8。

**开通服务器时，当出现了ip，不要立马去ping或者用xshell去连接，再等5分钟之后，有个缓冲时间。完成购买后，找到系统的密码记下来，部署服务器时需要用到。vps系统（推荐Debian 8）的密码获取方法如下图：**

![](https://raw.githubusercontent.com/Alvin9999/crp_up/master/pac教程05.png)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/Debian2.png)

**删掉服务器步骤如下图**：

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/de4.PNG)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/de2.PNG)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/de5.png)

一个被墙ip的vps被删掉后，其ip并不会消失，会随机分配给下一个在这个服务器位置新建服务器的人，这就是为什么开新服务器会有一定几率开到被墙的ip。被墙是指在国内地区无法ping通服务器，但在国外是可以ping通的，vultr是面向全球服务，如果这个被墙ip被国外的人开到了，它是可以被正常使用的，半年或1年后这个被墙的ip可能会被国内防火墙解封，那么这就是一个良性循环。

**第二步：部署VPS服务器**

购买服务器后，需要部署一下。因为你买的是虚拟东西，而且又远在国外，我们需要一个叫Xshell的软件来远程部署。Xshell windows版下载地址：

[国外云盘1下载](http://45.32.141.248:8000/f/d91974d046/)

[国外云盘2下载](https://nofile.io/f/eb5dUzYMQK4/Xshell_setup_wm.exe) 提取密码：666

[国外云盘3下载](https://www.adrive.com/public/NdK3Ez/Xshell_setup_wm.exe) 密码：123

如果你是苹果电脑操作系统，更简单，无需下载xshell，系统可以直接连接VPS。打开**终端**（Terminal），输入ssh root@ip 其中“ip”替换成你VPS的ip, 按回车键，然后复制粘贴密码，按回车键即可登录。粘贴密码时有可能不显示密码，但不影响， [参考设置方法](http://www.cnblogs.com/ghj1976/archive/2013/04/19/3030159.html) 如果不能用MAC自带的终端连接的话，直接网上搜“MAC连接SSH的软件”，有很多，然后通过软件来连接vps服务器就行，具体操作方式参考windows xshell。

部署教程：

下载windows xshell软件并安装后，打开软件

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/xshell11.png)

选择文件，新建

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/xshell12.png)

随便取个名字，然后把你的服务器ip填上

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/xshell13.png)

连接国外ip即服务器时，软件会先后提醒你输入用户名和密码，用户名默认都是root，密码是你购买的服务器系统的密码。

## 如果xshell连不上服务器，没有弹出让你输入用户名和密码的输入框，表明你开到的ip是一个被墙的ip，遇到这种情况，重新开新的服务器，直到能用xshell连上为止，耐心点哦！如果同一个地区开了多台服务器还是不行的话，可以换其它地区。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/xshell14.png)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/xshell2.png)

连接成功后，会出现如上图所示，之后就可以复制粘贴代码部署了。

## CentOS 6+ / Debian 6+ / Ubuntu 14.04 + brook一键部署管理脚本

wget -N --no-check-certificate wget -N --no-check-certificate [https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/brook.sh](https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/brook.sh)

chmod +x brook.sh

./brook.sh

> 如果提示 wget: command not found 的错误，这是你的系统精简的太干净了，wget都没有安装，所以需要安装wget。CentOS系统安装wget命令: yum install -y wget Debian/Ubuntu系统安装wget命令:apt-get install -y wget

———————————————————代码分割线————————————————

复制上面的**脚本一代码**到VPS服务器里，复制代码用鼠标右键的复制，然后在vps里面右键粘贴进去，因为ctrl+c和ctrl+v无效。接着按回车键，脚本会自动安装。以后只需要运行这个快捷命令就可以出现下图的界面进行设置，快捷管理命令为：bash brook.sh 或 ./brook.sh

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook1.PNG)

出现上图表明脚本安装成功，脚本安装成功后就可以输入快捷管理命令bash brook.sh 或 ./brook.sh 如果输入了快捷管理命令后，出现“Permission denied”字样，如下图，接着需要输入命令：chmod +x brook.sh 然后再输入快捷管理命令即可。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook2.PNG)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook3.PNG)

输入快捷管理命令后，出现上图，接着输入数字1来进行安装。会依次对**端口、密码、协议和版本号**进行选择。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook4.PNG)

端口输入1-65535之间的数字。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook5.PNG)

密码最好不要有特殊符号。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook6.PNG)

协议选择新版协议，输入1。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook7.PNG)

版本号输入v20190205

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook8.PNG)

最终安装成功后，会出现下面的界面。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brook9.PNG)

此脚本是开机自动启动，部署一次即可。最后可以重启服务器确保部署生效（一般情况不重启也可以）。重启需要在命令栏里输入reboot ，输入命令后稍微等待一会服务器就会自动重启，一般重启过程需要2～5分钟，重启过程中Xshell会自动断开连接，等VPS重启好后才可以用Xshell软件进行连接。如果部署过程中卡在某个位置超过10分钟，可以用xshell软件断开，然后重新连接你的ip，再复制代码进行部署。

**第三步：一键加速VPS服务器**

**【谷歌BBR加速教程】**

wget --no-check-certificate [https://github.com/teddysun/across/raw/master/bbr.sh](https://github.com/teddysun/across/raw/master/bbr.sh)

chmod +x bbr.sh

./bbr.sh

> 如果提示 wget: command not found 的错误，这是你的系统精简的太干净了，wget都没有安装，所以需要安装wget。CentOS系统安装wget命令: yum install -y wget Debian/Ubuntu系统安装wget命令:apt-get install -y wget

把上面整个代码复制后粘贴进去，不动的时候按回车，然后耐心等待，最后重启vps服务器即可。

演示开始，如图：

复制并粘贴代码后，按回车键确认

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/18.png)

如下图提示，按任意键继续部署

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/19.png)

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/20.png)

部署到上图这个位置的时候，等待3～6分钟

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/ss/21.png)

最后输入y重启服务器，如果输入y提示command not found ，接着输入reboot来重启服务器，确保加速生效，bbr加速脚本是开机自动启动，装一次就可以了。

服务器重启成功并重新连接服务器后，输入命令lsmod \| grep bbr 如果出现tcp\_bbr字样表示bbr已安装并启动成功。如图：

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/demo/tcp_bbr.PNG)

**注意1**：根据反馈，少部分人安装bbr脚本并重启后，几分钟过去了，发现xshell无法连接服务器且服务器ip无法ping通。解决方法是：开新服务器或者重装系统，然后先安装bbr脚本再安装brook脚本，或者改用锐速加速脚本。

**注意2**：如果创建的是centos7的服务器，安装bbr加速后需要使用命令关闭防火墙，可能无法使用代理。CentOS 7.0默认使用的是firewall作为防火墙。

查看防火墙状态命令：firewall-cmd --state

停止firewall命令：systemctl stop firewalld.service

禁止firewall开机启动命令：systemctl disable firewalld.service

【**Brook windows客户端下载及使用方法**】

第一步：下载Brook Tools，**本软件是一个辅助软件（可视化UI操作），他无法独立使用，需要配合 Brook Windows命令行版客户端使用**。

Brook Tools v1.0.8 [下载地址1](http://45.32.141.248:8000/f/0b46222653/) [下载地址2](http://108.61.224.82:8000/f/99a9b6d12a/)

第二步：下载Brook Windows命令行版客户端，地址：[https://github.com/txthinking/brook/releases](https://github.com/txthinking/brook/releases) ，如下图，windows32位系统选择第一个，64位系统选择第二个，下载后**重命名为**brook.exe

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brookwin1.png)

第三步：将下载好的Brook Tools压缩包解压出来，解压路径不要包含中文，将重命名好的Brook Windows命令行版客户端**放在同一个文件夹**。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brookwin2.png)

打开Brook Tools客户端，点击界面上的“浏览”后，打开同一文件夹的brook.exe ,之后点击“保存配置”、“启动”

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brookwin3.png)

Brook Tools客户端有2种代理方式，默认的是**http代理**，如果把前面的勾去掉，则变为**socks5代理**。

![](https://raw.githubusercontent.com/Alvin9999/PAC/master/brook/brookwin4.png)

浏览器代理相应设置为：如果是http代理，设置为HTTP 127.0.0.1 2080；如果是socks5代理，设置为socks5 127.0.0.1 2080；

**常见问题参考解决方法**：

1、账号无法使用，可能原因一：**客户端与服务端的设备系统时间相差过大。**

当vps服务器与本地设备系统时间相差过大，会导致客户端无法与服务端建立链接。请修改服务器时区，或者手动修改服务器系统时间（注意也要校准自己本地设备时间）！

修改vps时区为北京时区\(上海\)，输入命令：\cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 修改后重启vps服务器，然后再输入date命令检查下。

手动修改vps系统时间命令为（数字改为和自己电脑时间一致）：date -s "2018-11-02 19:14:00"

2、账号无法使用，可能原因二：**客户端与服务端版本不一致**

因为 Brook 每次更新的内容可能变动较大，所以如果客户端与服务端版本不一致，那么很有可能会导致客户端链接服务端被拒绝。包括Brook Tools 里调用的 Windows 命令行版客户端，所以请尝试更新服务端或客户端为最新版本。比如教程演示中服务端的版本是v20190205，那么下载的命令行版客户端也最好是v20190205

3、账号无法使用，可能原因三：**Windows 防火墙阻挡代理软件。**

目前发现 Windows 防火墙会阻挡代理软件对外建立的链接，所以需要关闭 Windows 自带的防火墙。

