---
title: "TimeMachine备份忽略node_modeules"
date: 2021-11-28T14:06:09+08:00
draft: true
tags: ["broker","soft"]
---
## 安装tmignore

> 系统要求： xcode4
> <https://github.com/samuelmeuli/tmignore>

``` sh
brew install samuelmeuli/tap/tmignore
```

## 配置ignore

``` sh
mkdir -p ~/.config/tmignore/
echo '{
 "searchPaths": ["~"],
 "ignoredPaths": ["~/.Trash", "~/Applications", "~/Downloads", "~/Library", "~/Music/iTunes","~/.vscode","~/.npm",".DS_Store","node_modules"]
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
