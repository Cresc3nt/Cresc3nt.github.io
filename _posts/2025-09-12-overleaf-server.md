---
layout: post
title: "在云服务器上部署私有 Overleaf 服务器"
date: 2025-09-12
# author: 你的名字
categories: [Blog]   # 或 [Notes]、[Tech] 等
tags: [LaTeX, Overleaf, Cloud Server]   # 随意
# description: "文章摘要（用于预览/SEO）"
permalink: /posts/2025/09/overleaf-server/   # 如需自定义链接可加
# image: /images/cover/xxx.jpg         # 如有封面图
---

## 前言

Overleaf 是全球最受欢迎的在线 LaTeX 编辑平台，但免费版的官方服务会限制编译时间，且项目私密性无法完全保证。本文推荐在自己的云服务器上部署私有 Overleaf 以获得更好的体验。

## 安装 Docker 与 Docker Compose

首先更新系统
```bash {.line-numbers}
sudo apt update && sudo apt upgrade -y
```

安装必要依赖
```bash {.line-numbers}
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

添加 Docker 官方 GPG 密钥
```bash {.line-numbers}
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

添加阿里云的 Docker APT 源（针对 Ubuntu 22.04 jammy）
```bash {.line-numbers}
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
现在 `$(lsb_release -cs)` 会自动输出 `jammy`（Ubuntu 22.04 的代号）。

接下来更新包索引并安装 Docker
```bash {.line-numbers}
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
这里我们安装的是 `docker-compose-plugin`（新版），不是老版独立 `docker-compose`。它通过 `docker compose`（注意中间无横杠）命令使用，是官方推荐方式。 

将当前用户加入 docker 组
```bash {.line-numbers}
sudo usermod -aG docker $USER
```
这个时候需要退出并重新登录 SSH 会话，才会使权限生效。

验证安装
```bash {.line-numbers}
docker run hello-world
```
看到 `Hello from Docker!` 即表示成功！

## 配置 Docker 镜像加速器

国内服务器访问 Docker Hub 经常超时，必须配置镜像加速器。首先创建配置文件
```bash {.line-numbers}
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://docker.1panel.live/"
  ]
}
EOF
```

重启 Docker
```bash {.line-numbers}
sudo systemctl daemon-reload
sudo systemctl restart docker
```

验证加速器
```bash {.line-numbers}
docker info | grep -A 5 "Registry Mirrors"
```
应看可以到配置的镜像地址。

