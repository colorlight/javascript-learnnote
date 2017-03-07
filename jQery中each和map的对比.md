##### 1. 最重要的一点
 * 如果你不想利用返回值，就用用each，因为each不返回值.
 * map是一定会返回一个新的值，只会返回调用他的==对象本身==
 * .map和.each都有两种应用场景
    1. 对单纯的数组应用，那么直接应用
        1. （map返回值）
    ```$.each([1,2,3,4],callback); ```
    ```$.map([1,2,3,4],callback); //返回值也是单纯的数组```
注意即使你map中的callback没有return，你仍然会有一个返回值，这个是个空的array
        2. （this指向  ）
        在单纯的数组应用中，each与map有明显的不同，each的this指向的是数组中当前处理的元素，而map的指向的是Window
    2. 单纯的数组应用的另一种形式  
    （this指向）： ```$([1,2,3]).each(function)``` 这个哥里面的this还是指向元素，而map里面的this指向也不在指向window了，而是指向数组的元素。
    3. 对jQuery对象应用（this的指向）  
    如果是在对jQuery对象应用map和each 如``` $('li').map()```, 这个时候的this都是指向当前的DOM节点的，并不是jQuery元素。


##### 2. 回调函数参数格式
```
map(arr, function(elem, index) {});
// versus 
each(arr, function(index, elem) {});
```
map的callback的第一个参数是元素，第二个才是索引。
each的callback第一个参数是索引，第二个才是元素

##### 3. callback中 的this指向问题
    参见1中的解释

 