# AJAX

作用：异步请求、局部刷新；

## 原生AJAX请求

基本使用

```js
//原生AJAX发送请求的基本步骤(GET)；
//第一步：创建xhr（XMLHttpRequest）对象；
var xhr = new XMLHttpRequest();
//第二步：和服务器建立连接；
xhr.open(string:请求方式，string:URL地址,boolean:是否异步,……);
//第三步：执行回调函数；
xhr.ready
//第四步：发送数据；
xhr.send(null);
```

