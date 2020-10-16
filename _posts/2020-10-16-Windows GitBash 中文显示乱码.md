---
layout: post
title: Windows GitBash 中文显示乱码
date: 2020-10-16 15:03:12
categories: Git
tags: [Git,GitBash]
---

## 问题描述

在项目目录下，在`Git Bash`执行 `ls` 或 `git status` 无法正常显示中文文件名称（乱码）

## 解决方案

首先在`Gt Bash`中执行
```
git config --global core.quotepath false
```

然后在`Git Bash`的界面中右击空白处，弹出菜单，选择`选项`->`文本`,按以下参数配置：
|参数|参数值|
|:---|:---|
|本地|zh_CN|
|字符集|UTF-8|

最后重启`Git Bash` 即可。
