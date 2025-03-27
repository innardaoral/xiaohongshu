# Forsaken Mail 临时邮箱搭建教程及最新云服务器优惠活动

## 一、Forsaken Mail 临时邮箱搭建指南

**Forsaken Mail** 是一款开源的临时邮箱系统，支持自定义邮箱前缀，邮件阅后即焚。本文将详细介绍通过 **Docker** 和 **NPM** 两种方式部署该服务。

### 前置准备
1. **VPS 要求**：需开启 **25 端口**（需联系主机商开通）
2. **域名解析**：为邮箱域名（如 `xx.com`）添加以下 **MX 记录**：
   - 记录类型：MX | 主机记录：`@` | 优先级：10 | 记录值：`mx.xx.com`
   - 记录类型：A | 主机记录：`mx` | 记录值：`服务器IP`

### 方法一：NPM 安装流程

#### 1. 安装 Node.js 环境
bash
# Debian/Ubuntu
curl -sL https://deb.nodesource.com/setup_16.x | bash -
apt-get install -y nodejs git screen

# CentOS
curl -sL https://rpm.nodesource.com/setup_16.x | bash -
yum install nodejs git screen -y

#### 2. 部署 Forsaken Mail
bash
git clone https://github.com/malaohu/forsaken-mail.git
cd forsaken-mail
npm install
npm install -g pm2

# 关闭冲突服务
systemctl stop postfix sendmail
systemctl disable postfix sendmail

# 启动服务
pm2 start bin/www
pm2 startup
pm2 save

#### 3. 防火墙配置（CentOS）
bash
# CentOS 7+
firewall-cmd --zone=public --add-port=3000/tcp --permanent
firewall-cmd --reload

访问地址：`http://mx.xx.com:3000`

👉 [【点击查看】2025年最新 Hosteons 优惠码及特价云服务器方案汇总](https://bit.ly/hosteons)

### 方法二：Docker 快速部署
bash
# 安装 Docker
curl -fsSL https://get.docker.com | sh
systemctl enable --now docker

# 一键部署
docker run --name forsaken-mail -d -p 25:25 -p 3000:3000 denghongcai/forsaken-mail

### HTTPS 安全访问配置
使用 **Caddy** 实现 HTTPS 反代：
bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/caddy_install.sh
chmod +x caddy_install.sh && ./caddy_install.sh

# 配置反代
echo "xx.com {
  gzip
  tls admin@xx.com
  proxy / mx.xx.com:3000
}" > /usr/local/caddy/Caddyfile

/etc/init.d/caddy start

访问地址：`https://xx.com`

## 二、最新云服务器优惠活动

**2024年精选促销方案**：

### 超值优惠码
- `RECURR50`：OpenVZ 套餐年付 **5折** 循环优惠
- `RECURR20`：KVM 套餐年付 **8折** 循环优惠（适用洛杉矶/纽约机房）

### 限时特权
1. **免费 Windows 2019 授权**：适用于 KVM VPS 3-7 套餐年付用户
2. **双倍存储空间**：使用优惠码 `DOUBLEDISK` 即可扩容
3. **免费控制面板**：
   - DirectAdmin（适用于 KVM VPS 2+/OVZ VPS 2+）
   - Blesta 授权（提交工单免费获取）

### 节点扩展
- 新增洛杉矶 OpenVZ 节点
- 纽约机房全新上线

> 提示：所有优惠可叠加使用，年付套餐性价比最高