## package.json

    每个项目的根目录下面，一般都有一个package.json文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。

## package.json生成
    npm init

>一个简单的package.json  

    {
  "name": "webpack_demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.5.2"
  }
}
## npm install

    使用 npm install 命令就可以根据这个配置文件，自动下载所需的模块，默认是dependencies 和 devDependencies 中的模块都会下载。
    如果一个模块不在package.json文件之中，可以单独安装这个模块，并使用相应的参数，将其写入package.json文件之中。
    $ npm install express --save
    $ npm install express --save-dev

## package.json字段
- scripts字段<br>

    "scripts": {
        "preinstall": "echo here it comes!",
        "postinstall": "echo there it goes!",
        "start": "node index.js",
        "test": "tap test/*.js"
    }
>上面指定了npm run preinstall、npm run postinstall、npm run start、npm run test时，所要执行的命令。<br>
>scripts指定了运行脚本命令的npm命令行缩写，比如start指定了运行npm run start时，所要执行的命令。
>[npm-scripts](https://docs.npmjs.com/misc/scripts)
## dependencies字段，devDependencies字段
>dependencies字段指定了项目运行所依赖的模块(代码用的依赖)，devDependencies指定项目开发所需要的模块。（打包所需的依赖）<br>
>它们都指向一个对象。该对象的各个成员，分别由模块名和对应的版本要求组成，表示依赖的模块及其版本范围。<br>
    {
    "devDependencies": {
        "browserify": "~13.0.0",
        "karma-browserify": "~5.0.1"
    }
    }
>对应的版本可以加上各种限定，主要有以下几种<br>
- 指定版本：比如1.2.2，遵循“大版本.次要版本.小版本”的格式规定，安装时只安装指定版本。
- 波浪号（tilde）+指定版本：比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2），但是不安装1.3.x，也就是说安装时不改变大版本号和次要版本号。
- 插入号（caret）+指定版本：比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，也就是说安装时不改变大版本号。需要注意的是，如果大版本号为0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。
- latest：安装最新版本

## main字段
    main字段指定了加载的入口文件，require('moduleName')就会加载这个文件。这个字段的默认值是模块根目录下面的index.js。

> **[CLI文档> 配置npm](https://docs.npmjs.com/files/package.json)**    
> **[阮一峰package.json文件](http://javascript.ruanyifeng.com/nodejs/packagejson.html)** 