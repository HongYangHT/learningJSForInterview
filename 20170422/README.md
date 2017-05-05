### es6 简单实现继承的方法

```javascript
	function Shape() {
      this.x = 0;
      this.y = 0;
    }
    
    Shape.prototype.move = function(x, y) {
        this.x += x;
        this.y += y;
        console.info("Shape moved.");
    };
    
    // Rectangle - subclass
    function Rectangle() {
      Shape.call(this); //call super constructor.
    }
    
    Rectangle.prototype = Object.create(Shape.prototype);
    
    var rect = new Rectangle();

    console.log(rect instanceof Rectangle); //true.
    console.log(rect instanceof Shape); //true.
    
    rect.move(); //"Shape moved."
```