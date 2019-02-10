---
title: 常用语句片段
date: 2018-09-30 11:05:42
categories: 
- scratch
tags:
- scratch
- 片段
---

# 镜像配置
## python
```
配置方法

在文件

~/.pip/pip.conf

中添加或修改:

[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

## tensorflow

SET PATH=C:\Develop\cuda\bin;%PATH%
SET PATH=C:\Develop\cuda\extras\CUPTI\libx64;%PATH%
SET PATH=C:\Develop\cuDNN\bin;%PATH%


## npm
- 1.通过config命令
``` bash
npm config set registry https://registry.npm.taobao.org 
npm info underscore （如果上面配置正确这个命令会有字符串response）
```

- 2.命令行指定
```bash
npm --registry https://registry.npm.taobao.org info underscore 
```

- 3.编辑 ~/.npmrc 加入下面内容
registry = https://registry.npm.taobao.org

- 切换回原来的源
如果你要在npm 发布组件，要记得切换回原来的源，不然用户登录报错
npm config set registry https://registry.npmjs.org/

> node-sass 安装失败的原因
npm 安装 node-sass 依赖时，会从 github.com 上下载 .node 文件。由于国内网络环境的问题，这个下载时间可能会很长，甚至导致超时失败。
这是使用 sass 的同学可能都会遇到的郁闷的问题。
解决方案就是使用其他源，或者使用工具下载，然后将安装源指定到本地。
解决方法一：使用淘宝镜像源
设置变量 sass_binary_site，指向淘宝镜像地址。示例：
	npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/

	// 也可以设置系统环境变量的方式。示例
	// linux、mac 下
	SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/ npm install node-sass

	// window 下
	set SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/ && npm install node-sass
或者设置全局镜像源：
1	npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
之后再涉及到 node-sass 的安装时就会从淘宝镜像下载。
解决方法二：使用 cnpm
另外，使用 cnpm 安装 node-sass 会默认从淘宝镜像源下载，也是一个办法：
1	cnpm install node-sass
解决方法三：下载 .node 到本地
到这里去根据版本号、系统环境，选择下载 .node 文件：
https://github.com/sass/node-sass/releases
然后安装时，指定变量 sass_binary_path，如：
1	npm i node-sass --sass_binary_path=/Users/lzwme/Downloads/darwin-x64-48_binding.node
安装失败后重新安装问题
最后，有同学问，之前安装失败，再安装就不去下载了，怎么办呢？那就先卸载再安装：
1	npm uninstall node-sass && npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/

## gradle
原创 2017年09月22日 14:27:35 
	• 标签：
	• gradle 
* 修改gradle初始化脚本
 
gradle 生命周期中有一个叫 初始化( Initialization )的过程，这个过程运行在 build script 之前，我们可以在这个地方做一点系统全局的设置，就比如*配置仓库地址*
你可以在这些地方使用你的初始化脚本：
1、命令行 (这个我就不说了
2、放一个init.gradle 文件到USER_HOME/.gradle/目录下
3、放一个后缀是.gradle的文件到 USER_HOME/.gradle/init.d/ 目录下
4、放一个后缀是.gradle的文件到 GRADLE_HOME/init.d/ 目录下.

本人使用的是第4种方法，  和gradle-4.1
init.gradle文件内容：
[html] view plain copy
	1. allprojects {  
	2.     repositories {  
	3.          maven {  
	4.              name "aliyunmaven"  
	5.              url "http://maven.aliyun.com/nexus/content/groups/public/"  
	6.          }  
	7.     }  
	8. }  


## CentOS
CentOS是Redhat的克隆版本，CentOS团队已经被Reahat收购。
默认Redhat使用自身的收费yum源，一般使用更改为国内镜像CentOS的源。

默认Redhat7.1不带三方源
	1. 获取repo列表
	wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
	2. 将CentOS-7.repo内的$releasever替换为7；
	3. yum make clean && yum make cache&&yum update

## MSSQL Server防火墙配置
需要允许这些端口80, 135, 443, 445, 1433, 1434, 1688, 2383, 3389, 5022,5985


## 安装.NET Framework 3.5
Dism /online /enable-feature /featurename:NetFX3 /All /Source:E:\sources\sxs /LimitAccess

## 时钟源配置
- Windows Time Service Tools and Settings
  - https://docs.microsoft.com/en-us/windows-server/networking/windows-time-service/windows-time-service-tools-and-settings

## Visual Studio 重置
方法：
1.开始->Microsoft Visual Studio 2013->Visual Studio  Tools->VS2013 x64 兼容工具命令提示
2.cd C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE     然后输入：devenv.exe /setup /resetuserdata /resetsettings
重启vs2012 就可以了。


## chrome在windows10下乱码问题
需要在windows10的可选功能中添加繁体中文字符集；


# 常用网站
- Intellij破解 http://idea.lanyus.com 

# 相关的操作

## 蓝灯VPN相关

```bash
lantern.exe --addr 10.100.1.141:11111
```

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