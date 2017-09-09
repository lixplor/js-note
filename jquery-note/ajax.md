# AJAX

* AJAX: Asynchronous JavaScript and XML, 异步JavaScrip和XML
    - JavaScript中有一个XMLHttpRequest对象, 可以向服务器发送异步请求. 利用XML格式传递数据




## jQuery中的AJAX

* jQuery简化了AJAX的操作

方法:
* `$(selector).load(url, data, callback)`
    - `url`: 地址
    - `data`: 与请求一同发送的查询字符串键值对集合
    - `callback`: 方法完成后所执行的函数
        - `responseTxt`: 包含调用成功时的结果内容
        - `statusTxt`: 包含调用的状态
        - `xhr`: XMLHttpRequest对象
* `$.get(url, callback)
* `$.post(url, data, callback)`
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
