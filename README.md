



- hexo安装

> $ npm install -g hexo

- 初始化项目（不提交到 github）

> $ cd /f/Workspaces/hexo/
>
> $ hexo init # 初始化
>
> $ hexo g # 生成 
>
> $ hexo s # 启动服务
>
> ----
>
> $ # init有问题可以使用以下指令代替 hexo init
>
> $ git clone https://github.com/hexojs/hexo-starter.git
>
> $ cd hexo-starter
>
> $ npm install 

- 修改模版: 
  - 官方模版主题：[hexo](https://hexo.io/themes/)

> $ git clone https://github.com/pinggod/hexo-theme-jekyll.git themes/jekyll
>
> $  npm install --save hexo-renderer-jade hexo-generator-feed # 参考模版github说明
>
> $ # 修改 _config.yml 中的 theme 配置为 jekyll
>
> $ hexo g # 重新生产，如果有异常，可以用 hexo clean 清理

- 数学引擎1：

  - 修改markdown渲染引擎

  > npm uninstall hexo-renderer-marked --save
  > npm install hexo-renderer-kramed --save
  - 修改渲染引擎模块

  ```js
    // node_modules\kramed\lib\rules\inline.js 
    //第11行的escape变量的值修改为如下：
    //escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
    escape: /^\\([`*\[\]()#$+\-.!_>])/,
        
    // 第20行的em变量也要做相应的修改:
    //em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
    em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  ```

  - 主题目录下_config.yml添加如下配置:

  ```yaml
  # MathJax Support
  mathjax:
    enable: true
    per_page: true
  ```

  - 在需要的文章下面添加开关：

  ```markdown
  ---
  title: index.html
  date: 2016-12-28 21:01:30
  tags:
  mathjax: true
  --
  ```

- 数学引擎2：

  - 添加插件：

  > npm install hexo-math --save

