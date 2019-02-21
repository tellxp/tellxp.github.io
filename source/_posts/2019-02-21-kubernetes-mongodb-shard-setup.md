---
layout: single
classes: wide
title:  "在Kubernetes中部署MongoDB Shard Cluster"
date:   2019-02-21 17:24:00 +0800
categories: kubernetes mongodb
---

# 安装MongoDB Enterprise Kubernetes Operator
1. 拉取官方的模板
    ```bash
    git clone https://github.com/mongodb/mongodb-enterprise-kubernetes.git
    ```

2. 使用helm生成yaml文件并执行
    ```bash
    helm template public/helm_chart > operator.yaml
    kubectl apply -f operator.yaml
    ```

3. 生成于mongodb ops manager连接需要的配置并应用
    ```yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ops-kubernetes-conf
      namespace: mongodb
    data:
      projectName: kubernetes
      orgId: 5c6a76e9c6079d3b30de9099
      baseUrl: http://mongodb-ops.dizall.com
    ```

4. 生成连接需要的凭证
    - 需要首先在Ops Manager上生成Public API Key
    ```bash
    kubectl -n mongodb  delete secret ops-cred

    kubectl -n mongodb  create secret generic ops-cred  \
    --from-literal="user=mongodb-ops@dizall.com" \
    --from-literal="publicApiKey=05fdfb7a-5430-4f90-a66b-b55b202d5cf4"
    ```

---

<!--more-->

# 部署shard cluster
1. 编辑需要的部署文件，这里使用了CRD
    ```yaml
    ---
    apiVersion: mongodb.com/v1
    kind: MongoDbShardedCluster
    metadata:
      name: ops-shard-cluster
      namespace: mongodb
    spec:
      shardCount: 12
      mongodsPerShardCount: 3
      mongosCount: 2
      configServerCount: 2
      version: 4.0.6-ent
      project: ops-kubernetes-conf
      credentials: ops-cred
      persistent: true
      configSrvPodSpec:
        cpu: '0.5'
        memory: 1G
      mongosPodSpec:
        cpu: '0.8'
        memory: 2G
      shardPodSpec:
        cpu: '0.6'
        memory: 3G
    ```

2. 拉取镜像
   ```bash
   docker pull tellxp/mongodb-enterprise-operator:0.7
   docker tag tellxp/mongodb-enterprise-operator:0.7 quay.io/mongodb/mongodb-enterprise-operator:0.7
   docker rmi tellxp/mongodb-enterprise-operator:0.7
   
   docker pull tellxp/mongodb-enterprise-database:0.7
   docker tag tellxp/mongodb-enterprise-database:0.7 quay.io/mongodb/mongodb-enterprise-database:0.7
   docker rmi tellxp/mongodb-enterprise-database:0.7
   ```

3. 在ops manager UI上面将operater所在的网段加入whitelist
4. 观察容器组的状态


# 部署的实例
- mmsBaseUri: http://mongodb-ops.dizall.com
- Account Public API Key: 05fdfb7a-5430-4f90-a66b-b55b202d5cf4

# 其他片段
- 批量操作pod和images
   ```bash
   # 批量操作pods
   kubectl -n mongodb get pods | grep Terminating | awk '{print $1}' | xargs kubectl -n mongodb  delete pod --grace-period=0 --force
   kubectl -n mongodb get pods | grep ops-shard | awk '{print $1}' | xargs kubectl -n mongodb  delete pod
   kubectl -n mongodb get pvc | grep ops-shard | awk '{print $1}' | xargs kubectl -n mongodb  delete pvc
   kubectl -n mongodb get pv | grep ops-shard | awk '{print $1}' | xargs kubectl -n mongodb  delete pv
   
   # 强制停止容器
   killall docker-containerd-shim
   
   # 批量操作容器
   docker ps -a | grep Exited | awk '{print $1}' | xargs docker rm
   docker ps -a | grep ops-shard | awk '{print $1}' | xargs docker stop
   docker ps -a | grep ops-shard | awk '{print $1}' | xargs docker rm -f
   ```

- 手动在pod中启用mongod
    ```bash
    /var/lib/mongodb-mms-automation/mongodb-linux-x86_64-4.0.5-ent/bin/mongod -f /data/automation-mongod.conf
    ```

- mongodb数据库操作
    ```
    # 显示和删除数据库
    show dbs
    use dbName
    db.dropDatabase()
    ```

- ssh远程执行命令
    ```bash
    ssh root@k8s-master-01 "systemctl restart docker && systemctl restart kubelet"
    ```


# 参考
1. [Install MongoDB Enterprise Kubernetes Operator](https://docs.opsmanager.mongodb.com/current/tutorial/install-k8s-operator/)
2. [Deploy a Replica Set](https://docs.opsmanager.mongodb.com/current/tutorial/deploy-replica-set/)
3. [Deploy a Sharded Cluster](https://docs.opsmanager.mongodb.com/current/tutorial/deploy-sharded-cluster/)
4. [Deploy a Standalone MongoDB Instance](https://docs.opsmanager.mongodb.com/current/tutorial/deploy-standalone/)
5. [MongoDB Kubernetes Object Specification](https://docs.opsmanager.mongodb.com/current/reference/k8s-operator-specification/)
6. [Backup and Restore Sharded Clusters](https://docs.mongodb.com/manual/administration/backup-sharded-clusters/)
