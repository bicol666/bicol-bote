# Node.js笔记

## 一、Node.js简介

> 官方解释：Node.js是一个基于Chrome V8引擎的JavaScript运行环境；

简单来说：Node.js就是一个JavaScript代码运行的环境；

--/ 注意

- 浏览器是JavaScript的前端运行环境；
- Node.js是JavaScript的后端运行环境；
- Node.js中无法调用DOM、BOM等浏览器内置的API；



--/ 学习路径对比

- 前端（浏览器）JAvaScript学习路径

  JavaScript基础语法 + 浏览器内置API（DOM、BOM、ajax……）+第三方库（jQuery……）；

- 后端（Node.js）JavaScript学习路径

  JAvaScript基础语法 + Node.js内置API（fs、path、http……）+第三模块（express、mysql……）；



## 二、Node.js的安装

访问[官网地址](https://nodejs.org/zh-cn/)下载安装即可；



## 三、Node.js常用命令

### 1、查看版本信息

--/ 命令

```shell
node -v
```



### 2、执行JS文件(代码)

--/ 命令

```shell
node 要执行的JS文件路径
```



--/ 示例

有个文件`test.js`，文件中有一行简单的代码；

![](E:\Nanum-Note\Web前端\Node.js\img\QQ截图20220103181652.png)

---/ test.js

<img src="E:\Nanum-Note\Web前端\Node.js\img\QQ截图20220103181752.png" style="zoom:150%;" />

---/ 终端运行结果

![](E:\Nanum-Note\Web前端\Node.js\img\QQ截图20220103182131.png)



## 四、Node.js内置模块（API）

### 1、导入模块

在使用具体的模块时，需要导入模块才能使用；

--/ 语法格式

```js
const m = require( '模块名' );
```



### 2、文件系统模块 (fs)

文件系统模块(fs)是Node.js官方提供的、专门用来操作文件的模块。



#### 2.1 读取指定文件中的内容

--/ 语法格式

```js
const fs = require('fs');
fs.readFile( 文件路径 , 编码格式 , 文件读取结束后的回调 );
```

---/ 说明

在回调当中我们可以添加参数`error`参数接收错误信息，添加参数`result`接收读取到的结果；



--/ 示例

将文件：`test.txt`中的内容读取并输出到控制台；

```js
// ①导入fs模块；
const fs = require('fs');
// ②调用readFile()函数读取文件；
//	__dirname 表示的是当前文件所处目录的路径；
fs.readFile(__dirname + '/test.txt','utf-8',function (error,result) {
    console.log('错误信息：'+error);
    console.log('读取结果：'+result);
});
```

---/ test.txt

```
这是一段测试文本；
```

---/ 运行结果

```shell
 E:\Web-WorkSpace\node-study> node test.js
错误信息：null
读取结果：这是一段测试文本；
```



--/ 判断文件是否读取成功

在函数readFile( …… , …… , callback )中，第三个参数是回调函数，此回调函数当中我们就可以拿到读取的结果以及失败信息；

```js
// 导入fs模块；
const fs = require('fs');
// 调用readFile()函数读取文件；
fs.readFile(__dirname + '/test.txt','utf-8',function (error,data) {
    if(error){
        console.log('文件读取失败；');
        return;
    }
    console.log('文件读取成功；');
});
```

当文件读取成功的时候，error是null；

当读取失败的时候，error是一个对象，里面封装了错误信息，data则为undefined；

根据error是否为null，我们就可以判断文件是否读取成功；



#### 2.2 向指定文件当中写入内容

--/ 语法格式

```js
const fs = require('fs');
fs.writeFile( 文件路径,要写入的内容,编码格式回调);
```

---/ 说明

在回调中可以添加参数`error`接收文件写入失败的信息；

```js
function(error){
    console.log(error.msg);
}
```



--/ 示例

```js
// 导入fs模块；
const fs = require('fs');
// 调用readFile()函数读取文件；
// 注意：该函数会把文件中的内容清空，然后再写入内容；
fs.writeFile(__dirname + '/test.txt','测试写入函数','utf8',function (error) {
    if(error){
        console.log('文件写入失败，'+error.msg);
        return;
       }
  	console.log('文件写入成功');
});
```





### 3、路径模块（path）

路径模块(path)是Node.js官方提供的用来处理路径的模块；



#### 3.1、路径拼接

使用函数path.join()可以把多个路径判断拼接为一个完整的路径；



--/ 语法格式

```js
path.join( 路径片段1 , 路径片段2 ， 路径片段3 );
```

---/ 说明

传入的参数是路径片段(string)，返回类型是string；



#### 3.2、获取路径中的文件名

假设有这样一个路径：D:test/test.txt，当我们需要获取这个路径中的文件名`test.txt`时，我们可以使用函数path.basename()来获取；

--/ 语法格式

```js
let file = path.basename(文件路径);
```

--/ 示例

```js
//导入path模块
const path =  require('path');
//路径
const fpath = 'D:test/test.txt';
//获取文件的名字（包含扩展名）
let file = path.basename(fpath)//-->test.txt
//获取文件的名字（不包含扩展名）
let file2 = path.basename(fpath , '.txt');//-->test
```



#### 3.3、获取路径中的文件扩展名

--/ 语法格式

```js
let extName = path.extname(文件路径);
```



--/ 示例

```js
//导入path模块
const path =  require('path');
//路径
const fpath = 'D:test/test.txt';
//获取文件的名字（包含扩展名）
let extName = path.extname(fpath);//-->.txt
```





### 4、http模块



