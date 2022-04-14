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

- String.prototype.repeat()

```
repeat() 构造并返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本。
str.repeat(count)
重复次数不能为负数。
"abc".repeat(-1)     // RangeError: repeat count must be positive and less than inifinity
"abc".repeat(0)      // ""
"abc".repeat(1)      // "abc"
"abc".repeat(2)      // "abcabc"
"abc".repeat(3.5)    // "abcabcabc" 参数count将会被自动转换成整数.
"abc".repeat(1/0)    // RangeError: repeat count must be positive and less than inifinity
```

```

描述

•输入一个字符串，请按长度为8拆分每个输入字符串并进行输出；

•长度不是8整数倍的字符串请在后面补数字0，空字符串不处理。


let line
while (line = readline()) {
    const overNumber = line.length % 8
    const result = line.concat(new String("0").repeat(overNumber ? 8 - overNumber : 0))
    for (let i = 0; i < result.length;i++) {
        console.log(result.substring(i, i += 8))
    }
}
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
- 

```

```

```

```