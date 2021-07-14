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
参考：

[DD Windows 一键脚本（GCP谷歌云Oracle甲骨文Azure微软云OVH云）](https://sunpma.com/137.html)

[Debian/Ubuntu/CentOS 网络重装系统一键脚本](https://sword.studio/218.html)

[网络重装DD一键脚本支持Debian/Ubuntu/CentOS系统](https://www.moxiaojiu.com/2633.html)

### 安装依赖

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

### 一键脚本
`wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd '[DD包直连地址]'`

### GCP谷歌云
GCP谷歌云、OVH云服务器等需要指定网络参数安装的方式，示例为GCP谷歌云

```bash
# 将X.X.X.X替换为自己的网络参数.
# --ip-addr :IP Address/内网IP地址
# --ip-mask :Netmask   /子网掩码
# --ip-gate :Gateway   /网关
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr X.X.X.X --ip-mask X.X.X.X --ip-gate X.X.X.X -dd 'DD包 直链地址'
```

DD win7 示例：
```bash
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr 10.128.0.5 --ip-mask 255.255.255.0 --ip-gate 10.128.0.1 -dd 'https://oss.sunpma.com/Windows/Whole/Win10_LTSC_64_Administrator_nat.ee.gz'
# 账户: Administrator
# 密码: nat.ee
```
DD win10 示例
```bash
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr 10.138.0.5 --ip-mask 255.255.255.0 --ip-gate 10.138.0.1 -dd 'https://oss.sunpma.com/Windows/Whole/Win7_sp1_64_Administrator_nat.ee.gz'
# 账户: Administrator
# 密码: nat.ee
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


