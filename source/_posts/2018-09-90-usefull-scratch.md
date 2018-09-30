---
title: 常用语句片段
date: 2018-09-30 11:05:42
categories: 
- scratch
tags:
- scratch
- 片段
---
# 相关的操作

## Kubernetes相关
- [安装](./cluster-setup/README.md)
- [Ceph安装和集成](./ceph/README.md)
- [mongodb分片安装和集成](./mongodb/README.md)
  ```bash
  # 删除一个Label，只需在命令行最后指定Label的key名并与一个减号相连即可：
  $ kubectl label nodes 1.1.1.1 role-

  # 修改一个Label的值，需要加上--overwrite参数：
  $ kubectl label nodes 1.1.1.1 role=apache --overwrite

  ```
<!--more-->

## Ubuntu 18.04相关

  ```bash
  # Linux下完全删除一个用户
  userdel -r k8s-admin

  #修改网络配置
  vi /etc/netplan/50-cloud-init.yaml
  # network:
  #     ethernets:
  #         eth0:
  #             addresses:
  #             - 192.168.15.72/20
  #             gateway4: 192.168.12.2
  #             nameservers:
  #                 addresses:
  #                 - 223.5.5.5
  #                 search: []
  #             optional: true
  #     version: 2
  netplan apply

  # 列出可以更新的软件包
  apt list --upgradable
  ```

## MySQL相关

```sql
-- 查询表行数
SELECT 
  table_name,
  table_rows 
FROM
  INFORMATION_SCHEMA.TABLES 
WHERE TABLE_SCHEMA = 'DATABASE_NAME' ;
```

## Ceph相关

  ```bash
  # 删除节点
  ceph-deploy purge ceph-node-01

  # 列出磁盘
  ceph-deploy disk list ceph-node-01

  # 原来生成了osd后删除

  #获取磁盘的label
  sudo lvm lvscan
  sudo lvm vgscan
  sudo lvm pvscan

  # 按照label删除磁盘
  sudo lvm lvmremove ...
  sudo lvm vgremove ...
  sudo lvm pvremove ...

  # 删除pool
  sudo systemctl stop ceph-mds.target
  sudo ceph fs rm cephfs --yes-i-really-mean-it
  sudo ceph osd pool delete cephfs_metadata cephfs_metadata --yes-i-really-really-mean-it 

  # 分发ceph配置
  ceph-deploy --overwrite-conf config push ceph-node-01 ceph-node-02 ceph-node-03
  ```


## bash脚本

```bash
#!/bin/bash
images=(kube-proxy-amd64:v1.9.0 kube-scheduler-amd64:v1.9.0 kube-controller-manager-amd64:v1.9.0 kube-apiserver-amd64:v1.9.0
etcd-amd64:3.1.10 pause-amd64:3.0 kubernetes-dashboard-amd64:v1.8.3 k8s-dns-sidecar-amd64:1.14.7 k8s-dns-kube-dns-amd64:1.14.7
k8s-dns-dnsmasq-nanny-amd64:1.14.7)
for image in ${images[@]} ; do
  docker pull shayu/$image
  docker tag shayu/$image gcr.io/google_containers/$image
  docker rmi shayu/$image
done
```