---
title: Hexo Next的界面优化
date: 2021-11-29
updated: 2022-04-13
categories:
- Hexo
tags:
- Hexo
---

<escape><!--more--></escape>

### 如何将页面背景改为黑色模式？

修改 Next 主题下的 _config.yml 配置文件：将 darkmode 的值修改为 true 。

如果主界面已经设置了 hexo-tag-cloud 标签云的展示，那么修改博客站点的配置文件，

_config.yml 中标签云模块下的 textColor 更改为 '#FFFFFF' # 白色，或者其他颜色即可。

----

### 如何为页面添加当前浏览进度？

修改 Next 主题下的 _config.yml 配置文件：

将 back2top 中的 enable 和 scrollpercent 值修改为 true 。

---

### 如何将页面的首页更改为归档？

修改 Next 主题下的 _config.yml 配置文件：

将 menu 下的 home 路径修改为 archive 即可，同时注释掉 archive。

```javascript
menu:
  home: /archives/ || fa fa-archive
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  #archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

*（提醒：如果担心更改错误，备份原文件）*

---

### 如何为页面添加加载特效？

参考文章：<a>[Hexo博客NexT主题美化之顶部加载进度条 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903789946896398)</a>

---

### 如何修改主题的段距？

回答在官方仓库的 Issue，链接：https://github.com/theme-next/hexo-theme-next/issues/1703

----

### 如何美化分类页面？

参考这两篇博客：

[Hexo+NexT博客归档/标签/分类页美化 | CodeHeap (gitee.io)](https://jrbcode.gitee.io/posts/be9758cd.html)

[Hexo-NexT 分类多层级描述 - 锦瑟，无端 - 博客园 (cnblogs.com)](https://www.cnblogs.com/cscshi/p/15196122.html)

前者是添加一些 CSS 样式以及美化，而后者是增强分类的观感。

---

### 如何把文章间的距离缩小？

参考 Issue：https://github.com/iissnan/hexo-theme-next/issues/591

----

### 如何修改文章链接样式？

修改 `themes\next\source\css\_common\components\post.styl` 文件，添加如下 CSS 样式：

```css
// 文章内链接文本样式
.post-body p a{
  color: #8CC7B5;
  border-bottom: none;
  border-bottom: 1px solid #8CC7B5;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```

`color` 部分可以根据个人喜好更改。
