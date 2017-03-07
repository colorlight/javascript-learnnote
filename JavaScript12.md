##### 1. setTimeout
```
function callback() {
    console.log('Done');
}
console.log('before setTimeout()');
setTimeout(callback, 1000); // 1秒钟后调用callback函数
console.log('after setTimeout()');
```
他是先等待1000ms再执行callback

##### 2. 函数内部的异步

```
function test(){
 var timeout = Math.random() * 2;
  console.log('set timeout to: ' + timeout + ' seconds.');
  setTimeout(function(){
       if(timeout < 1){
		  console.log('call < 2');
		}
		else{
			console.log('call > 2');
		}
	}, timeout * 1000);
  console.log('马上就要return了');
}
test();
console.log('跳出了函数');
```
执行结果：
```
set timeout to: 0.9568804048654154 seconds.
VM1106:12 马上就要return了
VM1106:15 跳出了函数
VM1106:6 call < 2
```
本函数执行完后再返回函数内部执行。

##### 3. 再看setTimeout
setTimeout他接受两个参数，第一个参数是一个函数，可以是一个指向函数的变量，也可以是一个匿名函数，直接定义的形式，你要习惯这种形式，看出这个结构仍然是一个函数就行了。第二个参数是多久以后执行这个回调函数。

##### 4. Promise所接受的参数

Promise只接受一个参数，这个参数是有固定的限制的，有点类似java中的接口。
==这个参数必须是一个函数==，而且这个函数必须接受两个参数，一个参数是resolve另一个是reject。这两个参数而且都是函数。 类似于Java中的实现了resolve和reject两个方法的接口。  
当new 一个Promise的对象的时候， 所接受的函数参数会立刻执行，甚至在返回promise对象之前就开始执行了

##### 5. resolve与reject函数
这个两个函数是干什么的？   
这两个函数分别负责promise对象的两个状态，如果异步操作成功的话，执行reslove函数，如果异步操作失败的话执行reject函数
```
function test(reslove, reject){
    setTimeout(function(){
    if(condition 1)
        resolve();
    else
        reject();
    }, delayTime);
}
```
看这个架构，传入promise的参数就是test这个函数，他有两个参数分别是两个resolve和reject, new Promise(test);之后，test会立即执行并且，初始化一个异步操作，当他不会执行setTimeout内部的函数就会直接返回，等到到了*delayTime*之后，会进入那个异步操作的函数中，决定是执行reslove还是reject，如果执行了resolve就会使该promise对象的状态改为成功执行，反之亦然。

##### 6. test的执行流程
```
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    console.log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function () {
 		reject('sdfsdfs');
		console.log('it can\'t be execute');
    }, timeOut * 10000);
    console.log('i'm preferred than above');
}
haha = new Promise(test);
```
首先当你new promise后，test就开始执行，发现有一个异步setTimeout就会跳过去，执行异步后面的操作,等到了时间以后会执行里面的操作，当发现是一个reject的时候，他不会执行，只有等你调用promise的catch方法他会执行，当发现是回一个resolve的时候，只有等你调用then方法才会执行。会先执行reject后面的console.log


##### 7. haha = new Promise(test);的返回值
new Promise(test);返回一个Promise对象，这个对象被两个属性描述，第一就是状态pending resolve和reject 第二就是传入resolve或者reject的值。   
==haha.then的返回值==  
* haha.then有两种返回的情况
    1. 当haha.then(expression)，expression不是function的时候，它会返回haha这个对象就是和不调用then一样的结果
    2. 当haha.then(expression),expression是funciton的时候，它会返回一个新的promise对象，这个对象状态是根据function是否是一个返回promise对象而决定的。如果这是一个不返回promise对象的，就直接返回resolve的状态，并且promisevalue的值为undefined
    3. haha.then(expression),expression 是一个function并且==是一个返回promise对象的函数==，那么其状态就是该promise的状态。then返回的就是一个函数内部的new promise行为。

##### 7. Promise的链式反应
结构
```
job1.then(job2).then(job3)
```
job1，job2，job3都是Promise对象，实际上job2只是一个函数，他绑定了job1的resolve，job2他不是一个普通的函数，他返回了一个promise对象，并且有自己的resolve和reject所以job1.then（job2）返回的就是job2的promise