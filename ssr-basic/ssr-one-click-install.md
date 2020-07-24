# SSR一键安装

搭建SSR的第一步，我们需要一台较为便宜的国外VPS服务器：

## 适合搭建SSR的VPS推荐

综合考虑性价比、使用方便程度、连接速度等几个方面，[Vultr](https://www.vultr.com/?ref=7236384)和[搬瓦工](https://bandwagonhost.com/aff.php?aff=19935)是搭建Shadowsocks不错的选择

#### 搬瓦工官方网站：

[![bwg](https://ssr.tools/wp-content/uploads/Banwagonhost.png)](https://bandwagonhost.com/aff.php?aff=19935)

#### Vultr官方网站：

Vultr （vultr在2019年1月的最新活动，针对新用户，直接送50美元！vultr全球15个服务器位置可选，KVM框架，最低2.5美元/月。支持支付宝和paypal付款。）

#### OneVPS官方网站

[https://www.onevps.com](https://www.onevps.com/portal/aff.php?aff=3257)

## SSR一键安装 （CentOS 7为例）

### 安装流程

1 登录 VPS服务器

2 登录成功后，依次输入一下三条命令：

```text
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

提示：如果输入第一条命令，出现找不到wget之类，说明系统没有预装wget，先运行以下命令安装wget：

```text
yum -y install wget
```

3 接下来有几个参数需要选择，依次为：

* 提示选择哪个版本安装，我们输入2后按回车，即选择SSR安装。

![&#x9009;&#x62E9;&#x7248;&#x672C;](https://ssr.tools/wp-content/uploads/2018-07-12_154217.jpg)

* 然后会提示设置SSR密码，输入自定义密码后按回车，建议不要使用默认密码。

![&#x8BBE;&#x7F6E;&#x5BC6;&#x7801;](https://ssr.tools/wp-content/uploads/2018-07-12_154332.jpg)

* 接下来选择SSR要使用的服务器端口，随便输入一个，也可以默认回车。

![&#x8BBE;&#x7F6E;&#x7AEF;&#x53E3;](https://ssr.tools/wp-content/uploads/2018-07-12_154422.jpg)

* 然后选择加密方式，如果选择chacha20的话，就输入对应序号12，按回车继续

![&#x8BBE;&#x7F6E;&#x52A0;&#x5BC6;&#x65B9;&#x5F0F;](https://ssr.tools/wp-content/uploads/2018-07-12_154549.jpg)

* 接下来选择协议，建议选择自auth\_aes128\_md5开始的以下的几种，输入对应序号按回车。

![&#x9009;&#x62E9;&#x534F;&#x8BAE;](https://ssr.tools/wp-content/uploads/2018-07-12_154640.jpg)

* 然后选择混淆方式，如下图所示，选择好后按回车。

![&#x6DF7;&#x6DC6;&#x65B9;&#x5F0F;](https://ssr.tools/wp-content/uploads/2018-07-12_154714.jpg)

* 以上参数选择完成后，按任意键开始安装。

![&#x5F00;&#x59CB;&#x5B89;&#x88C5;](https://ssr.tools/wp-content/uploads/2018-07-12_154823.jpg)

* 安装完成后，会有如下图安装成功的提示，记住刚才设置的各项参数，在客户端连接时需要用到

![&#x5B89;&#x88C5;&#x5B8C;&#x6210;](https://ssr.tools/wp-content/uploads/2018-07-12_155328.jpg)

* 经过以上几个简单的参数选择后，SSR服务器端已经自动安装成功了。保险期见，输入reboot重启VPS服务器，SSR会自动随系统重启

SSR服务端安装成功后，就可以在电脑、手机、路由器等设备的SSR客户端上，按照以上设置的各项参数进行连接了。

### SSR常用命令

```text
启动SSR：
/etc/init.d/shadowsocks-r start
退出SSR：
/etc/init.d/shadowsocks-r stop
重启SSR：
/etc/init.d/shadowsocks-r restart
SSR状态：
/etc/init.d/shadowsocks-r status
卸载SSR：
./shadowsocks-all.sh uninstall
```

另外如果需要更改SSR的相关配置参数，配置文件位置在：/etc/shadowsocks-r/config.json

#### 参考链接：

#### [SSR 一键安装脚本](https://ssr.tools/31)

