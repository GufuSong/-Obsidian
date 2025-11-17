# 一. VPS 

## $.1 网络通信基础 :

### &.1 计算机网络通信基础 :

**一. 基础信息 :**

**1. Internet Protocol Address IP :**
- 全称: 互联网协议地址 .
- 作用: 唯一标识一台联网设备（主机、手机、路由器等）的“网络身份” .
- 类型: IPv4（如 192.168.1.100）、IPv6（如 2408:xxxx::abcd）
- 用途: 用于**数据包的寻址和路由**，类似快递单上的收件人详细地址

**2. MAC（Media Access Control Address）:**
- **全称**：媒体访问控制地址
- **作用**：标识网卡的“物理地址”，世界唯一
- **格式**：通常写成 `AA:BB:CC:DD:EE:FF`（16进制）
- **用途**：用于**局域网内设备的识别和通信**，交换机靠MAC寻址帧 .  局域网内所有设备都能看到广播包（MAC为FF:FF:FF:FF:FF:FF），但只有目标MAC的设备响应。
- **特性**：由硬件厂商烧录在网卡芯片里

>帧结构(以太网): `[目标MAC 6字节][源MAC 6字节][类型2字节][数据][校验]`

**3. GW 与NAT（Gateway）:**
- **全称**：默认网关（Default Gateway）, 本质是指向路由器的地址, 填写路由器的ip.
- **作用**：
	- 局域网内设备访问外部网络的出口地址 ,  
	- 当你的电脑要访问一个**不在本地子网**（比如外网、其他局域网、互联网）的IP时，数据包首先被发给“默认网关”。
	- 网关负责把这些数据包“转发”到其他网络，直到到达目标。
- **NAT**
	- 家用网关（路由器）常带NAT功能，把内网IP转为公网IP，实现全网设备共享一个公网IP上网 .
- **用途**：
	- 网关(形式上)的本质就是内网中的一台设备 ,  它的IP地址就是"内网IP" 如果目标IP不在本地网段，数据包会被发给“网关”，由它负责继续转发 .
	- 网关（Default Gateway）本质上是一台网络设备（通常是路由器或三层交换机）上运行的功能，而不是在手机、电脑等终端设备上运行。
- **其它特性:**
	- 防火墙/访问控制: 网关常带有访问控制、端口过滤、防火墙、QoS等高级网络管理功能。
	- DHCP/分配管理: 路由器一般也是DHCP服务器，为内网设备分配IP和网络参数。

**4. DNS（Domain Name System）:**
- **全称**：域名系统（域名服务器）,作用在DNS服务器.
- **作用**：**负责把域名（如` www.baidu.com`）转换成IP地址** , 在本地填写DNS ,  
- **常见DNS服务器**：常见如 8.8.8.8（Google DNS）、114.114.114.114（中国电信）
- **用途**：
	- 浏览器先查询 DNS 缓存或本地配置的解析器。
	- 如果解析器也不知道，就会向根服务器（Root Server)发起请求，问：“请问`.com`的服务器在哪里？”  
根服务器不会直接给你 `example.com` 的 IP，而是提供 `.com` 顶级域的 DNS 服务器地址
- **比喻**：互联网的“电话簿”或“查号台”

**5. SubSubnet Mask :**
- **全称**: 子关掩码
- **作用**: 子网掩码帮助设备判断哪些 IP 应该直接局域网内通信，哪些需要发给网关转发对任意 IP 地址和子网掩码执行 .   **按位与（AND）运算**，可以得到该 IP 所属的网络地址 .
- **用途**: 
	- **定义网络范围**：让设备本地认识「谁是在本网络里」，“谁需要找网关上网”
	- **隔离广播域**：减少广播浪费和冲突，提高网络性能。
	- **支持子网划分**：通过调整掩码长度（如 /24、/26、/30），灵活划分子网，满足不同规模网络需求。
	- 子网掩码的作用是**将一个较大的网络划分成多个更小的子网**



**二. OSI模型 :**

**1. 运行逻辑 :**
- 每层路由器接受上一级IP,MAC ,  并存储记录 ,  并输出自身IP,MAC ,  此逻辑形成了互联网的物理基础 .

**2. 模型逻辑 :**
![[OSI.canvas]]
### &.2 GFW (Great Firewall) 数据跨境安全网关

**一. 访问国际互联网的流程 :**

**1. DNS 查询流程:**
![[DNS运行逻辑.canvas]]
**二. GFW 阻拦方法 - DNS污染 :**

