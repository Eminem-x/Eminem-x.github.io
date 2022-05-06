1. #### Git + 配置环境变量

   1. 安装 GIt 在 D 盘
   2. 配置环境变量：`D:\git\Git\cmd`
   3. 运行即可

2. #### Node.js + npm + cnpm

   1. `node -v` 和 `npm -v`

   2. `npm install -g cnpm --registry=https://registry.npm.taobao.org`

   3. 配置环境变量：
       ```
      D:\nodejs\
      C:\Users\DELL\AppData\Roaming\npm

   4. `cnpm -v`

3. #### Hexo Cli + Github + Next

   1. `cnpm install -g hexo-cli` 和 `hexo -v`

   2. `sudo hexo init` 就可以 `hexo s`

   3. GitHub 上新建仓库：`****.github.io`

   4. `cnpm install --save hexo-deployer-git`

   5. 修改 `_config.yml`：

      ```
      deploy:
      	type: 'git'
      	repo: https://github.com/***/***.github.io.git
      	branch: master
      ```

   6. `hexo d` 中间需要绑定 `Git` 和 `GitHub`，稍后即可查看部署结果

   7. 下载主题：

   8. 修改主题：

