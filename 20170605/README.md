### 详解BFC（Block Formatting Contexts块级上下文格式化）

##### 触发[BFC](http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html)，闭合浮动
- BFC的特性：
    1) 块级格式化上下文会阻止外边距叠加
    当两个相邻的块框在同一个块级格式化上下文中时，它们之间垂直方向的外边距会发生叠加。换句话说，如果这两个相邻的块框不属于同一个块级格式化上下文，那么它们的外边距就不会叠加。
     2) 块级格式化上下文不会重叠浮动元素
    根据规定，一个块级格式化上下文的边框不能和它里面的元素的外边距重叠。这就意味着浏览器将会给块级格式化上下文创建隐式的外边距来阻止它和浮动元素的外边距叠加。由于这个原因，当给一个挨着浮动的块级格式化上下文添加负的外边距时将会不起作用（Webkit和IE6在这点上有一个问题——可以看这个测试用例）。
    3) 块级格式化上下文通常可以包含浮动

- 那么如何触发BFC呢？  
    1、float 除了none以外的值  
    2、overflow 除了visible 以外的值（hidden，auto，scroll ）  
    3、display (table-cell，table-caption，inline-block)  
    4、position（absolute，fixed）  
    5、fieldset元素  

- 两种解决[办法](http://www.w3cplus.com/css/clear-float)
```css

.clearfix:before,
.clearfix:after {
 content: ".";
 display: block;
 height: 0;
 visibility: hidden;
  }
.clearfix:after {clear: both;}
.clearfix {zoom: 1;} /* IE < 8 */

.clearfix:before,
.clearfix:after {
  content:"";
  display:table;
}
.clearfix:after {
 clear:both;
 overflow:hidden;
}
.clearfix {
 zoom:1; /* IE < 8 */
}

```