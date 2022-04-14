### 查找数组元素位置
>indexOf()方法
function indexOf(arr, item) {
  if (Array.prototype.indexOf){   //判断当前浏览器是否支持
      return arr.indexOf(item);
  } else {
      for (var i = 0; i < arr.length; i++){
          if (arr[i] === item){
              return i;
          }
      }
  }     
  return -1;     //总是把return -1暴漏在最外层
}

### 数组求和
>函数式编程 map-reduce：
function sum(arr) {
    return arr.reduce(function(prev, curr, idx, arr){
        return prev + curr;
    });
}

**如何累计呢：**

**[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)这个感觉特别像递归**

**累加**
    var arr = [1, 3, 5, 7, 9];
    arr.reduce(function (x, y) {
        return x + y;
    });
**累乘**
    'use strict';

    function product(arr) {
    　　　var result = arr.reduce(function(x,y){
    　　　　return x*y;
    　　});

    　　return result;

    }

>递归
    function sum(arr) {
        var len = arr.length;
        if(len == 0){
            return 0;
        } else if (len == 1){
            return arr[0];
        } else {
            return arr[0] + sum(arr.slice(1));
        }
    }

### 移除数组中的元素
>Arra y.prototype.filter()
    function remove(arr,item){
        return arr.filter(function(ele){
            return ele != item;
        })
    }
>splice()
    function remove(arr,item){
        var newarr = arr.slice(0);
        for(var i=0;i<newarr.length;i++){
            if(newarr[i] == item){
                newarr.splice(i,1);
                i--;
            }
        }
        return newarr;
    }

### 添加元素
var append2 = function(arr, item) {
    var newArr = arr.slice(0);  // slice(start, end)浅拷贝数组
    newArr.push(item);
    return newArr;
};  

