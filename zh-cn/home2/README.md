# 快速开始

推荐全局安装 `docsify-cli` 工具，可以方便地创建及在本地预览生成的文档。

```bash
npm i docsify-cli -g
```

## 初始化项目

如果想在项目的 `./docs` 目录里写文档，直接通过 `init` 初始化项目。

```bash
docsify init ./docs
```

## 开始写文档

初始化成功后，可以看到 `./docs` 目录下创建的几个文件

- `index.html` 入口文件
- `README.md` 会做为主页内容渲染
- `.nojekyll` 用于阻止 GitHub Pages 忽略掉下划线开头的文件

直接编辑 `docs/README.md` 就能更新文档内容，当然也可以[添加更多页面](https://docsify.js.org/#/zh-cn/more-pages)。

## 本地预览

通过运行 `docsify serve` 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 [http://localhost:3000](http://localhost:3000/) 。

```bash
docsify serve docs
```

更多命令行工具用法，参考 [docsify-cli 文档](https://github.com/docsifyjs/docsify-cli)。



## CI/CD

### 部署 GitLab-Runner

How to setup a specific Runner for a new project

- Install a Runner compatible with GitLab CI (checkout the GitLab Runner section for information on how to install it).
- Specify the following URL during the Runner setup: http://172.20.3.29/ci
- Use the following registration token during setup: XXXXXXXX
- Start the Runner!

### 常用命令

- register
- start
- stop

## Docker

docker run --name docsify-windows  --volume=E:\GitLab-Runner\builds\9644acda\0\dev2\CGMdoc:/docs --workdir=/docs -p 4000:3000 --restart=no --label='description=A demo Dockerfile for build Docsify.' --runtime=runc -d docsify/docs:latest

### 提交自动升级

提交dev,合并至master。服务器自动拉取md文档。由此实现自动更新。
