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











