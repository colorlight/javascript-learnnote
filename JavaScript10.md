#### 表单提交方式1
```
<!-- HTML -->
<form id="test-form">
    <input type="text" name="test">
    <button type="button" onclick="doSubmitForm()">Submit</button>
</form>

<script>
function doSubmitForm() {
    var form = document.getElementById('test-form');
    // 可以在此修改form的input...
    // 提交form:
    form.submit();
}
</script>
```
应该是button的onclick代表着一次单击所需要执行的操作。
这个操作是自定义的一个函数。可以用来提交，也可以用来做其他的事情。   
==注意script里面是Javascript代码==

#### 表单提交方式2
```
<!-- HTML -->
<form id="test-form" onsubmit="return checkForm()">
    <input type="text" name="test">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var form = document.getElementById('test-form');
    // 可以在此修改form的input...
    // 继续下一步:
    return true;
}
</script>
```
表单自带的提交方式，注意这个button的类型是submit，而方式一的类型是button。 如果是return false则不提交