**1. DNS 污染 :**
- 在输入GOOGLE 域名时 ,  GFW会对内容进行解析 ,  如果为国际域名, 则阻拦解析 .
- 若没有阻拦解析 ,  则此域名会访问国际DNS服务器 ,  并返回IP地址 .  
- 在返回的过程中 ,  GFW会拆出输出包 ,  对返回的IP进行检测 .
- 如果为黑名单IP ,  则对数据进行修改 ,  将正确的IP 修改为 错误的IP , 如`8.8.8.8 -> 8.8.8.9` .
- 篡改的IP会返回你的客户端NDS服务器 ,  服务器会使用错误的IP进行连接 ,  导致浏览器一直转圈圈 .

**2. 应对措施 :**
- 由于此方法本质为修改DNS服务器返回的IP地址 ,  也就是DNS请求引起的 .  解决方案为 不发送DNS请求 . 

**3. 本地 Hosts 缓存法 :**
- 由于本地DNS 服务器会先访问本地Hosts 缓存 ,  因此 ,  只需要将国际互联网的关键域名的IP信息存储到本地即可解决问题 .
- host 位置为 C:\Windows\System32\drivers\etc\hosts .

**三. GFW阻拦方法 - 网络层IP地址检测 :**

**1. 网络层IP检测 :**
- **扼杀数据包** :  假设请求绕过了DNS污染 ,  GFW会检测网络层的IP地址 ,  并与IP黑名单进行校对 ,  如与黑名单相同 ,  则扼杀数据包 ,  导致无法与国际互联网达成TCP 握手协议 ,  达到阻拦公民访问国际互联网的目的 .
- **TCP重置攻击** :  在检测IP为黑名单IP时 ,  GFW 也会通过NAT 修改IP地址并返回伪造信息包 .  比如将传输请求的信息包伪造为"滚蛋" ,  于是你的机器就不再与Google 进行连接了 .  

**四. GFW阻拦方法 - 应用层解析请求检测 :**
- **应对国际互联网服务器多服务器的多IP情况** :  在客户端方位多IP的国际互联网时 ,  此IP不在GFW的IP黑名单中 ,  则GFW不会阻拦此数据包 .  此请求也会顺利建立TCP 连接协议成功握手 .  但是在GFW的应用层是 ,  GFW会解析最终的内容包的行为 ,  并与黑名单中的相似行为进行检测 ,  如发现是黑名单行为 ,  则会在建立`TCP`连接的情况下扼杀此数据包 .

**五. GFW阻拦方法 - 阻拦代理请求 :**
- **针对`HTTP`和`socks5`代理的内容包行为检测** , 通过 **深度包检测（DPI）识别敏感流量特征（如 HTTP 头、TLS SNI、关键词等）**，再 **注入伪造的 TCP RST 包、中断连接** 或 **进行 DNS 劫持/IP 阻断**，对跨境通信进行实时监控与封锁 .

### &.3 GFW 应对方法 

**一. 应对概念 :**

**1. 从数据包意图上 :**
- 归根结底 ,  GFW 阻拦链接的最终原因是数据包的意图被GFW得知 .

**2. 私密性不够 :**
- VPN 协议是可以加密数据流量 ,  如IPSec ,  OpenVPN 协议, 可以对数据进行加密 .  
- 但是缺陷为 ,  VPN 虽然可以加密数据内容 ,  但特征过于明显 ,  也就是GFW可以直接的得知你使用了VPN加密协议 .
- 虽然VPN的本质目的为合并企业的内网 ,  但是GFW会监控VPN连接是IP地址与流量 ,  在敏感时期会阻断监控的VPN连接 .

## $.2 对抗 GFW 的协议

### &.1 `shadowsocks` 传输协议

**一. 概述 SS 协议 :**

**1. 存在目的 :**
- SS 的存在目的为躲避GFW .  

**2. 运行原理 :**
- SS服务端 运行监听端口 `8388` .  
- 客户端使用`v2RayN`或`Shadowsocks` .  软件会监听本地的传输端口 ,  如`socks5 的 1080` 端口 ,  同时浏览器设置了相应的代理端口 (浏览的信息会优先走请求的这个端口 .)  
- 由于软件在监听此端口 ,  因此浏览器的请求会优先发送到软件客户端 .  软件会对这个请求使用对内容进行加密(如`aes-256-gcm`) .
- 被加密的内容包传输ISO模型 ,  进行互联网传输 .  
- 为了躲避GFW的IP黑名单 ,  我们必须配置一台VPS服务器 ,  这个数据包会在VPS服务器上进行解密 ,  并由VPS服务器执行被加密的线行为 ,  访问国际互联网.
- 国际互联网的相应会返回值你的VPS服务器 ,  VPS服务器再将数据包加密返回 ,  并通过GFW返回至你的个人ss客户端 ,   ss客户端对数据进行解密 ,  并返回给浏览器 ,  显示国际互联网 .  

