---
layout: single
classes: wide
title:  "安装Kubernetes 1.11.3"
date:   2018-09-23 20:17:38 +0800
categories: 
- docker
- kubernetes
---

# 0. 前置条件

1. 操作系统：ubuntu 18.04
2. 网络连接

# 1. 基础环境和配置


## 1.1 配置操作系统环境

该操作针对所有的节点

1. 启用root用户和root用户的ssh远程访问

    ```bash
    sudo passwd root
    su root
    vi /etc/ssh/sshd_config
    # 设置 PermitRootLogin yes 
    ```
重新使用root连接（根据各自需要，root要方便很多）

<!--more-->

2. 设置主机名

    ```bash
    vi /etc/cloud/cloud.cfg
    # 设置preserve_hostname:true
    vi /etc/hostname
    # 设置你的主机名
    vi /etc/hosts
    # 设置相关节点的IP和主机名映射
    ```

3. 关闭swap

    ```bash
    swapoff -a
    vi /etc/fstab
    # 注释掉含有swap的那一行
    ```


## 1.2 配置软件源仓库
该操作针对所有节点

1. 配置ubuntu仓库

    ```bash
    # 替换/etc/apt/source.list内容为

    deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
    # deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
    # deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
    # deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
    #deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
    # deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
    ```

2. 安装必要的工具

    ```bash
    apt-get update
    # 安装GPG密钥需要的工具
    apt-get -y install apt-transport-https ca-certificates curl software-properties-common
    ```

3. 配置docker仓库

    ```bash
    # 添加密钥
    curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | apt-key add -
    # 添加仓库地址
    vi /etc/apt/sources.list.d/docker.list
    # 插入 deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu bionic stable
    ```

4. 配置kubernetes仓库
        
    ```bash
    # 添加密钥
    curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
    # 添加仓库地址
    vi /etc/apt/sources.list.d/kubernetes.list
    # 插入 deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
    ```

6. 重启操作系统reboot

# 2. 安装软件

## 2.1 安装docker

```bash
apt-get update
apt-get install -y docker
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://nwnz1o41.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

```

## 2.2 安装kubernetes

```bash
apt-get update
apt-get autoremove -y kubelet kubectl kubeadm
apt-get install -y kubelet=1.11.3-00 kubectl=1.11.3-00 kubeadm=1.11.3-00
```
> 可以使用apt-get pkgname=version来安装特定版本号的软件

# 3. 初始化kubernetes集群
## 3.1 拉取需要的镜像
在master节点操作

```bash
# 镜像列表可以在kubernetes上获取
# 或者通过kubeadm init直接查看报错信息
docker pull gcrxio/kube-apiserver-amd64:v1.11.3                 
docker pull gcrxio/kube-controller-manager-amd64:v1.11.3              
docker pull gcrxio/kube-scheduler-amd64:v1.11.3                  
docker pull gcrxio/kube-proxy-amd64:v1.11.3                                
docker pull gcrxio/etcd-amd64:3.2.18                              
docker pull gcrxio/coredns:1.1.3   
docker pull gcrxio/pause:3.1                                    


# 重新tag镜像
docker tag gcrxio/kube-apiserver-amd64:v1.11.3                  k8s.gcr.io/kube-apiserver-amd64:v1.11.3         
docker tag gcrxio/kube-controller-manager-amd64:v1.11.3         k8s.gcr.io/kube-controller-manager-amd64:v1.11.3
docker tag gcrxio/kube-scheduler-amd64:v1.11.3                  k8s.gcr.io/kube-scheduler-amd64:v1.11.3         
docker tag gcrxio/kube-proxy-amd64:v1.11.3                      k8s.gcr.io/kube-proxy-amd64:v1.11.3             
docker tag gcrxio/etcd-amd64:3.2.18                             k8s.gcr.io/etcd-amd64:3.2.18                    
docker tag gcrxio/coredns:1.1.3                                 k8s.gcr.io/coredns:1.1.3               
docker tag gcrxio/pause:3.1                                     k8s.gcr.io/pause:3.1


# 删除原有镜像
docker rmi gcrxio/kube-apiserver-amd64:v1.11.3            
docker rmi gcrxio/kube-controller-manager-amd64:v1.11.3   
docker rmi gcrxio/kube-scheduler-amd64:v1.11.3            
docker rmi gcrxio/kube-proxy-amd64:v1.11.3                     
docker rmi gcrxio/etcd-amd64:3.2.18                            
docker rmi gcrxio/coredns:1.1.3     
docker rmi gcrxio/pause:3.1                   
```

## 3.2 初始化集群
1. 生成初始节点

    ```bash
    kubeadm init --config kubeadm.yaml
    ```

2. 建立控制台命令

    > 可以使用root或者其他非root账户执行如下命令

    ```bash
    mkdir -p $HOME/.kube
    cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    chown $(id -u):$(id -g) $HOME/.kube/config
    ```

