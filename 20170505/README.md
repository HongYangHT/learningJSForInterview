### promise 详尽学习
#### 1. promise的实现原理
- 原理图
![原理图](http://upload-images.jianshu.io/upload_images/2898168-8a4da28a5a247bd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  > 每个Promise存在三个互斥状态： pending fullfilled rejected 之间的状态的关系：
  ![状态关系图](http://upload-images.jianshu.io/upload_images/2898168-49e0d341d366ffe6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  (上面两图摘抄与MDN)

- promise的基础实现

```javascript
function Promise(fn) {
    var state = 'pending',
        value = null,
        deferreds = [];

    this.then = function (onFulfilled, onRejected) {
        return new Promise(function (resolve, reject) {
            handle({
                onFulfilled: onFulfilled || null,
                onRejected: onRejected || null,
                resolve: resolve,
                reject: reject
            });
        });
    };

    function handle(deferred) {
        if (state === 'pending') {
            deferreds.push(deferred);
            return;
        }

        var cb = state === 'fulfilled' ? deferred.onFulfilled : deferred.onRejected,
                ret;
        if (cb === null) {
            cb = state === 'fulfilled' ? deferred.resolve : deferred.reject;
            cb(value);
            return;
        }
        try {
            ret = cb(value);
            deferred.resolve(ret);
        } catch (e) {
            deferred.reject(e);
        }
    }

    function resolve(newValue) {
        if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
            var then = newValue.then;
            if (typeof then === 'function') {
                then.call(newValue, resolve, reject);
                return;
            }
        }
        state = 'fulfilled';
        value = newValue;
        finale(); // 执行下一个
    }

    function reject(reason) {
        state = 'rejected';
        value = reason;
        finale();
    }

    function finale() {
        setTimeout(function () {
            deferreds.forEach(function (deferred) {
                handle(deferred);
            });
        }, 0); // 异部执行
    }

    fn(resolve, reject);
}

function doPromise() {
    return new Promise(function(resolve){
        resolve('aaa')
    })
}
doPromise().then(function (a) {
    console.log('bbb',a)
})
```
