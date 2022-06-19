---
title: "macos装机开发环境配置"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["broker","soft"]
---

### 优化mac上的傻逼逻辑

``` sh
# 忽略密码必须4位数的傻逼规定
pwpolicy -clearaccountpolicies
# 彻底隐藏.DS_Store
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE # FALSE 是恢复
# 删除现有 .DS_Store 这里如果用xrgs rm 队友个bug：无法删除带空格的路径
find "`echo ~`/" -name .DS_Store -type f -delete
# sudo find / -name ".DS_Store" -depth -exec rm {} \;
# 检查
# find "`echo ~`/project" -name .DS_Store -type f | xargs echo

#Dock 配置
defaults write com.apple.Dock magnification -bool no
defaults write com.apple.dock tilesize -int 44
defaults write com.apple.dock largesize -int 48
defaults write com.apple.Dock autohide -bool yes
defaults write com.apple.Dock showhide -bool yes
defaults write com.apple.dock orientation bottom
defaults write com.apple.dock autohide-delay -float 0
defaults write com.apple.dock expose-animation-duration -float 0.12
killall Dock

#Finder
# https://blog.csdn.net/lovechris00/article/details/113280758
# defaults export com.apple.finder -
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES

# Screencapture
mkdir -p ~/Pictures/Screenshots
defaults write com.apple.screencapture location ~/Pictures/Screenshots
killall SystemUIServer

# Dashboard
# defaults write com.app.dashboard mcs-dis


#TrackPad 开三指拖拽，相信我，这个功能最重要
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag -bool YES
killall SystemUIServer
```

### 基础组件

``` sh
# bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# cocoapods
# gem 安装的时候1.8.4是网上建议，但是flutter 最小要求是1.9.0
# 可能会有个神奇的bug，考虑用brew绕坑
brew install cocoapods
# flutter
mkdir -p ~/Developments && cd ~/Developments && git clone https://github.com/flutter/flutter.git -b stable
flutter precache
# 只在新系统做吧，把golang bin 和flutter 的bin加入到gopath
echo "export PATH=\$PATH:`echo ~`/go/bin" > ~/.zshrc
echo "export PATH=\$PATH:`echo ~`/Developments/flutter/bin" >> ~/.zshrc
```

## git-crypt gpg 做些特殊场景的git加密

``` sh
brew install git-crypt gpg
```

### 配置全局Git ignore

> <https://www.jianshu.com/p/2b247923cb4b>

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

# node
bower_components/
node_modules/
">~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```
