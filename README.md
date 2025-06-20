# free-ipv6-university

### 1.注册azure学生优惠（需要edu邮箱）

[白嫖微软学生福利——通过Azure学生订阅创建一台免费的云服务器（每年可续期）-CSDN博客](https://blog.csdn.net/2301_79518550/article/details/147282995)

[手把手教你白嫖微软Azure学生免费服务器及配置教程（IPv4+IPv6）_azure ipv6-CSDN博客](https://blog.csdn.net/qq_33177599/article/details/132333921)

### 2.创建虚拟机及配置（ipv4 + ipv6）



[将双堆栈网络添加到现有虚拟机 - Azure Virtual Network | Microsoft Learn](https://learn.microsoft.com/zh-cn/azure/virtual-network/ip-services/add-dual-stack-ipv6-vm-portal?tabs=azureportal)

#### ssh连接虚拟机运行代码：

##### 1.更新系统并安装依赖
sudo apt update -y && sudo apt upgrade -y

sudo apt install -y curl socat wget

##### 2.使用 wget 或直接下载运行脚本
wget -qO- https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh | sudo bash

##### 3.开放防火墙端口
sudo ufw allow 54321/tcp
sudo ufw allow 12345/tcp
sudo ufw allow 22/tcp
sudo ufw enable  # 如果未启用防火墙
sudo ufw reload

##### 4.启用 BBR
echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

##### 5.检查 BBR 是否生效
lsmod | grep bbr

##### 6.重启
sudo reboot

#### 浏览器运行x-ui(ip:54321)

默认用户名和密码：admin



### 3.使用X-ui和v2rayN进行虚拟机代理

[实现校园网IPv6免流量上网与科学上网 | V2ray教程：X-ui与v2rayN ~ 极星网](https://www.jixing.one/vps/v2ray-xui-v2rayn/)
