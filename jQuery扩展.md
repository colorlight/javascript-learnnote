##### 1. 什么是扩展
所谓扩展就是 自己编一个默认的jQuery的方法。
```
$.fn.highlight1 = function () {
    // this已绑定为当前jQuery对象:
    this.css('backgroundColor', '#fffceb').css('color', '#d85030');
    return this;
}
```
return this的原因是jQuery对象支持链式调用，这个方法也应该返回一个jQuery对象，就是调用这个方法的本身。
##### 2. 参数处理的技巧
```$.extend(target, obj1, obj2, ...)```
它把多个object对象的属性合并到第一个target对象中
```
// 把默认值和用户传入的options合并到对象{}中并返回:
var opts = $.extend({}, {
    backgroundColor: '#00a8e6',
    color: '#ffffff'
}, options);
```
遇到同名属性，总是使用靠后的对象的值，也就是越往后优先级越高,可以用这个方法方便的设置默认值，又不影响参数的传入
例如
```
var hight = fucntion(options){
    var opt = $.extend({}, {
        backgroundColor:'#00a8e6',
        color:'#fffff'
    }, options);
}
```
hight既可以无参数运行，也可以导入参数。
##### 3. 方便修改默认参数的方法
通过给函数对象增添属性，然后再函数中读取属性的方法。外界可以直接修改函数对象的新增属性
```
$.fn.highlight = function (options) {
    // 合并默认值和用户设定值:
    var opts = $.extend({}, $.fn.highlight.defaults, options);
    this.css('backgroundColor', opts.backgroundColor).css('color', opts.color);
    return this;
}

// 设定默认值:
$.fn.highlight.defaults = {
    color: '#d85030',
    backgroundColor: '#fff8de'
}
```

    外界修改属性
```
$.fn.highlight.defaults.color = '#fff';
$.fn.highlight.defaults.backgroundColor = '#000';
```


#### 4. filter方法的注意
filter是不会过滤DOM节点的子节点的，他只会过滤当前jQuery对象中的元素的

#### 5. a标签
记住a标签可以用来做为超链接的节点
格式
```
<a href="https://baidu.com">baidu</a>
```
#### 6. 打开新的标签的方法
``` window.open(url); ```
url是一个网址。

#### 7. 给特定节点增加方法
利用filter方法，筛选出特定的节点，只对这些节点进行绑定函数，这样的设计模式
```
$.fn.external = function () {
    // return返回的each()返回结果，支持链式调用:
    return this.filter('a').each(function () {
        // 注意: each()内部的回调函数的this绑定为DOM本身!
        var a = $(this);
        var url = a.attr('href');
        if (url && (url.indexOf('http://')===0 || url.indexOf('https://')===0)) {
            a.attr('href', '#0')
             .removeAttr('target')
             .append(' <i class="uk-icon-external-link"></i>')
             .click(function () {
                if(confirm('你确定要前往' + url + '？')) {
                    window.open(url);
                }
            });
        }
    });
}
```
扩展的函数是叫external， 利用filter筛选出合适的节点，然后，利用each函数绑定具体的函数，最后要返回return each的返回值以满足jQuery对象的链式调用，注意each的返回值，就是调用他的元素