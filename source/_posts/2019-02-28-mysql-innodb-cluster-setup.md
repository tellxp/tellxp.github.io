---
layout: single
classes: wide
title:  "部署MySQL InnoDB Cluster"
date:   2019-02-28 21:59:00 +0800
categories: 
    - mysql
    - ha
---

# 准备安装环境
1. 准备3台Windows Server, 设置hostname分别为
   - ic-node-01: 主节点
   - ic-node-02: 从节点
   - ic-node-03: 从节点
   - mysql-em-01: 监控节点

2. 配置IP和并分发hosts文件

---

<!--more-->

# 安装MySQL实例
1. 使用Windows Installer安装MySQL Server


2. 在mysqld运行的node上使用mysql command，配置集群账户
    ```mysql
    create user 'cluster_admin'@'%' identified by 'Mf#llsy78g';
    grant all privileges on *.* to 'cluster_admin'@'%' with grant option;
    reset master;
    ```
   > 也可以配置权限更少的账户
    ```mysql
    GRANT SELECT ON mysql_innodb_cluster_metadata.* TO your_user@'%';
    GRANT SELECT ON performance_schema.global_status TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_applier_configuration TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_applier_status TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_applier_status_by_coordinator TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_applier_status_by_worker TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_connection_configuration TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_connection_status TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_group_member_stats TO your_user@'%';
    GRANT SELECT ON performance_schema.replication_group_members TO your_user@'%';
    GRANT SELECT ON performance_schema.threads TO your_user@'%' WITH GRANT OPTION;
    ```

# 安装mysql shell
mysql shell可以远程配置群集, 可以将mysql shell安装在管理节点上

1. 下载mysql-apt配置工具
   ```shell
   wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
   ```

2. 安装mysql-shell
    ```shell
    apt-get update && apt-get install mysql-shell
    ```
# 创建群集
1. 检查状态和配置，针对mysqld节点
    ```js
    // ic-node-01
    dba.checkInstanceConfiguration('cluster_admin@ic-node-01:3306',{'password':'Mf#llsy78g'});
    dba.configureInstance('cluster_admin@ic-node-01:3306',{'password':'Mf#llsy78g'});

    // ic-node-02
    dba.checkInstanceConfiguration('cluster_admin@ic-node-02:3306',{'password':'Mf#llsy78g'});
    dba.configureInstance('cluster_admin@ic-node-02:3306',{'password':'Mf#llsy78g'});

    // ic-node-03
    dba.checkInstanceConfiguration('cluster_admin@ic-node-03:3306',{'password':'Mf#llsy78g'});
    dba.configureInstance('cluster_admin@ic-node-03:3306',{'password':'Mf#llsy78g'});
    ```

2. 修改server id
    - 修改C:\ProgramData\MySQL\MySQL Server 8.0\my.ini
    - 分别设置node1-3的server_id为01-03，重启服务

3. 连接到主节点
    ```shell
    mysqlsh 'cluster_admin'@'10.100.1.181' --password=Mf#llsy78g
    ```
4. 执行创建群集的操作
    ```js
    cluster = dba.createCluster('dev_cluster');
    ```

5. 检查集群节点状态
    ```js
    cluster = dba.getCluster('dev_cluster');
    cluster.status();
    cluster.checkInstanceState('cluster_admin@ic-node-02:3306',{'password':'Mf#llsy78g'});
    cluster.checkInstanceState('cluster_admin@ic-node-03:3306',{'password':'Mf#llsy78g'});
    ```

6. 配置防火墙端口
    - 允许每个节点上3306, 33060,33061的TCP连接

7. 将其他节点加入群集
    ```js
    cluster.addInstance('cluster_admin@ic-node-02:3306',{'password':'Mf#llsy78g'});
    cluster.addInstance('cluster_admin@ic-node-03:3306',{'password':'Mf#llsy78g'});

    //在加入集群失败后，可以通过如下命令来重置群集
    cluster = dba.rebootClusterFromCompleteOutage();
    ```

# 安装router
- 在每个节点安装router, 指向本机的hostname;
- 允许6446-6448端口的TCP连接
- 配置前端haproxy的负载均衡

# 配置监控
1. 安装MySQL Enterprise Monitor Service

2. 在每个节点安装monitor agent
    - 需要配置agent的url指向monitor server节点，即mysql-em-01的DNS
    - 配置agent的密码为安装mysql enterprise monitor service设置的密码
        > 加入[query analyse](https://dev.mysql.com/doc/mysql-monitor/4.0/en/mem-qanal-using-performance-schema.html)

3. 更新自签名的ssl证书
    - 使用acme.sh获取证书后，通过openssh转为pem
       ```shell
       openssl pkcs12 -export -in /root/.acme.sh/dizall.com/dizall.com.cer -inkey /root/.acme.sh/dizall.com/dizall.com.key -out /root/ssl/tomcat/cert.pem -name tomcat
       ```
    - 使用java自带的keytool将密钥导入到tomcat中
        ```shell
        keytool -importkeystore -srckeystore cert.pem -srcstoretype pkcs12 -destkeystore "C:\Program Files\MySQL\Enterprise\Monitor\apache-tomcat\conf\keystore"  -deststoretype jks -srcalias tomcat -destalias tomcat
        ``` 
    - 查看tomcat的密钥
        ```shell
        keytool -keystore "C:\Program Files\MySQL\Enterprise\Monitor\apache-tomcat\conf\keystore" -list -v
        ```
   
# 其他
1. 检查mysqld运行节点主机名
    ```mysql
    SELECT coalesce(@@report_host, @@hostname);
    ```

2. 设置root远程访问
    ```mysql
    use mysql;
    select user,host from user;
    update user set host='%' where user='root';
    flush privileges;
    ```

3. mysql shell操作
    - shell连接
        ```shell
        mysqlsh 'cluster_admin'@'10.100.1.181' --password=Mf#llsy78g
        mysqlsh 'cluster_admin'@'10.100.1.182' --password=Mf#llsy78g
        mysqlsh 'cluster_admin'@'10.100.1.183' --password=Mf#llsy78g
        ```

    - 集群操作
        ```js
        // 删除实例
        cluster.removeInstance('cluster_admin@ic-node-03:3306',{'password':'Mf#llsy78g'});
        
        // 把节点重新加入集群
        cluster.rejoinInstance('cluster_admin@ic-node-02:3306',{'password':'Mf#llsy78g'});
        
        // 销毁群集
        cluster.dissolve({force:true});
        ```
# 参考
1. [InnoDB Cluster](https://dev.mysql.com/doc/refman/8.0/en/mysql-innodb-cluster-userguide.html)
2. [MySQL InnoDB Cluster : MySQL Shell and the AdminAPI](https://lefred.be/content/mysql-innodb-cluster-mysql-shell-and-the-adminapi/)
3. [MySQL 8.0 InnoDB Cluster – the quick hands-on manual](https://lefred.be/content/mysql-8-0-innodb-cluster-the-quick-hands-on-manual/)
4. [MySQL InnoDB Cluster 8.0 – A Hands-on Tutorial](https://mysqlserverteam.com/mysql-innodb-cluster-8-0-a-hands-on-tutorial/)
