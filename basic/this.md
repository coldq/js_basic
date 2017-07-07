# Js中this的相关知识

## this绑定机制

1. 默认绑定

    首先介绍的是函数调用类型：独立函数调用，在没有其他应用下的默认规则。
    这时候this指向了window。
    代码-1
    ```
    function foo(){
      var a = 2;
      function bar(){
        console.log(this.a)
      }
      bar();
    }
    foo();//console undefined
    ```
    这段代码中,foo函数中的bar函数运行时，由于默认绑定，this指向window，a未定义导致输出为undefined。