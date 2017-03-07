##### 1. JSON是什么
* JSON是一种数据交换格式，他是一种纯文本格式。XML也是一种数据交换格式，所以他也是一种纯文本格式。  
* 什么叫做数据交换格式：交换主要是指在网络上进行传递对象，比如A发送一个xiaoming对象给B，B得到了xiaoming对象。但是网络只能传递文本，如何将文本抽象成对象，如何将对象用文本实现，就是JSON所做的事情。
* 对象-->toJSONString->文本， 文本-->toObject->对象。
##### 2. JSON的特点
JSON是一种超轻量级的数据交换格式。他只支持javscript的数据格式。比如number boolean string null array object
* 特别注意：为了统一解析JSON字符串规定必须用""
* 
    ```
    '{"name":"小明","age":14,"gender":true,"height":1.65,"grade":null,"middle-school":"\"W3C\" Middle School","skills":["JavaScript","Java","Python","Lisp"]}'
    ```
* 以上是对象的JSON字符串的格式