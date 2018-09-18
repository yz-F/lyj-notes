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