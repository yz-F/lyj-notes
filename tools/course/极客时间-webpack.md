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
