### [hwjs](https://www.nowcoder.com/exam/oj/ta?tpId=37)

- lastIndexOf() 方法返回调用String 对象的指定值最后一次出现的索引，在一个字符串中的指定位置 fromIndex处从后向前搜索。如果没找到这个特定值则返回-1 。
```
输入：
hello nowcoder

输出：
8

说明：
最后一个单词为nowcoder，长度为8   

function getLastWordLength(words){
    return words.substring(words.lastIndexOf(' ')+1).length
}
const words = readline()
console.log(getLastWordLength(words))

```

- split() 方法使用指定的分隔符字符串将一个String对象分割成子字符串数组，以一个指定的分割字串来决定每个拆分的位置。

```

const str = 'The quick brown fox jumps over the lazy dog.';
const words = str.split(' ');
console.log(words[3]);
// expected output: "fox"

const strCopy = str.split();
console.log(strCopy);
// expected output: Array ["The quick brown fox jumps over the lazy dog."]

```

```
输出输入字符串中含有该字符的个数。（不区分大小写字母）

var str = readline().toLowerCase()
var key = readline().toLowerCase()
 
var count = 0;
 
console.log(str.split(key).length -1)


```