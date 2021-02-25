



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

- 