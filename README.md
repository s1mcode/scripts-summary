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

#### Oracle 甲骨文

参考：localhost

<mark style="color:red;">注意：dd 前先在 Oracle 机器面板中更改子网中的安全列表。</mark>

root用户下，安装运行库：

```
apt-get update
apt-get install -y xz-utils openssl gawk file
```

dd 脚本：

```
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd 'http://d.nat.ee/win/lite/win7-ent-sp1-x64-cn/win7-ent-sp1-x64-cn-efi.vhd.gz'
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

```bash
apt install firewalld -y               # 安装 firewalld
firewall-cmd --state                   # 查看防火墙状态
systemctl stop firewalld.service       # 停止防火墙
systemctl disable firewalld.service    # 禁止防火墙开机自启
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


