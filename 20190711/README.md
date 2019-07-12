### js 常用设计模式

- 工厂模式
> 工厂模式是为了解决多个类似申明或者说是实例化对象产生的重复问题

 - 组合继承
 ```js
 let Person = function (name) {
   this.name = name
 }
 Person.prototype = {
   getName: function () {
     console.log(this.name)
     return this.name
   }
 }

 let Child = function (options) {
   Person.call(this, options.name) // 立即调用构造函数，并将Child作为this传入，  继承了Person 且想Person传递参数， 构造函数继承 继承了Person的参数
   this.cname = options.cname
 }

 Child.prototype = new Person() // 将Person的prototype继承，包含 参数 与 方法（包含构造函数） 
 Child.prototype.constructor = Child // 这里需要将其转换成自己的构造函数

 Child.prototype.getCName = function () {
   console.log(this.cname)
   return this.cname
 }

 let child = new Child({
   name: 'name',
   cname: 'cname'
 })

 ```

 - 原型继承
 ```js
 function extend (SubClass, SupClass) {
   let F = function () {}
   F.prototype = SupClass
   SubClass.prototype = new F()
   SubClass.prototype.constructor = SubClass
   if (SupClass.prototype.constructor === Object.prototype.constructor) {
     SupClass.prototype.constructor = SupClass
   }
 }

 let Person = function () {}
 let Child = function () {
   Person.call(this)
 }

 extend(Child, Person)


 ```
- 单例模式
> 能且只能实例化一次

```js
let Singleton = function(name){
    this.name = name;
}
Singleton.prototype.getName = function(){
    return this.name
}
// 获取实例对象
let getInstance = (function() {
    let instance = null;
    return function(name) {
        if(!instance) {
            instance = new Singleton(name);
        }
        return instance;
    }
})()

// 封装
let getInstance = function(fn) {
    let result;
    return function(){
        return result || (result = fn.call(this,arguments));
    }
}
```

- 模块模式
> 为单体模式添加私有变量和私有方法能够减少全局变量的使用, 返回对象的匿名函数

```js
let singleMode = (function(){
    // 创建私有变量
    let privateNum = 112;
    // 创建私有函数
    function privateFunc(){
        // 实现自己的业务逻辑代码
    }
    // 返回一个对象包含公有方法和属性
    return {
        publicMethod1: publicMethod1,
        publicMethod2: publicMethod1
    }
})()
```

- 发布订阅模式
> 又称观察者模式

```js
let Event = (function(){
    let list = {},
          listen,
          trigger,
          remove;
          listen = function(key,fn){
            if(!list[key]) {
                list[key] = [];
            }
            list[key].push(fn);
        };
        trigger = function(){
            let key = Array.prototype.shift.call(arguments),
                 fns = list[key];
            if(!fns || fns.length === 0) {
                return false;
            }
            for(let i = 0, fn; fn = fns[i++];) {
                fn.apply(this,arguments);
            }
        };
        remove = function(key,fn){
            let fns = list[key];
            if(!fns) {
                return false;
            }
            if(!fn) {
                fns && (fns.length = 0);
            }else {
                for(let i = fns.length - 1; i >= 0; i--){
                    let _fn = fns[i];
                    if(_fn === fn) {
                        fns.splice(i,1);
                    }
                }
            }
        };
        return {
            listen: listen,
            trigger: trigger,
            remove: remove
        }
})();
// 测试代码如下：
Event.listen("color",function(size) {
    console.log("尺码为:"+size); // 打印出尺码为42
});
Event.trigger("color",42);
```