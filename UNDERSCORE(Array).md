##### 1. first/last
```
'use strict';
var arr = [2,4,6,8];
_.first(arr); //2
_.last(arr);  //8
```

##### 2. flatten
接受一个array，无论这个array里面嵌套了多少array， flatten最后douban他们变成一个array
```
'use strict'
_.flatten([1, [2], [3, [[4], [5]]]]);
```

##### 3. zip/unzip

```
var names = ['Adam', 'Lisa', 'Bart'];
var scores = [85,92,59];
_.zip(names, scores);
//[['Adam', 85], ['Lisa', 92], ['Bart', 59]]
```

unzip() 是反过来
```
'use strict';
var namesAndScores = [['Adam', 85], ['Lisa', 92], ['Bart',59]];
_.unzip(namesAndScores);
// [['Adam','Lisa', 'Bart'], [85,92,59]]
```

##### 4. object
吧两个数组组合成一个键值对
```
'use strict'
var names = ['Adam', 'Lisa', 'Bart'];
var scors = [85,92,59];
_.object(names, scores);
// {Adam:85, Lisa:92, Bart:59}
```
##### 5. range
range() 然呢快速生成一个序列
```
'use strict';

_.range(10); //[0,1,2,3,5,6,78,9]
_.range(1,11);//[1,2,3,4,5,6,7,9,10]
_.range(0,30,5); //[0,5,10,15,20,25]
_.range(0,-10,-1); //[0,-1,-2,-3,-4,-5,-6,-7,-8,-9]
```