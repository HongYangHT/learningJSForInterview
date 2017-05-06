### vue-resource 使用学习

#### vue-resource (1.0.3) 特性

- 支持 promise 和 URI Template
- 支持 http拦截器设置
- 仅支持IE9+
- 支持vue 1.x 和 2.x
- 大小 14k

#### vue-resource 的使用
    
- http request (get\post\put\delete\jsonp\patch\head)
  * 全局
   
    ```javascript
    Vue.http.get('/url',[body],[options]).then(resolve, rejected).then().catch()
    
    Vue.http.get('/url',[body],[options]).then(resolve).catch().then()
    延深：promise 的rejected 与catch的区别  
    ```
    [图示](https://cnodejs.org/topic/580985c727a1d99178a9904e)

    [options] 
    - url body headers params method responseType timeout before progress credentials emulateHTTP emulateJSON 
    - before 拦截器回调函数
    - timeout 设置timeout的时间（毫秒）
    - emulateHTTP 对于不支持put 、delete 、patch,使用post代替并将header头中X-HTTP-Method-Override
    - emulateJSON 以html from 表单的形式来提交参数， 并且vue-resource 会自动检测提交的request.body 的类型是否是formData类型
    
    ```javascript
    if (isFormData(request.body)) {

        request.headers.delete('Content-Type');

    } else if (isObject(request.body) && request.emulateJSON) {

        request.body = Url.params(request.body);
        request.headers.set('Content-Type', 'application/x-www-form-urlencoded');
    }
    ```

  * 组件

    ```javascript
    this.$http.get('/url',[body],[options]).then(resolve, rejected)
    ```

- http resource(get\save\query\update\remove\delete) 可以使用uri-template
  * 全局
    Vue.resource(url, [params], [actions], [options])
  * 组件
    this.$resource(url, [params], [actions], [options])
```javascript
let resource = this.$resource('/url{/id}')
// /url/1
resource.get({id:1},params).then().catch().then()
```

- response (返回结果)
  * url\headers\body\data\ok\status\statusText\bodyText\blob()\text()\json()
  * 返回数据是放在body里面
  * ok
   
  ```javascript
    this.ok = status >= 200 && status < 300;
  ```

  * data  
  ```
  Object.defineProperty(Response.prototype, 'data', {

      get () {
        return this.body
      },

      set (body) {
        this.body = body
      }

    })
  ```

- interceptor(拦截器)  