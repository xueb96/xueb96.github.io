# ubuntu开发环境

> - [ubuntu-server-22.04](https://ubuntu.com/download/server) 

## apt

```bash
# 设置国内源 https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list

# 更新所有apt包
sudo apt update
sudo apt upgrade

# 消除更新包时的弹窗 https://stackoverflow.com/questions/73397110/how-to-stop-ubuntu-pop-up-daemons-using-outdated-libraries-when-using-apt-to-i
vim /etc/needrestart/needrestart.conf
#$nrconf{restart} = 'i'; 改成
$nrconf{restart} = 'a';
```

## Docker

```bash
# 根据官方文档安装 https://docs.docker.com/engine/install/ubuntu/
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt update
sudo apt install ca-certificates curl gnupg
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 设置国内源 https://cloud.tencent.com/document/product/1207/45596#.E4.BD.BF.E7.94.A8.E8.85.BE.E8.AE.AF.E4.BA.91-docker-.E9.95.9C.E5.83.8F.E6.BA.90.E5.8A.A0.E9.80.9F.E9.95.9C.E5.83.8F.E4.B8.8B.E8.BD.BD
vim /etc/docker/daemon.json # 添加以下内容
{
   "registry-mirrors": [
       "https://mirror.ccs.tencentyun.com"
  ]
}

# 设置普通用户直接运行不用sudo https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

## Go

- 安装go [官方指引](https://go.dev/doc/install) | [国内下载镜像](https://studygolang.com/dl)

```bash
wget https://studygolang.com/dl/golang/go1.20.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
vim .profile
# 末尾增加一行：export PATH=$PATH:/usr/local/go/bin
go version
go env -w GO111MODULE=on
go env -w GOPROXY=https://mirrors.cloud.tencent.com/go/
```

## Python

- 安装`python`、`pip`

```bash
# ubuntu-server 22.04自带python3.10

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

