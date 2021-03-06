# 魔改BBR一键安装脚本

## BBR介绍

BBR是Google出品的TCP拥塞控制算法，目前集成在最新的Linux内核中。国外VPS服务器上安装BBR后，可以明显提高连接速度，降低丢包。 BBR对SS/SSR有明显的加速作用，看Youbube视频时更为明显。另外如果在国外VPS服务器上架设网站，BBR也可以加速网站的加载速度。

魔改版BBR，则是在原版BBR基础上的修改版本，通过参数的修改，使加速算法更为激进，比原版BBR有更为明显的加速效果。

## BBR安装要求

系统要求：CentOS+、Debian7+、Ubuntu12+。

另外不支持OpenVZ虚拟的VPS服务器，这也是我们不建议购买OpenVZ服务器的原因之一。

## 魔改BBR一键安装

1.用Putty连接VPS服务器，输入以下命令运行：

```text
wget --no-check-certificate https://raw.githubusercontent.com/tcp-nanqinlang/general/master/General/CentOS/bash/tcp_nanqinlang-1.3.2.sh
bash tcp_nanqinlang-1.3.2.sh
```

小提示：如果运行命令时英文提示找不到wget的错误，那么先运行以下命令安装wget：

```text
yum -y install wget
```

2.出现下图提示时，输入数字1选择安装内核，然后回车：

![](https://ssr.tools/wp-content/uploads/2018-11-29_183122.jpg)

3.接下来的安装过程中，部分系统可能会有如下提示，提示删除旧的内核，是否取消。

这时按方向右键， 选择No后回车，确认删除。

```text
sysctl net.ipv4.tcp_congestion_control
```

![](https://ssr.tools/wp-content/uploads/2018-11-29_185659.jpg)

4.出现如下提示后，输入reboot回车重启系统：

![](https://ssr.tools/wp-content/uploads/2018-11-29_185806.jpg)

5.系统重启完成后，重新Putty连接，输入以下命令重新运行脚本：

`bash tcp_nanqinlang-1.3.2.sh`

6.出现如下图提示后，输入2选择安装并开启算法：

![](https://ssr.tools/wp-content/uploads/2018-11-29_183122.jpg)

7.稍等片刻，安装成功后的提示如下图：

![](https://ssr.tools/wp-content/uploads/2018-11-29_190039.jpg)

如果报错：

```text
make -C /lib/modules/`uname -r`/build M=`pwd` modules CC=/usr/bin/gcc
./scripts/gcc-version.sh:行25: /usr/bin/gcc: 没有那个文件或目录
./scripts/gcc-version.sh:行26: /usr/bin/gcc: 没有那个文件或目录
make[1]: /usr/bin/gcc：命令未找到
make[1]: 进入目录“/usr/src/kernels/4.12.10-1.el7.elrepo.x86_64”
make[1]: /usr/bin/gcc：命令未找到
make[1]: /usr/bin/gcc：命令未找到
make[1]: /usr/bin/gcc：命令未找到
make[1]: /usr/bin/gcc：命令未找到
  CC [M]  /home/tcp_nanqinlang/tcp_nanqinlang.o
/bin/sh: /usr/bin/gcc: 没有那个文件或目录
make[2]: *** [/home/tcp_nanqinlang/tcp_nanqinlang.o] 错误 1
make[1]: *** [_module_/home/tcp_nanqinlang] 错误 2
make[1]: 离开目录“/usr/src/kernels/4.12.10-1.el7.elrepo.x86_64”
make: *** [all] 错误 2
[Error] load mod failed, please check!
```

原因：没有安装gcc

安装gcc：

`yum install gcc`

## 魔改BBR卸载

1.Putty连接VPS服务器，运行如下命令：

`bash tcp_nanqinlang-1.3.2.sh`

2.出现下图提示后，选择4进行卸载：

![](https://ssr.tools/wp-content/uploads/2018-11-29_183122.jpg)

3.卸载完成后重启。

注意：此卸载仅卸载算法，并不卸载内核。

VPS推荐：

[Vultr](https://www.vultr.com/?ref=7887711-4F) （vultr在2019年1月的最新活动，针对新用户，直接送50美元！vultr全球15个服务器位置可选，KVM框架，最低2.5美元/月。支持支付宝和paypal付款。）

### 参考链接：

[南琴浪版暴力魔改BBR一键安装脚本](https://ssr.tools/550)

