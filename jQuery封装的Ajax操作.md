#### 1. http的get与post方法
* get需要一个url来请求资源， url的格式是文件名加上参数 
参数与文件名是用?隔开的，参数参数之间是用&隔开的。参数不能有空格，空格用%20表示。
    ```
    /path/to/resource?name=Bob%20Lee&check=1
    ```
* post的url就是文件名，参数是在body里面的

#### 2. ajax(url, settings) 的参数

为支持ajax，jQuery有ajax（）方法
格式是ajax(url,settings);  
url是一个字符串，setting是一个对象，可以传入几个属性
 * async：是否异步执行AJAX请求，默认为true，千万不要指定为false；

 * method：发送的Method，缺省为'GET'，可指定为'POST'、'PUT'等；

 * contentType：发送POST请求的格式，默认值为'application/x-www-form-urlencoded; charset=UTF-8'，也可以指定为text/plain、application/json；

* data：发送的数据，可以是字符串、数组或object。如果是GET请求，data将被转换成query附加到URL上，如果是POST请求，根据contentType把data序列化成合适的格式；

* headers：发送的额外的HTTP头，必须是一个object；

* dataType：接收的数据格式，可以指定为'html'、'xml'、'json'、'text'等，缺省情况下根据响应的Content-Type猜测。
```
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
});
// 请求已经发送了
```
#### 3.  如何得到返回的data数据，如何根据服务器的响应进行处理
jqxhr可以继续调用函数done，来与请求成功相对应。  
类似promise还可以then后面接着catch，
jqxhr在done后面可以接着fail和always
```
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
}).done(function (data) {
    ajaxLog('成功, 收到的数据: ' + data.categories[0].name);
}).fail(function (xhr, status) {
    ajaxLog('失败: ' + xhr.status + ', 原因: ' + status);
}).always(function () {
    ajaxLog('请求完成: 无论成功或失败都会调用');
});

```

#### 4. 常用http方法的操作
==get操作==
```
var jqxhr = $.get('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```
第二个参数是get操作的参数，实际的url是
```
/path/to/resource?name=Bob%20Lee&check=1
```

==post操作==
```
var jqxhr = $.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```
body由第二个参数决定，具体怎么样的形式，好像根据content相关

==getJSON==   
由于JSON用得越来越普遍，所以jQuery也提供了getJSON()方法来快速通过GET获取一个JSON对象：
```
var jqxhr = $.getJSON('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
}).done(function (data) {
    // data已经被解析为JSON对象了
});
```
data是对象，不是字符串
