---
layout: single
classes: wide
title:  "安装Ceph并与Kubernetes集成"
date:   2018-09-30 08:36:38 +0800
categories: ceph
tags:
- kubernetes
- ceph
---

# 1. 系统环境配置
## 1.1 前提条件

    - 有至少4台Linux操作系统作为Ceph的部署环境，这里是Ubuntu 18.04

## 1.2 配置主机名

1. 4台Ceph主机在一个子网，分别为ceph-master-01, ceph-node-01, ceph-node-02, ceph-node-03

2. 为每个主机配置hosts文件，加入主机名的IP地址

    ```bash
    # /etc/hosts
    ceph-master-01  10.100.1.51
    ceph-node-01    10.100.1.52
    ceph-node-02    10.100.1.53
    ceph-node-03    10.100.1.54
    ```
<!--more-->

## 1.3 设置时区

```bash
timedatectl list-timezones
sudo timedatectl set-timezone Asia/Shanghai
```

# 2. 安装Ceph软件

1. 在ceph-master-01上安装软件包
    
    ```bash
    # 这里采用阿里云的镜像
    wget -q -O- 'https://mirrors.aliyun.com/ceph/keys/release.asc' | sudo apt-key add -

    echo deb https://mirrors.aliyun.com/ceph/debian-mimic/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list

    # 设置软件源环境变量
    export CEPH_DEPLOY_REPO_URL=https://mirrors.aliyun.com/ceph/debian-mimic
    export CEPH_DEPLOY_GPG_URL=https://mirrors.aliyun.com/ceph/keys/release.asc

    sudo apt update
    sudo apt install ceph-deploy
    ```

2. 在master上生成安装过程中所需要的用户

    ```bash
    sudo useradd -d /home/ceph-admin -m ceph-admin -s /bin/bash
    sudo passwd ceph-admin
    sudo echo "ceph-admin ALL=(root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ceph-admin
    sudo chmod 0440 /etc/sudoers.d/ceph-admin
    ```

3. 在每个node上生成安装过程中所需要的用户

    ```bash
    sudo useradd -d /home/ceph-admin -m ceph-admin -s /bin/bash
    sudo passwd ceph-admin
    sudo echo "ceph-admin ALL=(root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ceph-admin
    sudo chmod 0440 /etc/sudoers.d/ceph-admin
    ```

4. 在每个node上安装python依赖包

    ```
    sudo apt update
    sudo apt install -y python-minimal
    
    ```
5. 在ceph-master-01上生成ssh密钥并拷贝到各个node
    
    ```bash
    ssh-keygen
    ssh-copy-id ceph-admin@ceph-node-01
    ssh-copy-id ceph-admin@ceph-node-02
    ssh-copy-id ceph-admin@ceph-node-03
    ```

6. 配置ceph-master-01的ssh连接
    
    ```bash
    su ceph-admin
    vi ~/.ssh/config
    #Host ceph-node-01
    #    Hostname ceph-node-01
    #    User ceph-admin
    #Host ceph-node-02
    #    Hostname ceph-node-02
    #    User ceph-admin
    #Host ceph-node-03
    #    Hostname ceph-node-03
    #    User ceph-admin
    ```

# 3. 创建Ceph群集

1. 创建集群配置文件目录

    ```bash
    su ceph-admin
    mkdir ceph-cluster
    cd ceph-cluster
    ```

