# Js中this的相关知识

## this绑定机制

1. 默认绑定

    首先介绍的是函数调用类型：独立函数调用，在没有其他应用下的默认规则。
    这时候this指向了window。
    
    ```
    function b(){
      var a = 2;
      function aa(){
        console.log(this);
        var that = this;
	console.log(that.a)
      }
	aa();
    }
    b();
    ```
    这段代码中