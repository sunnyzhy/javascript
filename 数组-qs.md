# 数组

## GET 请求传递数组的方式

1. ```http://localhost:8080/users?roleIds[0]=1&roleIds[1]=2```
2. ```http://localhost:8080/users?roleIds[]=1&roleIds[]=2```
3. ```http://localhost:8080/users?roleIds=1&roleIds=2```
4. ```http://localhost:8080/users?roleIds=1,2```

## GET 请求的参数中带有中括号[]

**因为 ```[]``` 在 ```url``` 中属于功能性字符，所以如果 GET 请求的参数中带有中括号[]，那么前端必须使用转义，否则会出现 ```400 Bad Request``` 的错误。**

解决方法：

一、使用 qs 插件

```bash
npm install qs --save
```

qs 可以通过 ```arrayFormat``` 选项对数组进行格式化输出：

- ```qs.stringify({name: 'aaa',arr: [1,2,3]}}, { arrayFormat: 'indices' })``` ，输出：```name=aaa&arr%5B0%5D=1&arr%5B1%5D=2&arr%5B2%5D=3``` ，即 ```name=aaa&arr[0]=1&arr[1]=2&arr[2]=3```
- ```qs.stringify({name: 'aaa',arr: [1,2,3]}}, { arrayFormat: 'brackets' })``` ，输出：```name=aaa&arr%5B%5D=1&arr%5B%5D=2&arr%5B%5D=3``` ，即 ```name=aaa&arr[]=1&arr[]=2&arr[]=3```
- ```qs.stringify({name: 'aaa',arr: [1,2,3]}}, { arrayFormat: 'repeat' })``` ，输出：```name=aaa&arr=1&arr=2&arr=3```
- ```qs.stringify({name: 'aaa',arr: [1,2,3]}}, { arrayFormat: 'comma' })``` ，输出：```name=aaa&arr=1%2C2%2C3``` ，即 ```name=aaa&arr=1,2,3```

qs 可以通过 ```skipNulls``` 选项来跳过 ```null``` 或 ```undefined``` 值：

- ```qs.stringify({ a: 'b', c: null}, { skipNulls: true })``` ，输出：```a=b```

在 axios 的请求中处理:

```javascript
import qs from 'qs'

{
  url: '/user',
  method: 'get',
  baseURL: 'https://localhost/api/',
  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  paramsSerializer: function(params) {
    return qs.stringify(params, {arrayFormat: 'comma'})
  }
}
```

在 axios 的请求拦截器中处理：

```javascript
import qs from 'qs'

axios.interceptors.request.use(async (config) => {
//只对 GET 方式进行序列化
 if (config.method === 'get') {
   config.paramsSerializer = function(params) {
     return qs.stringify(params, { arrayFormat: 'comma' })
   }
 }
}
```

二、使用 encodeURIComponent() 函数转义

如果要处理单个字段，可以使用 encodeURIComponent() 函数转义

```javascript
var value = encodeURIComponent(params[i])
```
