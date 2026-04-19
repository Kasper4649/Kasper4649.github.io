---
title: Hello Hexo, Again
date: 2026-04-19 10:56:49
tags:
  - hexo
toc: true
---

时隔六年再次更新，这期间发生了太多事情，读研、实习、工作、谈恋爱，一切似乎都沿着正确的人生轨迹迈进，六年前的自己可能也想象不出现在的自己正在周末值班。

这次更新也是为了能再次捣鼓一下技术，毕竟自从离开字节选择国企后，就彻底跟技术无缘了，平时的工作内容基本也就是写文档统计表格，偶尔也会写一些脚本来辅助完成统计工作，但在如今大模型的浪潮下，确实技术停滞多年，重新部署 blog 也遇到了很多问题，刚好记录一下。

## blog 添加图片
在 `_config.yml` 中开启 
```yml
post_asset_folder: true
```
这样每次 `hexo new post "post name"` 之后会在 `source/_posts/post-name` 目录下创建一个同名的文件夹，专门用来存放该文章的资源文件。

此时 hexo 依然不能正确解析 markdown 语法 `![](image.png)`。

还需要在 `_config.yml` 中开启
```yml
marked:
  prependRoot: true
  postAsset: true
```

## 部署 GitHub Action
虽然也不是第一次部署，但 blog 一直没用过，这次部署在 AI 的帮助下还是踩了坑。由于把 theme 作为一个 submodule 引入了，所以在部署的时候也需要把 submodule 的内容拉取下来，否则生成的页面全是空白。

参考当时提的 commit [添加 submodule](https://github.com/Kasper4649/Kasper4649.github.io/commit/efe2b1fadaa2960b7084ef1d1e0c697efdf16d55)

---

不论是在工作中写脚本，还是部署 blog，真的能真真切切体会到 AI 大模型的强大，完全颠覆了工作流，一般不会太来回沟通几次就能得到想要的结果。可以预见互联网真的越来越卷，自己选择国企是不是的的确确是正确的，也许下一个六年就能有更清晰的认识了。