2. 创建群集

    ```bash
    # 如果原来有群集配置文件，需要删除
    # ceph-deploy purge {ceph-node} [{ceph-node}]
    # ceph-deploy purgedata {ceph-node} [{ceph-node}]
    # ceph-deploy forgetkeys
    # rm ceph.*

    # 初始化第一个mon节点
    ceph-deploy new ceph-node-01

    # 安装所有节点
    ceph-deploy install ceph-node-01 ceph-node-02 ceph-node-03

    # 部署监控节点
    ceph-deploy mon create-initial

    # 分发监控命令的key到每个节点
    ceph-deploy admin ceph-master-01 ceph-node-01 ceph-node-02 ceph-node-03

    # 部署manger的守护进程
    ceph-deploy mgr create ceph-node-01

    # 创建osd磁盘
    # 如果物理磁盘已经装载，需要使用lvm命令来删除分区
    ceph-deploy osd create --data /dev/sdb ceph-node-01
    ceph-deploy osd create --data /dev/sdb ceph-node-02
    ceph-deploy osd create --data /dev/sdb ceph-node-03

    # 查看节点状态
    sudo ceph -s

# 4. 创建metadata和增加监控

- 如果要使用CephFS，必须至少有一台metadata服务器
    ```bash
    ceph-deploy mds create ceph-node-01 ceph-node-02 ceph-node-03
    ```
- 可以将每个node都作为mon节点和mgr节点

    ```bash
    # 修改~/ceph-cluster/ceph.conf
    # public_network=10.100.1.0/24
    # 分发配置文件
    ceph-deploy --overwrite-conf config push ceph-node-01 ceph-node-02 ceph-node-03

    ceph-deploy mon add ceph-node-02
    ceph-deploy mon add ceph-node-03

    ceph-deploy mgr create ceph-node-02 ceph-node-03

    # 查看监控状态
    sudo ceph quorum_status --format json-pretty
    ```

# 5. Ceph文件系统和Kubernetes集成

## 5.1 创建pool

1. PG的计算
    - Total PGs = ((Total_number_of_OSD * 100) / max_replication_count) / pool_count
    - Total PGs = ((300)/1)/4

2. 创建CephFS的pool

    ```bash
    # 创建pool
    sudo ceph osd pool create cephfs_data 64
    sudo ceph osd pool create cephfs_metadata 64

    # 初始化文件系统
    sudo ceph fs new cephfs cephfs_metadata cephfs_data
    ```

3. 创建CephRBD的pool

    ```bash
    # 创建rbd的pool
    sudo ceph osd pool create kube 128
    
    # 定义池的使用类型
    ceph osd pool application enable rbd rbd --yes-i-really-mean-it
    ```

## 5.2 生成Ceph文件系统的访问密钥

1. 生成管理密钥

    ```bash

    # 在mon节点上获取admin的密钥
    sudo ceph auth get-key client.admin | base64

    # 生成ceph-secret-admin密钥文件
    ```

2. 生成用户密钥

    ```bash
    # 在mon节点上获取创建kube pool的用户
    sudo ceph auth get-or-create client.kube mon 'allow r' osd 'allow class-read object_prefix rbd_children, allow rwx pool=kube' -o ceph.client.kube.keyring 
    
    # 获取kube的密钥
    sudo ceph auth get-key client.kube | base64

    # 生成ceph-secret-rbd-kube密钥文件

    ```

## 5.3 创建部署

1. 创建CephFS的部署

    ```bash
    # 拉取镜像
    # docker pull tellxp/cephfs-provisioner:latest
    # docker tag tellxp/cephfs-provisioner:latest quay.io/external_storage/cephfs-provisioner:latest
    # docker rmi tellxp/cephfs-provisioner:latest

    # 注意cephfs的群集用户对于Secrets的访问权限需要具有get, create, delete
    kubectl apply -f ~./cephfs/deploy
    ```

2. 创建CephRBD的部署

    ```bash
    # 拉取镜像
    # docker pull tellxp/rbd-provisioner:latest
    # docker tag tellxp/rbd-provisioner:latest quay.io/external_storage/rbd-provisioner:latest
    # docker rmi tellxp/rbd-provisioner:latest

    # 注意cephfs的群集用户对于Secrets的访问权限需要具有get, create, delete
    kubectl apply -f ~./cephrbd/deploy
    ```

## 5.4 测试

1. CephFS的测试

    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
    name: cephfs-dpvc-test
    annotations:
        volume.beta.kubernetes.io/storage-class: "cephfs-class"
    spec:
    accessModes:
        - ReadWriteMany
    resources:
        requests:
        storage: 1Gi
    ```

2. CephRBD的测试

    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
    name: cephrbd-dpvc-test
    spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
        storage: 1Gi
    ```

# 6. 其他一些有用的命令片段

```bash
# 设置默认的storageclass，在不声明class注解的情况下默认调用
kubectl patch storageclass cephrbd-class -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

# 根据文本创建密钥，不需要base64编码
kubectl create secret generic ceph-secret-admin --from-literal=key='AQDd9qlbGhUwJBAAwxKgKzTEnhfSrXcz0AXwKg==' --namespace=kube-system --type=kubernetes.io/rbd
```


