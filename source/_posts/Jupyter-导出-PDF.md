---
title: Jupyter 导出 PDF
date: 2018-11-26
tags:
  - Jupyter
  - PDF
toc: true
---

# 必须安装的软件或 Python 库

Jupyter Notebook 导出成 PDF 需要 LaTeX 以及 Pandoc 支持。

1. 安装 [pandoc](https://github.com/jgm/pandoc/releases/tag/1.15.0.6)
2. 安装 [MIKTeX](http://www.miktex.org/download)
3. 安装 nbconvert 库

# 中文支持

由于默认模版的设定，Latex 无法识别中文无法导出。为了解决这个问题，需要将 ipynb 转为 tex，然后修改 tex 内容，最后由 MIKTeX 生成 PDF 文件。

## 修改 nbconvert

首先找到 nbconvert 库下的 `article.tplx`（`~\Anaconda3\Lib\site-packages\nbconvert\templates\latex`），修改 `\documentclass[11]{article}` 为 `\documentclass{ctexart}`。

## 安装 MIKTeX 依赖包

如果此时直接将 ipynb 导出为 PDF 则会显示错误，需安装依赖包。

![convert-to-pdf-error](convert-to-pdf-error.webp)
![install-packages](install-packages.webp)

安装依赖包过程中，会经常遇到无法连接到服务器，只需多次点击安装。

![connect-to-server-error](connect-to-server-error.webp)

用 Jupyter Notebook 可以直接将 ipynb 导出为 tex 文件，并直接修改其内容，在 `\documentclass{article}` 后面插入：

```
\usepackage{fontspec, xunicode, xltxtra}
\setmainfont{Microsoft YaHei}
```

## 生成 PDF

最后编译 tex，生成 PDF：

```bash
xelatex your_tex_name.tex
```
![convert-to-pdf](convert-to-pdf.webp)
此时通过 Jupyter Notebook 也可以直接导出为 PDF。