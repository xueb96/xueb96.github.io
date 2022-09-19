# Kubernets笔记

##  架构

![k8s架构图](https://cdn.jsdelivr.net/gh/xueb96/xueb96.github.io@master/img/k8s架构图.png)

![k8s架构](https://cdn.jsdelivr.net/gh/xueb96/xueb96.github.io@master/img/k8s架构.png)

![control-panel](https://cdn.jsdelivr.net/gh/xueb96/xueb96.github.io@master/img/control-panel.png)

![node](https://cdn.jsdelivr.net/gh/xueb96/xueb96.github.io@master/img/node.png)

## 本地搭建

> 本地运行（我的机器是MacOS on ARM64）k8s集群有几种方案：docker-desktop、minikube、kind
>
> 都试了一下感觉kind(Kubernetes IN Docker)最好用，可以参考[官方文档](https://kind.sigs.k8s.io/)和[B站视频](https://www.bilibili.com/video/BV1Xq4y1G7cF)

两种安装方式：

```bash
# 直接下载已发布的二进制文件
[ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.15.0/kind-darwin-arm64
chmod +x ./kind
sudo mv kind /usr/local/bin

# 或使用go install下载
go install sigs.k8s.io/kind@v0.15.0
sudo mv ~/go/bin/kind /usr/local/bin
```

新建kind集群配置文件`kind-cluster-config.yaml`：

```yaml
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: my-cluster
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30000
    hostPort: 80
    listenAddress: "0.0.0.0" # Optional, defaults to "0.0.0.0"
    protocol: TCP # Optional, defaults to tcp
- role: worker
- role: worker
```

以该配置文件创建kind集群：

```bash
kind create cluster --config=kind-cluster-config.yaml
```

新建一个k8s manifest（包含一个Deployment和一个service）文件`demo.yaml`：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    nodePort: 30000
```

根据该yaml文件创建资源：

```bash
kubectl apply -f demo.yaml
```

如下图

![kind-demo](https://cdn.jsdelivr.net/gh/xueb96/xueb96.github.io@master/img/kind-demo.png)

打开浏览器访问 http://localhost:80 可以就看到nginx页面了，证明我们的集群安装成功并且能在宿主机访问的到了～

清理资源：

```bash
# 删除demo资源
kubectl delete -f demo.yaml

# 删除集群
kind delete cluster --name my-cluster
```



## kubectl

```bash
# 查看kubectl的上下文（操作的哪个集群）
kubectl config get-contexts

# 切换上下文
kubectl config use-context [name]

# 查看节点
kubectl get nodes

# 创建deployment.apps/hello
kubectl create deployment hello --image=registry.k8s.io/echoserver:1.4

# 创建service/hello
kubectl expose deployment hello --type=NodePort --port=8080

# 查看service
kubectl get service hello

# 删除
kubectl delete service hello
kubectl delete deployment hello
```

