## map和parseInt结合

今天看到了一道题

```
["1", "2", "3"].map(parseInt);
```

在控制台输出了一下，结果出乎意料其输出结果是 ``[1, NaN, NaN]``，便研究了下这两个函数

### map

先看下map的函数使用方法：

```
let array = arr.map(function callback(currentValue, index, array) { 
    // Return element for new_array 
}[, thisArg])
```

callback函数接受三个参数，第一个是数组中正在处理的当前的值，第二个是当前值的下标，第三个参数是数组本身。


### parseInt

parseInt(string, radix)函数其接收两个参数，第一个参数为需要被解析的值，第二个参数为进制（介于2到36之间）。比如 ``parseInt(15,10)``就是返回15解析为10进制的数，``parseInt(15,8)``就是返回15解析为8进制的数。


所以，在```["1", "2", "3"].map(parseInt)```中，依次执行了：

```
parseInt("1", 0);  //第二个参数不传或者传0，默认解析为10进制，返回1
parseInt("2", 1);  //第二个参数不在2~36之间，返回NaN
parseInt("3", 2);  //3无法解析为2进制，返回NaN
```

所以结果输出为``[1, NaN, NaN]``

再看下这个数组的执行结果：

```
["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16"].map(parseInt)

//输出：[1, NaN, NaN, NaN, NaN, NaN, NaN, NaN, NaN, 9, 11, 13, 15, 17, 19, 21]
```

前面一堆输出NaN，到了下标为9到后面剩余几个值，执行的是
```
parseInt("10", 9)   // 1*9+0=9
parseInt("11", 10)   // 1*10+1=11
parseInt("12", 11)   // 1*11+2=13
.
.
.

```

从这中间可以总结出一点点规律，就是在parseInt函数中，**第一个参数里面每一位数的值都需要小于等于其需要转的进制**。

比如parseInt("9", 8)，9是没法转成8进制的，但是在parseInt("10", 9)中，十位数是1，个位数是0，都小于9，转成9进制的值是9。同理，在parseInt("16", 15)中，16的十位数是1，个位数是6，那转成15进制则是1*15+6=21。


### 参考资料

* [MDN web docs](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)