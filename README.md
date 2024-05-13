### 前言
本项目环境：
- eNSP V1.3.00.100
- VirtualBox-5.2.28-130011-Win
- Wireshark-win64-3.0.1

模拟器等环境搭建可参考网上资源。

【注】项目使用的防火墙需要导入防火墙包才能正常启动，由于文件过大不方便上传可自行网上查找。

### 网络规划
学校网络分为三层，接入层、汇聚层、核心层。

核心层接入运营商网络。

各设备IP地址如下表所示。
| 设备号 | 接口   | IP地址         | 子网掩码      |
| ------ | ------ | -------------- | ------------- |
| AR1    | G0/0/2 | 200.200.200.1  | 255.255.255.0 |
| AR1    | G0/0/0 | 192.168.60.100 | 255.255.255.0 |
| AR1    | G0/0/1 | 192.168.70.100 | 255.255.255.0 |
| AR2    | G0/0/2 | 200.200.201.2  | 255.255.255.0 |
| AR2    | G0/0/0 | 10.10.10.1     | 255.255.255.0 |

Vlan划分如下表所示

| VLAN号 | 区域子网        | 区域网关       |
| ------ | --------------- | -------------- |
| VLAN10 | 192.168.10.0/24 | 192.168.10.254 |
| VLAN20 | 192.168.20.0/24 | 192.168.20.254 |
| VLAN30 | 192.168.30.0/24 | 192.168.30.254 |
| VLAN40 | 192.168.40.0/24 | 192.168.40.254 |
| VLAN50 | 192.168.50.0/24 | 192.168.50.254 |

学校网络拓扑图如下图所示
![屏幕截图 2024-02-23 153252](https://github.com/IsNoooo/Network/assets/88282069/3e24b2d2-30a5-4f6d-a81b-82d8898f87aa)


### 使用到的网络技术
1、VLAN（虚拟局域网）技术
虚拟局域网，它可以划分网络区域，学校分为几个不同的区域所以可以使用这个技术来划分网络，它可以保证网络的稳定性，并且不同的虚拟网之间还不能之间访问，这样也可以保证不同虚拟局域网用户的数据安全。

2、DHCP（动态主机配置协议）技术
DHCP可以给用户的主机自动的配IP，它的工作原理大致分为四步首先客户机向发送一个包，去寻找可以为它提供IP的服务器，当服务器收到这个包后，服务器会发送一个提供包给客户端，然后客户端收到提供包后会发送一个需求包让服务器为它分配一个地址，最后服务器会发送一个包后给用户分发一个IP地址，DHCP技术可以免去管理人员手动的配置客户端的IP地址节省时间以及更好管理网络。

3、Mstp（多生成树协议）技术
多生成树协议可以很好的防止网络环路，在进行网络规划中为了保证网络的可靠性在重要的设备之间都会设置冗余设备或者冗余链路，这些链路都会造成网络的环路，当数据到达这些链路时不知道走哪条链路，这样会使网络不通，所以必须要使用多生成树协议。

4、VRRP（虚拟路由冗余协议）
VRRP的作用是可以防止单点故障，它可以把多台路由设备在逻辑上结合成一台虚拟的设备，它的优点是当局域网中的三层网络设备发生故障时，不用改之前的网络布局情况，它会自己换到backup路由设备上面，这样就防止网络全部瘫痪。

5、OSPF（开放式最短路劲优先）
OSPF协议可以让路由自动学习其他网段的信息，局域网发生单点故障时它可以自动发送报文去学习新的网段信息，可以保证网络的稳定性。

6、ACL（访问控制列表）
ACL访问控制列表可以用在三层上它可以对报文进行分类，控制数据包是否能访问外网，可以保证网络的安全性。

7、NAT（网络地址转换）
NAT技术可以让私网地址访问公网网，网络分为私网和公网NAT技术可以让私网用户发起访问公网而公网用户不能发起访问私网，这样可以保护公网用户的上网安全。

**以上协议的配置在配置文件代码文件中**
### 结语
有任何问题可在issues中提问。
