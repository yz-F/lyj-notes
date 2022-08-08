### proxy
- [proxy](https://juejin.im/post/5c0a6be4f265da616f6fc79c)
```
  module.exports = {
  devServer: {
    proxy: {
      // 反向代理
      '/api': {
        target: 'http://192.168.53.69:8080', // 实际请求的地址等于target加上/api
        ws: false, // 是否开启websocket
        changeOrigin: true, // 是否开启代理服务
        pathRewrite: {
          // 路径重写
          '^/api': '' // 将开头的/api换成空,所以实际的请求地址就变成了target加上你的路由路径
        }
      }
    }
  }
}
```

### proxy 有时候可能是(请求少带一斜杆)
```

// 请求拦截器
service.interceptors.request.use(
  config => {
    const urlPath = config.url.slice(0, 1);
    // 给请求header加token
    if (urlPath.indexOf('/') > -1) {
      config.url = 'apis/' + config.url.slice(1);
    } else {
      config.url = 'apis/' + config.url;
    }
```