# 问题

## 一、常见问题汇总

### 1、gitbook build 出现如下错误：

> gitbook init
>Error loading version latest: Error: Cannot find module 'extend'
    at Function.Module._resolveFilename (module.js:485:15)
    at Function.Module._load (module.js:437:25)
    at Module.require (module.js:513:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (/root/.gitbook/versions/3.2.2/lib/index.js:1:76)
    at Module._compile (module.js:569:30)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:503:32)
    at tryModuleLoad (module.js:466:12)
    at Function.Module._load (module.js:458:3)

TypeError: Cannot read property 'commands' of null

* 答：卸载gitbook-cli@2.3.1并安装gitbook-cli@2.3.0。
>$ rm -rvf ~ /.gitbook
>$ npm install -g gitbook-cli@2.3.0


> gitbook serve
>... Uhoh. Got error listen EADDRINUSE :::35729 ...
Error: listen EADDRINUSE :::35729
    at Object._errnoException (util.js:1024:11)
    at _exceptionWithHostPort (util.js:1046:20)
    at Server.setupListenHandle [as _listen2] (net.js:1351:14)
    at listenInCluster (net.js:1392:12)
    at Server.listen (net.js:1476:7)
    at Server.listen (C:\Users\61697\.gitbook\versions\3.2.3\node_modules\tiny-lr\lib\server.js:164:15)
    at Promise.apply (C:\Users\61697\.gitbook\versions\3.2.3\node_modules\q\q.js:1165:26)
    at Promise.promise.promiseDispatch (C:\Users\61697\.gitbook\versions\3.2.3\node_modules\q\q.js:788:41)
    at C:\Users\61697\.gitbook\versions\3.2.3\node_modules\q\q.js:1391:14
    at runSingle (C:\Users\61697\.gitbook\versions\3.2.3\node_modules\q\q.js:137:13)

You already have a server listening on 35729
You should stop it and try again.

TypeError: Cannot read property 'commands' of null

* 答：[使用gitbook为站点服务时如何更改默认侦听端口？](https://github.com/GitbookIO/gitbook/issues/622)<br>使用--lrport选项解决了这个问题
>接着报错gitbook serve命令找不到fontsettings.js
* 答：[random errors when using gitbook plugin on running "gitbook serve".](https://github.com/GitbookIO/gitbook/issues/1309#issuecomment-273584516)
gitbook --lrport 9999 serve
>~/.gitbook/versions/3.2.2/lib/output/website/copyPluginAssets.js
>在window下，这个目录在user下。找到.gitbook目录，找到copyPluginAssets.js，并且将113行的confirm: false改为confirm: true

> ctrl + s

    Starting server ...
    Serving book on http://localhost:4000
    Restart after change in file SUMMARY.md

    Stopping server
    events.js:141
        throw er; // Unhandled 'error' event
        ^

    Error: EPERM: operation not permitted, lstat 'G:\gitbook-touchevent\_book'
        at Error (native)

* 答：[文件更改时git服务无法重启？](https://github.com/GitbookIO/gitbook/issues/1379)<br>空白文件添加虚拟数据。

> 执行git clone提示“fatal: unable to access”

* 答：把https改成git即可

> gitignore配置

* 答：[gitignore配置](https://blog.csdn.net/ihtml5/article/details/87928109)

> 执行git pull 提示fatal: unable to access 'https://swca.aisino.com/aaics/web-ui-vue.git/': SSL certificate problem: unable to get local issuer certificate,

* 答：[git config --global http.sslVerify false](http://www.bubuko.com/infodetail-3611075.html)