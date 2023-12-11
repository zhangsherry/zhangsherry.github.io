---
title: "代码小白的网站折腾笔记"
description: 本人建站血泪史
date: 2022-04-18T22:09:32+08:00
lastmod:
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

# line 46 调整移动端字体大小
$single-column-post-title-size: 26px;
$single-column-post-summary-size: 16px;
$single-column-post-body-size: 18px;
$single-column-post-meta-size: 16px;

# line 901 调整正文首行缩进2字符
p {
          text-indent: 2em;
        }
```

非常感谢[AmazingRise](https://github.com/AmazingRise)提供的简约又美观的主题！要是我能多写几篇博文就好了！

## 2023.12.9

为了不再每次手动输入文章的修改时间，修改了`config.toml`文件：

```
# line 84
[frontmatter]
lastmod = ["lastmod" ,':fileModTime', ":git", "date"]
```

想要简化每次push到github仓库的步骤，于是将整个文件夹都push到仓库了，github pages部署方式改为github actions，结果部署完网站之后出现字体调整、正文缩进全部失效的问题，本地部署没问题。

从github远程仓库拉取到本地，发现之前对`themes/diary/assets/scss/journal.scss`的修改全部消失了！猜测应该是主题作者使用了[.gitmodules](https://github.com/zhangsherry/hugo-theme-diary/blob/main/.gitmodules)子模块，所以我在本地的修改无效。按照ChatGPT的建议，fork了仓库并且修改：

在主题文件仓库`assets/scss/journal.scss`文件中：

```
# line 6 调整字体为优先选择思源宋体、衬线字体
$default-font-list: 'Noto Serif SC', serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,

# line 9 调整等宽字体优先选择Consolas
$mono-font-list: Consolas, "Fira Mono", "Cousine", Monaco, Menlo, "Source Code Pro", monospace;

# line 12 保持修改，虽然不知道为什么毫无作用
$sans-preferred-font-list: 'Noto Serif SC', serif, $default-font-list;

# line 26 正文字体大小为17
$post-body-size: 17px;

# line 49 移动端字体大小为17
$single-column-post-body-size: 17px;

# line 901 调整正文首行缩进2字符
p {
          text-indent: 2em;
        }
```

`archetypes/default.md`文件调整为：

```
title: ""
date:
lastmod:
description: ""
tags: []
categories:
hidden: false
comments: false
draft: false
```

在根目录仓库`layouts/partials/extended_head.html`文件中：

```
# line 3
<script>
  loadCSS("https://fonts.googleapis.com/css?family=Consolas|Lora|Montserrat|Fira+Mono|Noto+Serif+SC|Material+Icons");
</script>
```

## 2023.12.10

折腾了一晚上，终于调整好字体了！！！感谢ChatGPT，使我理解了.gitmodules，从现在开始养成fork的好习惯，本地改了一万年，push到远程仓库发现完全没变化！

而且本来不至于折腾到今天的，使用命令`hugo server`之后本地看字体改过来了，过于欣喜，忘记publish还得使用命令`hugo`，push完发现字体又没变化，大崩溃！好在洗了个澡清醒多了。跌跌撞撞也在逐渐明白各种工具怎么用，很好！

lastmod日期不管怎么修改都不正确……暂时放弃了，实在不行就手动修改吧，一次失败的自动化尝试。更新：经过搜索，发现似乎是hugo与git的兼容性问题，这也解释了为什么github pages上部署会出错，vercel上部署就没有问题。在`workflow/hugo.yml`文件中加入以下代码解决：

```
# line 37
# Fix for https://github.com/gohugoio/hugo/issues/9810
      - name: Git config
        run: git config --global core.quotepath false
```

删掉了根目录仓库`layouts/partials/extended_head.html`文件的修改，每次都报错……

调整lastmod日期显示为精确到时分，查了很久发现主题作者是在语言文件里改的，在fork的主题仓库`i18n/zh.yaml`中修改了：

```
# line 2
translation: 最后修改于 {{ .Format "2006-01-02 15:04" }}
```

调整lastmod日期显示后，发现了以前一直隐藏的bug：github pages上的日期停留在一个非常奇怪的时间，并不是我实际修改的时间，另外使用`workflow/hugo.yml`文件部署后，似乎因为不是直接调用public文件夹，导致所有文章的最后修改日期都会随着每次部署变化为部署的日期……于是改用自己写的`workflow/main.yml`文件部署，结果workflow没有任何报错，但实际并没有成功部署到网址上去……后来参考了[plundration的文件写法](https://github.com/plundration/plundration.github.io/blob/hugo/.github/workflows/deploy.yaml)，先部署到仓库的另一个分支，最后由github-pages bot按该分支自动部署，终于解决了！实际上我前面能够顺利发现hugo与git的兼容性问题，也是因为plundration在hugo社区的提问。

这告诉我：看不懂的代码，抄就要抄全套啊！

我这个题目真的一点都没有取错！就是折腾！但真的实现了自动更新最后修改时间，很有成就感！

## 2023.12.11

感觉移动端看起来字又小又密，调整主题仓库`assets/scss/journal.scss`文件：

```
# line 47
$single-column-post-title-size: 24px;
$single-column-post-summary-size: 16px;
$single-column-post-body-size: 18px;
$single-column-post-meta-size: 16px;
```

电脑端看起来行距其实还行，考虑要不要调整行距为1.75倍。
