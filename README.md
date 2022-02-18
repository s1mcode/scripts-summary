# README

## 云服务器用户数据

### 挖矿脚本部署

#### NKN

**nkndev**

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/nkndev/main/nkn-commercial-install.sh
chmod +x nkn-commercial-install.sh
./nkn-commercial-install.sh NKN_address_address
```

**nkn-VPS镜像更改内部钱包地址**

```bash
#!/bin/bash
cd /var/lib/nkn/nkn-commercial/services/nkn-node
service nkn-commercial stop 
rm wallet.json
rm wallet.pswd && sudo echo 123123 | sudo tee wallet.pswd
./nknc wallet -c --password=123123
chown -R nkn wallet.json
chown -R nkn wallet.pswd
service nkn-commercial start
```

#### XMR

**xmrdev**

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-dev.sh
chmod +x xmr-dev.sh && ./xmr-dev.sh us-west.minexmr.com:443 855rHV5yHh8hnTkCvd74W62q2dvA8gRDMhbcykuTRgkLdrnoVAiCe5ZZBzJiwe1RWSCPYJKhH6DKyQdxP6Bnr7st2kx7wnn aws01
```

**xmr-proxy 代理端**

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-proxy/xmrig-proxy-build.sh
wait
chmod +x xmrig-proxy-build.sh && ./xmrig-proxy-build.sh
```

**xmr-proxy 矿工端**

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-proxy-mine.sh
wait
chmod +x xmr-proxy-mine.sh && ./xmr-proxy-mine.sh 3.233.245.246:443 aws01-02
```

### 启用 ssh root 用户登陆

```bash
#!/bin/bash
echo root:YOURPASSWORD |sudo chpasswd root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo reboot
```

## VPS 脚本

### DD 系统脚本

#### 萌咖 DD 脚本

参考：

