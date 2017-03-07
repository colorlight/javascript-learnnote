### JQuery

##### 1. $符号
jQuery把所有功能封装在了一个叫做==jQuery==的变量中，而$是变量jQuery的别名。
jQuery是一个函数。
##### 2. jQuery的核心是选择器
* **按ID查找 ：**
    ```
    var div = $('#abc'); // abc是id名
    ```
* **按tag查找**
    ```
    var ps = $('p');//返回所有的<p>节点
    ```
* **按class查找**
    ```
    var a = $('.red');
    //所有包含的class =
    //“red”的节点都会返回
     ```
     同时包含多个class的节点
     ```
     var a = $('.red.green');
     //符合条件的节点
     // <div class="red green">...</div>
    // <div class="blue green red">...</div>
     ```
* **按属性查找**
    ```
    var email = $('[name=email]');
    ```
    属性中的前缀后缀查找
    ```
    //前缀
    var icons = $('[name^=icon]');
    //匹配的元素name = "icon-1"
    
    //后缀
    var names = $('[name$=with]');
    //匹配的元素 name="startswith"
    ```
* **组合查找**
    ```
    交集查找
    var emailInput = $('input[name=email]');
    //tag名和属性名交集
    
    var tr = $('tr.red');
    //tag名和class交集
    ```
    
    ```
    并集查找
    
    var c = $('p,div');
    var d = $('p.red,p.green');
    //利用逗号隔开
    ```
##### 3. 层级选择器
    ```
    var a = $('ul.lang li.lang-javascript');
    //祖先节点是ul.lang
    //ul是tag名 lang是class名
    //空格后面是后代节点
    //li是tag名
    //lang-javascript是class名
    ```

**多层级选择**
```
$('form.test p input'); // 在form表单选择被<p>包含的<input>
```

**子选择器**
```
$('ul.lang>li.lang-javascript'); 必须是父子关系
```

##### 4. filter
```
$('ul.lang li:first-child');
$('ul.lang li:nth-child(2)');
//选出第2个child
```

##### 5. 针对表单的选择器
```
$(':button');
//选取所有 type="button" 的 <input> 元素 和 <button> 元素

$('input:button');
//选择input的button

$('input:file');
//效果和 input[type=file]一样

$('input:checkbox');选择复选框

$('input:radio'); // 选择单选框

$('input：focus'); //选择当前输入焦点的元素
//例如把光标放到一个<input>上，用$('input:focus')就可以选出

$('input:enabled');
//可以选择输入正常的input

$('input:disabled');
//可以选择不能输入的表单

```

##### 6. 其他选择器
```
$('div:visible'); // 所有可见的div
$('div:hidden'); //所有隐藏的div
```

##### 7. find()查找
jQuery对象的方法find，可以在其==子节点==中进行查找。
```
<!-- HTML结构 -->
<ul class="lang">
    <li class="js dy">JavaScript</li>
    <li class="dy">Python</li>
    <li id="swift">Swift</li>
    <li class="dy">Scheme</li>
    <li name="haskell">Haskell</li>
</ul>
```
用find()查找：
```
var ul = $('ul.lang'); // 获得<ul>
var dy = ul.find('.dy'); // 获得JavaScript, Python, Scheme
var swf = ul.find('#swift'); // 获得Swift
var hsk = ul.find('[name=haskell]'); // 获得Haskell
```

##### 8. parent查找
与find类似，但是返回的是父亲节点
```
var swf =  $('#swift');   // 获得Swift
var parent = swf.parent(); //获得ul
var a = swf.parent('div.red'); //// 从Swift的父节点开始向上查找，直到找到某个符合条件的节点并返回
```


##### 9. 同级之间的查找next() prev()
```
var swift = $('#swift');
swift.next(); // Scheme
swift.next('[name=haskell]'); // haskell,还具有选择性

swift.prev(); // Python
swift.prev('.js'); //Javascript
```

##### 10. 过滤filter()

filter方法可以过滤掉不符合选择器条件的节点。
```
var langs = $('ul.lang li');   // 假设拿到了JavaScript python swift scheme
var a = langs.filter('.dy'); // 拿到JavaScript python scheme
```
这个方法和find一样，==这是我后来加的内容，并不一样，一个很大的区别就是filter过滤的并不是子节点，就是过滤的当前节点的元素==  
但是filter可以传入函数
```
var langs = $('ul.lang li'); // 拿到了所有的子节点

langs.filter(function(){
    return this.innerHTML.indexOf('s') ===0;
}); //拿到了Swift和Scheme
```
==注意this被绑定为DOM对象，而不是JQuery对象==


##### 11. map方法
map方法把一个jQuery对象包含的若干DOM节点转化为其他对象
```
var langs = $('ul.lang li');
var arr = langs.map(function(){
    return this.innerHTML;
});
```
http://api.jquery.com/map/
这是map的官方说明，他返回的仍然是一个jQuery对象，如果想得到一个数组，就用.get()方法。
```
arr.get(); //得到：['JavaScript', 'Python', 'Swift', 'Scheme', 'Haskell']
```

##### 12. 类似数组的方法

```
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell

var js = langs.first(); //拿到JavaScript

var haskell = langs.last(); // 拿到Haskell

var sub  = langs.slice(2, 4)  
```