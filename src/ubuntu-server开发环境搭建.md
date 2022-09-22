# Ubuntu-server开发环境搭建

> 本文整理于2022.9
>
> 操作系统镜像：[ubuntu-server-22.04.1](https://ubuntu.com/download/server) 

## apt

- 替换[国内apt镜像](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

- 更新所有apt包

```bash
sudo apt update
sudo apt upgrade
```

- [消除更新包时的弹窗](https://stackoverflow.com/questions/73397110/how-to-stop-ubuntu-pop-up-daemons-using-outdated-libraries-when-using-apt-to-i)

## Docker

- 安装docker，参考[官方安装教程](https://docs.docker.com/engine/install/ubuntu/)

- 替换[国内dockerhub镜像（腾讯云）](https://cloud.tencent.com/document/product/213/8623)
- 设置[普通用户直接运行不用sudo](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

## Go

- 安装go

```bash
wget https://go.dev/dl/go1.18.6.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.18.6.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
go version
```

- 设置国内镜像

```bash
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

## Python

- 安装`python`、`pip`

```bash
# ubuntu-server-2022.04自带python3.10

# 直接使用python命令而不是python3
sudo apt install python-is-python3
# 安装pip
sudo apt install python3-pip
```

- 设置[国内PyPI镜像（腾讯云）](https://mirrors.cloud.tencent.com/help/pypi.html)

```bash
pip install -i https://mirrors.cloud.tencent.com/pypi/simple --upgrade pip
pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple
```

## Rust

- 安装rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

- 设置国内镜像 https://rsproxy.cn/