[萌咖大佬的一键DD脚本](https://github.com/veip007/dd)

[DD Windows 一键脚本（GCP谷歌云Oracle甲骨文Azure微软云OVH云）](https://sunpma.com/137.html)

[Debian/Ubuntu/CentOS 网络重装系统一键脚本](https://sword.studio/218.html)

[网络重装DD一键脚本支持Debian/Ubuntu/CentOS系统](https://www.moxiaojiu.com/2633.html)

**安装依赖**

更新系统：

```bash
sudo -i
apt-get update
```

必要依赖：

```bash
#Debian/Ubuntu:
apt-get install -y xz-utils openssl gawk file

#RedHat/CentOS:
yum install -y xz openssl gawk file wget
```

**一键脚本**

`wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd '[DD包直连地址]'`

**GCP 谷歌云**

GCP谷歌云、OVH云服务器等需要指定网络参数安装的方式，示例为GCP谷歌云

```bash
# 将X.X.X.X替换为自己的网络参数.
# --ip-addr :IP Address/内网IP地址
# --ip-mask :Netmask   /子网掩码
# --ip-gate :Gateway   /网关
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr X.X.X.X --ip-mask X.X.X.X --ip-gate X.X.X.X -dd 'DD包 直链地址'
```

DD win10 示例：

```bash
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr 10.128.0.5 --ip-mask 255.255.255.0 --ip-gate 10.128.0.1 -dd 'https://oss.sunpma.com/Windows/Whole/Win10_LTSC_64_Administrator_nat.ee.gz'
# 账户: Administrator
# 密码: nat.ee
```

DD win7 示例

```bash
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr 10.138.0.5 --ip-mask 255.255.255.0 --ip-gate 10.138.0.1 -dd 'https://oss.sunpma.com/Windows/Whole/Win7_sp1_64_Administrator_nat.ee.gz'
# 账户: Administrator
# 密码: nat.ee
```

#### DigitalOcean 特殊 DD 法

参考：

[Digital Ocean 搭建 windows 系统](https://www.jianshu.com/p/b9784e042b28)

[介绍几款 Windows DD 镜像 - 秋水逸冰](https://teddysun.com/545.html)

创建虚拟机。

点击新创建的实例，点击【Recovery】，点击【Boot from Recovery ISO】，点击右上角开关首先 Turn off，然后 Turn on。

点击 Console，进入实例的控制台。

一开始可能不会出现以下界面，按回车即会出现：

```bash
Root Password has been randomly set to:
3e01-0b9d-ba13-fbc4
---------------------------------------
1.Mount your Disk Image [Not Mounted]
2.Check Filesystem
3.Reset Droplet Root Password4. Configure Keyboard
5.Attempt to 'chroot' into installed system
6.Interactive Shell [/bin/bash]
Choose (1-6)and press Enter to continue.
```

原先创建机器的密码已经没用了，记住上面生成的密码，下面会用到，输入 `1`，回车。

使用 ssh 客户端登陆。

登录成功后，如果recovery console还在一开始的界面，输入6，回车，进入shell。

安装dd包：

```bash
wget -O- https://oss.sunpma.com/Windows/Whole/Win10_LTSC_64_Administrator_nat.ee.gz | gunzip | dd of=/dev/vda
# 账户: Administrator
# 密码: nat.ee
```

等待安装完成，完成后即可关闭会话。

回到 digital ocean 中实例的【Recovery】界面，首先点击右上角开关将其 Turn off，切换回 Boot from Hard Drive 模式后再 Turn on。

点击 Console，进入实例的控制台，使用 DD 包的账户密码登陆。

这时由于分辨率问题和网络问题，鼠标不好用，尽量用键盘操作，Tab 键、方向键 和 Enter 键配合使用：

开始菜单 -> Windows 系统 -> 控制面板 -> 网络和 Internet -> 网络和共享中心 -> 更改适配器设置 -> 以太网 -> 属性 -> Internet 协议版本4 (TCP/IPv4) -> 属性 -> 使用 DigitalOcean 的 Networking 中的信息配置好 ip 地址，并自定义 DNS 服务器地址：

```bash
PUBLIC IPV4 ADDRESS
143.198.151.74 # IP 地址
PUBLIC GATEWAY
143.198.144.1 # 默认网关
SUBNET MASK
255.255.240.0 # 子网掩码
```

将两个窗口都 确定 后，右侧会弹出是否连接到网络的选项，选择是。

完成，使用 RDP 客户端开始使用 Windows。

> \[!TIP] 此电脑 -> 右键属性 -> 远程设置 -> 取消框选"仅允许运行使用网络级别身份验证的远程桌面的计算机连接(建议)"

#### Oracle 甲骨文 DD win7

参考：localhost

<mark style="color:red;">注意：dd 前先在 Oracle 机器面板中更改子网中的安全列表。</mark>

root用户下，安装运行库：

```
apt-get update
apt-get install -y xz-utils openssl gawk file
```

dd 脚本：

```bash
# wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd 'http://d.nat.ee/win/lite/win7-ent-sp1-x64-cn/win7-ent-sp1-x64-cn-efi.vhd.gz'

wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd 'https://oss.sunpma.com/Windows/Oracle_Win7_sp1_64_Administrator_nat.ee.gz'
```

DD包来源：[http://d.nat.ee/?win/lite/win7-ent-sp1-x64-cn](http://d.nat.ee/?win/lite/win7-ent-sp1-x64-cn)

```
说明：
DD文件: win7-ent-sp1-x64-cn-efi.vhd.gz
注意：此包仅适合支持Uefi启动的VPS，例如 甲骨文(Oracle)
大小: (1.39G)1494465970 字节
MD5: 829ab43487e005d6493d5fb687768a79
SHA1: 2d9a3bf2a60c8d1df199b24d83fb91854636f367
账户：Administrator
密码：nat.ee
EFI分区默认分配：300MB
支持：部署安装时，自动扩展硬盘。
默认：无人值守，自动安装应答安装。
默认：开启远程桌面，端口：3389

漏洞补丁更新到2020年1月，累积重要更新

系统最低要求：
CPU => 1核
内存 => 512M
硬盘 => 5G

系统建议配置：
CPU => 2核
内存 => 2G
硬盘 => 15G

修改：
保留 NET(2-3-3.5)环境，可自行安装到更高版本！
保留 Wifi
防火墙 开启PING，禁用TCP/UDP高危端口:135 137 138 139 445
支持 KVM/XEN/Hyper-V 虚拟化
支持 Uefi/NVMe/USB3.0
更新 IE至IE11
更新 支持RDP8.1远程桌面，可以使用UDP模式连接。
深度精简 功能组件/注册表优化/组策略优化/服务优化

系统文件展开后3.83G左右
(默认开启虚拟内存，实际大于此3.83G，由于虚拟内存文件占用)
纯净 精简 无添加 全新安装 需自行KMS激活

KMS激活：
以管理员身份运行CMD命令窗口
输入1：slmgr /skms KMS服务器地址
输入2：slmgr /ato

此版本为深度精简，功能组件和系统更新均可用。

注意：【首次进入，请先使用tool.cmd脚本修改远程桌面端口和用户密码】

tool.cmd 在桌面，用来修改远程桌面端口和用户密码的。
重启按钮 在桌面，由于远程桌面无法使用开始菜单的重启功能，添加一个有效改善。

DD包自带 nat.cmd 在系统盘根目录，是用来给VPS服务器设置静态IP网关的，
若你的VPS服务器需要，请在DD完成，挂载进去修改此脚本，
无论你是否设置，安装完成进入桌面后此脚本都会自动删除。


关于远程桌面RDP8.1，你本地的系统要支持rdp8.0以上，才会连接更流畅。
早期的xp 2003 2008 Win7是没有支持的，Win8 Win10 以上默认支持。
至于Win7 sp1，可以打补丁支持到rdp8.1。

关于部分KVM的VPS用户无法识别2核心以上，这个是Windows 7 的硬伤，无法修复，原版也一样。家用版本就是这样，换 windows server 2008 r2 可解决。
除非VPS商家修改主机KVM参数才能识别。
```

#### DD 后系统激活

参考：[KMS服务,一句命令激活windows/office!](https://zhuanlan.zhihu.com/p/81561582)

```bash
# 使用管理员权限运行cmd执行
slmgr /skms kms.03k.org
slmgr /ato
```

### 测速脚本

#### SuperBench

来源：[SuperBench.sh 一键测试服务器的基本参数](https://www.oldking.net/350.html)

```bash
wget -qO- sb.oldking.net | bash
```

#### 本机 Speed test 测速

```bash
wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
chmod +x speedtest.py
./speedtest.py
```

### 流媒体及游戏区域限制测试脚本

#### RegionRestrictionCheck - 多种流媒体测试

来源：[RegionRestrictionCheck](https://github.com/lmc999/RegionRestrictionCheck)

```bash
bash <(curl -L -s https://raw.githubusercontent.com/lmc999/RegionRestrictionCheck/main/check.sh)
```

#### netflix-verify - 奈飞测试

来源：[netflix-verify](https://github.com/sjlleo/netflix-verify)

X86\_64：

```bash
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/2.6/nf_2.6_linux_amd64 && chmod +x nf && clear && ./nf
```

ARM64：

```bash
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/2.6/nf_2.6_linux_arm64 && chmod +x nf && clear && ./nf
```

### Acme 脚本申请证书

官网：[acme.sh](https://github.com/acmesh-official/acme.sh)

参考：

[申请SSL证书保姆级教程，包括FreeSSL申请、Acme脚本申请等方式](https://www.v2rayssr.com/ssl.html)

[Debian10手动使用acme.sh脚本申请证书](https://www.zhizhuzi.org/2021/07/29/debian10%E6%89%8B%E5%8A%A8%E4%BD%BF%E7%94%A8acme-sh%E8%84%9A%E6%9C%AC%E7%94%B3%E8%AF%B7%E8%AF%81%E4%B9%A6/)

安装 ACME：

```bash
curl https://get.acme.sh | sh
```

由于 acme.sh 现在使用 ZeroSSL 作为默认 CA，所以首先注册 [ZeroSSL](https://zerossl.com)。（参考：[ZeroSSL.com CA](https://github.com/acmesh-official/acme.sh/wiki/ZeroSSL.com-CA)）

```bash
~/.acme.sh/acme.sh --register-account -m yourZeroSSLmail@example.com
```

#### 申请证书

**使用 80 端口空闲的证书申请方式**

```bash
~/.acme.sh/acme.sh  --issue -d mydomain.com   --standalone
```

**使用 Cloudflare DNS 验证的方式申请证书**

参考：[How to use DNS API](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)

设置 Cloudflare API 令牌：

cloudflare 概述界面 -> 获取您的API令牌 -> API 密钥 -> Global API Key 查看 -> 复制 API 密钥

```bash
export CF_Key="API 密钥"
export CF_Email="CF 邮箱"
```

生成证书：

```bash
~/.acme.sh/acme.sh --issue --dns dns_cf -d mydomain.com -d *.mydomain.com
```

成功后会返回如下内容：

```bash
[Thu Jul 29 12:15:37 UTC 2021] Cert success.
-----BEGIN CERTIFICATE-----
********************
-----END CERTIFICATE-----
[Thu Jul 29 12:15:37 UTC 2021] Your cert is in  /root/.acme.sh/mydomain.com/mydomain.com.cer 
[Thu Jul 29 12:15:37 UTC 2021] Your cert key is in  /root/.acme.sh/mydomain.com/mydomain.com.key 
[Thu Jul 29 12:15:37 UTC 2021] The intermediate CA cert is in  /root/.acme.sh/mydomain.com/ca.cer 
[Thu Jul 29 12:15:37 UTC 2021] And the full chain certs is there:  /root/.acme.sh/mydomain.com/fullchain.cer
```

#### 安装证书

前面证书生成以后, 接下来需要把证书 copy 到真正需要用它的地方。

注意，默认生成的证书都放在安装目录下：\~/.acme.sh/， 请不要直接使用此目录下的文件, 例如：不要直接让 nginx/apache 的配置文件使用这下面的文件。 这里面的文件都是内部使用，而且目录结构可能会变化。

正确的使用方法是使用 `–installcert` 命令，并指定目标位置，然后证书文件会被copy到相应的位置。

```bash
~/.acme.sh/acme.sh --installcert -d mydomain.com --key-file /root/private.key --fullchain-file /root/cert.crt
```

#### 自动更新 `acme.sh`

```bash
~/.acme.sh/acme.sh --upgrade --auto-upgrade
chmod -R 755 /root/cert
```

#### 更新证书

目前证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心。

#### 撤销证书

参考：[How to revoke a cert](https://github.com/acmesh-official/acme.sh/wiki/revokecert)

出于安全原因，目前不允许用户通过 ZeroSSL 门户用户界面撤销通过 ACME 颁发的证书。 要撤销此类证书，必须使用 ACME 客户端的撤销功能。

```bash
~/.acme.sh/acme.sh --revoke -d mydomain.com
```

### 防火墙相关

注意VPS的防火墙可能有两层：

1.服务商的防火墙

2.操作系统的防火墙

#### 查看某端口是否开放

```bash
# 例如查看80端口是否开放，输入 lsof -i:端口号 ，如果没有信息出现则说明该端口还未开放
lsof -i:80
```

也可以通过以下命令，查看所有开放的端口

```bash
netstat -aptn
```

#### Firewalld

RedHat/CentOS/Oracle Linux 常用。Debian/Ubuntu往往不会默认启动。

注意： `iptables` 命令并不能设置这类防火墙。

```bash
firewall-cmd --state
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --permanent --zone=public --add-port=443/tcp
firewall-cmd --permanent --zone=public --add-port=7000/tcp
firewall-cmd --permanent --zone=public --add-port=7500/tcp
firewall-cmd --permanent --zone=public --add-port=10001-50000/tcp
firewall-cmd --permanent --zone=public --add-port=10001-50000/udp
firewall-cmd --reload
```

常用命令：

```bash
apt install firewalld -y               # 安装 firewalld
firewall-cmd --state                   # 查看防火墙状态
systemctl stop firewalld.service       # 停止防火墙
systemctl disable firewalld.service    # 禁止防火墙开机自启
```

#### ufw

Debian/Ubuntu往往会默认启动。

```bash
ufw status
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 7000/tcp
ufw allow 7500/tcp
ufw allow 10001:50000/tcp
ufw allow 10001:50000/udp
```

#### iptables 

不同服务商的系统可能有特殊化的定制。

```bash
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
iptables -I INPUT -p tcp --dport 443 -j ACCEPT
iptables -I INPUT -p tcp --dport 7000 -j ACCEPT
iptables -I INPUT -p tcp --dport 7500 -j ACCEPT
iptables -I INPUT -p tcp --dport 10001:50000 -j ACCEPT
iptables -I INPUT -p udp --dport 10001:50000 -j ACCEPT
```

### `iptables` 的使用

#### 安装 `iptables`

首先确定你的系统已经安装iptables

```bash
whereis iptables
```

如果能看到如下类似信息,说明你已经安装了 `iptables`

```bash
iptables: /sbin/iptables /usr/share/iptables /usr/share/man/man8/iptables.8.gz
```

如果不是这个提示,或者没有任何提示,那你的Debian上可能没有安装 `iptables`

请使用如下命令安装:

```bash
apt-get install iptables
```

#### 配置 `iptables`

查看iptables配置：

```bash
iptables -L -n
```

如果是以下内容说明跟没配置一样

```bash
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

编辑/新建一个新的iptables策略

```bash
vim /etc/iptables.test.rules
```

```bash
*filter

# 接受所有已建立的入站连接
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# 允许所有出站流量
# You could modify this to only allow certain traffic
-A OUTPUT -j ACCEPT

# 网站服务常用的80和443端口允许
-A INPUT -p tcp --dport 80 -j ACCEPT
-A INPUT -p tcp --dport 443 -j ACCEPT
-A INPUT -p tcp --dport 31570 -j ACCEPT

# 允许SSH连接
# The --dport 端口号要与 /etc/ssh/sshd_config 中的Port参数相同
-A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT

# Now you should read up on iptables rules and consider whether ssh access
# for everyone is really desired. Most likely you will only allow access from certain IPs.

# 允许 ping
#  note that blocking other types of icmp packets is considered a bad idea by some
#  remove -m icmp --icmp-type 8 from this line to allow all kinds of icmp:
#  https://security.stackexchange.com/questions/22711
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT

# log iptables denied calls (access via 'dmesg' command)
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7

# 除了上面设置端口的规则，其他都拒绝
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT
#
```

加载配置策略生效

```bash
iptables-restore < /etc/iptables.test.rules
```

配置开机自动加载配置策略

```bash
vim /etc/network/if-pre-up.d/iptables
```

```bash
#!/bin/bash
/sbin/iptables-restore < /etc/iptables.test.rules
```

脚本设置运行权限

```bash
chmod +x /etc/network/if-pre-up.d/iptables
```

### Linux 安装图形化界面

#### Azure Ubuntu 安装 xrdp

来源：[安装并配置 xrdp 以在 Ubuntu 上使用远程桌面](https://docs.microsoft.com/zh-cn/azure/virtual-machines/linux/use-remote-desktop)

以下示例在 Ubuntu 18.04 LTS VM 上安装轻型 [xfce4](https://www.xfce.org) 桌面环境。 其他发行版的命令略有不同（例如，使用 `yum` 在 Red Hat Enterprise Linux 上安装并配置适当的 `selinux` 规则，或者使用 `zypper` 在 SUSE 上安装）。

首先，通过 SSH 连接到 VM。&#x20;

接下来，使用 `apt` 安装 xfce：

```
sudo apt-get update
sudo apt-get -y install xfce4
sudo apt install xfce4-session
```

安装桌面环境后，请配置远程桌面服务来侦听传入连接。 [xrdp](http://xrdp.org) 是大多数 Linux 分发版中提供的开源远程桌面协议 (RDP) 服务器，可与 xfce 完美配合。 在 Ubuntu VM 上安装 xrdp：

```
sudo apt-get -y install xrdp
sudo systemctl enable xrdp
```

告诉 xrdp 在启动会话时要使用的桌面环境。 配置 xrdp 以使用 xfce 作为桌面环境：

```
echo xfce4-session >~/.xsession
```

重新启动 xrdp 服务使更改生效：

```
sudo service xrdp restart
```

如果在创建 VM 时已为用户帐户创建密码，请跳过此步骤。 如果仅使用 SSH 密钥身份验证，并且未设置本地帐户密码，请在使用 xrdp 之前指定密码以登录到 VM。 xrdp 无法接受使用 SSH 密钥进行身份验证。 以下示例为用户帐户 _azureuser_ 指定密码：

```
sudo passwd azureuser
```

指定密码不会将 SSHD 配置更新为允许密码登录（如果当前不允许）。 从安全角度看，可能想要使用基于密钥的身份验证通过 SSH 隧道连接到 VM，并连接到 xrdp。 如果是这样，请跳过以下创建网络安全组规则的步骤，以允许远程桌面流量。

若要允许远程桌面流量到达 Linux VM，需要创建网络安全组规则以允许端口 3389 上的 TCP 访问 VM。&#x20;

以下示例在端口 _3389_ 上使用 [az vm open-port](https://docs.microsoft.com/zh-cn/cli/azure/vm#az\_vm\_open\_port) 创建网络安全组规则。 使用 Azure CLI（而不是与 VM 的 SSH 会话），打开以下网络安全组规则：

```
az vm open-port --resource-group myResourceGroup --name myVM --port 3389
```

安装 firefox：

```bash
sudo apt-get install firefox
```

#### 甲骨文ARM安装带声音的RDP远程Linux桌面

来源：[甲骨文ARM安装带声音的RDP远程Linux桌面](https://hostloc.com/thread-871447-1-1.html)

> \[!NOTE] 本脚本暂只支持Ubuntu20.
>
> 仅在ARM架构进行常规测试,不保证完全可用,请提前做好回滚准备!
>
> 甲骨文封号机制不明,不保证账号稳定性!
>
> 环境: LXDE + XRDP + XRDP-PA

```bash
screen -S u2ad
wget https://gist.githubusercontent.com/flyqie/60a005535afc0b5d45255619299b9630/raw/fa54bccd0dad4fb488b79ffdf0d3afeb17daf4c1/ubuntu-20-arm-desktop.sh -O ubuntu-20-arm-desktop.sh ; sudo bash ubuntu-20-arm-desktop.sh | tee -a ubuntu-20-arm-desktop.log
# 一切确认无误后重启
sudo reboot
```

> \[!TIP]待重启后即可通过 SSH 隧道访问，如需公网访问请自行开放防火墙，参考：https://github.com/s1mcode/scripts-summary#%E9%98%B2%E7%81%AB%E5%A2%99%E7%9B%B8%E5%85%B3
>
> RDP Port: 3389
>
> RDP Username: rdpuser
>
> RDP Default Password: rdpuser\_password

Github Gist链接: [https://gist.github.com/flyqie/60a005535afc0b5d45255619299b9630](https://gist.github.com/flyqie/60a005535afc0b5d45255619299b9630)

参考链接:

[https://gist.github.com/rkttu/35ecab5604c9ddc356b0af4644d5a226](https://gist.github.com/rkttu/35ecab5604c9ddc356b0af4644d5a226) [https://gist.github.com/CraigCottingham/fad000cc2ec4678203acf62c4ad2ab23](https://gist.github.com/CraigCottingham/fad000cc2ec4678203acf62c4ad2ab23) [https://github.com/MeowLove/Linux-Remote-Desktop-Environment](https://github.com/MeowLove/Linux-Remote-Desktop-Environment) [https://github.com/neutrinolabs/pulseaudio-module-xrdp/issues/44](https://github.com/neutrinolabs/pulseaudio-module-xrdp/issues/44)

#### 甲骨文 Ubuntu ARM Gnome

参考：

[**Ubuntu ARM Gnome教程已发布到博客！**](https://hostloc.com/thread-899918-1-1.html)

[**甲骨文ARM Ubuntu Gnome桌面开启声音教程**](https://hostloc.com/thread-944424-1-1.html)

[**甲骨文ARM远程桌面转发声音**](https://hostloc.com/thread-935349-1-1.html)

[甲骨文ARM Ubuntu20.04原版Gnome桌面+frp内部转发教程](https://live.ikaiche.org/archives/%E7%94%B2%E9%AA%A8%E6%96%87armubuntu2004%E5%8E%9F%E7%89%88gnome%E6%A1%8C%E9%9D%A2frp%E5%86%85%E9%83%A8%E8%BD%AC%E5%8F%91%E6%95%99%E7%A8%8B)

[甲骨文ARMUbuntu20.04原版Gnome桌面+frp内部转发教程](https://life.ikaiche.org/2021/12/21/6/)

[甲骨文ARM Ubuntu20.04 Gnome远程桌面开启声音](https://life.ikaiche.org/2021/12/22/%E7%94%B2%E9%AA%A8%E6%96%87arm-ubuntu20-04-gnome%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E5%BC%80%E5%90%AF%E5%A3%B0%E9%9F%B3/)

[甲骨文ARM远程桌面转发声音](https://hostloc.com/thread-935349-1-1.html)

##### 准备

1.甲骨文ARM（为了性能保证系统体验，建议ARM 2核心或更高！）
2.系统必须得是Ubuntu20.04（甲骨文原版镜像或者DD萌咖大佬的ubuntu20.04最好！原版甲骨文镜像每次开机都要关闭防护墙，很烦！）
这里宣传一下萌咖大佬的Ubuntu20.04原版DD一键：

```bash
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/MoeClub/Note/master/InstallNET.sh') -u 20.04 -v arm64 --mirror http://archive.ubuntu.com/ubuntu -p "自定义root密码" -port "自定义ssh端口"
```

3.甲骨文门户面板里的端口建议全开！

##### 安装Ubuntu官方Gnome桌面

接下来做一次更新和升级，执行命令：

```bash
apt update -y ; apt upgrade -y
```

升级后设置一下语言：(为了让大家进入图形系统后能够设置中文界面！）

```bash
dpkg-reconfigure locales
```

在出现的第一画面里做如下选择，上下键切换，空格键是选择，Tab键可以跳到“OK”上，Enter键确认：

```bash
选择这两个：
【*】zh CN.GBK GBK
【*】zh_CN.UTF-8 UTF-8
Ok
```

再进入到第二画面后，同样通过空格键选择中文为默认的语言编码，用Tab键跳到“OK”上回车确认：

```bash
选中：
【*】zh_CN.UTF-8
OK
```

设置完毕后，重新连接一次服务器，会发现系统支持中文展示了。

下面我们开始真正的桌面环境的安装，这里安装ubuntu desktop而不是xfce，原汁原味！执行命令：

```bash
# apt install screen -y
# screen -R insdesk
apt install ubuntu-desktop
```

安装过程耗时较长，耐心等待，直至完成。

完毕后进行Xrdp的安装，提供远程桌面访问的能力，执行以下命令：

```bash
apt install xrdp -y
```

Xrdp会安装成服务，可以验证一下：

```bash
systemctl status xrdp
```

如果看到红色ERROR这一行信息，如何解决呢？

```bash
Cannot read private key file/etc/xrdp/key-pem:Permisslon denied
```

执行以下命令：

```
adduser xrdp ssl-cert
systemctl restart xrdp
systemctl status xrdp
```

可以看到红色的信息不见了：

```bash
12月 23 02:03:25 ubuntu xrdp[45134]: (45134)(281473022304272)[INFO ] address [0.0.0.0] port [3389] mode 1
12月 23 02:03:25 ubuntu xrdp[45134]: (45134)(281473022304272)[INFO ] listening to port 3389 on 0.0.0.0
12月 23 02:03:25 ubuntu xrdp[45134]: (45134)(281473022304272)[INFO ] xrdp_listen_pp done
12月 23 02:03:25 ubuntu xrdp[45134]: (45134)(281473022304272)[DEBUG] Closed socket 7 (AF_INET6 :: port 3389)
12月 23 02:03:25 ubuntu systemd[1]: xrdp.service: Can't open PID file /run/xrdp/xrdp.pid (yet?) after start: Operation not permitted
12月 23 02:03:26 ubuntu systemd[1]: Started xrdp daemon.
12月 23 02:03:27 ubuntu xrdp[45148]: (45148)(281473022304272)[INFO ] starting xrdp with pid 45148
12月 23 02:03:27 ubuntu xrdp[45148]: (45148)(281473022304272)[INFO ] address [0.0.0.0] port [3389] mode 1
12月 23 02:03:27 ubuntu xrdp[45148]: (45148)(281473022304272)[INFO ] listening to port 3389 on 0.0.0.0
12月 23 02:03:27 ubuntu xrdp[45148]: (45148)(281473022304272)[INFO ] xrdp_listen_pp done
```

这样桌面环境和远程服务安装好后，就可以连接到远程桌面。经过反复的测试和实验，使用Windows 10 默认的远程桌面工具mstsc，体验非常卡顿，具体原因具体可以总结一下，就是微软的rdp与Linux的Xrdp进行连接时存在兼容性问题，不要以为纯粹是网络问题，其实甲骨文ARM的性能非常夸张，可以很负责任的告诉大家，甲骨文的这个ARM芯片，相比于X86架构，你很难在消费市场上找到能比他强的，总之性能就是给的实在，具体大家深入体会后就明白了。

##### 安装Linux FRP服务

接下来FRP将会彻底解决远程桌面卡顿的问题，在这里给大家普及一下FRP的运作机制，首先FRP分为客户端frpc和服务端frps，大家只要记住，哪个要做中转机哪个就放服务端，客户端放在ARM上，如果像我这种用甲骨文春川的本来国内直连速度就比较好，那就可以服务端和客户端都放在ARM机器上，这样就相当于ARM本地做了一次端口转发，理解这个逻辑，那么就开始干！

FRP github链接
https://github.com/fatedier/frp/releases/tag/v0.34.2

大家看对版本，每个版本里都包含frps和frpc，如果大家用的是甲骨文春川，选linux_arm64就行。如果是比如用腾讯云做服务端的话，那么腾讯云是amd64的，甲骨文ARM是arm64的，在丢到各自的机器上时注意版本！！！重要的事情说三遍！别把版本放错了！别把版本放错了！别把版本放错了！再次重申frps是服务端,frpc是客户端。

客户端方面
1.解压包，把如下文件放到/etc/frp 目录下

```
frpc
frpc_full.ini 
frpc.ini
```

```
cd /etc
wget https://github.com/fatedier/frp/releases/download/v0.34.2/frp_0.34.2_linux_arm64.tar.gz
tar -zxvf frp_0.34.2_linux_arm64.tar.gz
mv frp_0.34.2_linux_arm64 frp
```

2.修改frpc.ini文件

```
[common]
server_addr = 127.0.0.1 #如果你就想在ARM机器本地内部做一次转发，就用127.0.0.1，如果你是用腾讯云香港做服务器转发，这里就填腾讯云的IP
server_port = 7000
token = 135792468

[13389]
type = tcp
remote_port = 13389
local_ip = 127.0.0.1
local_port = 3389
```

3.把systemd文件夹下的frpc.service，放到/etc/systemd/system下。
设置权限，chmod 754 frpc.service
设置开机启动即可 systemctl enable frpc.service

```
cd /etc/frp
cp ./systemd/frpc.service /etc/systemd/system
chmod 754 /etc/systemd/system/frpc.service
systemctl enable frpc.service
```

4.在/etc/frp/目录下

```
cd /etc/frp
cp frpc /usr/bin
chmod +x /usr/bin/frpc
systemctl start frpc
ps -ef|grep frpc
```

客户端结束

服务端方面
1.解压包，把如下文件放到/etc/frp 目录下

```
frps
frps_full.ini 
frps.ini
```

2.修改frps.ini文件（这里为了方便大家不掉坑，我这里直接给出自己已经搭建成功的配置文件给大家，完整覆盖过去）

```bash
[common]
bind_addr = 0.0.0.0
bind_port = 7000
bind_udp_port = 7001
kcp_bind_port = 7000
vhost_http_port = 8080
vhost_https_port = 8443
dashboard_addr = 0.0.0.0
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = 135792468
log_file = ./frps.log
log_level = info
log_max_days = 3
disable_log_color = false
token = 135792468
allow_ports = 13000-14000
max_pool_count = 5
max_ports_per_client = 0
subdomain_host = frps.com
tcp_mux = true
```

3.把systemd文件夹下的frps.service，放到/etc/systemd/system下
设置权限，chmod 754 frps.service
设置开机启动即可 systemctl enable frps.service

```bash
cd /etc/frp
cp ./systemd/frps.service /etc/systemd/system
chmod 754 /etc/systemd/system/frps.service
systemctl enable frps.service
```

4.在/etc/frp目录下

```bash
cd /etc/frp/
cp frps /usr/bin
chmod +x /usr/bin/frps
systemctl start frps
ps -ef|grep frps
```

完成，结束

至此，1.如果大家是在ARM本机内部做转发的，浏览器输入ARM机器IP地址:7500，进入后找TCP，如果能看到一条记录那就是成功启动FRP了。

2.如果大家是在比如腾讯云机器上做frps的，浏览器输入腾讯云机器IP地址:7500，进入后找TCP，如果能看到一条记录那就是成功启动FRP了。

连接到桌面后,一路点击Next,最后点击Start Using Ubuntu。

接下来设置中文及中文输入法支持，依次点击左上角的 Activities-Show Applications-Language Support，之后在Language Support中增加语言支持：如果问你是否安装，先点击安装。完成后点击"添加或删除语言"按钮，找到"中文（简体）"打勾，Apply。

完成后，顺便将键盘输入法系统选择成为“iBus”。

接下来重启系统（ssh中使用命令`reboot`），重新连接后会看到界面菜单均已中文显示。点击右上角电源标识旁边的小尖号，出现的菜单里点击设置，在设置的界面中找到“区域与语言”，能够看到语言已经设置成为“汉语”。我们接下来要设置中文输入法，点击下面的“+”，出现输入源小窗口，点击“汉语”，点击“中文（智能拼音）”进行“添加”就完成了输入法的设置。

最后在ssh中新建用户，桌面端用这个用户登录，并且在设置->电源中，将息屏设置为从不。

```bash
adduser rdpuser
# 删除用户
# userdel testuser
passwd rdpuser
usermod -G sudo rdpuser
```

##### 甲骨文ARM Ubuntu20.04Gnome远程桌面开启声音

修改ubuntu 默认的源地址

```
vim /etc/apt/source.list
```

添加：

```bash
#deb http://ports.ubuntu.com/ focal main restricted
#deb http://ports.ubuntu.com/ focal-updates main restricted
deb http://ports.ubuntu.com/ focal-security main restricted
# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://ports.ubuntu.com/ focal main restricted
# deb-src http://archive.ubuntu.com/ubuntu focal main restricted
## Major bug fix updates produced after the final release of the
## distribution.
deb http://ports.ubuntu.com/ focal-updates main restricted
# deb-src http://archive.ubuntu.com/ubuntu focal-updates main restricted
## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team. Also, please note that software in universe WILL NOT receive any
## review or updates from the Ubuntu security team.
deb http://ports.ubuntu.com/ focal universe
# deb-src http://archive.ubuntu.com/ubuntu focal universe
deb http://ports.ubuntu.com/ focal-updates universe
# deb-src http://archive.ubuntu.com/ubuntu focal-updates universe
## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://ports.ubuntu.com/ focal multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal multiverse
deb http://ports.ubuntu.com/ focal-updates multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal-updates multiverse
## N.B. software from this repository may not have been tested as
## extensively as that contained in the main release, although it includes
## newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review
## or updates from the Ubuntu security team.
deb http://ports.ubuntu.com/ focal-backports main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu focal-backports main restricted universe multiverse
## Uncomment the following two lines to add software from Canonical's
## 'partner' repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
deb http://archive.canonical.com/ubuntu focal partner
deb-src http://archive.canonical.com/ubuntu focal partner
deb http://ports.ubuntu.com/ubuntu-ports focal-security main restricted
deb-src http://ports.ubuntu.com/ubuntu-ports focal-security main universe restricted multiverse
deb http://ports.ubuntu.com/ubuntu-ports focal-security universe
# deb-src http://ports.ubuntu.com/ubuntu-ports focal-security universe
deb http://ports.ubuntu.com/ubuntu-ports focal-security multiverse
# deb-src http://ports.ubuntu.com/ubuntu-ports focal-security multiverse
deb http://ports.ubuntu.com/ubuntu-ports/ focal main universe restricted multiversedeb-src http://ports.ubuntu.com/ubuntu-ports/ focal main universe restricted multiverse #Added by software-properties
```

安装Pulseaudio所需的所有组件

```bash
sudo -i
cd /root
apt-get install software-properties-common -y
apt-get install git libpulse-dev autoconf m4 intltool dpkg-dev libtool libsndfile-dev libcap-dev libjson-c-dev -y
# apt-get build-dep pulseaudio -y
apt update
```

下载Pulseaudio源码

```bash
apt-get build-dep pulseaudio -y
apt source pulseaudio
```

此时cmd输出的最后一行为：

```bash
W: 由于文件'pulseaudio_13.99.1-1ubuntu3.8.dsc'无法被用户'_apt'访问，已脱离沙盒并提权为根用户来进行下载。 - pkgAcquire::Run (13: 权限不够)
```

当前文件夹已经多了一个文件夹和几个文件：

```bash
drwxr-xr-x 15 root root    4096 12月 24 02:47 pulseaudio-13.99.1/
-rw-r--r--  1 root root  152288 11月 23  2020 pulseaudio_13.99.1-1ubuntu3.8.debian.tar.xz
-rw-r--r--  1 root root    3685 11月 23  2020 pulseaudio_13.99.1-1ubuntu3.8.dsc
-rw-r--r--  1 root root 1955840 2月  26  2020 pulseaudio_13.99.1.orig.tar.xz
```

提权

```bash
chmod 777 pulseaudio_13.99.1-1ubuntu3.8.dsc
```

删除Palseaudio源码文件夹

```
rm -rf pulseaudio-13.99.1
```

重新下载源码，进入源码目录，配置

```bash
apt source pulseaudio
cd pulseaudio-13.99.1
./configure
```

克隆仓库

```bash
cd ../
git clone https://github.com/neutrinolabs/pulseaudio-module-xrdp.git
```

编译

```bash
cd pulseaudio-module-xrdp
./bootstrap
./configure PULSE_DIR="/root/pulseaudio-13.99.1"
make
```

安装动态链接库

```bash
cd src/.libs
install -t "/var/lib/xrdp-pulseaudio-installer" -D -m 644 *.so
install -t "/usr/lib/pulse-$pulsever/modules" -D -m 644 *.so
install -t "/usr/lib/pulse-13.99.1/modules" -D -m 644 *.so
ls /var/lib/xrdp-pulseaudio-installer
```

对于root用户想启用pulseaudio的话，还需要以下几行命令，把root添加到pulse（可选）

```
gpasswd -a root pulse
gpasswd -a root pulse-access
```

重启

```bash
reboot
```

重启后从 rdp 登录非 root 用户，打开 cmd 应用，输入以下命令

```bash
pulseaudio -k && pulseaudio
```

等几秒钟再ctrl+c,结束掉，然后输入命令

```bash
pulseaudio
```

（注意：如果有报错不用管，别关看声卡变成xrdp sink了没有，试一下声音，看看rdp声音开了没，有声音了直接关掉窗口）每个ubuntu账户都需要走一遍这个步骤，对应的ubuntu账户才能开启声音，后续如果重启都需要重复这一步！

##### 解决Ubuntu 20.04 安装xrdp后的远程黑屏问题

在Ubuntu 20.04安装xrdp后，使用Mac去连接
结果发现连接过去之后，一直是黑屏状态。浪费了大半天时间之后，终于解决：

```
vim /etc/xrdp/startwm.sh
```

加入文件最后面的session前面

```
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```

看起来像：

```
if test -r /etc/profile; then
        . /etc/profile
fi

unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR

test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```

最后重启使配置生效：

```
sudo systemctl restart xrdp
```

##### xrdp 配置文件设置

参考： 

[xrdp配置文件详解](https://blog.csdn.net/yyywxk/article/details/106571336)

[学习ubuntu远程桌面（二）：远程桌面](https://www.shuzhiduo.com/A/o75NLgRezW/?utm_source=pocket_mylist)

[如何设置重用现有会话的xrdp会话？ ](https://ubuntuqa.com/article/1739.html?utm_source=pocket_mylist)

[How do I set up xrdp session that reuses an existing session？](https://askubuntu.com/questions/133343/how-do-i-set-up-xrdp-session-that-reuses-an-existing-session)

1、会话 （session）设置

如果要修改会话 （session） 的设置，可以修改 xrdp 的 `sesman.ini` 配置文件。
进入 `sesman.ini` 文件的命令为：

```bash
sudo vim /etc/xrdp/sesman.ini
```

主要是 [Session] 下进行修改。其中常用的设置有：

- `MaxSessions=10` 代表最大会话数为10，如果要改变最大会话数可以修改这个地方；
- `KillDisconnected=0` 表示是否立即关闭断开的连接，如果为 1 则表示断开连接后会自动注销，我们如果想要断开后会话进程依旧运行可以设置此处为 0。
- `IdleTimeLimit=0` 空闲会话时间限制(0为没有限制)。
- `DisconnectedTimeLimit=0` 断开连接的存活时间(0为没有限制)。

修改后需要重启xrdp服务：

```bash
sudo systemctl restart xrdp
```

2、如何设置重用现有xrdp会话

开和编辑xrdp的配置文件：

```bash
sudo vim /etc/xrdp/xrdp.ini
```

默认情况下，第一个xrdp会话处理脚本如下所示：

```bash
[xrdp1]
name=sesman-vnc
lib=libvnc.so
username=ask
password=ask
ip=127.0.0.1
port=-1
```

重要的一行是`port=-1`，这使得xrdp总是寻找一个可以连接的空闲端口。如果在此处设置固定端口，则xrdp将始终返回并连接到同一会话。我换了我的，所以它看起来像这样：

```bash
[xrdp1]
name=customsessionname
lib=libvnc.so
username=myusername
password=ask
ip=127.0.0.1
port=5912
```

只需将`port=-1`更改为`port=5912`即可。

##### xrdp 其他问题

参考：

[Ubuntu]: https://www.its203.com/article/yyywxk/106136196?utm_source=pocket_mylist

1、Tab 键无法使用
使用时发现Tab 键无法使用，原因是快捷方式被占用，解决方案：

- 直接在远程桌面中设置，打开菜单->设置->窗口管理器，或者在终端中输入xfwm4-settings打开（xfwm4就是xfce4 window manger的缩写）。
- 选择键盘，可以看到窗口快捷键中动作一列有“切换同一应用程序的窗口”选项。
- 将该选项的快捷键清除后关闭窗口即可。

缺省的设置，该项的值为 super+制表，制表键就是 Tab键。这样修改后马上可以使用了。

2、在linux中使用firfox出现的问题

中文版显示：`Firefox 已经在运行，但是没有响应。如要打开新窗口，您必须先关闭该 Firefox 进程，或者重新启动您的系统。`
英文版显示：`Firefox is already running, but is not responding. To open a new window, you must first close the existing Firefox process, or restart your system.`
解决方法如下：
在linux的终端输入：
`firefox -profilemanager`
回车后会出现一个小窗口让你选择使用哪个 profile 来启动 firefox。首先将当前出错的Profile删除掉，然后新建个即可。

3、当前root用户会话锁屏

新建一个非root用户，并设置密码，以后使用该用户登录xrdp。

### ARM 远程桌面（甲骨文 Ubuntu ARM Gnome ）安装 IDEA

参考：

[Ubuntu 系统下安装 IntelliJ IDEA](https://developer.aliyun.com/article/7961)

[Ubuntu 系统下安装 IntelliJ IDEA](https://www.lhsz.xyz/read/ideabook/ubuntu-install.md)

[Intellij idea史上最简单的教程之Linux下安装与破解Intellij idea2017](https://www.1024sou.com/article/102050.html)

[在ubuntu中为IDEA，Pycharm等软件创建启动图标](https://www.jianshu.com/p/9699fef51588)

首先安装 JDK，以下代码在 RDP cmd 程序中运行：

```bash
# 查看版本
java -version
查看可以安装的版本
apt-cache search openjdk
##################无效步骤记录########################
apt-get install openjdk-8-jdk
# 发现安装不上 IDEA，报错，故卸载 
# 报错内容：JDKError occurred during initialization of VM
java.lang.UnsupportedClassVersionError: com/intellij/util/lang/PathClassLoader has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0
# 卸载JDK
sudo apt-get remove openjdk-8-jdk
# 此时 java -version 还是显示 1.8 版本
# 卸载 JRE
sudo apt-get remove openjdk-8-jre-headless
# 此时 java -version 竟然显示 11 版本
# 尝试再次安装 IDEA，发现运行了一段时间，报了别的错
# 报错内容：Illegal reflective access by com.intellij.util.lang.UrlClassLoader (file:/opt/idea-IU-213.6777.52/lib/util.jar) to field java.lang.ClassLoader.classLoaderValueMap
WARNING: Please consider reporting this to the maintainers of com.intellij.util.lang.UrlClassLoader
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
...
# 然后尝试手动安装 JDK 11，之后再安装竟然成功了
###################无效步骤记录########################
sudo apt install openjdk-11-jdk
java -version
# 此时 java -version 显示 11 版本
```

在[官网](https://www.jetbrains.com/idea/)下载以 .tar.gz 结尾的 Linux 版本安装包，下载好后开始安装：

```bash
 # 复制安装包到/opt
 sudo mv /home/rdpuser/下载/ideaIU-2021.3.2.tar.gz /opt
 #进入/opt，解压
 cd /opt
 sudo tar xfz ideaIU-2021.3.2.tar.gz
 # 开始安装
 # 以下代码必须在 RDP cmd 程序中运行
 cd /opt/idea-IU-213.6777.52/bin
 ./idea.sh
```

现在可以使用了，但是关闭后，以后再打开，还要在 cmd 中使用 `/opt/idea-IU-213.6777.52/bin/idea.sh`命令打开，很不方便，所以下面创建一个启动图标，方便使用。

打开终端输入并执行：

```bash
sudo vim /usr/share/applications/intellij-idea.desktop
```

然后在vim中输入下列代码：

```bash
[Desktop Entry]
Name= idea
Comment= idea
Exec= /opt/idea-IU-213.6777.52/bin/idea.sh
Icon=/opt/idea-IU-213.6777.52/bin/idea.png
Terminal=false
Type=Application
Categories=Application;Development;
```

最后在终端中输入并执行：

```bash
sudo chmod +x /usr/share/applications/intellij-idea.desktop
```

然后打开功能键输入 idea 即可看见快捷方式，当然也可以打开 /usr/share/applications/ 目录中寻找。

### DNS 解锁

#### dns-change-hostmsu

来源：[hostmsu](https://www.hostmsu.ru)

```bash
wget -N --no-check-certificate https://www.hostmsu.ru/modules/addons/stream_unblock/library/shell/dns-change.sh && chmod +x dns-change.sh && ./dns-change.sh 103.149.46.148
```

> \[!TIP] 如果想恢复备份，可在执行脚本文件时使用参数 `restore`

### windows server 中 openssh 使用

参考：[windows server 2012 R2 安装 openssh](https://blog.csdn.net/weixin\_42819452/article/details/115087464)

#### openssh 安装

若 winserver 上没有 openssh, 则手动安装：

访问[Openssh官网](https://github.com/PowerShell/Win32-OpenSSH/releases)并根据操作系统的位数选择合适的安装包下载

打开cmd终端，进入解压后的 OpenSSH 文件夹，在cmd终端输入下面指令：

```bash
powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1
```

#### 配置ssh服务

在防火墙开启端口22端口号：

```bash
netsh advfirewall firewall add rule name=sshd dir=in action=allow protocol=TCP localport=22
```

#### 启动ssh服务

```bash
net start sshd
```

### Python 相关命令

#### 安装指定版本 python

```bash
sudo apt install python3.9
```

#### 安装 pip3

```bash
sudo apt install python3-pip
```

#### 升级 pip

```
python -m pip install --upgrade pip
```

### 创建 Python 虚拟环境

通过运行以下命令来验证系统中是否安装了Python 3：

```
python3 -V
```

输出应如下所示：

```
Python 3.6.5
```

从Python 3.6开始，创建虚拟环境的推荐方法是使用`venv`模块。

我们先安装提供`venv`模块的`python3-venv`软件包。

```
sudo apt install python3-venv
# 或其他版本 python 环境
sudo apt install python3.9-venv
```

安装完模块后，我们就可以为Python 3创建虚拟环境了。

切换到要存储Python 3虚拟环境的目录。 在目录中，运行以下命令来创建新的虚拟环境：

```
python3 -m venv my-project-env
# 或其他版本 python 环境
python3.9 -m venv my-project-env
```

上面的命令创建一个名为`my-project-env`的目录，其中包含Python二进制文件，Pip包管理器，标准Python库和其他支持文件的副本。

要开始使用此虚拟环境，您需要通过运行`activate`脚本将其激活：

```
source my-project-env/bin/activate
```

一旦激活，虚拟环境的bin目录将添加到[`$PATH`](https://www.myfreax.com/how-to-add-directory-to-path-in-linux/)变量的开头。 此外，您的Shell提示符也会更改，并且会显示您当前正在使用的虚拟环境的名称。 在我们的例子中是`my-project-env`：

```
$ source my-project-env/bin/activate
(my-project-env) $
```

现在虚拟环境已激活，我们可以使用pip开始安装，升级和删除软件包。

让我们使用[Requests](http://docs.python-requests.org/en/master/)模块创建一个简单的Python脚本。

在虚拟环境中，可以使用命令`pip`代替`pip3`，并使用`python`代替`python3`。

第一步是使用Python包管理器pip安装模块：

```
pip install requests
```

要验证安装，您可以尝试导入模块：

```
python -c "import requests"
```

如果导入模块没有错误，则说明安装成功。

在此示例中，我们将使用[ httpbin.org ](https://httpbin.org)网站，该网站提供了简单的HTTP Request响应服务以打印所有标题条目。

打开您的[文本编辑器](https://www.myfreax.com/how-to-install-sublime-text-3-on-ubuntu-18-04/)，然后创建一个新文件：

```
nano testing.py
```

将以下内容粘贴到文件中：

```
import requests

r = requests.get('http://httpbin.org/get')  
print(r.headers)  
```

关闭并保存文件。

我们现在可以通过输入以下内容来运行脚本：

```
python testing.py
```

脚本将打印所有标题条目的字典，如下所示：

```
{'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Date': 'Tue, 18 Sep 2018 16:50:03 GMT', 'Content-Type': 'application/json', 'Content-Length': '266', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true', 'Via': '1.1 vegur'}
```

完成工作后停用虚拟环境，只需键入`deactivate`，您将返回到常规shell。

```
deactivate
```

### 网络监控

#### vnstat

vnStat 是一款开源的网络流量统计工具，可以方便的查看当天，当月流量统计。

官网地址：https://humdi.net/vnstat/

使用参考：https://www.jinnsblog.com/2020/08/vnstat-network-traffic-monitor.html

##### 安装
`sudo apt-get install vnstat`

##### 使用

查看vps网卡名称:
`ip a`

假设网卡名为 eth0，该配置在 /etc/vnstat.conf 中，安装结束后初始化数据库: 
`sudo vnstat -u -i eth0`

添加为开机启动: 
`sudo update-rc.d vnstat enable`

直接输入 vnstat:

```bash
vnstat -l  # 或者 `--live` 实时流量
vnstat -h  # 显示小时流量
vnstat -d  # 显示日流量信息
vnstat -w  # 显示周流量信息
vnstat -m  # 显示月流量信息
vnstat -t  # 显示流量最高top10天
```

图形化输出可以使用 vnstati ，将月流量统计图输出到图片:

```bash
vnstati -i eth0 - -months - -output /dir/month.png
```

## 其他脚本

### telegram-upload

[官网](https://pypi.org/project/telegram-upload/)

#### 安装依赖

**安装 pillow**

`pip install pillow`

> \[!NOTE] 若不安装会出现以下错误： `telethon.errors.rpcerrorlist.PhotoInvalidDimensionsError: The photo dimensions are invalid (hint:`pip install pillow`for`send\_file`to resize images) (causedby SendMediaRequest)`

**安装 ffmpeg**

> \[!NOTE] 上传视频时若没有 ffmpeg 环境，上传时会没有预览图，同时可能出现错误：`ffmpeg command is not available`

安装方法： 参考：[How to Install FFmpeg on Windows](http://blog.gregzaal.com/how-to-install-ffmpeg-on-windows/)

* [官网](http://ffmpeg.org)下载 ffmpeg
* 压缩包解压后重命名文件夹为 `ffmpeg` ，若不重命名，后续添加到系统环境变量会出问题
* 将文件夹的 bin 目录（例：`C:\ffmpeg\bin`）添加到系统环境变量 path 中
* 终端执行 `ffmpeg -version` 验证是否安装成功

#### 安装 telegram-upload

使用命令安装：

`pip install telegram-upload`

安装好后终端执行 `telegram-upload` 进行验证

> \[!IMPORTANT] 在某些环境中，使用 pip 安装后 cmd 无法识别 `telegram-upload` 命令，原因是 `telegram-upload` 没有被安装到 `...\Python39\Scripts\` 路径中，而是被安装到了 `C:\Users\username\AppData\Roaming\Python\Python39\Scripts` 路径中，将 `C:\Users\username\AppData\Roaming\Python\Python39\Scripts` 添加到 path 环境变量即可（该路径在执行 `pip uninstall telegram-upload` 时会出现）
>
> \[!TIP] Linux 中 使用 `sudo cp /home/USERNAME/.local/bin/telegram-upload /usr/local/bin/` 可以避免设置环境变量

#### 使用 telegram-upload

```bash
telegram-upload --to @CHATNAME --directories recursive --caption "" --large-files split PATH
```

## Ubuntu 配置 JavaWeb 运行环境
### 用户设置

添加用户：
```bash
# 添加用户
useradd itcast
# 设置用户口令
passwd itcast
```

把普通用户 itcast 增加到 sudo 组：
```bash
# 切换到root用户
su
# 添加sudo权限
usermod -G sudo itcast 
# /etc/sudoers文件默认是只读的，对root来说也是，因此需先添加sudoers文件的写权限
chmod u+w /etc/sudoers
# 修改 /etc/sudoers 文件
vim /etc/sudoers
```
修改文件如下：

```bash
root ALL=(ALL) ALL
itcast ALL=(ALL) ALL # 添加
```

撤销sudoers文件写权限：

```bash
chmod u-w /etc/sudoers
```

#### 安装 MySQL 5.6.48

下载：
```bash
# /home/itcast/develop 目录下, 创建 mysql 文件夹
cd /home/itcast/develop
mkdir mysql
cd mysql

# 选择版本后下载：https://downloads.mysql.com/archives/community/
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.6.48-linux-glibc2.12-x86_64.tar.gz
```

解压：

```bash
tar -zxvf mysql-5.6.48-linux-glibc2.12-x86_64.tar.gz
```

安装所需依赖：

```bash
sudo apt-get install libaio1
sudo apt-get install libncurses5 libstdc++6
# 32位使用
# sudo apt-get install lib32ncurses5 lib32stdc++6
```

开始安装：

```bash
# 切换回root用户
su
# 添加一个组，名字叫 mysql
groupadd mysql
# 添加一个用户 mysql 到 mysql 组中
useradd -r -g mysql mysql
# 复制准备工作中解压的 mysql 包到 /usr/local
cp /home/itcast/develop/mysql/mysql-5.6.48-linux-glibc2.12-x86_64 -r /usr/local
# 进入/usr/local目录
cd /usr/local
# 给上边复制的解压文件夹添加软链接，起名为mysql
ln -s mysql-5.6.48-linux-glibc2.12-x86_64 mysql
# 进入mvsql目录
cd mysql
# 设置mysql(软连接mysql指向上面解压的文件夹)目录的拥有者和所属组为mysql(刚才创建过的用户和组名)
chown -R mysql .
chgrp -R mysql .
# 执行mysqL的安装脚本
scripts/mysql_install_db --user=mysql
# 重新设置mysql的目录拥有者为root
chown -R root .
# 设置data目录的拥有者为mysql
chown -R mysql data
# 复制配置文件
cp support-files/my-default.cnf /etc/my.cnf
# 启动MySQL
bin/mysqld_safe --user=mysql & # 此时无法输入，执行 pwd 恢复输入
# 初始化root用户密码
bin/mysqladmin -u root password 'root_password'
# 复制 mysql.server 脚本
cp support-files/mysql.server /etc/init.d/mysql.server
# 给mysql命令配置软连接(不想配置环境变量可以这么干,给MySQL配置环境变量就不需要了)，可以全局使用 sql 命令
ln -s /usr/local/mysql/bin/mysql /usr/local/bin/mysql
```

开机自启：

```bash
update-rc.d -f mysql.server defaults
```

取消开机自启：

```bash
update-rc.d -f mysql.server remove
```

运行MySQL,进行测试：

```bash
mysql -uroot -p
show databases;
use mysql;
show tables;
exit
```

查看MySQL状态、启动、关闭、重启mysql服务:

```bash
service mysql.server [status|start|stop|restart]
```

#### 安装 JDK1.7

下载安装：

```bash
# https://www.oracle.com/java/technologies/javase/javase7-archive-downloads.html
# 找到 jdk-7u75-linux-x64.tar.gz 下载
cd /home/itcast/develop/jdk
# 在本地下载后再上传到服务器

# 解压
tar -zxvf jdk-7u75-linux-x64.tar.gz
# 将解压出的文件夹复制到 /usr/local/ 并重命名为 jdk1.7
mv jdk1.7.0_75/ /usr/local/jdk1.7
```

配置环境变量：

```bash
vim /etc/profile

# 末尾添加 
JAVA_HOME=/usr/local/jdk1.7
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH

# 使更改的配置立即生效
source /etc/profile
```

验证安装：

```bash
# 查看JDK版本信息，如果显示出 1.7.0_75 证明配置成功
java -version
```

#### 安装 Tomcat
参考：[如何在 Ubuntu 20.04 上安装 Tomcat 9](https://www.itcoder.tech/posts/how-to-install-tomcat-9-on-ubuntu-20-04/)

##### 安装并配置好JDK
安装配置过程省略。
通过检查 Java 版本来验证它：

```bash
java -version
```


##### 创建一个系统用户
以 root 用户运行 Tomcat 有一个安全风险。我们将会创建一个系统用户和用户组，其主目录为/opt/tomcat。我们将会使用这个用户来运行 Tomcat 服务。
```bash
sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
```

##### 下载并安装 Tomcat
```bash
cd /home/itcast/develop/
mkdir tomcat
cd tomcat/
# 下载： https://tomcat.apache.org/download-80.cgi 中 Binary Distributions -> Core 下的 tar.gz 文件
wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.73/bin/apache-tomcat-8.5.73.tar.gz  
```

解压 tar 文件到/opt/tomcat目录：
```bash
sudo tar -xf apache-tomcat-8.5.73.tar.gz -C /opt/tomcat
```

Tomcat 会定期更新 安全补丁和新功能。想要更好地升级版本和更新，我们将会创建一个符号链接，称为latest,指向 Tomcat 安装目录:
```bash
cd /opt/tomcat/
sudo ln -s ./apache-tomcat-8.5.73/ ./latest
```

稍后，当升级 Tomcat 时，解压新的版本，并且修改符号链接，指向它。

前面创建的系统用户必须对 tomcat 安装目录有访问权限。修改目录归属到用户和用户组 tomcat：
```bash
sudo chown -R tomcat: /opt/tomcat/
```

在 Tomcat bin目录下的 shell 脚本必须可执行：

```bash
sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'
```

##### 创建 SystemD 单元文件
与使用 shell 脚本来启动和停止 Tomcat 服务器相比，我们将会将它作为服务来运行。

打开你的文本编辑器，并且在/etc/systemd/system/目录下创建一个 tomcat.service 单元文件。
```bash
sudo vim /etc/systemd/system/tomcat.service
```

粘贴下面的配置文件（如果你的 Java 安装路径不一样，请修改`JAVA_HOME`环境变量。）：
```bash
[Unit]
Description=Tomcat 8 servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/local/jdk1.7"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"

Environment="CATALINA_BASE=/opt/tomcat/latest"
Environment="CATALINA_HOME=/opt/tomcat/latest"
Environment="CATALINA_PID=/opt/tomcat/latest/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/latest/bin/startup.sh
ExecStop=/opt/tomcat/latest/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```

保存并且关闭文件，通知 systemd 一个新的单元文件存在：

```bash
sudo systemctl daemon-reload
```

启用并且启动 Tomcat 服务：
```bash
sudo systemctl enable --now tomcat
```

检查服务状态：
```bash
sudo systemctl status tomcat
# 按 q 退出查看
```

输出应该显示 Tomcat 服务器已经启用，并且运行了：

```bash
● tomcat.service - Tomcat 8 servlet container
     Loaded: loaded (/etc/systemd/system/tomcat.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2021-11-27 08:01:20 UTC; 1min 6s ago
    Process: 108242 ExecStart=/opt/tomcat/latest/bin/startup.sh (code=exited, status=0/SUCCESS)
   Main PID: 108249 (java)
      Tasks: 18 (limit: 2344)
     Memory: 118.7M
     CGroup: /system.slice/tomcat.service
             └─108249 /usr/local/jdk1.7/bin/java -Djava.util.logging.config.file=/opt/tomcat/latest/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.security.egd=file:///dev/urandom -Djava.aw>

Nov 27 08:01:20 ubuntu-s-1vcpu-2gb-intel-sfo3-01 systemd[1]: Starting Tomcat 8 servlet container...
Nov 27 08:01:20 ubuntu-s-1vcpu-2gb-intel-sfo3-01 startup.sh[108242]: Tomcat started.
Nov 27 08:01:20 ubuntu-s-1vcpu-2gb-intel-sfo3-01 systemd[1]: Started Tomcat 8 servlet container.
```

你可以 像其他 systemd 服务一样 启动，停止和重启 Tomcat：

```bash
sudo systemctl start tomcat
sudo systemctl stop tomcat
sudo systemctl restart tomcat
```

##### 配置防火墙

如果你的服务器被防火墙保护，并且你想从外面访问你的Tomcat，你需要打开8080端口。

使用下面的命令打开必要的端口：

```bash
sudo ufw allow 8080/tcp
```

通常，当在生产环境运行 Tomcat 时，你应该是使用一个负载均衡，或者反向代理服务器。这是仅仅允许从你的本地网络访问`8080`端口的最佳实践。

##### 配置 Tomcat 网页管理界面
略。

##### 测试 Tomcat 安装
打开你的浏览器，输入：http://<your_domain_or_IP_address>:8080
假设安装成功，会出现 Tomcat 界面。

Tomcat 网页应用管理在：
http://<your_domain_or_IP_address>:8080/manager/html

Tomcat 虚拟主机管理在：
 http://<your_domain_or_IP_address>:8080/host-manager/html

#### 安装 Redis

ubuntu安装redis，可以通过源码make编译安装，也可以通过apt-get安装，apt-get安装更加简便。
本文采用的就是apt-get安装。

##### 安装

```bash
sudo apt-get install redis-server
```

##### redis配置

修改redis.conf。该文件路径默认安装地址为：/etc/redis/redis.conf

打开文件。（文件内容太多，查找不方便。在normal模式下按下/即可进入查找模式。vim命令行模式下查找 ：/** 。就可以查找了** 。** 就是我们需要查找的内容。比如：/127.0.0.1 就可以快速查找到127.0.0.1。，输入要查找的字符串并按下回车。Vim会跳转到第一个匹配。按下n查找下一个，按下N查找上一个。）

修改第一处：注释掉这个 bind 127.0.0.1，就能远程访问redis。
修改第二处：修改redis.conf将daemonize属性改为yes。这样redis-server就能后台运行了。
修改第三处：如果要给redis设置密码，就需要取消下图这块的注释，引入redis密码,设置为123456。

在文件中，找到supervised指令。 该指令允许您声明一个init系统来管理Redis作为服务，从而为您提供对其操作的更多控制。 受supervised指令默认设置为no 。 由于您正在运行使用systemd init系统的Ubuntu，请将其更改为systemd。

修改后，保存。重启redis服务：

```bash
sudo service redis restart
```

#### Redis 服务端控制

启动 redis 服务：

```bash
sudo service redis start
```

查看 Redis 的运行状态：

```bash
sudo systemctl status redis
```

关闭 redis 服务：
```bash
sudo service redis stop
```

重启 redis 服务：


```bash
sudo service redis restart
```

##### redis 客户端连接
在命令行中输入如下命令来登陆进 redis 客户端：
```bash
redis-cli
# 如果设置了密码： redis-cli -a your_password
```

#### 查看端口使用状态

```bash
apt-get install net-tools
netstat -ntpl
```





















