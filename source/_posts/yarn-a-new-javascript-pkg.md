---
title: Yarn - Javascript 新一代套件管理
tags: yarn
categories: yarn
abbrlink: 61460
date: 2016-10-14 23:25:51
---
日前，Facebook 发布了全新的 JS 套件管理工具 [Yarn](https://github.com/yarnpkg/yarn)，这是一个新的快速安全可信赖的可以替代NPM的依赖管理工具。
在取代npm客户端和其他包管理器现有工作流的同时，又保留了对npm代理的兼容性。它拥有与现有的工作流相同的特性，只是操作起来更快、更安全、更可靠。
<!--more-->

## Yarn 提供一个更快更稳定的套件管理方案
1. 透过 yarn.lock ，锁住套件版本，因此可以确保安装之套件在每台机器上都能保持一致。
2. 安装过的套件，都会加入到 global cache 中，下次有砍掉要重装，或是不同资料夹要装，都可以在无网络情況底下安裝。
3. 非常快，平行化处理每个 operation，全新的 resolving 演算法。

## 特性功能
除了让安装过程更快更可靠，Yarn 还有额外的特性来更好地简化依赖管理的工作流。

- 兼容 npm 和 bower 工作流，并且支持混合注册。
- 能够限制已安装模块的证书以及输出证书信息。
- 暴露一个稳定公开的JS API，通过构建工具提供抽象的日志记录。
- 可读、最小化、良好的命令行输出。

## 使用
```
// 以前装过 npm 再安装 yarn
npm install -g yarn

// 直接安装 (mac为例，其余官网有介绍)
curl -o- -L https://yarnpkg.com/install.sh | bash

// 一般安装 (等同 npm install)
yarn

// 安装特定套件 (等同 npm install --save)
yarn add react         
yarn add react@15.3.2

// 更新特定套件 (等同 npm upgrade)
yarn upgrade react

// 移除特定套件 (等同 npm uninstall)
yarn remove react

// 新增 package.json
yarn init

// 新增全域套件
yarn global add

// 跑 script
yarn run

// 其他常用选项
--offline   (离线模式，只拉 cache)
--flat      (将套件扁平化，一個资料夹只会有一個套件)
--dev       (加入到 devDependencies)
--peer      (加入到 peerDependencies)
--optional  (加入到 optionalDependencies)
```

## Cheat
<table><thead><tr><th>NPM</th><th>YARN</th><th>说明</th></tr></thead><tbody><tr><td>npm init</td><td>yarn init</td><td>初始化某个项目</td></tr><tr><td>npm install/link</td><td>yarn install/link</td><td>默认的安装依赖操作</td></tr><tr><td>npm install taco —save</td><td>yarn add taco</td><td>安装某个依赖，并且默认保存到package.</td></tr><tr><td>npm uninstall taco —save</td><td>yarn remove taco</td><td>移除某个依赖项目</td></tr><tr><td>npm install taco —save-dev</td><td>yarn add taco —dev</td><td>安装某个开发时依赖项目</td></tr><tr><td>npm update taco —save</td><td>yarn upgrade taco</td><td>更新某个依赖项目</td></tr><tr><td>npm install taco --global</td><td>yarn global add taco</td><td>安装某个全局依赖项目</td></tr><tr><td>npm publish/login/logout</td><td>yarn publish/login/logout</td><td>发布/登录/登出，一系列NPM Registry操作</td></tr><tr><td>npm run/test</td><td>yarn run/test</td><td>运行某个命令</td></tr></tbody></table>

## 参考
- [Yarn: A new package manager for JavaScript](https://code.facebook.com/posts/1840075619545360/yarn-a-new-package-manager-for-javascript/)
- [Yarn: a new program for installing JavaScript dependencies](https://blog.getexponent.com/yarn-a-new-program-for-installing-javascript-dependencies-44961956e728#.qf8fmeg4g)
- [npm-vs-yarn-cheat-sheet](https://shift.infinite.red/npm-vs-yarn-cheat-sheet-8755b092e5cc#.dcd5qeolm)
