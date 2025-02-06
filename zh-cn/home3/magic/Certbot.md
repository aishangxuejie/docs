# Cerbot自动化部署和管理 SSL/TLS 证书

介绍

> Certbot 是一个由 Let's Encrypt 开发的免费开源工具，用于自动化部署和管理 SSL/TLS 证书。它具有以下几个显著的好处：
>
> - 免费证书：Certbot 使用 Let's Encrypt 作为其证书颁发机构，Let's Encrypt 提供免费的 SSL/TLS 证书。这意味着您可以使用 Certbot 轻松获取和更新有效的证书，而无需支付费用。
> - 自动化：Certbot 可以自动化证书签发和更新的过程，减少了手动操作的工作量和错误的风险。您可以设置定期任务，让 Certbot 自动检查证书的到期日期，并在需要时自动进行更新。
> - 安全性：使用 SSL/TLS 证书可以加密网站与用户之间的通信，确保数据在传输过程中的安全性。Certbot 简化了证书的获取和管理过程，使您能够快速轻松地为您的网站启用 HTTPS，提供更安全的访问方式。

官网：[Certbot 使用说明 |Certbot 公司 --- Certbot Instructions | Certbot](https://certbot.eff.org/instructions?ws=nginx&os=snap)

域名HTTPS证书自动续期

```
sudo snap install --classic certbot

sudo ln -s /snap/bin/certbot /usr/bin/certbot


```
