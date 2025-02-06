

## 官网：[Tinyproxy](https://tinyproxy.github.io/)

什么是 HTTP 隧道，怎么理解 HTTP 隧道呢？ （http://t.cn/EaoCveH）

今天我们来介绍一款同时支持 `HTTP/HTTPS` 的轻量级代理软件 `TinyProxy`，`TinyProxy` 支持以下功能特性：

•支持匿名模式。•支持 HTTPS，可以通过 CONNECT 请求来转发 HTTPS 连接。•远程监视：可远程查看日志和访问信息。•负载监视：可配置成当负载达到某个程度时，拒绝新的代理请求。•访问控制：可设置特定的 IP 地址或者 IP 段才可访问。•安全：不需要 root 权限。•轻量化：只需要极小的系统资源。•支持基于 URL 的过滤。•支持透明代理。•支持多级代理。

## 安装

```
sudo apt-get update
sudo apt-get install tinyproxy
```

## 配置

```
Port 8888  # 代理port，默认是8888, 可以改成任何自己想要的

Allow 127.0.0.1 # 允许能通过该代理的服务器IP
#若想任何IP都可以连到Proxy, 在Allow前面用 # 注释
```

## 启动

```bash
sudo systemctl restart tinyproxy.service

/etc/init.d/tinyproxy {start|stop|status|restart|condrestart|try-restart|reload|force-reload}
```

## 问题

```bash
# 客户端测试
curl -x vpn.99kids.icu:7444 https://google.com
	curl: (56) Recv failure: Connection was reset
# 服务端日志
ERROR     Nov 20 03:53:02.249 [329863]: read_request_line: Client (file descriptor: 3) closed socket before read.
ERROR     Nov 20 03:53:02.252 [329863]: Error reading readable client_fd 3
```



![image-20241120134318179](F:/OneDrive/markdown/assets/image-20241120134318179.png)

> issue：
>
> 这个问题的原因是因为china firewall 检测到你用代理访问被墙网址, 就恶意修改的你的请求, 把你客户端tcp的ack修改了, 导致服务端reset了这次连接 。
>
> 和 tinyproxy无关。 要访问被墙的, 请使用vpn而不是代理。vpn会加密数据, 代理是明文传输，能被firewall检测到

