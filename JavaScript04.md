#### 1. 函数定义和调用
```
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

 * 关键字function 指明这是一个函数
 * abs 是函数名
 * x是参数，任意的
 * 大括号内是函数的具体定义  

#### 2. 匿名函数
 * JavaScript中，函数也是一种对象，所以还可以这样定义函数
 ```
 var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
 ```
 * abs是这个对象的引用，与函数名是等价的。
 
 #### 3. 调用函数
 * abs(-10);
 * abs(-10,10);
 
 #### 4. arguments关键字
 * 可以利用arguments很方便的分析参数
 * arguments相当于一个数组，它包含了所有的输入参数
 ```
 function foo(x) {
    alert(x); // 10
    for (var i=0;     i<arguments.length; i++) {
        alert(arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
 ```
 #### 5. rest参数
  * 为了方便提取某些个参数以外的参数，可以利用rest
  ```
  function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}
foo(1,2,3,4,5);
//结果
//a = 1
//b = 2
//[3,4,5]
  ```
  * rest 也是相当于一个array
 