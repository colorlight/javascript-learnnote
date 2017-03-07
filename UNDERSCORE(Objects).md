#### 1. keys /allKeys
keys 返回一个对象里面的所有key，但是并不包含他继承的属性。
```
'use strict''
function Student(name, age){
    this.name = name;
    this.age = age;
}

var xiaoming = new Student('小明', 20);
_.keys(xiaoming); //  ['name', 'age']
```

allKeys 返回所有的key包括继承的key
```
'use strict';

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype.school = 'No.1 Middle School';
var xiaoming = new Student('小明', 20);
_.allKeys(xiaoming); // ['name', 'age', 'school']
```

#### 2. values
返回对象的所有值，不包括继承的属性的值
```
'use strict'
var obj = {
    name: '小明',
    age: 20
}

_. values(obj);  // ['小明'， 20]
```
注意没有allValues

#### 3. mapObject
针对object的map版本
```
'use strict'
var obj = {a: 1, b:2, c:3};
_.mapObject(obj, (v,k) => 100 + v);
// {a:101, b :102, c: 103};
```

#### 4. invert
交换key与value
```
'use strict';
 var obj={
    Adam: 90,
    Lisa: 85,
    Bart: 59
 };
 
 _.invert(obj); // {'59':'Bart', '85'：'Lisa', '90':'Adam'}
 注意这个是字符串的属性值
```

#### 5. extend/ extendOwn

```
'use strict';

var a = {name:'Bob', age: 20};
_.extend(a, {age:15, {age:88, city:'Beijing'}}); 
//{name:'Bob', age:88, city:'Beijing'}
注意a的值也改变了
```
后面的值会覆盖掉前面的值

extendOwn()与extend类似，但是不会获取原型继承下来的值。

#### 6. clone
```
var a = {name:'lighting', age: 15};
var c = _.clone(a);

```
但是注意clone是潜复制，如果属性的值是一个对象，那么那么两个属性是指向同一个对象的。
```
'use strict';
var source = {
    name: 'xiaoming',
    age : 20
    skills:['JavaScript', 'CSS', 'HTML']
}
var copied = _.clone(source);
```
copied.skills与 source.skills指向的是一个相同的对象，改变其中一个，另一个也会被改变的。


#### 7. isEqueal
比较两个对象内容的方法。
```
'use strict';
var o1 = [1,2,3,4];
var o2 = [1,2,3,4];
_.isEqual(o1,o2); // true
```