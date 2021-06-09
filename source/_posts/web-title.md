---
title: 前端记录
date: 2021-06-03 09:27:19
tags:
---
<meta name="referrer" content="no-referrer"/>
<!--more-->

#### 1. 数组扁平化
  题目描述:实现一个方法使多维数组变成一维数组
  递归版本：
    ```
    function flatter(arr) {
      if (!arr.length) return;
        return arr.reduce(
          (pre, cur) =>
            Array.isArray(cur) ? [...pre, ...flatter(cur)] : [...pre, cur],
          []
        );
      }
    console.log(flatter([1, 2, [1, [2, 3, [4, 5, [6]]]]]));
    ```
    迭代思路实现：
    
    ```
    function flatter(arr) {
      if (!arr.length) return;
        while (arr.some((item) => Array.isArray(item))) {
          arr = [].concat(...arr);
      }
      return arr;
    }
    console.log(flatter([1, 2, [1, [2, 3, [4, 5, [6]]]]]));
    ```

#### 2. bind、call、apply 改变this指向 三者的区别

  三种方法都可以用来改变函数this对象指向的，第一个参数都是this要指向的对象，后面的参数进行传参
  demo:
    ```
     var xw = {
      name : "小王",
      gender : "男",
      age : 24,
      say : function() {
              alert(this.name + " , " + this.gender + " ,今年" + this.age);                                
      }
    }

    var xh = {
      name : "小红",
      gender : "女",
      age : 18
    }
    xw.say();
    ```



  改变this指向调用say函数 输出小红的内容

  1. bind方法
    由于bind方法返回的是一个函数，所以在执行时需要再进行调用
    `xw.say.bind(xh, '传递的参数')()`
    
  2. call方法
    `xw.say.call(xh, '传递的参数')`

  3. apply方法
    该方法第二个参数是一个数组，在传递时 使用数组传递
    `xw.say.apply(xh, [传递的参数])`


  ##### 区别
    三个方法都是用来改变this指向的
    bind方法返回的是一个函数，需要进行执行来调用 传递参数和call方法一样
    call方法和apply方法都是对函数直接进行调用 传递参数不同 apply方法传递的参数是一个数组形式


  #### 3. js实现大文件分片上传的方法
    借助js的Blob对象FormData对象可以实现大文件分片上传的功能，关于Blob和FormData的具体使用方法可以到如下地址去查看
    [FormData 对象的使用](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects)
    [Blob 对象的使用](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)
    
    以下是实现代码，本例中后端代码使用php来实现，只是演示基本功能，具体一些文件验证逻辑先忽略。
    前端代码：
    
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>upload</title>
        <script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
    </head>
    <body>
      <input type="file" name="file" id="file">
      <button id="upload" onClick="upload()">upload</button>
        <script type="text/javascript">
            var bytesPerPiece = 1024 * 1024; // 每个文件切片大小定为1MB .
            var totalPieces;
            //发送请求
            function upload() {
                var blob = document.getElementById("file").files[0];
                var start = 0;
                var end;
                var index = 0;
                var filesize = blob.size;
                var filename = blob.name;

                //计算文件切片总数
                totalPieces = Math.ceil(filesize / bytesPerPiece);
                while(start < filesize) {
                    end = start + bytesPerPiece;
                    if(end > filesize) {
                        end = filesize;
                    }

                    var chunk = blob.slice(start,end);//切割文件    
                    var sliceIndex= blob.name + index;
                    var formData = new FormData();
                    formData.append("file", chunk, filename);
                    $.ajax({
                        url: 'http://localhost:9999/test.php',
                        type: 'POST',
                        cache: false,
                        data: formData,
                        processData: false,
                        contentType: false,
                    }).done(function(res){ 

                    }).fail(function(res) {

                    });
                    start = end;
                    index++;
                }
            }
        </script>
    </body>
    </html>
    ```
  后端php代码：
  ```
  <?php

  header('Access-Control-Allow-Origin:*');
  header("Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept");

  $file = $_FILES['file'];
  $filename = $file['name'];
  file_put_contents($filename, file_get_contents($file['tmp_name']), FILE_APPEND);
  ```
