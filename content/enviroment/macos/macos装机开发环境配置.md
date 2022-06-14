---
title: "setup"
date: 2021-11-28T14:06:09+08:00
name: xxx
draft: true
tags: ["broker","soft"]
---

# 基础环境配置
### 忽略密码必须4位数的傻逼规定
> https://macoshome.com/course/1542.html
``` sh
pwpolicy -clearaccountpolicies
```
### brew
> https://brew.sh
``` sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 我的工具箱【各位看官自便】
> git-crypt gpg 做些特殊场景的git加密
``` sh
brew install git-crypt gpg
brew install hugo@0.87.0
```
### 配置全局Git
> https://www.jianshu.com/p/2b247923cb4b
``` sh
echo "# .gitignore_global
####################################
######## OS generated files ########
####################################
.DS_Store
.DS_Store?
*.swp
._*
.Spotlight-V100
.Trashes
Icon?
ehthumbs.db
Thumbs.db
####################################
############# packages #############
####################################
*.7z  
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip">~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```