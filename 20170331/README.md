### 由函数创建到函数自执行，再到插件写法
- 函数创建,常见的函数创建方式分为两种，一种声明式函数创建，一种匿名函数式创建

```
声明式函数创建
function fun(){ /* do something */}   
```

```
匿名函数式创建
var fun = function(){ /* do something */ }

创建匿名函数并将其赋值给fun变量
```

- 匿名函数（LIFT模式）

```
(function(){})() /* 常见写法 */

/* 其他写法还包括,一元操作符 */
-(function(){})()
~(function(){})()
+(function(){})()
!(function(){})()
```

- 自执行函数（立即调用函数表达式）
  () 表示一个表达式代码块，这也就是说中间不能参杂语句
  {} 表示一个代码块，代码块在es6中是有代码块作用域的

```
// 由于括弧()和JS的&&，异或，逗号等操作符是在函数表达式和函数声明上消除歧义的

var fun = function(){}(); // 提供接口模式

```

- 函数原型与原型链
  每一个函数都有一个prototype(原型)属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法

   - 原型

```
  原型使用1
   var Calculator = function (decimalDigits, tax) {
        this.decimalDigits = decimalDigits;
        this.tax = tax;
   };
   Calculator.prototype = {
        add: function (x, y) {
            return x + y;
        },

        subtract: function (x, y) {
            return x - y;
        }
    };
```

```
  原型使用2 封装私有的function，通过return的形式暴露出简单的使用名称
   var Calculator = function (decimalDigits, tax) {
        this.decimalDigits = decimalDigits;
        this.tax = tax;
   };
   Calculator.prototype = (function () {
        var add = function (x, y) {
            return x + y;
        },

        subtract = function (x, y) {
            return x - y;
        }
        return {
            add: add,
            subtract: subtract
        }
    } ());
```

   - 原型链

```
var person = function(){}

function Person(){}

person.prototype.fun = function(){console.log(1)}

Person.prototype.fun = function(){console.log(1)}

```

- Module 模式（闭包模式）

```
  var module = (function(){
  	return {};
  }());

  var module = (function (my) {
    
    return my;
  } (module || {}));  
```
