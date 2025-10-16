---
layout: post
title: 在云服务器上部署私有 Overleaf 服务器
date: 2025-09-12 10:00:00
description: Overleaf 是全球最受欢迎的在线 LaTeX 编辑平台，但免费版的官方服务会限制编译时间，且项目私密性无法完全保证。本文推荐在自己的云服务器上部署私有 Overleaf 以获得更好的体验。
tags: LaTeX Overleaf "Cloud Server"
categories: engineering
---

## 安装 Docker 与 Docker Compose

首先更新系统
```bash
sudo apt update && sudo apt upgrade -y
```

安装必要依赖
```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

添加 Docker 官方 GPG 密钥
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

添加阿里云的 Docker APT 源（针对 Ubuntu 22.04 jammy）
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
现在 `$(lsb_release -cs)` 会自动输出 `jammy`（Ubuntu 22.04 的代号）。

接下来更新包索引并安装 Docker
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
这里我们安装的是 `docker-compose-plugin`（新版），不是老版独立 `docker-compose`。它通过 `docker compose`（注意中间无横杠）命令使用，是官方推荐方式。 

将当前用户加入 docker 组
```bash
sudo usermod -aG docker $USER
```
这个时候需要退出并重新登录 SSH 会话，才会使权限生效。

验证安装
```bash
docker run hello-world
```
看到 `Hello from Docker!` 即表示成功！

## 配置 Docker 镜像加速器

国内服务器访问 Docker Hub 经常超时，必须配置镜像加速器。首先创建配置文件
```bash
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
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

验证加速器
```bash
docker info | grep -A 5 "Registry Mirrors"
```
应看可以到配置的镜像地址。

## 部署 Overleaf Toolkit

Overleaf 官方提供了 `overleaf/toolkit` 工具包，简化了部署流程。

首先克隆仓库
```bash
git clone git@github.com:overleaf/toolkit.git ./overleaf-toolkit
cd overleaf-toolkit
```

初始化配置
```bash
bin/init
```
这会在 `config/` 目录下生成 `overleaf.rc`, `variables.env`, `version` 三个文件。

修改 `overleaf.rc` 中的内容
```bash
## 设置访问地址（必须！）
export OVERLEAF_URL="http://<your-server-ip-or-domain>:<your-port>"

## 监听所有 IP（必须！）
OVERLEAF_LISTEN_IP=0.0.0.0

## 设置服务端口（必须！）
OVERLEAF_PORT=<your-port>   # 需要在服务器设置中开放相应的端口

## 设置管理员邮箱（必须！）
export OVERLEAF_ADMIN_EMAIL="your-admin-email@example.com"

## 关闭公开注册（可选）
# export OVERLEAF_SIGNUPS_ENABLED=false
```

修改 `variables.env` 中的内容
```bash
## 设置 Overleaf 实例在系统内部的应用名称（用于日志、后台管理等）
OVERLEAF_APP_NAME="Your Custom Overleaf Instance"

## 设置用户访问该 Overleaf 实例的完整公网 URL（必须包含协议、IP/域名、端口）
OVERLEAF_SITE_URL=http://<your-server-ip-or-domain>:<your-port>

## 设置网页顶部导航栏或浏览器标签页中显示的标题（用户可见的品牌标识）
OVERLEAF_NAV_TITLE=Your Custom Overleaf Instance

## 设置管理员联系邮箱（用于接收系统通知、用户联系、密码重置等）
OVERLEAF_ADMIN_EMAIL=your-admin-email@example.com

## （可选）自定义页面顶部 Logo 图片地址，当前注释表示使用默认样式
# OVERLEAF_HEADER_IMAGE_URL=http://example.com/logo.png
```

修改 `lib/docker-compose.base.yml` 中的内容，将 `image: "${IMAGE}` 修改为 `image: "sharelatex/sharelatex:latest"`。

接下来启动服务
```bash
bin/up
```
现在会看到来自 docker 容器的一些日志输出，表示正在拉取镜像，后续会自动运行容器。如果在终端上按下 `Ctrl` + `c`，服务将关闭，可以通过命令 `bin/start` 来重新启动它们（不附加到日志输出）。

## 安装完整的 texlive

社区版使用的 texlive 是最小安装的 texlive ，需要将其升级到完整版。
```bash
# 进入容器
bin/shell
# 查看版本
tlmgr --version
# 更换镜像源，我用腾讯云的镜像
tlmgr option repository http://mirrors.cloud.tencent.com/CTAN/systems/texlive/tlnet
# 查看
tlmgr option show repository

# 在你选择的镜像站里，找到升级脚本
cd ~
wget http://mirrors.cloud.tencent.com/CTAN/systems/texlive/tlnet/update-tlmgr-latest.sh

chmod +x update-tlmgr-latest.sh
./update update-tlmgr-latest.sh

# 更新
tlmgr update --self --all 

# 安装完整的包，要花挺长一段时间，尽量选速度快的源
tlmgr install scheme-full

# 重启容器
bin/stop 
bin/start
```