2. 安装网络组件

    - 下载flannel镜像

        ```bash
        # 使用aliyun的镜像服务拉取的quay.io的镜像
        docker pull registry.cn-hangzhou.aliyuncs.com/tellxp_quay_io/flannel:v0.10.0-amd64
        docker tag registry.cn-hangzhou.aliyuncs.com/tellxp_quay_io/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64
        docker rmi registry.cn-hangzhou.aliyuncs.com/tellxp_quay_io/flannel:v0.10.0-amd64
        ```
    - 设置iptable
        > 直接执行

        ```bash
        sysctl net.bridge.bridge-nf-call-iptables=1
        systemctl restart kubelet
        ```

        > 或者使用配置文件

        ```bash
        vi /etc/sysctl.d/kubernetes.conf
        # net.bridge.bridge-nf-call-ip6tables = 1
        # net.bridge.bridge-nf-call-iptables = 1
        # net.ipv4.ip_forward = 1
        sysctl -p /etc/sysctl.d/kubernetes.conf
        ```
        
    - 应用flannel配置
        
        ```bash
        kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.10.0/Documentation/kube-flannel.yml
        ```

3. 查看master的情况
    ```bash
    # 查看node的状态是否为Ready
    kubectl get nodes
    # 查看所有的pods是否是Running
    kubectl get pods --all-namespaces
    ```

## 3.3 将其他节点加入集群
1. 拉取镜像

    ```bash
    # 每个节点加入前执行
    docker pull registry.cn-hangzhou.aliyuncs.com/tellxp_quay_io/flannel:v0.10.0-amd64
    docker tag registry.cn-hangzhou.aliyuncs.com/tellxp_quay_io/flannel:v0.10.0-amd64 quay.io/coreos/flannel:v0.10.0-amd64
    docker rmi registry.cn-hangzhou.aliyuncs.com/tellxp_quay_io/flannel:v0.10.0-amd64

    docker pull gcrxio/kube-proxy-amd64:v1.11.3   
    docker tag gcrxio/kube-proxy-amd64:v1.11.3                      k8s.gcr.io/kube-proxy-amd64:v1.11.3             
    docker rmi gcrxio/kube-proxy-amd64:v1.11.3                     

    docker pull gcrxio/pause:3.1                                    
    docker tag gcrxio/pause:3.1                                     k8s.gcr.io/pause:3.1
    docker rmi gcrxio/pause:3.1                   
    ```
2. 设置flannel的iptable
    > 直接执行

    ```bash
    sysctl net.bridge.bridge-nf-call-iptables=1
    systemctl restart kubelet
    ```

    > 或者使用配置文件

    ```bash
    vi /etc/sysctl.d/kubernetes.conf
    # net.bridge.bridge-nf-call-ip6tables = 1
    # net.bridge.bridge-nf-call-iptables = 1
    # net.ipv4.ip_forward = 1
    sysctl -p /etc/sysctl.d/kubernetes.conf
    ```
3. 使用master初始化成功后的提示加入集群

    ```bash
    kubeadm join 10.100.1.51:6443 --token bdrxv1.b2mhvktnb8o3xe0f --discovery-token-ca-cert-hash sha256:38bddbac12f93ea8b28a72ca22f0518d67c7901ca303c69232a56d3f3dd887de
    ```

    > token过期或者忘记了执行

    ```bash
    kubeadm token list # 查看token
    kubeadm token create # 生成token

    # 查看hashcode
    openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \  openssl dgst -sha256 -hex | sed 's/^.* //'
    ```

4. 查看node是否加入成功

    ```bash
    # 查看node的状态是否为Ready
    kubectl get nodes
    # 查看所有的pods是否是Running
    kubectl get pods --all-namespaces
    ```


# 附录 其他一些附加bash片段

```bash
# 强制升级kubernetes
kubeadm upgrade apply v1.11.3 --config kubeadm.yaml --ignore-preflight-errors=all --allow-experimental-upgrades

# 删除所有镜像
docker rmi $(docker images -q)

# 卸载apt软件包
apt-get autoremove -y kubelet kubeadm kubectl

# 强制删除pod
kubectl delete pods <pod> --grace-period=0 --force 

# 设置默认的storageclass
kubectl patch storageclass <your-class-name> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

# 根据grep的条件删除所有的pod 
kubectl get pods | grep Terminating | awk '{print $1}' | xargs kubectl delete pod --grace-period=0 --force

# 强制删除-n名字空间下的所有pod
kubectl -n kubeapps get pods | awk '{print $1}' | xargs kubectl  -n kubeapps delete pod --grace-period=0 --force

# 列出需要的镜像
kubeadm config images list --kubernetes-version=v1.11.1

# 设置ubuntu代理
export HTTP_PROXY=http://10.100.1.141:11111/
export HTTPS_PROXY=https://10.100.1.141:11111/
export NO_PROXY=localhost,127.0.0.1,10.100.0.0/16,10.244.0.0/16,10.96.0.0/16

# 取消代理
unset HTTP_PROXY
unset HTTPS_PROXY
unset NO_PROXY

# 设置docker代理  
vi /etc/systemd/systemd/docker.service.d/http-proxy.conf

[Service]
Environment="HTTP_PROXY=http://10.100.1.141:11111/"
Environment="HTTPS_PROXY=https://10.100.1.141:11111/"
Environment="NO_PROXY=localhost,127.0.0.1,10.100.0.0/16,10.244.0.0/16"
systemctl daemon-reload
systemctl restart docker
systemctl show --property=Environment docker

```

