##### 1. base64
用64个字符表示二进制数据的一种方法，将6个bit二进制数据映射到64个字符里，那么6个bit就变为了8个bit的字符。

##### 2. JavaScript的执行模式:异步与同步

**同步模式**: 执行顺序与代码顺序相同，按照代码顺序执行
```
f1();
f2();
```
先执行f1，在执行f2

**异步模式**:执行顺序并不是按照代码顺序。

#### 3. AJAX
Asynchronous JavaScript and XML  
意思是用javascript完成异步网络操作。

如果不用的话，点一次提交会等待一次Http request，会刷新页面的完成，就是同步执行嘛，执行完一个函数，再执行一个函数，这是javascript的执行模式，单线程。

AJAX的横空出世一举解决了这个问题，它采用异步的模式，再当有response到达时才将结果传给浏览器，很像IO复用，linux中的select系统调用。

**例程**
```
function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}

var request = new XMLHttpRequest(); // 新建XMLHttpRequest对象

request.onreadystatechange = function () { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
        // 判断响应结果:
        if (request.status === 200) {
            // 成功，通过responseText拿到响应的文本:
            return success(request.responseText);
        } else {
            // 失败，根据响应码判断失败原因:
            return fail(request.status);
        }
    } else {
        // HTTP请求还在继续...
    }
}

// 发送请求:
request.open('GET', '/api/categories',false);
request.send();
//alert('请求已发送，请等待响应...');

```
XMLHttpRequest对象的open()方法有3个参数，第一个参数指定是GET还是POST，第二个参数指定URL地址，第三个参数指定是否使用异步，默认是true，所以不用写。

注意，千万不要把第三个参数指定为false，否则浏览器将停止响应，直到AJAX请求完成。如果这个请求耗时10秒，那么10秒内你会发现浏览器处于“假死”状态。

最后调用send()方法才真正发送请求。GET请求不需要参数，POST请求需要把body部分以字符串或者FormData对象传进去。

#### 用Promise来简化Ajax
```
'use strict';

// ajax函数将返回Promise对象:
function ajax(method, url, data) {
    var request = new XMLHttpRequest();
    return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
            if (request.readyState === 4) {
                if (request.status === 200) {
                    resolve(request.responseText);
                } else {
                    reject(request.status);
                }
            }
        };
        request.open(method, url);
        request.send(data);
    });
}

var log = document.getElementById('test-promise-ajax-result');
var p = ajax('GET', '/api/categories');
p.then(function (text) { // 如果AJAX成功，获得响应内容
    log.innerText = text;
}).catch(function (status) { // 如果AJAX失败，获得响应代码
    log.innerText = 'ERROR: ' + status;
});

```

##### 4.跨域访问URL资源的问题

浏览器有自己的安全考虑，AJAX是不允许跨域名访问资源的，就是只能访问本域名下的资源，如何访问其他url的资源，具体看教程。

