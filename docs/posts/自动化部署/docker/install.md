---
title: docker 安装
date: 2024-11-15
tags:
  - docker
  - centos
  - ci
  - linux
---

# docker install

## centos

### 使用官方安装脚本自动安装

```sh
url -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

### 手动安装

#### 卸载旧版本

```sh
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine                 
```

### 安装 Docker Engine-Community

> 在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

#### 设置仓库

> 安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

```sh
yum install -y yum-utils device-mapper-persistent-data lvm2
```

#### 使用以下命令来设置稳定的仓库。

```sh
# 使用官方源地址（比较慢）
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 阿里云
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 清华大学源
yum-config-manager --add-repo https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
```

#### 安装 Docker Engine-Community

安装最新版本的 Docker Engine-Community 和 containerd，或者转到下一步安装特定版本：

```sh
# 如果提示您接受 GPG 密钥，请选是
# Docker 安装完默认未启动。并且已经创建好 docker 用户组，但该用户组下没有用户
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

##### 要安装特定版本的 Docker Engine-Community，请在存储库中列出可用版本，然后选择并安装：

1. 列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序

```sh
yum list docker-ce --showduplicates | sort -r
```

2. 通过其完整的软件包名称安装特定版本，该软件包名称是软件包名称（docker-ce）加上版本字符串（第二列），从第一个冒号（:）一直到第一个连字符，并用连字符（-）分隔。例如：docker-ce-18.09.1

```sh
yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
# yum install docker-ce-24.0.7-1.el7 docker-ce-cli-24.0.7-1.el7 containerd.io
```

3. 启动 Docker

```sh
systemctl start docker
```

4. 通过运行 hello-world 镜像来验证是否正确安装了 Docker Engine-Community

```sh
docker run hello-world
```

### 卸载 docker

删除安装包：

```sh
yum remove docker-ce
```

删除镜像、容器、配置文件等内容：

```sh
rm -rf /var/lib/docker
```

## docker-compose

### 安装

```sh
curl -SL https://github.com/docker/compose/releases/download/v2.23.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

docker compose version
```
