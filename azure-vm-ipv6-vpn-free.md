**免费**本地IPV6走代理IPV4+IPV6，实现本地IPV6连接和代理外网，**每月100G流量**。

IPV6、校园网相关知识参考：[参考前几段中文](https://github.com/Toothbrush-Lee/Free-IPv6)

要求：

1. edu邮箱
2. 2-3小时左右

### 1.注册azure学生优惠（需要edu邮箱）

[白嫖微软学生福利——通过Azure学生订阅创建一台免费的云服务器（每年可续期）-CSDN博客](https://blog.csdn.net/2301_79518550/article/details/147282995)

参考两个链接前几步，获得微软Azure的学生资格（100美元，12个月免费服务，每年可认证一次）。**注意**：注册学生资格时务必**关闭代理**，否则会非常难受。

### 2.创建虚拟机及配置（ipv4 + ipv6）

参考链接：[白嫖微软学生福利——通过Azure学生订阅创建一台免费的云服务器（每年可续期）-CSDN博客](https://blog.csdn.net/2301_79518550/article/details/147282995)如何创建一台免费的Linux虚拟机。

服务器地区可以选**east asia**（香港）或者**jpan east**(日本)，香港代理无法使用chatgpt，最好选择日本，当然其它区域的节点也是可以的，延时其实差不太多。

创建具体流程如下：

![image-20250710084928871](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710084928871.png)

![image-20250710085009490](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710085009490.png)

![image-20250710085038097](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710085038097.png)

![image-20250710085952181](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710085952181.png)

![image-20250710085112806](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710085112806.png)

![image-20250710085131044](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710085131044.png)

![image-20250710085628246](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710085628246.png)

创建完成后，因为SKU不在支持基本（basic），只能设置标准（Standard），可能会扣费，但扣的很少，注册学生的100美元完全够用。

现在需要给虚拟机添加IPV6，并设置入站规则。

**设置IPV6：**

![image-20250710091505480](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710091505480.png)

![image-20250710091420658](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710091420658.png)

![image-20250710091310617](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710091310617.png)

添加入站规则：

虚拟机-->网络->网络设置->创建端口规则->入站规则

![image-20250710091916864](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710091916864.png)

以上，虚拟机部分完成。

通过ssh在本地windows上连接虚拟机，左下角开始菜单栏搜索**终端**并打开。返回虚拟机界面，找到连接->本地SSH->3 复制并执行SSH命令，按照上面保存的密钥路径和名称，如：~/.ssh/linux_vm_key.pem，把上面这个路径复制到**3 复制并执行SSH命令**里，点下面复制按钮（使用指定私钥通过 SSH 连接到 VM。），将这段代码粘贴到刚刚windows本地打开的终端里，成功连接虚拟机。

依次运行下面代码：（参考：[实现校园网IPv6免流量上网与科学上网 | V2ray教程：X-ui与v2rayN ~ 极星网](https://www.jixing.one/vps/v2ray-xui-v2rayn/)大佬用的不是azure，虚拟机服务器也不是ubuntu系统，因此代码需要改动，但流程一样）

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

然后，回到虚拟机页面，记录概述里的公共IP地址和公共IP地址(IPV6)，本地windows打开浏览器。

![image-20250710093331876](C:\Users\GA\AppData\Roaming\Typora\typora-user-images\image-20250710093331876.png)

默认用户名和密码：admin

后面流程参考：[实现校园网IPv6免流量上网与科学上网 | V2ray教程：X-ui与v2rayN ~ 极星网](https://www.jixing.one/vps/v2ray-xui-v2rayn/)中的5和6，**区别在于IPV6地址刚刚已经记录，不需要再查看了。**

### 3.测试

测试之后延迟很低，手机、电脑、Linux都能使用，非常稳定，比之前买的机场代理服务器稳定的多得多的多。
