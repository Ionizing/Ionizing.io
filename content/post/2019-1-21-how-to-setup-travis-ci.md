---
title: 如何为 github 上的 cpp 项目设置 Travis-CI 自动测试
date: 2019-01-21 11:32:17
tags:
  - cpp
  - travis-ci
categories:
  - Programming
---

# 前言

Travis-CI 是一个持续集成测试网站，其 org 版对用户免费开放，对用户的 public 仓库可以免费提供 持续集成测试服务（私有仓库要使用 com 版）

# 设置步骤

1. 访问 [](https://travis-ci.org) 并使自己的 github 账户与 travis-ci 关联，并给予它足够的权限 （一路下一步就行）；
2. 点击右上角的用户头像，进入自己的 repositories 配置面板，找到自己想要开启集成测试的项目， 开启集成测试功能；
3. 进入刚刚项目集成测试的 settings ，在页面最下面的 Cron Jobs 一栏设置自己想要的定时测试任务 配置；
4. 在**本地**该仓库的根目录下写好 `Makefile` ；
5. 在**本地**该仓库的根目录下创建 `.travis.yml` ，里面写上如下内容：
```yml
language: cpp     # 指定项目语言类型
dist: trusty
sudo: false       # 是否需要 sudo 权限
matrix:           # 配置 travis-ci 系统变量
  include:
    - os: linux   # 指定构建环境的系统类型
    - compiler: gcc  # 指定编译器

addons:           # 第三方依赖库的添加
  apt:
    packages:
      - libfftw*

script:           # 构建的命令，默认为 ./configure && make
  - make
```
6. git push

然后 Travis-CI 应该就开始进行构建了，在构建的页面上还有构建状态的 badge ，可以放到 github 的 `README.md` 上标注构建状态 = =

# 后记

Travis-CI 还有很多功能没有用到，不过对于现在我的 repo 已经完全够用了。

GitHub 都已经开通了免费的 private repo ，travis-ci 什么时候也能开通免费版的 private plan 呢？


参考来源: https://blog.csdn.net/u012348774/article/details/78663381
