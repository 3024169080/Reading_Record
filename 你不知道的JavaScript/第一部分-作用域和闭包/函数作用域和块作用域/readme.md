# 函数作用域和块作用域

- 函数中的作用域  

  函数作用域的含义是指，属于这个函数的全部变量都可以在整个函数的范围内使用及复用(事实上在嵌套的作用域中也可以使用)

- 隐藏内部实现  

  从所写的代码中挑选一个任意的片段，然后用函数声明对它进行包装，实际上就是把这些代码"隐藏"起来了  

  可以把变量和函数包裹在一个函数的作用域中，然后用这个作用域来"隐藏"它们

- 规避冲突  

  "隐藏"作用域中的变量和函数所带来的另一个好处，是可以避免同名标识符之间的冲突  

  冲突会导致变量的值被意外覆盖  

  举个栗子：
  ```
  function foo() {
      function bar(a) {
          i = 3; // 修改for循环所属作用域中的i
          console.log(a + i);
      }

      for(var i = 0; i < 10; i++) {
          bar(i * 2); // 糟糕，无限循环了！
      }
  }
  foo()
  ```
  bar(...)内部的赋值 i = 3，意外的覆盖了声明在foo(...)内部for循环中的i  

  ps：在ES6中可以使用 let 来避免这个冲突

- 函数作用域
  - 在任意代码片段外添加包装函数，可以将内部的变量和函数定义"隐藏"起来，但是该函数本身会污染其所处的作用域  

    解决方案：立即执行函数 --- (function foo() {})()
  - 如果function是声明中的第一个词，那么就是一个函数声明，否则就是一个函数表达式

- 匿名和具名
  - 最熟悉的匿名函数：回调函数  

    举个栗子：
    ```
    setTimeout(function() {
        console.log('I waited 1 second!');
    }, 1000);
    ```
    这叫作匿名函数表达式，因为你function(...)没有名称标识符  

  - 函数表达式可以是匿名的，而函数声明则不可以省略函数名 —— 在JavaScript的语法中这是非法的
  - 匿名函数的缺点
    1. 匿名函数在栈追踪中不会显示出有意义的函数名，使得调试很困难
    2. 如果没有函数名，当函数需要引用自身时只能使用已经过期的arguements.callee引用，比如在递归中，另一个函数需要引用自身的例子，是在事件触发后，事件监听器需要解绑自身
    3. 匿名函数省略了对于代码可读性/可理解性很重要的函数名。一个描述性的名称可以让代码不言自明
  - 始终给函数表达式命名是一个最佳实践

- 立即执行函数表达式  

  举个栗子：
  ```
  var a = 2;

  (function foo() {
      var a = 3;
      console.log(a); // 3
  })();
  
  console.log(a); // 2
  ```
  第一个( )将函数变成表达式，第二个( )执行了这个函数

- 块作用域
  - with  

    块作用域的一个例子，用with从对象中创建出的作用域仅在with声明中而外部作用域中有效
  - try/catch  

    catch分支会创建一个块作用域，其中声明的变量仅在catch内部有效
  - let  

    可以将变量绑定到所在的任意作用域中(通常是{...}内部)  

    换句话说，let为其声明的变量隐式地劫持了所在的块作用域
  - const  
  
    ES6引入了const，同样可以用来创建块作用域变量，但其值是固定的(常量)，之后任何试图修改值的操作都会引起错误
