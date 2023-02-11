# 创建React新项目

- 操作系统：macOS Ventura 13.2
- 更新时间：2023.2.11

## 运行时：Node

前往[Node.js官网](https://nodejs.org/zh-cn/)下载安装包并安装

- Node.js v18.14.0 to `/usr/local/bin/node`

- npm v9.3.1 to `/usr/local/bin/npm`

设置npm国内代理，这里选择[腾讯云](https://mirrors.cloud.tencent.com/help/npm.html)

## 包管理器：Yarn

node包管理器，这里使用Yarn。常用的还有npm（老派）、pnpm（新兴）

```bash
# 安装yarn最新版本 (1.22.19)
sudo npm i -g yarn
```

## 构建工具：Vite

这里使用[现代前端构建工具vite](https://cn.vitejs.dev/)，代替古早的`打包工具Webpack`和`项目启动工具create-react-app`

```bash
yarn create vite
# 按照提示创建项目
```

## 语法检查：ESLint

大家都使用的ESLint，配置选用`create-react-app`的标准配置

```bash
# 添加到项目开发依赖
yarn add --dev eslint-config-react-app eslint@^8.0.0
```

创建一个文件 `.eslintrc.json`

```bash
{
  "extends": "react-app"
}
```

如果使用webstrom可以在`Settings-Languages&Frameworks-JavaScript-Code Quality Tools-ESLint`选择`Automatic ESLint configuration`和`Run eslint -- fix on save`

## 启动项目

```bash
# 安装依赖
yarn
# 启动项目
yarn dev
```

