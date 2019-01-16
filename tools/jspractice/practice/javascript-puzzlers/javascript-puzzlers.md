## javascript-puzzlers
- [javascript-puzzlers](http://javascript-puzzlers.herokuapp.com/)<br>
- [javascript-puzzlers答案](https://flyfy1.github.io/language/2014/02/23/js-tricks-from-js-puzzlers.html)
### reduce() 

> array.reduce(function(total, currentValue, currentIndex, arr), initialValue)<br>
> 计算数组元素相加后的总和：125<br>

    var numbers = [65, 44, 12, 4];
    
    function getSum(total, num) {
        return total + num;
    }
    function myFunction(item) {
        document.getElementById("demo").innerHTML = numbers.reduce(getSum);
    }

### math.pow( x, y )  
> pow() 方法返回 xy（x的y次方） 的值。<br>
> 计算数组元素相加后的总和：125

    var numbers = [65, 44, 12, 4];
    
    function getSum(total, num) {
        return total + num;
    }
    function myFunction(item) {
        document.getElementById("demo").innerHTML = numbers.reduce(getSum);
    }    

### js小数计算无法保证正确的值 
> 采用 IEEE 754[1] 的实现的系统应该都会存在这个问题。
> 解决思路：转换成整数计算，即先乘一个大的数再除去一个数，因为计算误差是由浮点运算引起的
### new String(x) !== x 
> 解决方法：typeof str == "string" || typeof str.substring == "function"<br>
> Javascript创建者为string或int等基本类型创建了包装器，使其与java类似。不幸的是，如果someome使用新的String（“x”），则元素的类型将是“object”而不是“string”。<br>
>String(x) does not create an object but does return a string, i.e. typeof String(1) === "string"<br>
>[JavaScript中 new String（“x”）的意义何在？](https://stackoverflow.com/questions/5750656/whats-the-point-of-new-stringx-in-javascript)
### JavaScript Infinity 属性
> Infinity 属性用于存放表示正无穷大的数值。<br>
> 无法使用 for/in 循环来枚举 Infinity 属性，也不能用 delete 运算符来删除它。Infinity 不是常量，可以把它设置为其他值。<br>
>Infinity % 2 gives NaN

### JavaScript Infinity 属性
> parseInt(string, radix)
> radix可选。表示要解析的数字的基数。

    该值介于 2 ~ 36 之间。
    如果省略该参数或其值为 0，则数字将以 10 为基础来解析。如果它以 “0x” 或 “0X” 开头，将以 16 为基数。
    如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN。

### javaScript函数(arguments,this)
> [JavaScript函数](https://www.jianshu.com/p/3177b1f82bc0)
 
        function sidEffecting(ary) {
        ary[0] = ary[2];
        }
        function bar(a,b,c) {
        c = 10
        sidEffecting(arguments);
        return a + b + c;
        }
        bar(1,1,1)

### javaScript大数与小数相加
> [JavaScript函数](https://www.jianshu.com/p/3177b1f82bc0)
 

        var a = 111111111111111110000,
        b = 1111;
        a + b;

> 答：传入的值是number类型有bug，会传入科学计数法字符串
> [大整数相加，相乘](https://blog.csdn.net/mine666/article/details/8840164)
### [].reverse()
> [].reverse() 返回this : []
 

        var x = [].reverse;
        x();

> 答：没有接收，则返回window

### 数组比较

        var a = [1, 2, 3],
            b = [1, 2, 3],
            c = [1, 2, 4]
        a ==  b false
        a === b false
        a >   c false
        a <   c true
> 数组按字典顺序与>和<进行比较，但不与==和===进行比较

### 原型

    var a = {}, b = Object.prototype;false
    [a.prototype === b, Object.getPrototypeOf(a) === b] true
        
> 函数具有原型属性，但其他对象不具有原型属性，因此未定义a.prototype。 而是每个Object都有一个可通过Object.getPrototypeOf访问的内部属性