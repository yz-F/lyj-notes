### 为什么使用构建工具

- 网站 CAN I USE

### webpack
- item2

```
PWD

LS

CODE .

TAB 提示

rm -rf dist/

/node_modules/.bin/webpack

command + ~ :   切换VSCODE的窗口
```

- script中配置打包命令

- entry 单页面和多页面
```
单入口 和 多入口
```

### 第二章
- 插件：文件的优化，资源管理，环境变量注入，作用于整个构建过程
- mode指定当前构建环境，开启内置的参数配置（production和development）
- mode设置development,有默认值process.env.NODE.ENV的值为development,开启NameChunksPlugin 和 NamedModulesPlugin

### babel
-  npm i @babel/core @babel/preset-env babel-loader -D
- babel 分为两部分 preset(plugin的集合) 和 plugin

### jsx
```

```

### css

- 链式调用，从右到左，从css-loader到STYLE-LOADER
- CSS-LOADER 用于解析文件，生成在COMMOM，style-loader生成在<STYLE>
标签

- less

### image
- file-loader
- url-loader: 也可以打包图片和字体，并且设置限制大小，自动BASE64

### watch
- 轮询判断文件的最后的编辑时间是否更新

### 热更新 webpack-dev-serve
- 不刷新浏览器，不输出文件。放在内存中，使用H
- webpack-dev-middleware

### 文件指纹
- 三种指纹
- css文件指纹

### MiniCssExtractPlugin

###

> vue文档

### publicPath配置
```
output: {
    // 每次打包文件编辑的文件，可以加上hash
    filename: '[name].js',
    // 配置以何种方式导出库
    libraryTarget: 'commonjs2',
    path: path.join(__dirname, '../dist/electron')
},
```

- 需要把构建出的资源文件上传到 CDN 服务上，以利于加快页面的打开速度。配置代码如下
  ```
    filename:'[name]_[chunkhash:8].js'
    publicPath: 'https://cdn.example.com/assets/'
  ```
- 这时发布到线上的 HTML 在引入 JavaScript 文件时就需要：
  ```
    <script src='https://cdn.example.com/assets/a_12345678.js'></script>
  ```
- Webpack 输出的部分代码块可能需要异步加载(类似于jsonp异步加载)
output.crossOriginLoading则是用于配置这个异步插入的标签的crossorigin值
  - anonymous(默认) 在加载此脚本资源时不会带上用户的 Cookies；
  - use-credentials在加载此脚本资源时会带上用户的 Cookies。
-  libraryTarget 和 library配置？(当用 Webpack 去构建一个可以被其他模块导入使用的库时需要用到它们)
  - output.libraryTarget配置以何种方式导出库。
  - output.library配置导出库的名称。

### commonjs配置，模块发布到NPM代码仓库名称

- 编写的库将通过 CommonJS 规范导出
- 假如配置了output.library='LibraryName'，则输出和使用的代码如下
```

// Webpack 输出的代码
exports['LibraryName'] = lib_code;

// 使用库的方法
require('library-name-in-npm')['LibraryName'].doSomething();
```

- 其中library-name-in-npm是指模块发布到 Npm 代码仓库时的名称