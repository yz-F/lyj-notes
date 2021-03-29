## Wh-coder：


        npm 是包管理器，初始化 npm init 会生成一个 package.json 文件，这个是单个项目的描述文件，项目有哪些运行脚本，有哪些依赖，项目叫什么，版本等等都是写在这个文件。
        下面来说下载包，npm install xxx，这个命令会把依赖文件存到 node_modules 文件夹下，但是这个操作没有留下任何记录，其他人使用项目时，并不知道你用了什么依赖，所以指令后面加上 --save 或者 -S 就是把下载包的名称版本记录在 package.json 的dependencies字段，这样别人拿到项目看 package.json 就知道下载了什么依赖。
        接下来依赖分为两种，一种是代码中用到的，比如 vue，一种是调试打包用到的，比如 webpack，后者加的参数是 --save-dev 或者 -D

        在接下来，这些包都不能在命令行直接调用，他们只能在 npm run xxx 使用，这里的 xxx是 package.json 中scripts配置，如果我们要想在命令行运行包，就必须把包存在全局，npm install -g webpack

        现在的问题是全局 webpack 版本是4,需要降级。所以要先写在全局包 npm uninstall webpack -g，然后重新下载指定版本 npm install webpack@3 -g，下载完成使用 webpack -v 查看版本

## 相关npm常用命令
- [npm 常用命令详解](https://zhuanlan.zhihu.com/p/27449990)
- [npm 安装文档](http://caibaojian.com/npm/) 

### npm是什么
>NPM的全称是Node Package Manager，是随同NodeJS一起安装的包管理和分发工具，它很方便让JavaScript开发者下载、安装、上传以及管理已经安装的包。

### npm install 安装模块
>NPM的全称是Node Package Manager，是随同NodeJS一起安装的包管理和分发工具，它很方便让JavaScript开发者下载、安装、上传以及管理已经安装的包。
### npm install gulp
>安装包，默认会安装最新的版本
>npm install gulp@3.9.1
### npm uninstall 卸载模块 
>如卸载开发版本的模块
>npm uninstall gulp --save-dev
### npm outdated 检查模块是否已经过时
>npm outdated [[<@scope>/]<pkg> ...]
>此命令会列出所有已经过时的包，可以及时进行包的更新
### npm ls 查看安装的模块
    npm ls [[<@scope>/]<pkg> ...]

    aliases: list, la, ll
>npm ls -g 查看全局安装的模块及依赖 
### npm init 在项目中引导创建一个package.json文件
>安装包的信息可保持到项目的package.json文件中，以便后续的其它的项目开发或者他人合作使用，也说package.json在项目中是必不可少的
### npm help 查看某条命令的详细帮助 
>例如输入npm help install，系统在默认的浏览器或者默认的编辑器中打开本地nodejs安装包的文件/nodejs/node_modules/npm/html/doc/cli/npm-install.html
>npm help install
### npm root 查看包的安装路径
>输出 node_modules的路径
### npm config 管理npm的配置路径
>npm update [-g] [<pkg>...]
>对于config这块用得最多应该是设置代理，解决npm安装一些模块失败的问题
- 例如我在公司内网，因为公司的防火墙原因，无法完成任何模块的安装，这个时候设置代理可以解决
   npm config set proxy=http://xxx.com:8080
- 又如国内的网络环境问题，某官方的IP可能被和谐了，幸好国内有好心人，搭建了镜像，此时我们简单设置镜像
   npm config set registry="http://r.cnpmjs.org"
- 也可以临时配置，如安装淘宝镜像
   npm install -g cnpm --registry=https://registry.npm.taobao.org
### npm start 启动模块
>该命令写在package.json文件scripts的start字段中，可以自定义命令来配置一个服务器环境和安装一系列的必要程序，如
    "scripts": {
    "start": "gulp -ws"
}
### yarn
> Yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具 ，正如官方文档中写的，Yarn 是为了弥补 npm 的一些缺陷而出现的。
- npm缺陷
1. npm install的时候巨慢。特别是新的项目拉下来要等半天，删除node_modules，重新install的时候依旧如此。
2. 同一个项目，安装的时候无法保持一致性。由于package.json文件中版本号的特点，下面三个版本号在安装的时候代表不同的含义
   "5.0.3",
   "~5.0.3",
   "^5.0.3

   5.0.3”表示安装指定的5.0.3版本，“～5.0.3”表示安装5.0.X中最新的版本，“^5.0.3”表示安装5.X.X中最新的版本。这就麻烦了，常常会出现同一个项目，有的同事是OK的，有的同事会由于安装的版本不一致出现bug。
3. 安装的时候，包会在同一时间下载和安装，中途某个时候，一个包抛出了一个错误，但是npm会继续下载和安装包。因为npm会把所有的日志输出到终端，有关错误包的错误信息就会在一大堆npm打印的警告中丢失掉，并且你甚至永远不会注意到实际发生的错误

- Yarn的优点
>速度快 
1. 并行安装：无论 npm 还是 Yarn 在执行包的安装时，都会执行一系列任务。npm 是按照队列执行每个 package，也就是说必须要等到当前 package 安装完成之后，才能继续后面的安装。而 Yarn 是同步执行所有任务，提高了性能。
2. 离线模式：如果之前已经安装过一个软件包，用Yarn再次安装时之间从缓存中获取，就不用像npm那样再从网络下载了。
>安装版本统一
>更简洁的输出
>多注册来源处理
- Yarn和npm命令对比
   npm install === yarn 
   npm install taco --save === yarn add taco
   npm uninstall taco --save === yarn remove taco
   npm install taco --save-dev === yarn add taco --dev
   npm update --save === yarn upgrade

>[npm和yarn的区别]<https://zhuanlan.zhihu.com/p/27449990>   

