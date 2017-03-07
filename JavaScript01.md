2016.12.1
1. 可以通过打开F12进入console 运行javascript代码
2. javacript都是写在head中，既可以用文件，也可以写在代码中
3. javascript中声明变量是用var关键字的
4. 要使用 三等号 比较，不要使用"=="，三等号会先判断类型，在判断值。两等号会直接转化至相同类型再比较值。
5. 判断一个值是否等于NaN需要用isNaN()函数，因为NaN和本身进行三等号比较的话是false的。
6. javascript中2/3得到的是1.5而不是取整。
7. 浮点数不要进行相等比较，计算它们之差的绝对值，看是否小于某个阈值
8. 除了null，JavaScript还多了一个undefined，同样的意思。
9. JavaScript的元素可以包含各种类型
10. 对象类型
```
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```
键值对，键是字符串
11. strict 模式， 只允许var声明变量
12. 多行字符串和模板字符串都用``符号
13. 
```
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
```
14.
```
`这是一个
多行
字符串`;
```
15.可以对字符串的操作
```
var s = 'hello world'
s.length ;
s[0]; // 'h'
s.toUpperCase();
```
16. 字符串是不可变的，对其元素赋值是没有效果的
17. indexOf
```
var s = 'hello , world';
s.indexof('world'); // 返回world所在的位置 7
```
18. substring
```
var s  = 'hello, world'
s.substring(0,5); //返回hello
```