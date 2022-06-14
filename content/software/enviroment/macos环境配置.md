---
title: "macos环境配置"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["broker","soft"]
---

# 基础环境配置
### 忽略密码必须4位数的傻逼规定
> https://macoshome.com/course/1542.html
``` sh
pwpolicy -clearaccountpolicies
```
### 安装brew
> https://brew.sh
``` sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```