**3. GFW 主动检测系统 :**
- GFW会主动向境外VPS服务器进行探针访问 ,  通过返回结果检测你的VPS服务器是否使用了SS协议 ,  如果你的VPS服务器被探针检测成功 ,  则会将你的VPS服务器IP列入黑名单 .
- **重放攻击:** GFW会存储你的数据包 ,  并使用你的数据包发送至VPS ,  探测是否使用了ss .


**二. SS Plugin 插件伪装 :**

**1. 运行原理 :**
- 在数据包中加入`http`协议头进行伪装 ,  可以防止GFW向服务器发送探针 . 

### &.2 `trojan`传输协议

**一. 概述`trojan`协议 :**

**1. 存在目的 :**
- trojan 天生就是讲数据伪装为https 流量来进行数据加密的协议


**二. 概述https :

**1. HTTPS 简述 :**
- https 就是 HTTP + TLS  ,  TLS 负责加密 ,  HTTP生成请求后 ,  经过 TLS ,  变成一串加密后的数据 .  加密后的数据无法被 GFW 篡改 .  
- 一个网站使用HTTPS 访问时 ,  必须要有一个证书 .  此证书需要在CA机构那里去申请  .  
- 申请证书之前 ,  需要先具有公钥与私钥 ,  CA只会根据公钥颁发证书 ,  私钥则始终保存在服务器上 .  和公钥一起配合加密与解密 .
- 在传输过程中 ,  只会使用公钥进行加密 ,  只有对应的私钥可以解密 .  并且通过数字签名 ,  你的私钥可以对消息进行签名 ,  证明消息确实为由你发送的 .

**2. 非对称加密算法 :**
- 公钥根据私钥生成 ,  私钥自行生成 .
- 公钥面向互联网 ,  私钥储存在服务器中 ,  严格保密 .

**3. HTTPS 申请流程 :**
- 先生成一个私钥 ,  并将使用私钥生成的公钥发送至GA .
- GA需要令我们证明域名为自身的 .  
- 证书存储 颁发至xx的域名信息 ,  颁发机构 ,  有效期 ,  公钥 .
- 有证书才能建立https连接 .

**4. HTTPS 连接流程 :**
- 在HTTP阶段生成数据包 ,  并将数据包发送给TLS .
- TLS 与服务器进行握手连接 ,  服务器会返回 HTTPS 证书 .
- TLS 会对证书进行验证 ,  根据电脑中的 受信任颁发机构 ,  与其他信息 .
- 证书验证通过验证后 ,  根据证书 ,  与服务器返回的对称或非对称算法进行加密 .  
- 加密后的数据会传输至互联网 ,  GFW 无法读写里面的内容 .  
- 数据包到达VPS服务器后 ,  会使用私钥对数据解密 ,  并获得相应信息 .

**5. SNI 加密 :**
- TLS 1.2 不支持对SNI 加密 ,  1.3 可以 ,  也就是加密为ESNI .  
- GFW 会直接扼杀TLS 1.3 加密的数据 ,  因此此方法在中国行不通 .


**三. Trojan 传输协议与原理 :**

**1. 适配Trojan 的VPS 服务器 :**
- 如果需要使用Trojan 对抗GFW ,  则需要为服务器申请 HTTPS证书 .  开启HTTPS 的访问 .
- HTTPS 的默认端口为 443 ,  如果改变端口 ,  则会降低伪装性 .  
- Trojan 也可以定义密码 ,  此密码不被用来加密 ,  而是用于身份验证 .
- 如果客户端发来的地址不正确 ,  则跳转至指定地址 .

**2. Trojan 客户端运行原理 :**
- 在HTTP 协议输出至TLS 之前 ,  添加Trojan的协议头 .  
- 使用TLS 加密 ,  连带Trojan的协议头与内容 .
- 加密后的数据包会与 VPS 服务器进行握手 . 
- 服务器会把证书发送会客户端 .  并将内容与证书进行核对 .
- 核对通过后 ,  TLS 会使用 证书上的公钥将数据包加密 .  只会加上公开的VPS网址 .
- VPS 服务器的IP 可以通过GFW 的审查 ,  并会接受到内容包 .  
- VPS 的私钥会对内容进行解密 ,  露出Trojan协议的内容包 .
- 使用Trojan 协议进行处理 ,  经过密码检测后 ,  将Trojan协议头删除 ,  仅剩客户端的原始请求 .
- Trojan 会将请求发送给目标网站 ,  并成功发送请求 .
- 假设Trojan协议密码错误 ,  则不会转发流量 ,  并返回定义服务器的流量 .  此操作可以规避GFW的探针攻击 .

