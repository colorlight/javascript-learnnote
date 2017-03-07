##### 1. 题目
```
<form id="test-form" action="#0" onsubmit="return false;">
    <p><label>Name: <input name="name"></label></p>
    <p><label>Email: <input name="email"></label></p>
    <p><label>Password: <input name="password" type="password"></label></p>
    <p>Gender: <label><input name="gender" type="radio" value="m" checked> Male</label> <label><input name="gender" type="radio" value="f"> Female</label></p>
    <p><label>City: <select name="city">
        <option value="BJ" selected>Beijing</option>
        <option value="SH">Shanghai</option>
        <option value="CD">Chengdu</option>
        <option value="XM">Xiamen</option>
    </select></label></p>
    <p><button type="submit">Submit</button></p>
</form>
```

在这个表单中选出所有的输入框，radio要求是已经被选择的,
```
var json= {};
var arr=$('#test-form p label').find('input:not(:radio),input:checked,select');
for(var i=0;i<arr.length;i++) {
 console.log(arr[i].name)
 json[arr[i].name] = arr[i].value

}
 json = JSON.stringify(json)
```

学习下这个代码的:not()的选择器的用法，与已被选择的radio是用input:checked，以及貌似返回的是一个数组，元素是jQuery，以及.name，arr[0]应该和arr.get(0)是一样的效果吧   
jQuery对象类似数组，但他不是数组。数组没有get方法，get方法与用索引的效果是一样的。