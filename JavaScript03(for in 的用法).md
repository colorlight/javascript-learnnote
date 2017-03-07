1. 
```
for...in 可以把一个对象的所有
属性依次循环出来
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    alert(key); // 'name', 'age', 'city'
}


还可以把数组的索引循环出来
var a = ['A', 'B', 'C'];
for (var i in a) {
    alert(i); // '0', '1', '2'
    alert(a[i]); // 'A', 'B', 'C'
}
```
2. Map
    * 一组键值对的结构，具有极快的查找速度
    * 用法
    ```
    var m = new Map([['Michael', 95], ['Bob', 75],['Tracy', 85]]);
    m.get('Michael'); // 95
    m.set('Adam', 67);
    m.delete('Adam'); // 删除key 'Adam'
    ```
3. Set    
    * Map类似，也是一组key的集合，但不存储value  
    * 用法
    ```
    var s1 = new Set(); // 空Set
    var s2 = new Set([1, 2, 3])
    var s = new Set([1, 2, 3, 3, '3']);
    s; // Set {1, 2, 3, "3"}
    s.add(4);
    s.delete(3);
    ```
4. iterable
    * array可以通过下标索引，但是set与map不能通过下标索引。所以引入了iterable类型。
    * 具有iterable类型的集合可以通过新的for ... of循环来遍历。
    * 用法
    ```
    var a = ['A', 'B', 'C'];
    var s = new Set(['A', 'B', 'C']);
    var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
    for (var x of a) { // 遍历Array
        alert(x);
    }
    for (var x of s) { // 遍历Set
        alert(x);
    }
    for (var x of m) { // 遍历Map
        alert(x[0] + '=' + x[1]);
    }

    ```
    * 另一种遍历的方法 用foreach方法
    * 用法
    ```
    /*****数组： ******/
    var a = ['A', 'B', 'C'];
    a.forEach(function (element, index, array){

    alert(element);
    });
    就是在这种格式，index是下标，array就是a本身
    
    /*******Set：******/
    var s = new Set(['A', 'B', 'C']);
    s.forEach(function (element, sameElement, set) {
    alert(element);
    });
    因为set没有下标，所以前两参数一样
    
    // Map：
    var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
    m.forEach(function (value, key, map) {
    alert(value);
    });
    ```