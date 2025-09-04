# Hysteria 2 搭建教程

## 官网

官网教程：[安装 - Hysteria 2](https://v2.hysteria.network/zh/docs/getting-started/Installation/)

### 编辑配置文件

```
nano /etc/hysteria/config.yaml
```

### 服务管理

设置开机自启， 并立即启动服务。

```
systemctl enable --now hysteria-server.service
```

重启服务， 通常在修改配置文件后执行。

```
systemctl restart hysteria-server.service
```

查询服务状态。

```
systemctl status hysteria-server.service
```

### 日志

查询服务端日志。

```
journalctl --no-pager -e -u hysteria-server.service
```

### 安装或升级

安装或升级到最新版本。

```
bash <(curl -fsSL https://get.hy2.sh/)
```

安装或升级为指定版本，不进行版本检查。

```
bash <(curl -fsSL https://get.hy2.sh/) --version v2.6.0
```

### 卸载

移除 Hysteria 及相关服务。

```
bash <(curl -fsSL https://get.hy2.sh/) --remove
```

## 准备工作

1. 购买VPS

   Google、AWS、Azure、Oracle

2. 域名并解析好（非必须）

   可以不使用自己的域名使用自签名证书，后面配置过程会讲到。

## 服务端部署

采用官方提供的脚本

```
# 切换到 root
sudo -i
# 一键安装 Hysteria2 
bash <(curl -fsSL https://get.hy2.sh/)
```

输出提示

Congratulation! Hysteria 2 has been successfully installed on your server. 表示安装成功
并提示我们下一步要：

1. 修改服务端配置文件 /etc/hysteria/config.yaml
2. 启动 hysteria systemctl start hysteria-server.service
3. 设置开机自启 systemctl enable hysteria-server.service

先设置开机自启

```
systemctl enable hysteria-server.service
```

生成自签名证书

```
openssl req -x509 -nodes -newkey ec:<(openssl ecparam -name prime256v1) -keyout /etc/hysteria/server.key -out /etc/hysteria/server.crt -subj "/CN=bing.com" -days 36500 && sudo chown hysteria /etc/hysteria/server.key && sudo chown hysteria /etc/hysteria/server.crt
```

#### 设置配置文件

```shell
cat << EOF > /etc/hysteria/config.yaml
listen: :443 #监听端口

#有域名，使用CA证书 
#acme:
#  domains:
#    - test.heybro.bid #你的域名，需要先解析到服务器ip
#  email: augustdoit@gmail.com

#使用自签名证书
tls:
  cert: /etc/hysteria/server.crt
  key: /etc/hysteria/server.key

auth:
  type: password
  password: Se7RAuFZ8Lzg #设置认证密码

#伪装
masquerade:
  type: proxy
  proxy:
    url: https://bing.com/ #伪装网址
    rewriteHost: true
EOF
```

```yaml
# 例
listen: :7443 #默认 443

#acme:
#  domains:
#    - baidu.com #例
#  email: @qq.com #例
#使用自签证书
tls:
  cert: /etc/hysteria/server.crt
  key: /etc/hysteria/server.key
auth:
  type: password
  password: 8KE0Z3mU
masquerade:
  type: proxy
  proxy:
    url: https://bing.com/
    rewriteHost: true

```



#### 启动Hysteria2服务

```
systemctl start hysteria-server.service
```

其他命令

```
# 查看状态及日志
systemctl status hysteria-server.service -l
# 停止Hysteria2
systemctl stop hysteria-server.service
# 设置开机自启
systemctl enable hysteria-server.service
# 重启
systemctl restart hysteria-server.service
```

### 客户端

#### windows

karing 下载：[下载Karing | Karing - Clash compatible & Powerful proxy utility](https://karing.app/download/)

v2rayN 下载：https://github.com/2dust/v2rayN/releases/latest
Hysteria 2下载：https://github.com/apernet/hysteria/releases

客户端配置文件

关于带宽字段的设置

```
bandwidth:
  up: 20 mbps
  down: 9 mbps
```

根据自己使用的带宽酌情设置，官方说法如下：

**Brutal 如果带宽设置低于实际最大值也能正常运行；相当于限速。重要的是不要将其设置得高于实际最大值，否则会因为补偿机制导致连接速度更慢且不稳定。**

另外，服务端、客户端的配置文件均可设置带宽字段，这样会有三种情形：

- 服务端、客户端两端均设置：两个方向都会使用 Brutal，客户端的 up 应当等于服务端的 down，两端设置数据不同时，实际控制速度为相对小的值。
- 仅一端设置：按这一端设置值使用 Brutal 控制速度
- 两侧均不设置，则双方均使用 BBR

一个特殊情况是当服务端配置文件启用了 `ignoreClientBandwidth` 选项，无论客户端的带宽值如何，双方始终都会使用 BBR。

如果你自建 Hysteria 服务器并且只给你自己使用， 可以直接删除服务端配置中的 `bandwidth` 与 `ignoreClientBandwidth`， 使用客户端配置中的 `bandwidth` 指定带宽， 从而将上述流程简化

```
server: IP:443
auth: Se7RAuFZ8Lzg
# 注意服务器上传速度对应的是客户端下载速度，反之亦然
bandwidth:
  up: 20 mbps  #设置最高上行带宽，从运营商查或不挂梯子测速speedtest.cn
  down: 90 mbps #设置最高下行带宽，从运营商查或不挂梯子测速speedtest.cn
ignoreClientBandwidth: false 

tls:
  sni: bing.com  #自己域名，若没有填写伪装站点
  insecure: true #使用自签名证书时时需要改成true,如为 CA 证书建议修改为 false

socks5:
  listen: 127.0.0.1:1080
http:
  listen: 127.0.0.1:8080
```

#### 手机客户端

各种客户端陆续开始支持 hysteria 2

###### karing 配置文件

```json
{
  "attach": "",
  "outlet_ip": "",
  "outlet_region": "",
  "server": "",
  "server_port": 443,
  "tag": "",
  "type": "hysteria2",
  "up_mbps": 10,
  "down_mbps": 100,
  "password": "user:password",
  "tls": {
    "enabled": true,
    "server_name": "bing.com",
    "insecure": true,
    "alpn": [
      "h3"
    ]
  }
}
```

###### sing-box 配置文件

```json
{
  "dns": {
    "servers": [
      {
        "tag": "cf",
        "address": "https://1.1.1.1/dns-query"
      },
      {
        "tag": "local",
        "address": "223.5.5.5",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "geosite": "category-ads-all",
        "server": "block",
        "disable_cache": true
      },
      {
        "outbound": "any",
        "server": "local"
      },
      {
        "geosite": "cn",
        "server": "local"
      }
    ],
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "inet4_address": "172.19.0.1/30",
      "auto_route": true,
      "strict_route": false,
      "sniff": true
    }
  ],
  "outbounds": [
    {
      "type": "hysteria2",
      "tag": "proxy",
      "server": "IP",             // VPS IP
      "server_port": 443,         // 端口
      "up_mbps": 30,              //上传速率，按本地速度填写
      "down_mbps": 100,           //下载速率，按本地速度填写
      "password": "88888888",     //hysteria2 服务密码
      "tls": {
        "enabled": true,
        "server_name": "www.bing.com",
        "insecure": true              
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    },
    {
      "type": "dns",
      "tag": "dns-out"
    }
  ],
  "route": {
    "rules": [
      {
        "protocol": "dns",
        "outbound": "dns-out"
      },
      {
        "geosite": "cn",
        "geoip": [
          "private",
          "cn"
        ],
        "outbound": "direct"
      },
      {
        "geosite": "category-ads-all",
        "outbound": "block"
      }
    ],
    "auto_detect_interface": true
  }
}

```

