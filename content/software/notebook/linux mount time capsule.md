---
title: "Linux 挂载 Apple time capsule 实录"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["time capsule","Apple","Linux"]
---

## 设备分析

通过设备分析我们得知，固件升级到最新7.9.1的设备只支持SMB/CIFS 1.0协议以及AFP苹果自己研发的协议，所以做出以下操作。

## 配置密码访问
>
> 1. 在mac或ios 打开Airport Utility软件，输入密码后进入设备
> 2. 在菜单的Disks【磁盘里】，把Secure Shared Disks【密码分享方式】修改为With Accounts【适用账号密码】，然后在下面的框里加入密码

**Tip:**

## 挂载命令

``` sh
mkdir -p /mnt/share
mount.cifs //服务器/路径「保持和账号相同」 /mnt/share --verbose  -o username=账号,pass=密码,sec=ntlm,vers=1.0,dir_mode=0755,file_mode=0644,noperm
```

### 参数解析

>1. -o 的配置信息是能够连接的精髓
>2. sec 安全模式需要手动指定为ntlm
>3. vers 需要手动设置为1.0
>4. dir_mode 设置目录权限
>5. file_mode 设置文件权限【这两部分应该都懂吧】
>6. noperm 跳过权限验证

## 在k3s/k8s的应用

> 由于笔者酷爱k3s，只提供k3s的示例。项目网址：<https://github.com/fstab/cifs>

安装cifs插件到每台设备

``` sh
VOLUME_PLUGIN_DIR="/usr/libexec/kubernetes/kubelet-plugins/volume/exec"
mkdir -p "$VOLUME_PLUGIN_DIR/fstab~cifs"
cd "$VOLUME_PLUGIN_DIR/fstab~cifs"
curl -L -O https://raw.githubusercontent.com/fstab/cifs/master/cifs
chmod 755 cifs
$VOLUME_PLUGIN_DIR/fstab~cifs/cifs init
```

配置secret文件

``` sh
kubectl create secret generic cifs-secret --type fstab/cifs --from-literal=username=账号 --from-literal=password=密码 -n pods所在命名空间
```

测试Pod

``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
spec:
  containers:
  - name: busybox
    image: busybox
    command:
      - sleep
      - "3600"
    imagePullPolicy: IfNotPresent
    volumeMounts:
    - name: test
      mountPath: /data
      subPath: test_path
  volumes:
  - name: test
    flexVolume:
      driver: "fstab/cifs"
      fsType: "cifs"
      secretRef:
        name: "cifs-secret"
      options:
        networkPath: "//server/share"
        mountOptions: "dir_mode=0755,file_mode=0644,noperm"
```

这里推荐参考笔者的用helm 安装gogs的项目，并实现通过Time Machine 自动备份。项目网址：<https://github.com/hkloudou/helm-charts/tree/main/charts/gogs>

使用方法：

``` sh
helm repo add hkloudou https://hkloudou.github.io/helm-charts/
helm install gogs --namespace gogs -f ./gogs.yaml hkloudou/gogs
```
