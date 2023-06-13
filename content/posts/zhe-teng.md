---
title: "代码小白的网站折腾笔记"
description: 本人建站血泪史
date: 2022-04-18T22:09:32+08:00
lastmod: 2023-06-13T22:01:51+08:00
tags: [CS]
categories: 2022
hidden: false
comments: false
draft: false
---

## 2022.4.18

起因是发现safari浏览器不能正常加载谷歌字体库，一篇文章打开字体不一致真的逼死强迫症。于是想要把网站用的谷歌字体库替换成国内的镜像源，翻了半天material for MKdocs的文档，无果，最后还是靠别人的博客[Mkdocs 谷歌字体加载失败解决方法 - 运维笔记 (zongming.net)](http://zongming.net/read-1426/)解决了问题。

修改了以下两个文件：

```
# mkdocs/themes/readthedocs/base.html 查找 
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato:400,700|Roboto+Slab:400,700|Inconsolata:400,700" /> 

# 替换为    
<link rel="stylesheet" href="https://fonts.geekzu.org/css?family=Lato:400,700|Roboto+Slab:400,700|Inconsolata:400,700" /> 

# material/base.html 查找 
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family={{ 

# 替换为 
<link rel="stylesheet" href="https://fonts.geekzu.org/css?family={{
```
## 2023.6.13

又到了折腾博客主题的时间！起因是给朋友写了一篇租房指南，准备发一篇博客，恰好换了新电脑，github仓库里只放了网站文件没有放其他的……呃，生成网站的那些文件，完全没有考虑过换电脑会带来这些问题！所以不得不重新下载hugo啊配置环境啊，那不就顺手换个主题嘛（反正也不写博客）。看来看去，还是爱这一款[diary](https://github.com/AmazingRise/hugo-theme-diary)主题！是的，就是我上上次用的主题，人会在不同的时候踏入同一条河流……

因为我基本都是中文写作，所以调整了字体、字体大小、正文缩进。

均在`assets/scss/journal.scss`文件中：

```
# line 9 调整等宽字体优先选择Consolas
$mono-font-list: Consolas, "Fira Mono", "Cousine", Monaco, Menlo, "Source Code Pro", monospace;

# line 12 调整字体为优先选择思源宋体、衬线字体、
$sans-preferred-font-list: 'Noto Serif SC', serif, $default-font-list;

# line 26 调整正文字体大小为18px
$post-body-size: 18px;

# line 901 调整正文首行缩进2字符
p {
          text-indent: 2em;
        }
```

非常感谢[AmazingRise](https://github.com/AmazingRise)提供的简约又美观的主题！要是我能多写几篇博文就好了！