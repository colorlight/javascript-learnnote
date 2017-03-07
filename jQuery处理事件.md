##### 1. 给某个DOM节点绑定事件
```
HTML:
<a id="test-link" href="#0">点我试试</a>

var a = $('#test-link');
a.on('click', function(){
    alert('Hello!');
}) //给这个DOM节点绑定一个单击事件，只要单击这个DOM，就会触发绑定的函数alert


简单写法

a.click(function(){
    alert('hello');
});
```


##### 2. JavaScript执行顺序
the scirpt is executed in the order it is encountered in the page 就是按照html中的位置，在上面就早执行。  所以这样的脚本运行会失败的 
```
<html>
<head>
    <script>
        // 代码有误:
        $('#testForm).on('submit', function () {
            alert('submit!');
        });
    </script>
</head>
<body>
    <form id="testForm">
        ...
    </form>
</body>
```
会获取不到testForm节点，因为在执行的时候，还没有载入<form>。

##### 3. ready 事件
ready事件是当所有的DOM节点对象都被浏览器加载完后就会触发。

```
一般用法
$(document).ready(function(){
    //某个行为
});
```

```
<html>
<head>
    <script>
        $(document).on('ready', function () {
            $('#testForm).on('submit', function () {
                alert('submit!');
            });
        });
    </script>
</head>
<body>
    <form id="testForm">
        ...
    </form>
</body>
```

这样写，即使写在了上面，提交函数也是在所有DOM节点加载完后才会执行的。

```
极简格式
$(function(){
 // 某些行为
});
```

##### 4.  绑定鼠标移动的事件
```
$(function () {
    $('#testMouseMoveDiv').mousemove(function (e) {
        $('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = ' + e.pageY);
    });
});
```

是在testMouseMoveDiv这个Dom节点上绑定的mousemove事件的，只要在这个区域中移动了鼠标，就会触发里面定义的函数，注意，他传入了一个e，这个e是Event对象，它就叫做事件的参数。跟前面的click事件对比，click绑定的函数是没有参数的，这个有。而且，只要一移动就会执行里面的函数，就是不断更新text内容，是内容一直更新鼠标的坐标

==绑定按键事件与此类似，也会要传入一个event对象，我猜应该可以通过这个event对象读取到按键的值==

##### 5. 取消绑定
1. 取消 某个事件的某个函数
    ```
    function hello() {
        alert('hello!');
    }
    
    a.click(hello); // 绑定事件
    
    // 10秒钟后解除绑定:
    setTimeout(function () {
        a.off('click', hello);
    }, 10000);
    ```

2. 取消 某个事件的所有函数
    ```
    a.off('click');
    ```

3. 取消该节点所有事件的所有函数
    ```
    a.off();
    ```
    
#### 6. 由用户出发的事件和程序触发的事件
文本框的Change事件    
 * 当在用户在里面进行输入时，就会触发change事件
 * 但是用程序直接改变input的val值是不会出发change事件的。

通过程序还是能够出发change事件的
```
input.change();  //这样就可以触发了
```

#### 7. 有些代码是只能在用户的操作下才能执行的

```
var button1 = $('#testPopupButton1');
var button2 = $('#testPopupButton2');

function popupTestWindow() {
    window.open('/');
}

button1.click(function () {
    popupTestWindow();
});

button2.click(function () {
    // 不立刻执行popupTestWindow()，100毫秒后执行:
    setTimeout(popupTestWindow, 100);
});
```

button1 and button2 both execute open the new window, but button2 are bidden, because window.open of button2 is excuted by system while the function of button1 is executed by human.