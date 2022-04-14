### string 基础

- String.prototype.split()

```
该split()方法将 a 划分 String为子字符串的有序列表，将这些子字符串放入一个数组中，并返回该数组。划分是通过搜索模式来完成的；其中模式作为方法调用中的第一个参数提供。

const str = 'The quick brown fox jumps over the lazy dog.';

const words = str.split(' ');
console.log(words[3]);
// expected output: "fox"

const chars = str.split('');
console.log(chars[8]);
// expected output: "k"

const strCopy = str.split();
console.log(strCopy);
// expected output: Array ["The quick brown fox jumps over the lazy dog."]

```


```
将一个英文语句以单词为单位逆序排放。例如“I am a boy”，逆序排放后为“boy a am I”

所有单词之间用一个空格隔开，语句中除了英文字母外，不再包含其他字符


输入：
I am a boy
复制
输出：
boy a am I

let str = readline()
let str1 =''
let arr= str.split(' ')

for(let i=arr.length-1;i>=0;i--){
    str1 += arr[i]
    if(i!=0){
        str1 += ' '
    }
}
console.log(str1)

```
- 

```


```
- 

```


```
- 

```


```
- 

```


```
- 

```


```
- 

```


```


###

-  str.charCodeAt()

```
返回ascii 编码

```

```
输入描述：
输入一行没有空格的字符串。

输出描述：
输出 输入字符串 中范围在(0~127，包括0和127)字符的种数。

输入：
aaa

输出：
1

let cha = readline();
let arr=[];
for (let i = 0; i < cha.length; i++) {
    if(arr.indexOf(cha[i])== -1 && cha.charCodeAt(i)<=127&&cha.charCodeAt(i)>=0) {
        arr.push(cha[i]);
    } 
}
console.log(arr.length);
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
- 

```


```

```

```