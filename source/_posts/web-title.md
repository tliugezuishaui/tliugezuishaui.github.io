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
