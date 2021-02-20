---
layout: post
title:  评论区上线了
author: maiming
date:   2020-04-29
categories: thesite
tags: 评论区
description: 都来说说话吧！
enable_comments: true
---

在鸽了几天之后，本站的评论区上线了。之前所有文章也都开启了评论功能。

拖延的原因是因为纠结于选择技术方案。我原定是在[Isso](https://posativ.org/isso/)或者[Staticman](https://staticman.net/)之中选择，这两种方式都是我能够完全掌控的，不依赖于第三方。试着配置之后，我发现需要的时间超过了我的预期，就放弃了。现在使用的方案是[Valine](https://valine.js.org/)，依赖于[LeanCloud](https://www.leancloud.cn/)的服务。

评论区支持Markdown，你可以参考这个简明的[手册](https://github.com/gnipbao/markdown-handbook)。

开启评论区就意味着本站**能够**搜集的信息更多了，这对你的隐私没有好处。我之前[承诺]({% post_url 2020-04-26-huanying-tougao %})过

> 如果你是读者，本站不会使用任何方式收集你的个人信息。

这句话需要进一步地说明。你在评论时，可以选择输入你的昵称、邮箱和网站地址，但是**你也可以不输入**。如果不输入昵称，你的评论将会显示为匿名。无论什么情况，你使用的操作系统和浏览器都会被记录，并公开发布。你的 IP 地址不会被记录，但是我无法保证LeanCloud，或者是网络中其他任何一方不会记录。本站也不会利用Cookies来追踪你。如果你不想泄露以上信息，最好的办法是不评论，甚至直接禁用Cookies和JS。

评论数据现在储存在LeanCloud上，我会不定时地将其上传到GitHub仓库中，以满足开源的承诺。

最后提醒，「评论千万条，友善第一条」。在贴吧我几乎不删帖，但我也会偶尔移除过分的评论，在这里也是一样。

祝评论愉快。