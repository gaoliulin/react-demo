# react-demo

搭建react 脚手架

### 1.`npm init ` 初始化项目， 项目目录如下
   
```
react-demo
├─ LICENSE
├─ package.json
└─ README.md

```
  新增并配置 .gitignore 文件
  
```
/node_modules
/dist
/.git
```



### 2. 安装webpack 相关包

- 2.1
```js
// webpack 构建
npm install webpack webpack-cli webpack-dev-server -D
```

- 2.2
```js
// 生成，清除 html 文件
npm install html-webpack-plugin clean-webpack-plugin -D
```
新增 public 文件下的配置
index.html文化

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>webpack+react 脚手架</title>
</head>
<body>
    <h1>webpack+react 脚手架</h1>
    
</body>
</html>
```

- 2.3
```js
// 区分环境 合并
npm install webpack-merge -D
```
新增 config 文件下的配置

在配置 Webpack 的时候少不了会与 文件路径打交道，避免路径零乱，单独创建一个路径调用的配置文件，也是参考 CRA 的

**paths.js**



```js
const path = require("path");
const fs = require("fs");
// 获取当前工作目录
const appDirectory = fs.realpathSync(process.cwd());
// 从相对路径中解析绝对路径
const resolveApp = (relativePath) => path.resolve(appDirectory, relativePath);
// 默认的模块扩展名
const moduleFileExtensions = ["js", "jsx", "ts", "tsx", "json"];
// 解析模块路径
const resolveModule = (resolveFn, filePath) => {
    // 查看文件存不存在
    const extension = moduleFileExtensions.find((extension) =>
        fs.existsSync(resolveFn(`${filePath}.${extension}`))
    );
    if (extension) {
        return resolveFn(`${filePath}.${extension}`);
    }
    return resolveFn(`${filePath}.js`); // 如果没有默认就是js
};

module.exports = {
    appBuild: resolveApp("build"), // 打包路径
    appPublic: resolveApp("public"), // 静态资源路径
    appHtml: resolveApp("public/index.html"), // html 模板路径
    appIndexJs: resolveModule(resolveApp, "src/index"), // 打包入口路径
    appNodeModules: resolveApp("node_modules"), // node_modules 路径
    appSrc: resolveApp("src"), // 主文件入口路径
    moduleFileExtensions, // 模块扩展名
};

```


项目目录如下

```
react-demo
├─ .gitignore
├─ config
│  ├─ paths.js
|  ├─ webpack.common.js
│  ├─ webpack.dev.js
│  └─ webpack.prod.js
├─ LICENSE
├─ package-lock.json
├─ package.json
├─ public
│  ├─ index.html
│  └─ react-logo.png
├─ README.md
└─ src

```

### 3.