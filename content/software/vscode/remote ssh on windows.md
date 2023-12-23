---
title: "如何在Windows下开启OpenSSH，后续可用于Vscode的Remove SSH开发"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["windows","OpenSSH"]
---

# 如何在windows启用openssh

## 在Windows安装 OpenSSH 服务器
1. 在“设置”中，找到并点击“应用”部分。
2. 在“应用与功能”页面中，点击“可选功能”。
3. 在“可选功能”页面中，点击“添加功能”。
4. 在列表中找到“OpenSSH 服务器”，然后点击“安装”。

## 启动和配置 OpenSSH 服务器
1. 按 Win + R 键打开“运行”对话框，输入 services.msc 并回车。
2. 在服务列表中找到“OpenSSH SSH 服务器”服务。
3. 右击该服务，选择“属性”。在“启动类型”中选择“自动”。
4. 点击“开始”按钮以启动服务。
5. 点击“确定”或“应用”保存更改。

## 配置防火墙
确保 Windows 防火墙允许 SSH 连接。

1. 打开“控制面板”。
2. 选择“系统和安全” > “Windows 防火墙”。
3. 点击左侧的“允许应用或功能通过 Windows 防火墙”。
4. 确保“OpenSSH 服务器 (sshd)”在私有和公有网络上都被允许。

## 配置密钥访问
> 在Windows OpenSSH中，默认的授权密钥存放位置为ProgramData\ssh\administrators_authorized_keys，此位置对应为管理用户权限。因此需要修改默认授权文件位置。通过文本编辑器（推荐vscode）打开ProgramData\ssh\sshd_config，修改以下条目

``` sh
#允许公钥授权访问，确保条目不被注释
PubkeyAuthentication yes

#授权文件存放位置，确保条目不被注释
AuthorizedKeysFile	.ssh/authorized_keys

#可选，关闭密码登录，提高安全性
PasswordAuthentication no

#注释掉默认授权文件位置，确保以下条目被注释
#Match Group administrators
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

## 创建.ssh目录
- 在用户的主目录下创建一个名为.ssh的目录（如果还不存在的话）。例如，对于用户名为win10的用户，路径通常是C:\Users\win10\.ssh。
## 创建或编辑authorized_keys文件
- 在.ssh目录内，创建或编辑一个名为authorized_keys的文件。
- 将你希望允许通过SSH密钥认证登录的用户的公钥粘贴到这个文件中。每个公钥应该占一行。

## 设置权限
``` sh
cd C:\Users\win10
icacls .ssh
icacls .ssh /reset
icacls .ssh /grant "win10:R" /T

cd .ssh
icacls authorized_keys
icacls authorized_keys /reset
icacls authorized_keys /grant "win10:RW"
```

## 大功告成
> 在服务里重启OpenSSH