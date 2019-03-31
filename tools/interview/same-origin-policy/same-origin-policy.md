### 同源策略
#### 同源的概念
从A站点获取一个页面，然后在这个页面中去访问B站点的资源，就会产生跨域。
使用XMLHttpRequest对象去发送请求，就会有浏览器的限制，跨域。
我们用script标签发送请求，就不会有跨域的限制。

#### 同源含义
1. 协议相同
2. 域名相同
3. 端口号相同
http://www.example.com/dir/page.html这个网址，协议是http://，域名是www.example.com，端口是80（默认端口可以省略）。它的同源情况如下。
- http://www.example.com/dir2/other.html：同源
- http://example.com/dir/other.html：不同源（域名不同）
- http://v2.www.example.com/dir/other.html：不同源（域名不同）
- http://www.example.com:81/dir/other.html：不同源（端口不同）

#### 同源的目的
同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

设想这样一种情况：A网站是一家银行，用户登录以后，又去浏览其他网站。如果其他网站可以读取A网站的 Cookie，会发生什么。

####  限制范围
1. Cookie、LocalStorage 和 IndexDB 无法读取。
2. DOM 无法获得。
3. AJAX 请求不能发送。

#### AJAX
**同源政策规定，AJAX请求只能发给同源的网址，否则就报错。**
除了架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务），有三种方法规避这个限制。
- JSONP
- WebSocket
- CORS


