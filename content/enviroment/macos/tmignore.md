---
title: "TimeMachine备份忽略node_modeules"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["broker","soft"]
---
## 安装tmignore

> 系统要求： xcode
> <https://github.com/samuelmeuli/tmignore>

方式一：需要Xcode，不推荐

``` sh
brew install samuelmeuli/tap/tmignore
```

方式二：直接下载bin

``` sh
curl -L https://github.com/samuelmeuli/tmignore/releases/download/v1.2.2/tmignore > /usr/local/bin/tmignore
chmod u+x /usr/local/bin/tmignore
```

## 快速删除Ds

``` sh
find "`echo ~`/project" -name .DS_Store | xargs rm -rf
```

## 添加系统级跳过

``` sh
sudo tmutil addexclusion -p "`echo ~`/.Trash" "`echo ~`/Downloads" "`echo ~`/Downloads"

# devtool cache
sudo tmutil addexclusion -p `echo ~`/.cache `echo ~`/.npm `echo ~`/.node-gpy `echo ~`/.gradle `echo ~`/.dartserver `echo ~`/.pub-cache

# xcode
sudo tmutil addexclusion -p "/Applications/Xcode.app" "/Applications/Xcode.appdownload"
# golang
tmutil addexclusion -v "`echo go env GOMODCACHE`"
```
<!-- ~/Library/Application Support/Caches -->
## 配置ignore

``` sh
mkdir -p ~/.config/tmignore/
echo '{
    "searchPaths": ["~"]
}'> ~/.config/tmignore/config.json
```

## 命令

> If you want to run the script once:

```sh
tmignore run
```

> To schedule the script to run once a day:

```sh
brew services start tmignore
```

- **`run`:** Searches the disk for files/directories ignored by Git and excludes them from future Time Machine backups
- **`list`:** Lists all files/directories that have been excluded by `tmignore`
- **`reset`:** Removes all backup exclusions that were made using `tmignore`
