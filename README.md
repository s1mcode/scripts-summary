# �Ʒ������û�����

## �ڿ�ű�����

### NKN
#### nkndev

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/nkndev/main/nkn-commercial-install.sh
chmod +x nkn-commercial-install.sh
./nkn-commercial-install.sh NKN_address_address
```

#### nkn-VPS��������ڲ�Ǯ����ַ

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

#### xmr-proxy �����

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-proxy/xmrig-proxy-build.sh
wait
chmod +x xmrig-proxy-build.sh && ./xmrig-proxy-build.sh
```

#### xmr-proxy �󹤶�

```bash
#!/bin/bash
wget https://raw.githubusercontent.com/s1mcode/xmrdev/main/xmr-proxy-mine.sh
wait
chmod +x xmr-proxy-mine.sh && ./xmr-proxy-mine.sh 3.233.245.246:443 aws01-02
```

## ���� ssh root �û���½

```bash
#!/bin/bash
echo root:YOURPASSWORD |sudo chpasswd root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo reboot
```

# VPS �ű�
## DD ϵͳ�ű�
�ο���

[DD Windows һ���ű���GCP�ȸ���Oracle�׹���Azure΢����OVH�ƣ�](https://sunpma.com/137.html)

[Debian/Ubuntu/CentOS ������װϵͳһ���ű�](https://sword.studio/218.html)

[������װDDһ���ű�֧��Debian/Ubuntu/CentOSϵͳ](https://www.moxiaojiu.com/2633.html)

### ��װ����

����ϵͳ��
```bash
sudo -i
apt-get update
```

��Ҫ������

```bash
#Debian/Ubuntu:
apt-get install -y xz-utils openssl gawk file

#RedHat/CentOS:
yum install -y xz openssl gawk file wget
```

### һ���ű�
`wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd '[DD��ֱ����ַ]'`

### GCP�ȸ���
GCP�ȸ��ơ�OVH�Ʒ���������Ҫָ�����������װ�ķ�ʽ��ʾ��ΪGCP�ȸ���

```bash
# ��X.X.X.X�滻Ϊ�Լ����������.
# --ip-addr :IP Address/����IP��ַ
# --ip-mask :Netmask   /��������
# --ip-gate :Gateway   /����
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr X.X.X.X --ip-mask X.X.X.X --ip-gate X.X.X.X -dd 'DD�� ֱ����ַ'
```

dd win7 ʾ����
```bash
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr 10.128.0.5 --ip-mask 255.255.255.0 --ip-gate 10.128.0.1 -dd 'https://oss.sunpma.com/Windows/Whole/Win10_LTSC_64_Administrator_nat.ee.gz'
# �˻�: Administrator
# ����: nat.ee
```
dd win10 ʾ��
```bash
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh --ip-addr 10.138.0.5 --ip-mask 255.255.255.0 --ip-gate 10.138.0.1 -dd 'https://oss.sunpma.com/Windows/Whole/Win7_sp1_64_Administrator_nat.ee.gz'
# �˻�: Administrator
# ����: nat.ee
```


















