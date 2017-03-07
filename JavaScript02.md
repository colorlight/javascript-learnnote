#### 数组
1. 讲解数组的时候讲解了很多数组的方法比较有意思的
 * join ()他会把数组的内容转化成字符串，并用自己制定的字符串连接起来
 ```
 var arr = ['a' , 'b' , 'c'];
 arr.join('-lighting-');//"a-lighting-b-lighting-c"
 ```
 
 #### 对象
1.  键值对组成的
 ```
 var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};

    xiaoming.name;//小明
  ``` 

要想用.访问属性的值，就要求属性名必须是有效变量，就是不能包含特殊字符  '-'  
2. 包含了特殊字符的声明方法
```
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};
```
 访问方法
 xiaohong['middle-school']  
 也可以用xiaohong['name']访问name属性
 