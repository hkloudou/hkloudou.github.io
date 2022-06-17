---
title: "TimeMachine备份忽略 .gitignore"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["broker","soft"]
---
## 安装tmignore

> <https://github.com/samuelmeuli/tmignore>

方式一：需要Xcode，不推荐

``` sh
brew install samuelmeuli/tap/tmignore
```

方式二：直接下载bin

``` sh
curl -L https://github.com/hkloudou/tmignore/releases/download/v1.2.6/tmignore > /usr/local/bin/tmignore
chmod u+x /usr/local/bin/tmignore
```

## 快速删除Ds

``` sh
find "`echo ~`/project" -name .DS_Store | xargs rm -rf
```

## 添加系统级跳过

``` sh
U=~
CT1=$U/Library/Containers
CA1=$U/Caches

IG_Containers_podcast=("$CT1/com.apple.podcasts" "$CT1/com.apple.podcasts.MacPodcastsStorageExtension" "$CT1/com.apple.podcasts.MacQuicklookExtension" "$CT1/com.apple.podcasts.PodcastsNotificationExtension" "$CT1/com.apple.podcasts.widget")
IG_Containers_music=("$CT1/com.apple.Music.MusicCacheExtension" "$CT1/com.apple.Music.MusicStorageExtension")
IG_Containners=($IG_Containers_podcast $IG_Containers_music)

IG_SYSTEM=("$U/.Trash" "$U/Downloads" "$U/Music" "$U/Movies" "$U/Pictures" "$U/Developments")

IG_APP_XCODE=("/Applications/Xcode.app" "$U/Library/Developer/CoreSimulator" "$U/Library/Developer/CoreSimulator" "/Applications/Xcode.appdownload")
IG_APP_ANDROID=("/Applications/Android Studio.app" "$U/Library/Android")
IG_APP=($IG_APP_XCODE $IG_APP_ANDROID)

IG_CACHE_DEV=("$U/.cache" "$U/.npm" "$U/.node-gpy" "$U/.gradle" "$U/.dartserver" "$U/.pub-cache")
IG_CACHE_APP=("$CT1/com.apple.Safari/Data/Library/Caches" "$U/Library/Caches/com.apple.dt.Xcode")
IG_CACHE=($IG_CACHE_DEV $IG_CACHE_APP)
echo $IG_SYSTEM $IG_APP $IG_CACHE $IG_Containners "`go env GOMODCACHE`" "`go env GOCACHE`"
sudo tmutil addexclusion -p $IG_SYSTEM $IG_APP $IG_CACHE $IG_Containners "`go env GOMODCACHE`" "`go env GOCACHE`"
# 删除常见开发环境缓存（日常安全删除）
# sudo rm -rf $IG_CACHE "`go env GOMODCACHE`" "`go env GOCACHE`"

# sudo tmutil removeexclusion -p $IG_SYSTEM $IG_APP $IG_CACHE_DEV $IG_CACHE, but we suggest you delete them in setting ui
```

## 配置ignore

> 1. 自动扫描 ~ 目录里的gitignore 文件，并自动排除到tmutil 工具，注意，这里不显示到控制台，所以需要依赖tmignore的内存文件 ~/Library/Caches/tmignore
> 2. 通过分析源码：<https://github.com/hkloudou/tmignore> 我们得知：目前功能比较基础
> 3. ignoredPaths用来配置忽略扫描的目录

``` sh
mkdir -p ~/.config/tmignore/
echo '{
    "searchPaths": ["~"],
    "ignoredPaths": ["~/.Trash", "~/Applications", "~/Downloads", "~/Library", "~/Music/iTunes", "~/Music/Music", "~/Pictures/Photos Library.photoslibrary"]
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
