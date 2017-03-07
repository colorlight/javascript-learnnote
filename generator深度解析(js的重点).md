##### 1. 与闭包的相似性
都是在执行函数的时候，并没有释放内存。
```
fib.next();//   这条语句执行完后，如果fib还有yield则不会释放内存。
```
所以与闭包相似，它也具有记忆性。可以保存执行的状态。对于那种需要执行一次，返回状态，下次执行时候，继续在原来的基础上累加的函数很方便。
##### plus: fib.next()返回的是一个对象{value:3,done:false}

##### 2.  next里是可以传入参数的
```
function* gen(x){
  var y = yield x + 2;
  return y;
}
var t = gen(1);
t.next(); // {value:3 done: false}
t.next(3);//{value:3, done:true}
```
传入的参数会作为上一次yield的返回值。
就是说在gen内部 yield x + 2 这个的整体的返回值就是这个参数。所以第二次调用next的时候，传入的参数3，作为返回值，赋值给了y。

##### 3. generator函数可以捕获外部的的错误

```
function* gen(x){
  try {
    var y = yield x + 2;
  } catch (e){ 
    console.log(e);
  }
  return y;
}

var g = gen(1);
g.next();
g.throw('出错了');
// 出错了
```

虽然是在外部的错误，但是通过g.throw()方法却可以将错误传入到里面。

==实现了 出错代码与处理错误的代码时间和空间上的分离 这对异步编程很重要==

##### 4. generator的执行流程
generator最大的特点就是可以暂停函数和继续执行，感觉和异步操作关系很大。
```
function *test(){
	yield setTimeout(function(){
		console.log('1...\n');
		it.next();
	},1000);
     console.log('2...\n');
}
var it = test();
```
var it = test();函数并没有执行，只是返回了一个iterator的指针对象。
当it.next()才真正的执行函数。并且执行到第一个yield后面的语句并且暂停。
在运行next继续执行到一个yield暂停。这样知道return。

```
function *test(){
	yield setTimeout(function(){
		console.log('1...\n');
	},1000);
     console.log('2...\n');
}
var it = test();
it.next();
it.next();
```

这个例子主要是为了说明当yield后面是异步操作的时候会发生什么。  
这个例子会先输出一个2在输出一个1，因为第一个yield后面是一个异步操作，所以并不会立即执行，而此时已经第二个next了，所以会立刻执行下一个console。
#### 5. Generator的流程管理 （非异步操作）
```
function *gen(){
    //....
}

var g = gen();
var res = g.next();

while(!res.done){
    console.log(res.value);
    res = g.next();
}
```
用这种方法gen会自动执行完所有步骤，但是这不适合异步操作。异步操作要求必须上一步执行完后才执行下一步。

#### 6. Generator的流程管理（异步操作）

* ==generator 可以用同步代码的样子实现异步操作。==
这大概就是Generator的精髓。

具体实现是靠Generator内部函数（功能实体，也可以称作业务实体）和Generator的外部函数run（）函数这是一个控制实体两部分组成的。  
==控制实体控制着异步操作的同步执行。==  
举例
```
功能业务实体
var fs = require('fs');
var thunkify = require('thunkify');
var readFile = thunkify(fs.readFile);

var gen = function* (){
  var r1 = yield readFile('/etc/fstab');
  console.log(r1.toString());
  var r2 = yield readFile('/etc/shells');
  console.log(r2.toString());
};
```
这里面用了一个thunkify进行了一次封装对fs.readFile，==这样使Generator函数的内部实际上是没有异步函数的==。他将异步函数移到了Generator函数的外部。
```
手动的控制实体，便于分析
var g = gen();

var r1 = g.next();
r1.value(function(err, data){
  if (err) throw err;
  var r2 = g.next(data);
  r2.value(function(err, data){
    if (err) throw err;
    g.next(data);
  });
});
```


控制实体所做的就是，在yield处暂停Generator函数的执行，将执行权交给自己，并执行异步操作，当异步操作执行完毕后，会给系统一个响应，将执行权交给callback函数，而控制实体此时会继续Generator函数的执行，因为第一个yield所代表的异步操作（注意是代表的异步操作，因为genreator内部实际上是没有异步操作的）已经完成。所以继续执行Generator内部函数，业务实体获得控制权，并将业务继续执行。在进行到第二个yield语句的时候，又遇到了一个代表着异步操作的语句，此时又会将处理权交给外部，外部执行真正的一步操作r2.value();等到异步操作响应，继续进入callback， callback中会继续进入业务实体，因为所代表的的异步操作已经完成，继续执行业务。业务实体执行至函数结束前停下，将控制权交给控制实体，控制实体最后一次执行next，进入业务实体，业务实体没有return，最后返回控制实体，结束整个流程。


手动控制实体仅是为了展示通过Generator函数进行异步操作的架构。而实际上是给控制实体写了一个函数的run（），run(gen)可以自动控制着gen函数的执行。下面是run函数的源码。
```
function run(fn) {
  var gen = fn();

  function next(err, data) {
    var result = gen.next(data);
    if (result.done) return;
    result.value(next);
  }

  next();
}

function* g() {
  // ...
}

run(g);
```
gen首先获得了Generator函数的内部指针。然后定义了一个next函数，这个next函数是一个递归调用的函数。然后执行这个next函数，第一次执行next函数是没有参数传入的，所以data是undefined，err也是undefined的，所以第一条可以改写成result = gen.next(); 此时是把控制权交给了业务实体，业务实体会yield一个thunk函数出来，就是result的value函数，下面控制实体会先判断是否业务实体已经到了return，如果到了业务实体的return，就返回，说明整个流程已经走完，如果没有到，说明业务实体还要继续。此时，控制实体继续执行，它执行了一个真正的异步操作，==实际上此时run函数已经退出了，因为result.value(next);是一个异步操作。== 但是当异步操作开始响应的时候，又会进入next函数，开始将执行权交给业务实体，业务实体执行业务逻辑后，又将控制权交给next，next函数执行异步操作，result.value（）。 执行完后，跳出next，注意此时控制权已经不在run里了，因为run已经执行完了，后面的一切行为都是靠回调next来完成执行的。所以严格上，这并不是一个递归函数，这并没有涉及到栈的递增。是靠异步响应来完成的。这样异步调用next，直到发现已经执行完业务逻辑了之后result.done为true，就返回，不再执行后面的控制实体，就是执行异步操作。此时，算完全完成了所有的流程。