### 删除数组最后一个元素
>利用slice
function truncate(arr) {
    return arr.slice(0,-1);//-1为最后一个元素
}
>利用filter
function truncate(arr) {
    return arr.filter(function(v,i,ar) {
        return i!==ar.length-1;//注意这里也要return
    });
}
>利用push.apply+pop
function truncate(arr) {
    var newArr=[];
    [].push.apply(newArr, arr);//继承[].push方法。参数arr,this指向newArr
    newArr.pop();
    return newArr;
}
[前端学习笔记之js中apply()和call()方法详解,apply实现方法的继承](https://www.jianshu.com/p/80ea0d1c04f8)
>利用join+split+pop    注意！！！：数据类型会变成字符型
function truncate(arr) {
    var newArr = arr.join().split(',');
    newArr.pop();
    return newArr;
}
[split和join的用法](https://www.cnblogs.com/nklzj/p/6123735.html)
>利用concat+pop
function truncate(arr) {
    var newArr = arr.concat();
    newArr.pop();
    return newArr;
}
### 添加元素
>splice
function insert(arr, item, index) {
     //复制数组
     var a = arr.slice(0);
     a.splice(index, 0, item);
     return a;
 }

 ### 计数
 >reduce reduce()-->从数组的第一项开始，逐个遍历到最后；
function count(arr, item) {
            var count = arr.reduce(function(prev, curr) {
                return curr === item ? prev+1 : prev;
            }, 0);
            return count;
        }
[JS高级函数--------map/reduce](https://blog.csdn.net/baidu_36065997/article/details/79079880)        

>filter()-->利用指定的函数确定是否在返回的数组中包含某一项
        function count(arr, item) {
            var count = arr.filter(function(a) {
                return a === item;   //返回true的项组成的数组
            });
            return count.length;
        }
>map()-->对数组中的每一项进行给定函数，
        //返回每次函数条用的结果组成的数组；
        function count(arr, item) {
            var count = 0;
            arr.map(function(a) {
  
                if(a === item) {
                    count++;
                }
            });
            return count;
        }

### 查找重复元素
>***将传入的数组arr中的每一个元素value当作另外一个新数组b的key，然后遍历arr去访问b[value]，若b[value]不存在，则将b[value]设置为1，若b[value]存在，则将其加1。可以想象，若arr中数组没有重复的元素，则b数组中所有元素均为1；若arr数组中存在重复的元素，则在第二次访**问该
**b[value]时，b[value]会加1，其值就为2了。最后遍历b数组，将其值大于1的元素的key存入另一个数组a中，就得到了arr中重复的元素。**

function duplicates(arr) {
     //声明两个数组，a数组用来存放结果，b数组用来存放arr中每个元素的个数
     var a = [],b = [];
     //遍历arr，如果以arr中元素为下标的的b元素已存在，则该b元素加1，否则设置为1
     for(var i = 0; i < arr.length; i++){
         if(!b[arr[i]]){
             b[arr[i]] = 1;
             continue;
         }
         b[arr[i]]++;
     }
     //遍历b数组，将其中元素值大于1的元素下标存入a数组中
     for(var i = 0; i < b.length; i++){
         if(b[i] > 1){
             a.push(i);
         }
     }
     return a;
 }
 ### 查找元素位置
>如果或的前者为真，则后面的就不执行了
function findAllOccurrences(arr, target) {
var temp = [];
    arr.forEach(function(val,index){
        val !== target ||  temp.push(index);
    });
    return temp;
}

### 避免全局变量
>在Javascript语言中，声明变量使用的都是关键字var，如果不使用var而直接声明变量，则该变量为全局变量。

### 正确的函数定义
>错误
function functions(flag) {
    if (flag) {
      function getValue() { return 'a'; }
    } else {
      function getValue() { return 'b'; }
    }

    return getValue();
}

>正确
function functions(flag) {
   var getvalue=null;
    if (flag) {
      getValue = function(){ return 'a'; }
    } else {
      getValue = function() { return 'b'; }
    }

    return getValue();
}

***考点：function 函数名(){}   和  var 函数名 = function（）{}的解析顺序的区别前者是在执行之前就会被解析    后者是在执行过程中***
    这道题是考函数声明与函数表达式的区别，原题的写法，是在两个逻辑分支里面各有一个函数声明，但是对于函数声明，解析器会率先读取并且让其在执行任何代码前可用，意思就是别的代码还没运行呢，两个getValue声明已经被读取，所以总是执行最新的那个。函数表达式，当解析器执行到它所在的代码行时，才会真正被解释执行，所以两个逻辑分支可以分别执行
### 正确的使用 parseInt

>按10进制去处理字符串，碰到非数字字符，会将后面的全部无视

function parse2Int(num) {
     return parseInt(num,10);
 }
 当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。
 举例，如果 string 以 "0x" 开头，parseInt() 会把 string 的其余部分解析为十六进制的整数。如果 string 以 0 开头，那么 ECMAScript v3 允许 parseInt() 的一个实现把其后的字符解析为八进制或十六进制的数字。如果 string 以 1 ~ 9 的数字开头，parseInt() 将把它解析为十进制的整数。

###实现一个打点计时器，要求
    1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console.log 一个数字，每次数字增幅为 1
    2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作
    3、第一个数需要立即输出

function count(start, end) {

  //立即输出第一个值
  console.log(start++);
     var timer = setInterval(function(){
         if(start <= end){
             console.log(start++);
         }else{
             clearInterval(timer);
         }
     },100);
    //返回一个对象
     return {
         cancel : function(){
             clearInterval(timer);
         }
     };
 }

### 题目描述
    实现 fizzBuzz 函数，参数 num 与返回值的关系如下：
    1、如果 num 能同时被 3 和 5 整除，返回字符串 fizzbuzz
    2、如果 num 能被 3 整除，返回字符串 fizz
    3、如果 num 能被 5 整除，返回字符串 buzz
    4、如果参数为空或者不是 Number 类型，返回 false
    5、其余情况，返回参数 num 

function fizzBuzz(num) {

    if(num%3 == 0 && num%5 == 0)
        return "fizzbuzz";
    else if(num%3 == 0)
        return "fizz";
    else if(num%5 == 0)
        return "buzz";
    else if(num == null || typeof num != "number")
        return false;
    else return num;

}

### 将数组 arr 中的元素作为调用函数 fn 的参数
>用函数可以使用call或者apply这两个方法，区别在于call需要将传递给函数的参数明确写出来，是多少参数就需要写多少参数。而apply则将传递给函数的参数放入一个数组中，传入参数数组即可。

function argsAsArray(fn, arr) {
  return fn.apply(this,arr);
}



### 将函数 fn 的执行上下文改为 obj 对象
function speak(fn, obj) {
    return fn.call(obj,[]);
}
> 在JavaScript中，函数是一种对象，其上下文是可以变化的，对应的，函数内的this也是可以变化的，函数可以作为一个对象的方法，也可以同时作为另一个对象的方法，可以通过Function对象中的call或者apply方法来修改函数的上下文，函数中的this指针将被替换为call或者apply的第一个参数。将函数 fn 的执行上下文改为 obj 对象，只需要将obj作为call或者apply的第一个参数传入即可。

### 实现函数 functionFunction，调用之后满足如下条件：?????
 1、返回值为一个函数 f
 2、调用返回的函数 f，返回值为按照调用顺序的参数拼接，拼接字符为英文逗号加一个空格，即 ', '
 3、所有函数的参数数量为 1，且均为 String 类型

 function functionFunction(str) {
    var ret = Array.prototype.slice.call(arguments).join(', ');
    var temp = function(str) {
        ret = [ret, Array.prototype.slice.call(arguments).join(', ')].join(', ');
        return temp;
    };
    temp.toString = function(){
        return ret;
    };
    return temp;

### 题目描述
 实现函数 makeClosures，调用之后满足如下条件：
 1、返回一个函数数组 result，长度与 arr 相同
 2、运行 result 中第 i 个函数，即 result[i]()，结果与 fn(arr[i]) 相同

>输入
[1, 2, 3], function (x) { 
	return x * x; 
}
>输出
4

function makeClosures(arr, fn) {
var result = [];
     arr.forEach(function(e){
         result.push(function(num){
             return function(){
                 return fn(num);
             };
         }(e));
     });
     return result;
}
