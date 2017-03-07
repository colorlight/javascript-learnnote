###### 1. innerHTML 和innerText
```
<!-- HTML结构 -->
<ul id="test-ul">
    <li class="js">JavaScript</li>
    <li name="book">Java &amp; JavaScript</li>
</ul>
```
jQuery代码
```
var a = $('#test-ul');
var b = a[0]; 
b.innerHTML     //<li name="book">Java &amp; JavaScript</li>
b.innerText     //JavaScript  Java & JavaScirpt
```


##### 2. HTML中的转以符号
< > & 和 " 是保留的，要想显示他们需要用到& character;这个语法

```
Character	Entity	Note
&	&amp;	Character indicates the beginning of an entity.
<	&lt;	Character indicates the beginning of a tag
>	&gt;	Character indicates the ending of a tag
"	&quot;	Character indicates the beginning and end of an attribute value.
```

##### 3. jQuery中对DOM的读取与修改
还是上面的HTML结构
读取DOM节点
```
$('#test-ul li[name=book]').text(); //得到的是Java & JavaScript

$('test-ul li[name=book]').html(); //得到的是'Java &amp; JavaScript'
```

修改DOM节点
```
$('#test-ul li[name=book]').text('fsfsadfsf'); 那么Java & JavaScript就修改为fsfsadfsf
```

##### 4. 注意jQuery.html()就是innerHTML的实现
html（）有一个特点就是，他返回原始的文本，但是不包含当前标记的符号，只返回当前标记所包裹的内容
```
<ul id="test-ul">
    <li class="js"><span style="color: red">JavaScr23123123ipt</span></li>
    <li name="book">JavaScript &gt; ECMAScript</li>
</ul>
```

$(#test-ul li.js).html返回的是li标签所包裹的内容是
```
<span style="color: red">JavaScr23123123ipt</span>
```

##### 5. 一个操作，作用在所有的DOM节点上

```
$('#test-ul li').text('JS'); //两个节点都会变为JS
```

##### 6. 修改CSS
利用jQuery对象的.css()方法可以对节点进行批量的css格式修改  
有html结构如下
```
<!-- HTML结构 -->
<ul id="test-css">
    <li class="lang dy"><span>JavaScript</span></li>
    <li class="lang"><span>Java</span></li>
    <li class="lang dy"><span>Python</span></li>
    <li class="lang"><span>Swift</span></li>
    <li class="lang dy"><span>Scheme</span></li>
</ul>
```

让所有的动态语言都高亮
```
$('#test-css li.dy>span').css('background-color', '#ffd351');
```
注意span这个标记，好像是只包含文字的覆盖范围这个意思。

##### 7. 修改class属性的方法
```
var div = $('#test-div');
div.hasClass('highlight');
div.addClass('highlight');
div.removeClass('highlight');
```

##### 8. 显示和隐藏DOM
要隐藏一个DOM，可以将其css的display属性更改为none，但是要恢复的话，你要知道他之前是什么属性，所以你在更改之前还要记下来。  
但是jQuery通过hide和show方法，可以方便的显示和隐藏DOM

##### 9. jQuery对象与DOM节点的区别
```
var a  = $('#test-id');
a[0] // DOM节点
a.first();  //代表着a这个jQuery对象中的第一个jQuery对象
a.slice(0,1); // 也代表着第一个jQuery对象
a[0]; //这是个DOM节点，所以他不能用jQuery的各种方法。
```

##### 10. 判断radio和select有没有被选择上的方法
```
var radio = $('#test-radio');
radio.attr('checked');   // 'checked'

radio.prop('checked');   // true
radio.is(':checked');    // true

var select = $('#test-select');
select.is(':selected');
```


##### 11.  修改节点的value
用.val()方法。