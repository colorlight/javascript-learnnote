##### 1. 方法
定义：在一个对象中绑定函数就说是这个对象的方法。
```
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

```
##### 2.this的解析
this是一个特殊变量，它始终指向当前对象。但是这个当前对象却分了几种情况
* 常规情况:  
  ```
    xiaoming.age(); // 返回正常年龄
  ```
* 特殊情况一：
  ```
  function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
    }

    var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
    };

    xiaoming.age(); // 25, 正常结果
    getAge(); // NaN,调用getAge的是浏览器windows
  ```
  当把函数写在对象外面的时候，如果直接调用函数的话，那么当前的对象被默认为是window，因为this总是指向当前对象的。
  但是xiaoming.age()返回正常，因为当前对象就是xiaoming
  
* 特殊情况二：但是如果你这样写
```
var fn = xiaoming.age;
fn(); // NaN
```
还是返回错误，this还是被当做window，要想保证this指向正确，就必须用xiaoming.age()这种形式。
* 特殊情况三：
```
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); 
```
这样还是不对的，因为，this在嵌套的函数中，又不指向xiaoming了，又指向了window  
**改正方法： 在age的funcition获取this**
```
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // 25
```

##### 3. apply与call
this还是可以控制其指向的。可以用apply方法与call方法，每个函数本身还有一个apply方法和call方法，它可以指定this为小明
```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```
call与apply唯一的区别是apply把参数打包成Array再传入，而call是直接写入参数
```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); 
```

### 4. 装饰器：利用apply() 改变函数的行为
JavaScript 的所有对象都是动态的，即使是内置的函数，也可以重定向，让他变成我们自己定义的函数。  
```
var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};

// 测试:
parseInt('10');
parseInt('20');
parseInt('30');
count; // 3
```
内置函数parseInt被我们重定义了，增加了一个count+=1的模块，然后通过apply方法再调用之前函数，但是我也不知道为什么要加一个apply，直接用oldParseInt好像也可以。     

官方解答  
**用apply你不必关心原函数参数的个数
因为parseInt可以接收1-2个参数**

新增解答  
apply与arguments联系紧密，你不能通过``` return oldPareseInt(arguments);```
arguments只是一个参数的集合，你并不能传入一个参数的集合。所以你只能通过apply方法，apply方法的第二个参数就是要把参数打包成arry再传入，所以arguments正好可以传入，作为apply的第二个参数。