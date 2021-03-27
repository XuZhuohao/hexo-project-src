---
title: github workflows
categories: other
date: 2021-03-27 15:39:27
tags:
mathjax: false
---

#  workflows



## 0.参考文档

1. [工作流程语法](https://docs.github.com/cn/actions/reference/workflow-syntax-for-github-actions)
2. [创建个人访问令牌](https://docs.github.com/cn/github/authenticating-to-github/creating-a-personal-access-token)
3. [官方 action 仓库](https://github.com/marketplace?type=actions)
4. [awesome-actions 仓库](https://github.com/sdras/awesome-actions)

## 1.base

### 1.1 创建 workflows 流程

**创建 workflows 文件有两种方式：**

**第一种是在仓库./.github/workflows/ 目录下创建 yaml 文件，**

**第二种方式是在github action 中创建，并用在线编辑器编写，一下使用这种方式演示**

<!--more-->>>

![create_action](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/create_actions.png)



第一步先 选中 actions 选项卡，然后根据自己需求创建 yml 脚本

- 选中 `set up a workflow yourself`  创建普通 yml 模板文件
- 或者选中 github 感觉项目代码推荐的模板

----

![create_actions_01](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/create_actions_01.png)

第二步使用在线编辑器编辑，编辑器页面主要使用有：

- 上方填写文件名（生成后为 ./github/workflows/name.yml 文件）
- 主要区域是编辑器
- 右方式官方 action 市场，和简要文档

写完后通过右上角 `start commit` 按钮提交

----

![action_job](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/action_job.png)

创建完文件后，`Action` 选项卡会变成如上图界面，左边区域为所有 workflows，点击对应的 workflows(这里点中 hero-deploy 任务)，右边展示对应 workflows 的执行记录和过滤器，点击对应记录可以看到各个执行记录

----

![action_job](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/action_job_log_1.png)

显示 hero-deploy 里的所有任务完成情况（这里只有一个任务）

----



![action_job](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/action_job_log_2.png)

查看详细每个任务里面的每一个`step`的日志

----

### 1.2 示例：auto build&deploy hero blog

#### 1.2.1 说明

1. [对应 github 仓库点击查看](https://github.com/XuZhuohao/hexo-project-src)
2. 参考章节 1.1
3. `workflows` 运行于仓库 `hexo-project-src` ，运行产出物推送到仓库 `xuzhuohao.github.io`
4. 流程场景说明：
   - 在使用 hero 根据 markdown 生成 blog 代码，并在 github 发布，作用域名访问时，常需要如下操作：
     1. 安装 npm，git，hexo-cli，clone code，npm install ( *第一次在电脑上使用*)
     2. 把新增的 markdown 放到 `hexo-project-src` 项目下 (*每次都要*)
     3. 使用 hexo 生成静态文件(hexo clean && hexo g) (*每次都要*)
     4. 把静态文件推送到仓库 `xuzhuohao.github.io`
     5. 把更新的 markdown 提交到仓库 `hexo-project-src`
   - 该过程过于繁琐和复制
   - 使用 `workflows` 后操作如下：
     1. 提交新增 markdown 到仓库 `hexo-project-src` (通过git，或者直接 github web 提交) 
     2. 自动化构建和发布(不需要操作)
   - 使用 `workflows`，让自己偷个懒吧

----



#### 1.2.2 流程演示

##### **step1:** 生成并配置 `secrets.ACCESS_TOKEN` 供 step2 使用

1. 在账号设置中获取 access_token (settings -> Developer settings -> Personal access tokens)

   - settings

   ![example_setting_01](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_01.png)

   

   - Developer settings

   ![example_setting_02](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_02.png)

   

   -  Personal access tokens (cenerate)

   ![example_setting_03](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_03.png)

   -  填写token名称和根据需求勾选对应权限，拉到最下面点击 cenerate 创建 token

   ![example_setting_4](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_04.png)

   -  生成token（需要负责处理自己保存，离开页面后再次进入，只显示之前取得名称了）

   ![example_setting_05](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_05.png)

2. 打开仓库 `hexo-project-src`，配置 `ACCESS_TOKEN`

   - 点击 Settings 标签页，点击 Secrets 选项，点击 New repository secret 按钮

   ![example_setting_06](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_06.png)

   - 参数命名并填写对应的值（在`workflows`中的使用方法为 `${{secrets,yourName}}` ）

   ![example_setting_07](https://raw.githubusercontent.com/XuZhuohao/picture/master/github/workflows/example_setting_07.png)

   

---



##### **step2:** 根据 `1.1` 在仓库 `hexo-project-src` 创建并提交 `workflows文件`如下：

文件 .github/workflows/hero-deploy.yml : [github address](https://github.com/XuZhuohao/hexo-project-src/blob/master/.github/workflows/hero-deploy.yml)

```yaml
# workflows name
name: hero-deploy

# 触发条件：master push
on:
  push:
    branches: [ master ]
    
# 任务    
jobs:
  # 任务1：deploy(job_id)
  deploy:
    # deploy 任务的名称(说明)
    name: deploy hero project
    # 运行环境(GitHub 托管的运行器)
    runs-on: ubuntu-latest
    # 步骤
    steps:
      # 步骤1：每一步可以使用不同的action或者run不同的指令
      - name: checkout code
        uses: actions/checkout@master
      
      - name: install node
        uses: actions/setup-node@v1
        with:
          node-version: 12.x
      
      - name: cache node modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
      
      - name: install hero
        run: |
          npm install hexo-cli gulp -g
          npm install
          
      - name: generate file
        run: |
          hexo clean
          hexo generate
       
#       - name: Execute gulp task
#         run: gulp   
        
      - name: deploy hexo
        env: 
          # Github 仓库
          GITHUB_REPO: github.com/XuZhuohao/xuzhuohao.github.io
        # 推送
        run: |
          cd ./public && git init && git add
          git config user.name "yui_htt"
          git config user.email "786725551@qq.com"
          git add *
          git commit -m "GitHub Actions Auto Builder at $(date +'%Y-%m-%d %H:%M:%S')"
          git push --force --quiet "https://${{ secrets.ACCESS_TOKEN }}@$GITHUB_REPO" master:master
```

该脚本大意如下：

- 创建一个 `workflows`，名字为 hero-deploy， 触发条件为 `master` 分支接受到 `push` 请求
- 该 `workflows` 包含一个 `job`， `job` 步骤如下：
  1. 代码 checkout
  2. 使用 node.js 镜像
  3. 使用缓存
  4. npm instal 项目以及项目操作所需要的 hexo-cli 和 gulp 程序
  5. 执行 hexo 指令，生成部署文件 `public`  文件夹
  6. 将生成的 `public` 文件夹推送到仓库 `xuzhuohao.github.io`（**注意这里是另一个仓库了**）
  7. 注意最后推送记录的 `secrets.ACCESS_TOKEN` 在 step1 已经进行创建







### 1.3 基础语法

```yaml
name: the-workflows-names

on: 
  push: 
    targs:
      - "*"
      
jobs:
   job1:
     name: job1-name
     steps:
       - name: first step
         run: echo hello word
         
       - name : second step
         uses: actions/checkout@v2
         with:
           ref: ${{github.ref}}
```



**未完待续**



