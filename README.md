# 云服务器用户数据

## 挖矿脚本部署

### NKN
#### nkndev

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/nkndev/main/nkn-commercial-install.sh
chmod +x nkn-commercial-install.sh
./nkn-commercial-install.sh NKN_address_address
```

#### nkn-VPS镜像更改内部钱包地址

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
### XMR
#### xmrdev

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-dev.sh
chmod +x xmr-dev.sh && ./xmr-dev.sh us-west.minexmr.com:443 855rHV5yHh8hnTkCvd74W62q2dvA8gRDMhbcykuTRgkLdrnoVAiCe5ZZBzJiwe1RWSCPYJKhH6DKyQdxP6Bnr7st2kx7wnn aws01
```

#### xmr-proxy 代理端

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-proxy/xmrig-proxy-build.sh
wait
chmod +x xmrig-proxy-build.sh && ./xmrig-proxy-build.sh
```

#### xmr-proxy 矿工端

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-proxy-mine.sh
wait
chmod +x xmr-proxy-mine.sh && ./xmr-proxy-mine.sh 3.233.245.246:443 aws01-02
```

## 启用 ssh root 用户登陆

```bash
#!/bin/bash
echo root:YOURPASSWORD |sudo chpasswd root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo reboot
```

# VPS 脚本
## DD 系统脚本

### 萌咖 DD 脚本

参考：

[DD Windows 一键脚本（GCP谷歌云Oracle甲骨文Azure微软云OVH云）](https://sunpma.com/137.html)

[Debian/Ubuntu/CentOS 网络重装系统一键脚本](https://sword.studio/218.html)

[网络重装DD一键脚本支持Debian/Ubuntu/CentOS系统](https://www.moxiaojiu.com/2633.html)

#### 安装依赖

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

#### 一键脚本

`wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd '[DD包直连地址]'`

#### GCP 谷歌云

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

### DigitalOcean 特殊 DD 法

参考：

[Digital Ocean 搭建 windows 系统](https://www.jianshu.com/p/b9784e042b28?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

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

> [!TIP]
> 此电脑 -> 右键属性 -> 远程设置 -> 取消框选"仅允许运行使用网络级别身份验证的远程桌面的计算机连接(建议)" 

## `iptables` 的使用
### 安装 `iptables`
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


### 配置 `iptables`
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

## 测速脚本
### SuperBench

来源：[SuperBench.sh 一键测试服务器的基本参数](https://www.oldking.net/350.html)

```bash
wget -qO- sb.oldking.net | bash
```

### 本机 Speed test 测速

```bash
wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
chmod +x speedtest.py
./speedtest.py
```

## 流媒体平台及游戏区域限制测试脚本

来源：[RegionRestrictionCheck](https://github.com/lmc999/RegionRestrictionCheck)

```bash
bash <(curl -L -s https://raw.githubusercontent.com/lmc999/RegionRestrictionCheck/main/check.sh)
```

# 其他脚本
## telegram-upload
[官网](https://pypi.org/project/telegram-upload/)

### 安装依赖

#### 安装 pillow
`pip install pillow`

> [!NOTE]
> 若不安装会出现以下错误：
> `telethon.errors.rpcerrorlist.PhotoInvalidDimensionsError: The photo dimensions are invalid (hint: `pip install pillow` for `send_file` to resize images) (causedby SendMediaRequest)`

#### 安装 ffmpeg
> [!NOTE]
> 上传视频时若没有 ffmpeg 环境，上传时会没有预览图，同时可能出现错误：`ffmpeg command is not available`

安装方法：
参考：[How to Install FFmpeg on Windows](http://blog.gregzaal.com/how-to-install-ffmpeg-on-windows/)

- [官网](http://ffmpeg.org/)下载 ffmpeg
- 压缩包解压后重命名文件夹为 `ffmpeg` ，若不重命名，后续添加到系统环境变量会出问题
- 将文件夹的 bin 目录（例：`C:\ffmpeg\bin`）添加到系统环境变量 path 中
- 终端执行 `ffmpeg -version` 验证是否安装成功

### 安装 telegram-upload

使用命令安装：

`pip install telegram-upload`

安装好后终端执行 `telegram-upload` 进行验证

> [!IMPORTANT]
> 在某些环境中，使用 pip 安装后 cmd 无法识别 `telegram-upload` 命令，原因是 `telegram-upload` 没有被安装到 `...\Python39\Scripts\` 路径中，而是被安装到了 `C:\Users\username\AppData\Roaming\Python\Python39\Scripts` 路径中，将 `C:\Users\username\AppData\Roaming\Python\Python39\Scripts` 添加到 path 环境变量即可（该路径在执行 `pip uninstall telegram-upload` 时会出现）