## 安装中文字体
将本地的中文字体复制到服务器，Windows 系统的的字体储存在 `C:\windows\Fonts` 目录，将其复制到服务器的 `/root/Fonts` 目录后，在服务器内完成安装
```bash
# 进入Fonts目录
cd Fonts/

# 删除其中的.fon字体文件(否则可能会报错)
rm -r *.fon

# 返回上层目录并打包
cd ..
tar -zcvf winfonts.tar.gz Fonts/

# 把压缩文件传到sharelatex容器的root目录下
docker cp winfonts.tar.gz sharelatex:/root

# 进入容器的命令行界面
docker exec -it sharelatex bash

# 在容器内更新软件包列表
apt-get update

# 通过安装wqy字体同时安装xfont工具
apt-get install xfonts-wqy

# 进入root目录，解压winfonts.tar.gz，并移动到系统字体目录下
cd ~
tar -zxvf winfonts.tar.gz
mv Fonts /usr/share/fonts/

# 进入字体目录安装字体
cd /usr/share/fonts/Fonts
mkfontscale
mkfontdir
fc-cache -fv

# 检查确认中文字体安装成功
fc-list :lang=zh-cn
#此时会出现已经安装的中文字体
```

重启服务
```bash
bin/stop
bin/up
```

## 清除原有镜像与容器

有时，我们会不小心安装错误的镜像，这个时候我们需要确保旧的、错误的镜像和容器被完全清除。
```bash
# 停止当前所有服务
cd ~/overleaf-toolkit
bin/stop

# 列出所有与项目相关的容器
# 通常容器名会包含 "overleaf" 或 "sharelatex"
docker ps -a --filter "name=overleaf" --filter "name=sharelatex"

# 强制删除所有列出的相关容器
# 请将 <CONTAINER_ID_OR_NAME> 替换为上一步命令中列出的实际容器ID或名称
docker rm -f <CONTAINER_ID_OR_NAME_1> <CONTAINER_ID_OR_NAME_2>

# 列出所有本地镜像，查找非官方的或可疑的镜像
docker images | grep -E "(sharelatex|overleaf)"

# 删除非官方的自定义镜像
# 请将 <CUSTOM_IMAGE_NAME:TAG> 替换为上一步中找到的、非 "sharelatex/sharelatex:latest" 的镜像
docker rmi <CUSTOM_IMAGE_NAME:TAG>

# （可选）清理所有无用的Docker对象，获得一个纯净的环境
docker system prune -a
# 系统会提示确认，输入 'y' 并回车。
# 注意：此命令会删除所有未被容器使用的镜像、网络、构建缓存等。
```

之后便可以启动服务
```bash
bin/up
```

## 登录

首先访问 `http://<your-server-ip-or-domain>:<your-port>/launchpad` 创建管理员，之后便可以访问 `http://<your-server-ip-or-domain>:<your-port>/login` 登录用户。

![登录界面](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/login.png)

## 添加账户

在管理员账户主页，点击 `admin - Manage Users` 即可添加账户。

![管理员添加账户](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/add-users.png)

在输入要添加账户的邮箱后，会为新账户生成一个设置密码链接，访问该链接设置密码即可完成新账户的注册。

![设置密码链接](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/register.png)

接下来，就可以愉快地使用 overleaf 啦。

![使用示例](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/example.png)

## VS Code/Cursor + Overleaf

在 VS Code 扩展中搜索插件 [Overleaf Workshop](https://github.com/iamhyc/Overleaf-Workshop) ，点击安装。

安装成功后，可以在左侧导航栏看到 Overleaf 的 logo。

![Overleaf 插件](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/overleaf-plungin.png)

点击图中所示的加号，将部署的 Overleaf 服务器地址 `http://<your-server-ip-or-domain>:<your-port>` 填进框中。

![输入 Overleaf 服务器地址](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/input-url.png)

在左侧可以进行登录。

![登录账户](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/sign-in.png)

点击 `Login with Password`，根据提示输入账号密码。

![输入账号密码](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/input-password.png)

点击图中的按钮，便可以使用 VS Code 编辑 LaTeX 项目了。

![打开项目](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/open-project.png)

![编辑项目](https://cresc3nt.github.io/assets/img/blogs/overleaf-server/edit-project.png)

在 Cursor 中，我并没有在插件市场搜索到 Overleaf Workshop 插件，这时可以通过手动安装 VSIX 文件的方式来解决这个问题。

在 VSCode 中，右键点击已安装的插件 → `Export Extension`。在 Cursor 中，按 `Ctrl`+`Shift`+`P` 打开命令面板，输入 `Install from VSIX`，选择下载的 `.vsix` 文件。安装完成后，重启 Cursor 即可使用。

## 总结

本文提供了一套在国内网络环境下可复用、可维护且体验流畅的 Overleaf 私有部署方案。
