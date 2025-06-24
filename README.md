**免费**本地IPV6走代理IPV4+IPV6，实现本地IPV6连接和代理外网，**每月100G流量**。

IPV6、校园网相关知识参考：[参考前几段中文](https://github.com/Toothbrush-Lee/Free-IPv6)

要求：

1. edu邮箱
2. 2-3小时左右

### 1.注册azure学生优惠（需要edu邮箱）

[白嫖微软学生福利——通过Azure学生订阅创建一台免费的云服务器（每年可续期）-CSDN博客](https://blog.csdn.net/2301_79518550/article/details/147282995)

[手把手教你白嫖微软Azure学生免费服务器及配置教程（IPv4+IPv6）_azure ipv6-CSDN博客](https://blog.csdn.net/qq_33177599/article/details/132333921)

参考两个链接前几步，获得微软Azure的学生资格（100美元，12个月免费服务，每年可认证一次）。**注意**：注册学生资格时务必**关闭代理**，否则会非常难受。

### 2.创建虚拟机及配置（ipv4 + ipv6）

参考第二个链接中：[手把手教你白嫖微软Azure学生免费服务器及配置教程（IPv4+IPv6）_azure ipv6-CSDN博客](https://blog.csdn.net/qq_33177599/article/details/132333921)如何创建一台免费的Linux虚拟机，服务器地区可以选east asia（香港）或者jpan east(日本)，香港代理无法使用chatgpt，最好选择日本。创建完成后，因为SKU不在支持基本（basic），只能设置标准（Standard），可能会扣费，但扣的很少，注册学生的100美元完全够用。

创建完成后，点击虚拟机界面里的链接，通过本地ssh连接后，运行以下代码：

运行界面参考：[实现校园网IPv6免流量上网与科学上网 | V2ray教程：X-ui与v2rayN ~ 极星网](https://www.jixing.one/vps/v2ray-xui-v2rayn/)（大佬用的不是azure，虚拟机服务器也不是ubuntu系统，因此代码需要改动，但流程一样）

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

##### 代码后面的配置X-ui面板和配置windows客户端跟上面大佬的步骤一样。

#### 浏览器运行x-ui(ip:54321)

默认用户名和密码：admin

### 3.测试

测试之后延迟很低，手机、电脑、Linux都能使用，非常稳定，比之前买的机场代理服务器稳定的多得多的多。
