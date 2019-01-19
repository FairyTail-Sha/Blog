---
title: Hexo overview
date: 2019-01-11 12:40:29
tags: hexo blog nodejs
---

# Hexo 使用小手册

## 基本命令

新建文章 `hexo  new <文章名>`  
发布文章 `hexo generate`

什么是 Hexo？  
**Hexo** 是一个快速、简洁且高效的博客框架。

**scaffolds**  
模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

**source**  
资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _(下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

**themes**  
主题 文件夹。Hexo 会根据主题来生成静态页面。