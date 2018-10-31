---
title: GitHub + Hexo 搭建博客
tags: 
  - hexo
  - github
categories: 
  - note
---

## 事先准备

* Node.js
* git

## 创建 GitHub Pages
这个百度都有

## 安装 Hexo 并检查是否安装成功

``` bash
cd D://hexo
npm install hexo -g
hexo -v
```

## 初始化 Hexo

``` bash
hexo init
```

## 依赖包安装

``` bash
npm install
```

## 编译

``` bash
hexo g
```

## 打开服务器

``` bash
hexo s
```

默认是 localhost:4000

## 联系到 GitHub

打开 Hexo 文件夹里的 _config.yml 文件
配置Deployment

``` bash
deploy:
  type: git
  repository: 你的 repo 值
  branch: master
```

## 安装扩展

``` bash
install hexo-deployer-git --save
```

## 写作

``` bash
hexo new <file-name>
```

## 部署到GitHub

``` bash
hexo d
```