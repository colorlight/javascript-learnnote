[本文的出处](https://cnodejs.org/topic/5640b80d3a6aa72c5e0030b6)
#### 1. async和await可以搭建一个特殊的函数，在这个函数的内部，使用await等待promise对象。
#### 2. 实例如下
```
var sleep = function (time) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve();
        }, time);
    })
};

var start = async function () {
    // 在这里使用起来就像同步代码那样直观
    console.log('start');
    await sleep(3000);
    console.log('end');
};

start();
console.log('haha');
//start
//haha
//end
```
start是被async封装的函数，他的内部有一个sleep是一个异步操作，就是一个promise对象，await会等待一个promise对象，直到resolve被调用。并且直接返回传入resolve中的值。然后才开始执行后面的语句。
#### 3. resolve 与reject所对应的情况
有例子可以看出，async和await是对promise的一种改写，省略了then与catch， await后面的语句对应着then的第一个function，像下面这样
```
var sleep = function (time) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            // 返回 ‘ok’
            resolve('ok');
        }, time);
    })
};

var start = async function () {
    let result = await sleep(3000);
    console.log(result); // 收到 ‘ok’
};
```
而catch是由try catch来实现的
```
var sleep = function (time) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            // 模拟出错了，返回 ‘error’
            reject('error');
        }, time);
    })
};

var start = async function () {
    try {
        console.log('start');
        await sleep(3000); // 这里得到了一个返回错误
        
        // 所以以下代码不会被执行了
        console.log('end');
    } catch (err) {
        console.log(err); // 这里捕捉到错误 `error`
    }
};
循
```
这样可以简化了promise的catch，按照同步的形式写了出来。