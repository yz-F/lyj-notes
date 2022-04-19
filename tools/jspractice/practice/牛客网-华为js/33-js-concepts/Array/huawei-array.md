

### jichu

- arr.join()

```
join() 方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符。

const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```
- Array.prototype.sort()

```
sort() 方法用原地算法对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的

const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// expected output: Array ["Dec", "Feb", "Jan", "March"]

const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);
// expected output: Array [1, 100000, 21, 30, 4]
```
- Array.prototype.toString()

```
toString() 返回一个字符串，表示指定的数组及其元素。
const array1 = [1, 2, 'a', '1a'];

console.log(array1.toString());
// expected output: "1,2,a,1a"
```
- String.prototype.charCodeAt() 和String.prototype.charAt()

```
该charCodeAt()方法返回一个整数0，65535表示给定索引处的 UTF-16 代码单元

const sentence = 'The quick brown fox jumps over the lazy dog.';

const index = 4;

console.log(`The character code ${sentence.charCodeAt(index)} is equal to ${sentence.charAt(index)}`);
// expected output: "The character code 113 is equal to q"

```
- 

```

```



### test

- arr.join('')
```
将这个整数以字符串的形式逆序输出

let str = readline();
let arr = str.split('').reverse();
console.log(arr.join(''));
```



-arr.sort()
```
var num = readline();
num = Number(num);
let arr=[];
for(let i = 0; i<num; i++){
    arr.push(readline());
}
arr.sort();
for(let i = 0;i<num;i++){
    console.log(arr[i])
}
```

```
9
cap
to
cat
card
two
too
up
boat
boot

boat
boot
cap
card
cat
to
too
two
up
```

- arr.toString().split('.')[0] 取除数 %取余数。并且用到了递归
```
输入一个 int 型的正整数，计算出该 int 型数据在内存中存储时 1 的个数。

输入：
5

输出：
2
```

```
var sourceNum = readline();

var resultCount = 0;
while(sourceNum >= 1){ 
    if(1 == sourceNum % 2){
        resultCount ++;    
    }

    sourceNum = (sourceNum / 2).toString().split('.')[0]; 
} 
console.log(resultCount);
```

-
```

```

```

```

-
```

```

```

```

-
```

```

```

```




