# AJAX

* AJAX: Asynchronous JavaScript and XML, 异步JavaScrip和XML
    - JavaScript中有一个XMLHttpRequest对象, 可以向服务器发送异步请求. 利用XML格式传递数据

## XMLHttpRequest

* `XMLHttpRequest`对象
    - 属性:
        - `onreadystatechange`: 当XMLHttpRequest的readyState属性改变时触发一个函数
        - `readyState`: 返回当前请求的状态
            - 值:
                - `0`: 未初始化. 对象已建立, 但尚未初始化(没有调用open方法)
                - `1`: 初始化. 对象已建立, 尚未调用send方法
                - `2`: 发送数据. send方法已调用, 但当前的状态及http头未知
                - `3`: 数据传送中. 已接收部分数据, 因为响应及http头不全, 这时通过responseBody和responseText获取部分数据会出现错误
                - `4`: 完成. 数据接收完毕, 此时可以通过responseBody和responseText获取完整的响应数据
        - `responseBody`: 响应信息的unsigned byte数组
        - `responseText`: 响应信息的XML Document对象
        - `status`: 状态码
        - `statusText`: 状态码对应信息
    - 方法:
        - `open(请求方式, 请求路径, 是否异步请求)`: 打开请求连接
        - `send(请求参数)`: 设置请求参数并发送请求
        - `setRequestHeader(头键, 头值)`: 设置请求头


```javascript
// 不同浏览器中创建XMLHttpRequest对象
function createXmlHttpRequest() {
    var xhr;
    // Firefox, Opera 8+, Safari
    try {
        xhr = new XMLHttpRequest();
    } catch (e) {
        // IE
        try {
            xhr = new ActiveXObject("Msxm12.XMLHTTP");
        } catch (e) {
            try {
                xhr = new ActiveXObject("Microsoft.XMLHTTP");
            } catch (e) {}
        }

    }
    return xhr;
}

// 发送GET请求, 参数可以拼接在url中
function get(url, callback) {
    // 创建XMLHttpRequest对象
    var xhr = createXmlHttpRequest();
    // 设置状态改变监听回调函数
    xhr.onreadystatechange = function() {
        // 判断readyState状态
        if (xhr.readyState == 4 && xhr.status == 200) {
            // 请求成功, 响应成功
            var data = xhr.responseText;
            callback(data);
        }
    }
    // 设置为异步GET请求
    xhr.open("GET", url, true);
    // 发送请求, 没有请求体参数
    xhr.send(null);
}

// 发送POST请求, 参数是键值对字符串, 用&连接
function post(url, param, callback) {
    // 创建XMLHttpRequest对象
    var xhr = createXmlHttpRequest();
    // 设置状态改变监听回调函数
    xhr.onreadystatechange = function() {
        // 判断readyState状态
        if (xhr.readyState == 4 && xhr.status == 200) {
            // 请求成功, 响应成功
            var data = xhr.responseText;
            callback(data);
        }
    }
    // 设置为异步GET请求
    xhr.open("POST", url, true);
    // 设置请求头
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    // 发送请求
    xhr.send(param);
}
```


## jQuery中的AJAX

* jQuery简化了AJAX的操作
* `$(selector).load(url, data, callback)`: 加载远程HTML代码并插入DOM中. 默认使用GET方式, 传递参数后自动转为POST方式
    - `url`: 地址
    - `data`: 与请求一同发送的查询字符串键值对集合
    - `callback`: 方法完成后所执行的函数
        - `responseTxt`: 包含调用成功时的结果内容
        - `statusTxt`: 包含调用的状态
        - `xhr`: XMLHttpRequest对象
* `$.get(url, callback)`: 异步发送GET请求
* `$.post(url, data, callback)`: 异步发送POST请求
* `$.getJSON(url, data, successCallback(data, status, xhr))`


```javascript
// load
$("button").click(function(){
  $("#div1").load("demo_test.txt",function(responseTxt,statusTxt,xhr){
    if(statusTxt=="success")
      alert("外部内容加载成功!");
    if(statusTxt=="error")
      alert("Error: "+xhr.status+": "+xhr.statusText);
  });
});

// get
$("button").click(function(){
  $.get("demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
  });
});

// post
$("button").click(function(){
    $.post("/try/ajax/demo_test_post.php",
    {
        name:"菜鸟教程",
        url:"http://www.runoob.com"
    },
        function(data,status){
        alert("数据: \n" + data + "\n状态: " + status);
    });
});

// getJSON
$(document).ready(function(){
    $("button").click(function(){
        $.getJSON("demo_ajax_json.js",function(result){
            $.each(result, function(i, field){
                $("div").append(field + " ");
            });
        });
    });
});
```


## 功能实现

### 异步校验用户名或密码

* 步骤:
    - 用户名输入框在blur事件时, 获取输入框的用户名
    - 利用AJAX发送异步请求获取用户名是否可用
    - 在表单上显示用户名是否被占用

### 输入框搜索联想

* 步骤:
    - 搜索框在keyup事件时, 获取输入框的内容
    - 利用AJAX发送异步请求获取相关结果
    - 在搜索框下显示结果列表, 当搜索框内容清空后, 将结果列表隐藏
