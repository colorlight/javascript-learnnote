##### 1. arguments仅在当前函数里有效
```
function thunkifyd(){
	(function(){
		for(var i of arguments){
			console.log(i);
		}
	})();
}

thunkifyd(1,23,23,4,5);// undefined
```
并没有什么输出结果，因为consolelog的arguments是第二个function里面的function，而你传给第二个function的参数是没有的，所以不会输出内容的。

```
function thunkifyd(){
	(function(){
		for(var i of arguments){
			console.log(i);
		}
	})('sdfsdf',1,23,2,3,2,3);
}
thunkifyd();
```
此时会consolelog出各种参数，因为第二个function传入了很多参数值。
