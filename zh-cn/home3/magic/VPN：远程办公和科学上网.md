

# VPN：远程办公和科学上网



## 1.PPTP 简单易用的老牌选手

大多数设备都支持PPTP，因为设置起来非常简单。但是，速度是以弱加密性为代价的。在所有协议中，PPTP加密级别最低。简单来说，如果你只关心速度，那么PPTP就是你的首选。

```bash
apt-get update -y
apt-get install pptpd -y
# pptpd服务配置
vim /etc/pptpd.conf
vim /etc/ppp/pptpd-options
vim /etc/sysctl.conf
# 用户密码
vim /etc/ppp/chap-secrets
sysctl -p
iptables-save
#查看网络状态
apt install net-tools
netstat -nap | grep pptpd

# 运行控制
service pptpd status
service pptpd start
service pptpd restart
service pptpd stop

# 服务状态
systemctl status pptpd
systemctl status ipsec
systemctl status xl2tpd
 
```

## 2.OpenVPN 开源受欢迎

它是一个开源协议，这意味着编程人员可以修改协议以及仔细检查源代码中可能的漏洞。

OpenVPN使用SSL技术，几乎所有平台都可以使用，包括Windows，Linux，iOS，Android，macOS，Blackberry和路由器。它在第2层和第3层上运行，而且还包含有助于传输IPX数据包和以太网帧的额外功能。此外，它还具有NetBIOS功能，可与HTTPS共享端口443。

OpenVPN非常安全，因为它使用160位SHA1哈希算法，AES的256位密钥加密以及2048位的RSA身份验证。

当然，以上也表示OpenVPN有一个显著的弱点——延迟，对于日常操作来说，操作之间的延迟相当大。不过，通过使用更强大的计算机和SSL证书的利用，可以有效降低延迟。



## 3.L2TP/IPsec 安全性与灵活性的平衡

L2TP只是一种隧道协议，既不提供加密也不保证隐私。由于缺乏加密，L2TP不能单独作为安全协议使用，必须与IPsec配对，IPsec是一种带有加密功能的安全协议。L2TP和IPsec协议的组合使用带来一种双封装的概念。（在双封装中，第一个封装将创建客户端到远程主机的PPP连接，第二个封装将使用IPsec）

L2TP支持AES 256加密算法——可以说是最安全的——它可以防止中间人攻击。

请记住，由于双重封装，协议的速度较慢。此外，L2TP协议只能通过UDP协议进行通信。一旦UDP协议受到限制，会极大影响L2TP。

#### 安装说明

首先，更新你的服务器：运行 `sudo apt-get update && sudo apt-get dist-upgrade` (Ubuntu/Debian) 或者 `sudo yum update` 并重启。这一步是可选的，但推荐。

要安装 VPN，请从以下选项中选择一个：

**选项 1:** 使用脚本随机生成的 VPN 登录凭证（完成后会显示）。

```
wget https://get.vpnsetup.net -O vpn.sh && sudo sh vpn.sh
```

**选项 2:** 编辑脚本并提供你自己的 VPN 登录凭证。

```
wget https://get.vpnsetup.net -O vpn.sh
nano -w vpn.sh
[替换为你自己的值： YOUR_IPSEC_PSK, YOUR_USERNAME 和 YOUR_PASSWORD]
sudo sh vpn.sh
```

**注：** 一个安全的 IPsec PSK 应该至少包含 20 个随机字符。

**选项 3:** 将你自己的 VPN 登录凭证定义为环境变量。

```
# 所有变量值必须用 '单引号' 括起来
# *不要* 在值中使用这些字符：  \ " '
wget https://get.vpnsetup.net -O vpn.sh
sudo VPN_IPSEC_PSK='你的IPsec预共享密钥' \
VPN_USER='你的VPN用户名' \
VPN_PASSWORD='你的VPN密码' \
sh vpn.sh
```

## 4.IKEv2/IPsec 移动端支持

IKEv2是一种提供安全密钥交换会话的隧道协议。该协议是微软和思科合作的成果。与L2TP类似，它通常与IPsec配对使用以提供身份验证和加密功能。

IKEv2非常适合移动版VPN解决方案。这是因为它可以在任何暂时失去互联网连接的情况下重新连接。其次，它很擅长在网络交换期间重新连接（例如，从移动数据到WiFi网络时）。

```
-rw------- 1 ubuntu ubuntu 12542 Nov  5 07:26 vpnclient.mobileconfig
-rw------- 1 ubuntu ubuntu  4344 Nov  5 07:26 vpnclient.p12
-rw------- 1 ubuntu ubuntu  6309 Nov  5 07:26 vpnclient.sswan

```

客户端配置参考：[IKEv2 VPN 配置和使用指南](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/ikev2-howto-zh.md)

## 5.HTTP代理

HTTP代理通过接收客户端的HTTP请求，代为向目标服务器发出请求，并将服务器的响应返回给客户端。HTTP代理通常用于网页浏览、内容过滤和缓存。

HTTP代理适合用于网页浏览、内容过滤和缓存等场景，尤其是在企业和学校环境中广泛应用。

### 优点

- 简单易用：配置和使用相对简单。
- 内容过滤：可以过滤不良内容和广告。
- 缓存功能：可以缓存常用资源，提高访问速度。

### 缺点

- 仅支持HTTP/HTTPS：只能处理HTTP和HTTPS协议的请求。
- 安全性较低：不提供强大的加密和认证机制。



## 6.SOCKS5代理

SOCKS5代理通过接收客户端的请求，代为向目标服务器发出请求，并将服务器的响应返回给客户端。与HTTP代理不同，SOCKS5代理可以处理任意类型的网络流量，包括TCP和UDP协议。

SOCKS5代理适合用于需要处理多种网络流量的场景，如P2P下载、在线游戏、远程访问等。

### 优点

- 通用性强：支持多种协议和应用，包括HTTP、FTP、SMTP等。
- 高安全性：支持身份验证和加密。
- 防火墙穿透能力强：能够绕过大多数防火墙和代理服务器。

### 缺点

- 配置复杂：相对于HTTP代理，配置和使用较为复杂。
- 不提供内容过滤：无法过滤不良内容和广告。