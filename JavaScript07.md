 ##### 1. filter的用法
  ```
  var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
  ```
  与map类似，就是如果回调函数返回true就保留这个值，如果返回false就过滤掉这个值。
##### 2. filter回调函数被传入的参数
  可以传入element,index,和arr 3个值
##### 3. 利用filter巧妙过滤重复元素的方法
    ```
    r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
    });
    ```
    indexOf是数组的一个方法，返回这个元素在数组中第一次出现时的索引。

##### 4. sort的用法  
    sort也是数组的一个方法，但是他的默认排序是非常不好用的。但是他也是一个高阶函数，可以接受回调函数，来定义自己的比较规则。
     ```
     var arr = ['Google', 'apple', 'Microsoft'];
    arr.sort(function (s1, s2) {
        x1 = s1.toUpperCase();
        x2 = s2.toUpperCase();
        if (x1 < x2) {
            return -1;
        }
        if (x1 > x2) {
            return 1;
        }
        return 0;
    }); // ['apple', 'Google', 'Microsoft']
     ```
##### 5. 闭包
* 我对闭包的理解，闭包指的是一种特殊的函数。这个函数是其他函数生成的。
* JS中函数完全被等价为一个变量，可以被生成，被当做参数调用。比C的指针感觉要常用。

常用用法
    
    1. 延迟运算
   
    function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
    }
    
    var f = lazy_sum([1, 2, 3, 4, 5]); // function sum()
    f();    //15
    
    2. 实现封装 等同class机制.该闭包携带了局部变量x，并且，
    从外部代码根本无法访问到变量x。
    换句话说，闭包就是携带状态的函数，并且它的状态可
    以完全对外隐藏起来。
    
    function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
    }
    var c1 = create_counter();
    c1.inc(); //1
    c1.inc(); //2
    c1.inc(); //3
    var c2 = create_counter(10);
    c2.inc(); //11
    c2.inc(); //12
    
    3.闭包还可以把多参数的函数变成单参数的函数。例如
    ，要计算xy可以用Math.pow(x,y)函数，不过考虑到
    经常计算x2或x3，我们可以利用闭包创建新的函数pow2和pow3：
    function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
    }

    // 创建两个新函数:
    var pow2 = make_pow(2);
    var pow3 = make_pow(3);

    pow2(5); // 25
    pow3(7); // 343

需要注意的地方   
* 闭包引用循环变量的方法
    ```
    function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

    var results = count();
    var f1 = results[0];
    var f2 = results[1];
    var f3 = results[2];
    
当运行f1(),f2(),f3()的时候，结果都是16，因为是延迟运行，到最后的时候i已经变成了4.所以一直都是16
不能够使用后期会变化的值。

* 解决的办法： 将i作为参数传入到闭包函数中
```
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));   //这个相当于 function(n){} (i),i作为参数，并且立刻执行
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];

f1(); // 1
f2(); // 4
f3(); // 9
```