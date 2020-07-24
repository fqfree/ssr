# trojan一键安装

# trojan 一键安装教程
## 本文目录
trojan简介  

trojan安装

    准备事项  
    安装trojan服务端
    trojan服务端注意事项

trojan windows客户端使用教程

    运行trojan客户端  
    设置系统代理上网  
    借助v2rayN上网  

其他事项

## trojan简介

trojan是近两年兴起的网络工具，项目官网是 [https://github.com/trojan-gfw](https://github.com/trojan-gfw)，文档官网是[https://trojan-gfw.github.io/trojan](https://trojan-gfw.github.io/trojan)。与强调加密和混淆的SS/SSR等工具不同，trojan将通信流量伪装成互联网上最常见的https流量，从而有效防止流量被检测和干扰。在敏感时期，基本上只有trojan和 v2ray伪装 能提供稳如狗的体验。

与v2ray相比，trojan有如下特点：

* v2ray是一个网络框架，功能齐全；trojan只是一个绕过防火墙的工具，功能简单；
* v2ray和trojan都能实现https流量伪装；
* v2ray内核用go语言开发，trojan是c++实现，理论上trojan比v2ray性能更好；
* v2ray名气大，使用的人多，客户端很好用；trojan关注和使用的人少，客户端简陋。

本教程先介绍trojan服务端的安装部署，然后以windows系统为例讲解客户端使用。下载客户端请访问：trojan客户端下载。

## 安装trojan

### 准备事项

按照本教程安装trojan需要如下前提条件：

1. 有一台运行Linux的境外vps；购买vps可参考：一些VPS商家整理；

2. 有一个域名；购买域名可参考：Namesilo购买域名详细教程；

3. 为域名申请一个证书；可参考：使用Let’s Encrypt的免费证书；

4. 通过终端连接到vps；Windows系统请参考 Bitvise连接Linux服务器教程，mac用户请参考 Mac电脑连接Linux教程。

注意：根据trojan的原理，理论上可以无需域名，对ip自签发证书也能配置和使用（v2ray同理）。这种情况下相当于对流量做了TLS加密，不如有域名的稳（虽然也能用）。

### 安装trojan服务端

本教程服务端系统是CentOS 7，其他系统的命令基本类似，请自行转换。

连到VPS后，终端输入如下命令安装trojan：

`sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"`

该命令会下载最新版的trojan并安装。安装完毕后，trojan配置文件路径是 /usr/local/etc/trojan/config.json，其初始内容为：
```
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "password1",
        "password2"
    ],
    "log_level": 1,
    "ssl": {
        "cert": "/path/to/certificate.crt",
        "key": "/path/to/private.key",
        "key_password": "",
        "cipher": "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384",
        "cipher_tls13": "TLS_AES_128_GCM_SHA256:TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384",
        "prefer_server_cipher": true,
        "alpn": [
            "http/1.1"
        ],
        "reuse_session": true,
        "session_ticket": false,
        "session_timeout": 600,
        "plain_http_response": "",
        "curves": "",
        "dhparam": ""
    },
    "tcp": {
        "prefer_ipv4": false,
        "no_delay": true,
        "keep_alive": true,
        "reuse_port": false,
        "fast_open": false,
        "fast_open_qlen": 20
    },
    "mysql": {
        "enabled": false,
        "server_addr": "127.0.0.1",
        "server_port": 3306,
        "database": "trojan",
        "username": "trojan",
        "password": ""
    }
}

```

请重点关注配置文件中的如下参数：

* `local_port`：监听的端口，默认是443，除非端口被墙，不建议改成其他端口；
* `remote_addr`和`remote_port`：非trojan协议时，将请求转发处理的地址和端口。可以是任意有效的ip/域名和端口号，默认是本机和80端口；
* `password`：密码。需要几个密码就填几行，最后一行结尾不能有逗号；
* `cert`和`key`：域名的证书和密钥，Let’s Encrypt申请的证书可用 certbot certificates 查看证书路径；
* key_password：默认没有密码（如果证书文件有密码就要填上）；
* alpn：建议填两行：http/1.1和h2，保持默认也没有问题。

根据自己的需求修改配置文件（大部分参数保持默认即可），保存，然后设置开机启动：`systemctl enable trojan`，并启动trojan： `systemctl start trojan`。

检查trojan是否在运行：`ss -lp | grep trojan`，如果输出为空，可能的原因包括：

* config.json文件有语法错误：请注意是否少了逗号，有特殊字符等；
* 开启了selinux： setenforce 0关闭再启动 trojan。

软件运行没问题的话，最后一步是防火墙放行端口（如果开了防火墙的话）：

```
firewall-cmd --permanent --add-service=https # 端口是443
firewall-cmd --permanent --add-port=端口号/tcp # 其他端口号
firewall-cmd --reload # 重新加载防火墙
```

### trojan服务端注意事项

以下是一些注意事项：

1. 为了让伪装更正常，配置文件中的 remote_addr 和 remote_port 请认真填写。如果使用默认的 127.0.0.1 和 80，请运行以下命令安装Nginx并放行80端口：

```
yum install -y epel-release && yum install -y nginx
systemctl enable nginx; systemctl start nginx
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
```

完成后打开浏览器输入域名，应该出现Nginx欢迎页。更换伪装网站页面只需上传文件到 /usr/share/nginx/html 目录即可。

2. remote_addr 和 remote_port也可以填其他ip/域名和端口。例如将所有请求转发到本站，remote_addr 填 tlanyan.me，remote_port 填443。做大死的行为是remote_addr填 facebook/google/twitter等敏感域名，GFW过来一看可能就直接把你的ip安排得明明白白。

3. 如果vps网页后台有防火墙（阿里云/谷歌云/aws买的服务器），请记得放行相应端口。

到此服务端应该已经安装好并运行正常，接下来是配置客户端使用。

## trojan windows客户端使用教程

本节以windows系统为例，讲解trojan客户端的配置和使用。

### 运行trojan客户端

首先 [下载trojan客户端](https://tlanyan.me/trojan-clients-download/)，解压压缩包，进入trojan文件夹。用记事本打开 config.json 文件，做如下修改：

trojan客户端配置文件

![trojan客户端配置文件](https://tlanyan.me/trojan-tutorial/trojan%e5%ae%a2%e6%88%b7%e7%ab%af%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6-2/)

remote_addr可以设置成vps的ip，这时verify和verify_hostname需要设置成false

改好后保存并关闭文件，双击文件夹内的 trojan.exe 文件，trojan程序运行，出现如下黑窗口：

trojan运行窗口

![trojan运行窗口](https://tlanyan.me/trojan-tutorial/trojan%e8%bf%90%e8%a1%8c%e7%aa%97%e5%8f%a3/)

如果无法运行，先双击“VC_redist.x86.exe”安装依赖，然后再运行。

与SS/SSR/v2ray等客户端不同，trojan运行出现上述界面后，浏览器无法直接上外网，需要进行额外的设置。

本文介绍两种方式：

1. 设置系统代理；

2. 借助v2rayN。

### 设置系统代理上网

1. 打开windows设置 -> 网络和Internet -> 代理，出现如下界面：

windows系统代理设置

![windows系统代理设置](https://tlanyan.me/trojan-tutorial/windows%e7%b3%bb%e7%bb%9f%e4%bb%a3%e7%90%86%e8%ae%be%e7%bd%ae/)

2. 设置pac方式上网（推荐！）：关闭“自动检测设置”，打开“使用设置脚本”，在脚本地址一栏填入`https://tlanyan.me/trojan-pac.php?p=端口号`（端口号改成电脑上trojan配置文件中的local_port，例如1080），然后点击”保存”：

![windows设置pac](https://tlanyan.me/trojan-tutorial/windows%e8%ae%be%e7%bd%aepac/)


 
3. 如果你需要全局代理模式，请这样设置：关闭“自动检测设置”和“使用设置脚本”，打开“使用代理服务器”，地址填入127.0.0.1，端口填入trojan配置文件中的local_port，下面一栏填入以下内容：

```
localhost;127.*;10.*;172.16.*;172.17.*;172.18.*;172.19.*;172.20.*;172.21.*;172.22.*;172.23.*;172.24.*;172.25.*;172.26.*;172.27.*;172.28.*;172.29.*;172.30.*;172.31.*;172.32.*;192.168.*
```
windows系统设置全局代理

![windows系统设置全局代理](https://tlanyan.me/trojan-tutorial/windows%e7%b3%bb%e7%bb%9f%e8%ae%be%e7%bd%ae%e5%85%a8%e5%b1%80%e4%bb%a3%e7%90%86/)

然后点击保存。

无论哪种方式，配置正确的话应该都能正常上外网。

### 借助v2rayN上网

设置系统代理方式不能方便的切换pac和全局模式，本节介绍使用v2rayN客户端更方便的达到相同目的。

1. 首先从 v2ray客户端 [下载v2rayN](https://www.hijk.pw/v2ray-windows-client-download/)，然后解压进入v2rayN-Core文件夹。双击文件夹内的 v2rayN.exe 启动，在桌面右下角找到v2rayN的图标（logo是V），双击打开配置界面，按下图添加socks5服务器：

v2rayN添加socks服务器

![v2rayN添加socks服务器](https://tlanyan.me/trojan-tutorial/v2rayn%e6%b7%bb%e5%8a%a0socks%e6%9c%8d%e5%8a%a1%e5%99%a8/)

2. 在弹出来的配置界面分别填入 127.0.0.1 和设置的端口，别名随便填一个，比如 trojan，然后点击保存：

v2rayN设置服务器信息

![v2rayN设置服务器信息](https://tlanyan.me/trojan-tutorial/v2rayn%e8%ae%be%e7%bd%ae%e6%9c%8d%e5%8a%a1%e5%99%a8%e4%bf%a1%e6%81%af/)
3. 右下角找到v2rayN图标，点右键，在”Http代理”中选择PAC模式或者全局模式：

v2rayN切换代理模式

![v2rayN切换代理模式](https://tlanyan.me/trojan-tutorial/v2rayn%e5%88%87%e6%8d%a2%e4%bb%a3%e7%90%86%e6%a8%a1%e5%bc%8f/)

接下来，就可以愉快的上外网了。

## 其他事项

1. 可以用SwitchOmega等插件、浏览器设置代理等方式达到同样效果；

2. v2rayN界面的服务器列表栏点右键可以测试延迟，设置活动服务器等。

本教程到此结束，如有问题请留言。

#### 参考链接

1. [trojan – An unidentifiable mechanism that helps you bypass GFW.](https://trojan-gfw.github.io/trojan/overview)

2. [Trojan-GFW](https://github.com/trojan-gfw)

3. [网络上的HTTPS加密](https://transparencyreport.google.com/https/overview)

4. [trojan安装](https://tlanyan.me/trojan-tutorial/)
