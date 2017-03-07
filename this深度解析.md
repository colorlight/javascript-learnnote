只需记住什么情况下 this不指向window

xiaoming.age();

age函数里面有this的话，this就是xiaoming，但是如果age里面还有函数的话，内部函数的this就不是指向小明了

```
var t = xiaoming.age;
t(); // 这样也是不行
```
