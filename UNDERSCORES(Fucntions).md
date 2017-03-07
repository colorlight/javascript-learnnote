因为underscore为了充分发挥JavaScript的函数编程特性，所以提供了大量的JavaScript本身没有的高阶函数。

##### 1. this
注意function中的this一般是调用他的对象
```
fucntion loglighitng(){
    console.log(this);
}

var haha = {name:'lighting', myfunction: loglighting};

haha.myfunction();
//haha这个对象
```

如果是直接调用loglighting的话，是输出的window这个对象。

##### 2. bind
```
var log = _.bind(console.log, console);
```
这样就可以将console对象直接绑定在log()的this指针上

##### 3. partial
为一个函数创建偏函数， 偏函数就是固定原函数某个参数不变而得到的。
```
var pow2N = _.partial(Math.pow, 2); // Math.pow(x,y); 传入两个参数，第一个参数为底，第二个为次数，现在固定第一个参数为2，生成一个新的函数。
pow2N(3); // 8
```
不固定第一个参数，固定第二个参数的方法。

```
'use strict';

var cube = _.partial(Math.pow, _,3);
固定了第二个参数为3，用UNDERSCORE占位。
var cube(3); // 27
```
##### 4. memoize

为了避免重复调用相同的函数，memoize可以使第二次调用的时候，直接调用结果，而不进行真正的计算。
```
'use strict';

var factorial = _.memoize(function(n) {
    console.log('start calculate ' + n + '!...');
    var s = 1, i = n;
    while (i > 1) {
        s = s * i;
        i --;
    }
    console.log(n + '! = ' + s);
    return s;
});

// 第一次调用:
factorial(10); // 3628800
// 注意控制台输出:
// start calculate 10!...
// 10! = 3628800

// 第二次调用:
factorial(10); // 3628800
// 控制台没有输出
```
但是当计算factorial(9);的时候还是会再次调用，可以采用递归的方式，来让factoria记住所有的调用
```
'use strict';

var factorial = _.memoize(function(n) {
    console.log('start calculate ' + n + '!...');
    if (n < 2) {
        return 1;
    }
    return n * factorial(n - 1);
});

factorial(10); // 3628800
// 输出结果说明factorial(1)~factorial(10)都已经缓存了:
// start calculate 10!...
// start calculate 9!...
// start calculate 8!...
// start calculate 7!...
// start calculate 6!...
// start calculate 5!...
// start calculate 4!...
// start calculate 3!...
// start calculate 2!...
// start calculate 1!...

factorial(9); // 362880
// console无输出
```

##### 5. one
保证函数只能执行一次
```
'use strict';
var register = _.once(function () {
    alert('Register ok!');
});
// 测试效果:
register();
register();
register();
//只会执行一次，弹出一次alert框
```

##### 6. delay
与setTioueout一样
```
'use strict';

_.delay(alert,2000);
```