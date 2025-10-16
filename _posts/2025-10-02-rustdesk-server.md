---
layout: post
title: 在云服务器上部署 RustDesk 自建服务器
date: 2025-10-02 10:00:00
description: RustDesk 作为一款开源、安全、跨平台的远程桌面软件，因其轻量、高效、无广告且支持自建服务器等特性，受到越来越多用户的青睐。自建 RustDesk 服务器不仅可以摆脱对官方服务器的依赖，还能完全掌控连接数据、提升连接速度与稳定性，尤其适合对隐私和性能有较高要求的个人或团队使用。
tags: RustDesk "Remote Desktop" "Cloud Server"
categories: engineering
---

## RustDesk 服务器介绍

RustDesk 服务器有两个可执行程序：
- `hbbs` - RustDesk ID 注册服务器，是管各个客户端 ID 的，每个客户端都有一个唯一的 ID 。
- `hbbr` - RustDesk 中继服务器，是负责检测、中转各个客户端连接和数据传输。

在部署前，请务必在保证云服务器开放了以下端口：
- TCP: 21115-21119
- UDP: 21116

RustDesk Server 的进程占用端口情况如下：
- `hbbs` - 21115(TCP), 21116(TCP/UDP), 21118(TCP)
- `hbbr` - 21117(TCP), 21119(TCP)

每个端口都有其各自的作用：
- 21115(TCP) - 用作 NAT 类型测试
- 21116(UDP) - 用作 ID 注册 与 心跳服务
- 21116(TCP) - 用作 TCP打洞 与 连接服务
- 21117(TCP) - 用作中继服务
- 21118/21119(TCP) - 为了支持网页客户端

💡 如果启动的时候不加 `-k _` 参数，则不使用 `key` 也可以连接服务器。
{: .notice}

## 环境配置

设置时区（可选但推荐）
```bash
# 设置时区为东八区的上海
sudo timedatectl set-timezone Asia/Shanghai
date +%Z  # 应输出 CST
```

更新系统并安装基础依赖
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl htop wget unzip -y
```

安装 Node.js 与 npm
```bash
sudo apt install nodejs npm -y
node -v  # 查看 Node.js 版本
npm -v   # 查看 npm 版本
```

安装 pm2
```bash
sudo npm install -g pm2
pm2 --version  # 查看pm2的版本
```

创建 pm2 开机启动脚本
```bash
pm2 completion install  # 根据提示信息完成
```

设置 pm2 开机启动
```bash
sudo env PATH=$PATH:/usr/bin /usr/local/lib/node_modules/pm2/bin/pm2 startup systemd -u ${USER} --hp ${HOME}
sudo systemctl enable pm2-${USER}
```

## 安装并部署 RustDesk-Server

执行以下命令自动获取 GitHub 上 `rustdesk/rustdesk-server` 仓库的最新发布版本并下载

```bash
REPO="rustdesk/rustdesk-server"
latest_tag=$(curl -s https://api.github.com/repos/$REPO/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
echo "Using rustdesk-server version $latest_tag"

wget https://github.com/$REPO/releases/download/$latest_tag/rustdesk-server-linux-amd64.zip
```

解压并整理文件
```bash
unzip rustdesk-server-linux-amd64.zip
mv amd64 ~/rustdesk-server
rm -f rustdesk-server-linux-amd64.zip
```

此时，RustDesk 的两个核心可执行文件 `hbbs` 和 `hbbr` 已位于 `~/rustdesk-server/` 目录下。

进入 RustDesk 目录并启动两个服务
```bash
cd ~/rustdesk-server
pm2 start hbbs -- -k _
pm2 start hbbr -- -k _
```

💡 `-k _`：表示不启用密钥验证（即客户端无需填写密钥即可连接）。若需启用密钥认证，可替换 `_` 为自定义密钥，如 `-k mysecret123` 。
{: .notice}

保存 PM2 进程状态（用于开机恢复）
```bash
pm2 save
```

此命令会将当前运行的进程列表保存，配合之前配置的 `pm2 startup`，确保系统重启后服务自动恢复。

验证服务是否正常运行
```bash
pm2 list
```

应看到类似如下输出
```bash
┌────┬──────────┬─────────────┬─────────┬─────────┬──────────┐
│ id │ name     │ namespace   │ version │ mode    │ status   │
├────┼──────────┼─────────────┼─────────┼─────────┼──────────┤
│ 0  │ hbbs     │ default     │ N/A     │ fork    │ online   │
│ 1  │ hbbr     │ default     │ N/A     │ fork    │ online   │
└────┴──────────┴─────────────┴─────────┴─────────┴──────────┘
```

若状态为 `online`，说明服务已成功启动。

此时查看 rustdesk-server 目录，可以看到生成了一些数据库文件和一个证书文件
```bash
rustdesk-server
├── db_v2.sqlite3
├── db_v2.sqlite3-shm
├── db_v2.sqlite3-wal
├── hbbr
├── hbbs
├── id_ed25519
├── id_ed25519.pub
└── rustdesk-utils
```

需要拷贝公钥文件 `id_ed25519.pub` 中的内容（例如`C6bJn7*******************50nCK3y4=`），用于接下来的本地配置。

## 客户端配置

打开 RustDesk 客户端，点击左上角 `ID` 旁边的三点图标，进入设置。

切换到 `网络` 选项卡，点击 `ID/中继服务器` ：
- 在 `ID 服务器` 字段中填写你的云服务器公网 IP 地址或域名。
- 在 `key` 字段中填写刚才拷贝的公钥内容（例如 `C6bJn7*******************50nCK3y4` ）

点击 `确认`，回到主页，等待客户端与服务端建立连接，即可完成配置。
