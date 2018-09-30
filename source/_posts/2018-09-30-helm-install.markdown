---
layout: single
classes: wide
title:  "在Kubernetes中使用helm"
date:   2018-09-30 08:36:38 +0800
categories: 
- kubernetes
tags:
- kubernetes
- helm
---

# 下载helm

1. 在[Helm Release](https://github.com/helm/helm/releases)上下载Linux amd64版本的helm包

2. 解压并移动到bin目录
    ```bash
    tar -zxvf helm-v2.0.0-linux-amd64.tgz
    mv linux-amd64/helm /usr/local/bin/helm
    ```
# 拉取tiller镜像

```bash
docker pull tellxp/tiller:v2.11.0
docker tag tellxp/tiller:v2.11.0 gcr.io/kubernetes-helm/tiller:v2.11.0
docker rmi tellxp/tiller:v2.11.0
```

# 创建tiller服务账户

```bash
kubectl create serviceaccount --namespace kube-system tiller

# 将服务账户绑定到cluster-admin
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller

kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
```

# 初始化
```bash
helm init --upgrade --stable-repo-url (repo-url)
#可选的国内repo
#https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts 已过期
#https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts/ charts较少
#https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts-incubator/ charts较少

```

# 使用

```bash
helm install .....
```

# 其他一些有用的命令

```bash
# 仅客户端运行
helm init --client-only --stable-repo-url 
```
