1. promise是异步编程的一种解决方案
2. promise简单说是一个容器，里面包含着某个未来才会结束的事件。
3. 三种状态：pending resolved rejected，只有事件本身可以改变这个状态。
4. 一旦状态改变，就不会再变，任何时候都可以得到这个结果，不会像event事件一样，一旦错过，再去监听就得不到结果了。
5. 状态改变只有两种可能：从pending变为resolved，从pending变为rejected。
6. promise对象可以用同步的编程方式来写异步操作。
7. promise的缺点： 无法取消，一旦新建立即执行。
8. 缺点二：内部抛出的错误，不会反应到外部
9. 缺点3，当处于pending状态的时候，无法知道进展到哪一个阶段了。
10. 基本用法：promise是一个构造函数，用来生成一个promise实例。

    ```
    var promise = new Promise(function(resolve, reject) {
    // ... some code

     if (/* 异步操作成功   */){
      resolve(value);
     } else {
      reject(error);
     }
    });
    ```
11. Promise构造函数接受一个函数作为参数，这个函数==一般包含异步操作==，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。
12. resolve的作用是将pending变成resolved，并将异步操作的结果作为参数传递出去，value。 reject的作用是将pending变成rejected并将结果传递出去。
13. 实例的回调函数。
14. 当实例生成后，调用then可以分别制定resolved状态和reject状态的回调函数。
15. then方法可以接受两个回调函数。第一个回调函数是当promise对象的状态是resolved的时候会调用，第二个参数当promise对象的状态是rejected的时候会调用。
```
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```
两个函数都接受promise对象传出值的作为参数。
16. 帮助理解的例子
```
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
```
then函数是根据promise对象的状态决定执行function1还是function2，还是保持悬挂。如果promise是pending状态，他会不执行，继续监听，如果从pending变到了resolved状态就会执行function1，如果从pending状态变为rejectd就直接执行function2，如果本来就是resolved了，就直接执行function1，如果本来就是rejected转态，就直接执行function2.
17. then将在所有同步任务执行完才会执行
```
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('Resolved.');
});

console.log('Hi!');
// Promise
// Hi!
// Resolved
```
18. 如果resolve的参数是另一个promise对象，那么这个promise对象会被劫持
```
var p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error('fail')), 3000)
})

var p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))
// Error: fail
```
p2不再是p2了，而是完全变身为p1，所以p2绑定then就是p1绑定then。
19. ##### ==promise的主要应用场景==  
异步操作的顺序拼接。  
如何让一个异步操作在另一个异步操作之后执行。
```
p1.then(function(para1){
    return new Promise(function(reslove,reject){
        reslove(para3);
    })
})
.then(function(para2){
    
});
```
如果p1的回调函数返回的是一个promise对象，那么链式调用的then的回调函数传入的参数para2的值就是para3，而不是return的promise对象。  
这种形式就可以完成链式调用。保证第二个then的function在第一个function之后调用。