### &.3 搭建 Trojan 服务器

>请自行获得 VPS 资源 .

**一. 搭建 Ubuntu 服务器 :**

**1. 配置Trojan :**
- 使用`Trojan-go` 搭建VPS .  由于Trojan-go没有内置到Ubuntu 中 ,  因此需要手动下载 .[https://github.com/p4gefau1t/trojan-go](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbEx3aXp3OTZkT1Q2MmhnS0QxX1N2X0poRndsQXxBQ3Jtc0tsblluMWxyaTZRMVZRMjBzdEw5czBYcEtMbTJoZS1RQ1UxODN1QkY2RzkzMWJ5SGFYOGpIYWJFNjZ5bFNSQkNTc2Q1QzF5azFPUTNEX1g2eWxYenp5ZHI0bExMM1EwcVVsakduc3JzOHpTR0M3UGQ1RQ&q=https%3A%2F%2Fgithub.com%2Fp4gefau1t%2Ftrojan-go&v=gw2Vl1h89Wo)
- `mkdir trojan`  创建trojan文件夹
- `cd trojan` 进入trojan 文件夹
- `wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.10.6/trojan-go-linux-amd64.zip` 下载压缩包
- `unzip trojan-go-linux-amd64.zip` 解压压缩包
- `is` 查看目录文件 .
- `ll`切换列表形式 .
- 绿色的即为可执行文件 ,  调用可执行文件 .
- `./trojan-go` 调用文件
- 在trojan目录下创建名为`config.json`的配置文件
```json
{ 
	"run_type": "server",
	"local_addr": "0.0.0.0", 
	"local_port": 443, 
	"remote_addr": "192.83.167.78", //trojan连接失败时,访问什么ip
	"remote_port": 80, 
	"password": [ 
		"your_awesome_password" //输入你的密码 
	], 
	"ssl": { 
		"cert": "server.crt", //证书文件
		"key": "server.key" //私钥文件
	} 
}

```
- `./trojan-go -config config.json` 使用相应配置文件启动服务

**2. 申请HTTPS 证书 - 自签证书 :**
- `cur https://get.acme.sh | sh` 安装acme . 此步骤可能需要更新`curl` 
```更新curl
sudo apt update
sudo apt install curl
curl --version
```
- `apt install socat` 安装socat
- `ln -s /root/.acme.sh/acme.sh /usr/local/bin/acme.sh` 添加软链接
- `acme.sh --register-account -m my@example.com`注册账号
- `ufw allow 80` 开放80 端口 .
- `ufw allow 443`放行443 端口 .
- ==有域名则正常申请证书 .==
- `acme.sh --issue -d 你的域名 --standalone -k ec-256` 申请证书
- 如果CA无法颁发 ,  可以切换下列CA:
```
切换 Let’s Encrypt：acme.sh --set-default-ca --server letsencrypt 
切换 Buypass：acme.sh --set-default-ca --server buypass 
切换 ZeroSSL：acme.sh --set-default-ca --server zerossl
```

- `acme.sh --installcert -d 你的域名 --ecc --key-file /root/trojan/server.key --fullchain-file /root/trojan/server.crt`安装完整证书链 .
- ==无域名自签证书==
- `openssl ecparam -genkey -name prime256v1 -out ca.key`生成私钥
- `openssl req -new -x509 -days 36500 -key ca.key -out ca.crt -subj "/CN=bing.com"`生成证书
- 在config.json文件中更改私钥与证书位置

- `./trojan-go`最终, 运行trojan-go文件即可 .

**3. 无证书客户端配置 :**
- `certmgr.msc` 控制证书是否受信任 .

**4. 令`trojan`自行运行 :**
- `nohup ./trojan-go > trojan.log 2>&1 &` 令`trojan`后台运行 ,  并输出日志至`trojan.log` 


```
curl https://get.acme.sh | sh; apt install socat -y || yum install socat -y; ~/.acme.sh/acme.sh --set-default-ca --server letsencrypt


~/.acme.sh/acme.sh --issue -d 66-42-55-6.nip.io --standalone -k ec-256 --force --insecure



~/.acme.sh/acme.sh --install-cert -d 66-42-55-6.nip.io --ecc --key-file /etc/x-ui/server.key --fullchain-file /etc/x-ui/server.crt
```