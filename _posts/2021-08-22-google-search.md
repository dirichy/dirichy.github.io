---
layout: post
title: Google Search
subtitle: Google Search
date: 2021-6-12 22:31:54
author: gankai
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
  - Google Search
  - Google Search
---


> 谷歌是搜索引擎行业的主导力量，它是 Android 智能手机和 Chrome 等网络浏览器的默认搜索引擎。如果您目前对 Google 的使用仅限于输入几个词并更改您的查询，直到找到您要查找的内容，那么我在这里告诉您有一种更好的方法——而且它并不难学。

### 1. 使用`site` :  `site:github.com geekskai`

使用 `site:`将在特定网站内进行搜索。例如，如果您想查找仅在 [github.com](http://github.com/) 上发表的有关geekskai的文章，您只需输入 `site:github.com geekskai`。

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/29650b10-ca49-424d-9e31-da2b23f63207/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210821%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210821T160753Z&X-Amz-Expires=86400&X-Amz-Signature=3593c91a2d34a2e81fe2c451c197a0e6f77484fa34254a898da8b786ca8e0920&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### 2. 可以使用 `*` 来填充缺少的单词或短语。

例如：`How to integrate * in react`


![Untitled.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efe114e6228b41a685330a8b14614a19~tplv-k3u1fbpfcp-watermark.image)

### 3.使用引号`""`精确匹配,只查找完全匹配的搜索结果

通常 Google 搜索的关键词可以是词组（中间没有空格），也可以是句子，但是**用句子做关键词，就必须加英文引号**，否则会被拆分成几个词组进行检索。用于搜索单词或短语。

例如，当我们的代码出现报错时，会有报错日志，`You may need an additional loader to handle the result of these loaders.` 要搜索的时候，使用英文的引号包裹着这段代码进行搜索！


![Untitled-2.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f72e12f23e534e8883aba64e74ce5ee9~tplv-k3u1fbpfcp-watermark.image)

### 4.使用`related` 查找类似相关的网站

查找与github.com类似的网站`related:github.com`

> 我们应该见过related：当我们使用搜索引擎搜出一个结果的时候，在搜索结果页面底部的还有额外的八个搜索结果。这些就是与你[搜索结果相关](https://www.siteguru.co/seo-academy/related-searches)的！


![Untitled.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8d852ba9f08d44169205be4c0839ab69~tplv-k3u1fbpfcp-watermark.image)

### **5.**  使用 `link:` 查找链接到另一个页面的页面，例如：`link:geekskai.com`


![Untitled-2.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b6a8058a5e834015a81315cc70439a82~tplv-k3u1fbpfcp-watermark.image)

### 6.使用 `-` 操作符排除关键词

如果你不想搜索到某个内容，可以在搜索关键词后面使用 - 操作符对指定内容进行排除。

例如：我不想搜索关键词:`geekskai` 在知乎上的相关文章 我可以这么做`"geekskai"-知乎` 来排除知乎上的搜索结果！(其实还可以搜索：`web前端 -广告`,搜索的就是排除广告)


![Untitled-3.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/217b06738b464fc79f5902d13027cd89~tplv-k3u1fbpfcp-watermark.image)

### 7.使用`..`搜索数字范围

例如：`vuejs 2019..2021` 搜索vuejs最近3年的相关信息


![Untitled.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/784117e637a44abb9869136e571d5e69~tplv-k3u1fbpfcp-watermark.image)

### 8.`OR`

例如： `react OR vue` 搜索包含 vue 或者 react，或者两者均有的中英文网站。


![Untitled-2.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4963fcbbadb44c791bc34a73eb3f8e5~tplv-k3u1fbpfcp-watermark.image)

### 9.`AMD`

例如：`react AND redux` 搜索react和redux两个关键词的搜索结果。


![Untitled-3.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4c26399f66814a389d7b19f6ba26784b~tplv-k3u1fbpfcp-watermark.image)

### 10.针对文件类型搜索

例如：如果你只想查找 PDF 文件、Word 文档、或 EXCEL 表格，就可以使用 filetype: 操作符，支持的格式还有 ps 、dwf 、kml/kmz、ppt、rtf、swf 等。

`filetype:PDF you don't know JavaScript`


![Untitled-4.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c3cbf712f8c422596209c9a795cca21~tplv-k3u1fbpfcp-watermark.image)

### 11.同义词搜索`～`

有时，搜索不太具体的术语很有用。如果您不确定将使用哪个术语，您可以使用同义词搜索。


![Untitled-5.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/694ef738cff241fca9f320d4a0076923~tplv-k3u1fbpfcp-watermark.image)

### 12. 查找您的 IP 地址:

如果您不知道自己的 IP 地址是什么，请搜索`my ip address`，Google 会显示您的公共 IP 地址。


![Untitled-6.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/95ab6df642b54bbe8b11b6e989b078d8~tplv-k3u1fbpfcp-watermark.image)

### 13. 找到你的安卓手机

![Untitled-7.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e9d01d96b8b54d179546fb5a03f701f7~tplv-k3u1fbpfcp-watermark.image)

### 14.使用谷歌的高级搜索页面

如果您不想记住所有搜索运算符，您还有另一种选择。为 [Google 的高级搜索页面](https://www.google.com/advanced_search)添加书签，并使用它来缩小搜索结果的范围。您可以指定语言、地区、更新时间、文件类型等以优化搜索查询。


![Untitled-8.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fafd0bed93b24dc5801fa15d6c7e3140~tplv-k3u1fbpfcp-watermark.image)

### 15. 国际搜索

```
  通常，当您搜索 Google 时，结果会[根据您的 IP 地址根据](<https://support.google.com/websearch/answer/1696588>)Google 认为您所在的国家/地区进行定制。例如，如果您在印度，系统会将您定向到 Google.co.in 而不是 Google.com。但是，如果您想获得其他国家/地区的结果，您可以通过一些技巧来实现。

  要使用 Google.com 而不是您的本地版本，请访问[google.com/ncr](<http://www.google.com/ncr>)并将其加入书签以备将来使用。NCR 代表无国家/地区重定向。

   或者，如果您从 Google.com 重定向到另一个 Google 网站（例如 Google.co.in），请点击页面右下角的“使用 Google.com”链接以获取国际版 Google。

  根据您所在的位置，您可能还会看到用英语搜索本地 Google 的选项，这在您前往英语不是主要语言的地方时非常方便。
```


![Untitled-9.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a4a941af97e84a7e8a9170af6d8f1837~tplv-k3u1fbpfcp-watermark.image)

## 最后，**更多用于缩小搜索结果的 Google 搜索运算符**

下面是一些最有用的 Google 搜索运算符的小备忘单，包括我们已经提到的那些：





![Xnip2021-08-22_00-36-11.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c89a253a4100456abffaf281dd33fb97~tplv-k3u1fbpfcp-watermark.image)




![Xnip2021-08-22_00-36-57.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/10d142d6e5f249cabe80beb8e169052f~tplv-k3u1fbpfcp-watermark.image)



❤️ 看完三件事:

<p>如果你觉得这篇内容对你挺有启发，我想邀请你帮我个小忙：</p>
<ol data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">点赞，让更多的人也能看到这篇内容，也方便自己随时找到这篇内容（收藏不点赞，都是耍流氓 -_-）；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">关注我们，不定期分好文章；</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">也看看其它文章；</section></li></ol>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">🎉欢迎你把自己的学习体会写在留言区，与我和其他同学一起讨论。如果你觉得有所收获，也欢迎把文章分享给你的朋友。</p>



