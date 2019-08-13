---
title: Hello World
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

### 亲测步骤
> 创建远程git
> ! new repo:     名称方式 githubName.github.io - 全局只能有一个

> 本地git
> 我有两个github  
```bash
  当前项目中:
   git remote add origin https://lipeijuan423@github.com/lipeijuan423/lipeijuan423.github.io.git
   存在则: git  remote remove origin
```
> 配置
> _config.yml
  ```bash
    deploy:
    type: git
    repository: https://lipeijuan423@github.com/lipeijuan423/lipeijuan423.github.io.git // ? 我的需要这样
    branch: master
  ```
> hexo init
> hexo install
> git 上传代码

$ hexo g #完整命令为hexo generate,用于生成静态文件
$ hexo s #完整命令为hexo server,用于启动服务器，主要用来本地预览
$ hexo d #完整命令为hexo deploy,用于将本地文件发布到github上
$ hexo n #完整命令为hexo new,用于新建一篇文章