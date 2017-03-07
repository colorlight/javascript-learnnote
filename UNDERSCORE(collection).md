##### 1. Collections指什么：
Array和Object， 并不包含Map和Set
##### 2. _map
```
var obj = var obj = {
    name: 'bob',
    school: 'No.1 middle school',
    address: 'xueyuan road'
};
var upper = _.map(obj, function(value, key){
    return value;
}) // upper 返回的是一个字符串集合。

```
##### 3. _every/_some
当集合的所有元素满足条件_every 返回true，当部分满足条件_some返回true
```_.every([1,3,7,-3,-9], (x) => x > 0); // false```
```_.some([1,4,7,-3,-9], (x) => x > 0); // true```
##### 4. _max/_min
```
正常情况
var arr = [3,5,6,9];
_.max(arr);//9
_.min(arr);//3

特殊情况
_max([]);   // -Infinity
_.min([]);  // Infinity

对于Object 只比较其value，不比较key
_.max({a:1,b:2,c:3}); // 3
```

##### 5. _groubBy
让元素按照自己指定的key分类
```
'use strict';

var scores = [20, 81, 75, 40, 91, 59, 77, 66, 72, 88, 99];
var groups = _.groupBy(scores, function (x) {
    if (x < 60) {
        return 'C';
    } else if (x < 80) {
        return 'B';
    } else {
        return 'A';
    }
});
// 结果:
// {
//   A: [81, 91, 88, 99],
//   B: [75, 77, 66, 72],
//   C: [20, 40, 59]
// }
```

##### 6. shuffle/sample
shuffle用洗牌算法随机打乱一个集合
```
_.shuffle([1, 2, 3, 4, 5, 6]); // [3, 5, 4, 6, 2, 1]
```
sample()  则是随机选择一个或多个元素
```
'use strict';
// 注意每次结果都不一样：
// 随机选1个：
_.sample([1, 2, 3, 4, 5, 6]); // 2
// 随机选3个：
_.sample([1, 2, 3, 4, 5, 6], 3); // [6, 1, 4]
```