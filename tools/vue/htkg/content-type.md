### Content-Type:
application/x-www-form-urlencoded:当传入的数据是FormData的格式时候，network里面Content-Type类型是application/x-www-form-urlencoded

### Form的enctype属性为编码方式，常用有两种

application/x-www-form-urlencoded和multipart/form-data，默认为application/x-www-form-urlencoded

### 如果传的参数不是formData，还是application/x-www-form-urlencoded

```
// 强制转化成application/json
export function equipmentLoginApi(data) {
  return axios({
    url: '/user/user/v2/masterapi/loginByTaxDevice',
    method: 'post',
    headers: {
      'Content-Type': 'application/json'
    },
    data
  })
}

```
