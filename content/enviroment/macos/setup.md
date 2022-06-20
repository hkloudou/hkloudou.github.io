---
title: "macos装机开发环境配置"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["broker","soft"]
---

``` sh
rm -rf ~/.starter && mkdir -p ~/.starter
curl -sSL https://github.com/hkloudou/macstarter/archive/706a744076d64ca294a8b75bd1d20101f96c121d.tar.gz | tar xz --strip 1 -C ~/.starter

if [ "1" -ne $(defaults read com.apple.dock autohide) ]; then
   sh ~/.starter/system/env.sh
   sh ~/.starter/system/trackpad.sh
   sh ~/.starter/system/dock.sh
   sh ~/.starter/system/finder.sh
   sh ~/.starter/system/screenshot.sh
fi


sh ~/.starter/installers/xcode.sh
sh ~/.starter/installers/go.sh
sh ~/.starter/installers/flutter.sh
sh ~/.starter/installers/tmignore.sh
sh ~/.starter/installers/clashx.sh

sh ~/.starter/apps/git.sh

rm -rf ~/.starter
```
