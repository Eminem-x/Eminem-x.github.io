---
title: Hexo的迁移
date: 2022-4-28
categories:
- Hexo
tags:
- Hexo
---

<escape><!--more--></escape>

### Preface

因为更换了电脑，从 Windows 迁移到了 MacOs，花费了将近两天的时间，才完整地将博客迁移。

在这个过程中，主要遇到了以下几个问题：

1. 如何将博客的所有资源以及配置迁移到新电脑：通过 `Git` 解决；
2. 图片无法正常显示：通过修改 `node_modules` 下的包文件；
3. 生成日期和修改日期被全部重置：通过添加头描述。

上述问题，最开始我以为是 `OS` 不同导致的问题，后来通过深入，发现并不是其造成的，

而是一些基础的操作，以及对于 Hexo 部署和生成以及一些工具的熟练使用。

也借着这次经历，加深了对于 `Git` 的理解，也明白了 Hexo 的部署方式。

`Git` 操作可以参考资源：http://iissnan.com/progit/

关于 MacOs 的 `Git` 安装按照官网的指引操作即可，配置个人信息和密钥，可以参考其他博客。

### Details

#### 1. 如何迁移博客

因为我的 `Hexo` 博客是部署在 `GitHub` 上的，所以可以采用 `branch` 的方式，去分离资源和渲染。

在理解这个之前，先来大概了解一下 `Hexo` 的工作原理和其相关文件的作用，

`Hexo` 本身是一个静态博客，在本地修改增加博客的时候，并不是将所有东西都部署在远端，

而是在本地渲染资源后，生成 `.deploy_git` 文件部署在 `Github` 上，而后渲染，显示站点内容。

因此资源和渲染结果是分离的，相当于资源在本地，而 `deploy` 的内容在 `GitHub` 上，

因此可以通过 `Git` 的 `branch` 功能将本地的资源也上传到远端，从而可以拉取在新电脑。

![](文件作用.png)

如上图所示，在原有的 `master` 分支上，新建 `source` 分支，用来存储资源文件。

<strong>具体操作流程如下：</strong>

1. 克隆远程仓库的项目：`git clone remote_url`

2. 和远程仓库建立关联：`git remote add origin remote_url`

3. 查看所有分支：`git branch -a` 和 `git branch -r`

4. 本地创建新的分支：`git checkout -b source`

5. 将新分支推送至 `GitHub`：`git push origin source`

6. 查看创建分支的结果：`git branch -r`

7. 在 `GitHub` 上更改博客仓库的默认分支为 `source`

8. 删除原仓库内文件，而后将资源文件复制到本地项目，注意 `.git` 文件

9. 确保 `.gitignore` 文件为以下内容：

   ```
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   ```

10. 执行以下命令：

    ```
    git add .
    git commit -m 'hexo source post'
    git push origin souce
    ```

11. 在新电脑中，参考我的其他相关 `Hexo` 博客配置，然后 `clone` 即可

#### 2. 图片显示问题

在最开始的搭建中，图片并未采用图床或者上传到 `GitHub` ，而是 `![]()` 本地的形式，

所以需要安装 `npm install hexo-asset-image `，可以参考我的其他相关 Hexo 博客配置，

当然并不需要重新安装，因为 `package.json` 文件已经包含了相关的依赖，但是需要改代码，

打开 `/node_modules/hexo-asset-image/index.js`，将内容更换为如下代码：

```js
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
        var link = data.permalink;
    if(version.length > 0 && Number(version[0]) == 3)
       var beginPos = getPosition(link, '/', 1) + 1;
    else
       var beginPos = getPosition(link, '/', 3) + 1;
    // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
    var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
        if ($(this).attr('src')){
            // For windows style path, we replace '\' to '/'.
            var src = $(this).attr('src').replace('\\', '/');
            if(!/http[s]*.*|\/\/.*/.test(src) &&
               !/^\s*\//.test(src)) {
              // For "about" page, the first part of "src" can't be removed.
              // In addition, to support multi-level local directory.
              var linkArray = link.split('/').filter(function(elem){
                return elem != '';
              });
              var srcArray = src.split('/').filter(function(elem){
                return elem != '' && elem != '.';
              });
              if(srcArray.length > 1)
                srcArray.shift();
              src = srcArray.join('/');
              $(this).attr('src', config.root + link + src);
              console.info&&console.info("update link as:-->"+config.root + link + src);
            }
        }else{
            console.info&&console.info("no src attr, skipped...");
            console.info&&console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```

#### 3. 日期修改问题 

因为重新拉取和上传，导致博客的生成日期和修改日期都变成了一天，也就是拉取的这天，

经过查阅资料和不断尝试以及思考，找到了原因：文件的创建和修改日期即为此日期。

![](日期.png)

来到主题文件中 `themes/next/layout/_macro/post.swig`，可以看到如下代码：

![](日期代码.png)

大概意思是，每个 `post` 也就是文章，有上传日期和修改日期，即 `date` 和 `updated`，

如果二者相同，那么只显示生成日期，如果不相同额外显示更改日期。

有了上面两个要素，就明白了如何去修改回原本的日期，但是以前并未在头部添加 `date`，

并且更改文件本身的生成日期是麻烦的，最开始考虑的是写一个脚本，批处理一下所有文件，

在原电脑上为每一个文章额外添加生成和修改日期，因为刚好最近在学习 `shell` 相关内容，

但是发现比较困难，并且因为以前写博客经验较少，很多内容和排版都有问题，

因此借着这个机会，将以前的内容进行精简和必要的重新排版，就采用最原始的方法，

在每个文件的 `YAML` 头部信息中添加 `date` 和 `updated` 内容，也希望以后注意此问题。

