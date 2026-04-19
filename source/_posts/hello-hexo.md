---
title: hello hexo
date: 2018-05-05
tags:
  - hexo
toc: true
---

关于搭建博客的教程网上也很多，在此不过多赘述，只是提一些本人搭建时遇到的一些问题以及解决方案。

## 创建新仓库

在 GitHub 上面创建一个 repository 时，项目名称必须为 **用户名.github.io**。

## 关于 git 的操作

Git 是开源的分布式版本控制系统，用于敏捷高效地处理项目。  
安装成功后需要将 Git 与 GitHub 进行绑定，右键 git bash here 设置 user.name 和 user.email 配置信息

```bash
git config --global user.name "GitHub 用户名"
git config --global user.email "GitHub 注册邮箱"
```

生成秘钥文件：

```bash
ssh-keygen -t rsa -C "GitHub 注册邮箱"
```

直接回车便可默认生成 `.ssh` 文件。打开 `id_rsa.pub` 密钥，将内容全部复制，在 GitHub 中 new SSH key 粘贴。  
在 git bash 中检测 GitHub 公钥是否配置成功：

```bash
ssh git@github.com
```

若显示 `Hi xxx!` 则配置成功。

设置 GitHub 密钥是因为通过非对称加密的公钥与私钥来完成加密，公钥放置在 GitHub 上，私钥放置在自己的电脑里。GitHub 要求每次推送代码都是合法用户，所以每次推送都需要输入账号密码验证推送用户是否是合法用户，为了省去每次输入密码的步骤，采用了 ssh，当你推送的时候，Git 就会匹配你的私钥跟 GitHub 上面的公钥是否是配对的，若是匹配就认为你是合法用户，则允许推送，这样可以保证每次的推送都是正确合法的。

## hexo 常用命令

```bash
npm install hexo -g    # 安装 Hexo
npm update hexo -g     # 升级
hexo init              # 初始化博客
hexo new "我的博客"    # 新建文章（hexo n "我的博客"）
hexo generate          # 生成（hexo g）
hexo server            # 启动服务预览（hexo s）
hexo deploy            # 部署（hexo d）
hexo server -s         # 静态模式
hexo server -p 5000    # 更改端口
hexo server -i 192.168.1.1  # 自定义 IP
hexo clean             # 清除缓存，若是网页正常情况下可以忽略这条命令
```

## 后记

网上关于搭建博客的教程众多，给大家提供了便利，但也正因如此，各自教程里的一些细节说法不一，导致本人搭建博客时遇到了大大小小的问题，前前后后大概花了四天时间才搭好了最基本的界面- -！

最后，博客总算是搭建好了，希望自己以后能坚持写写博客，记录自己的学习经历。