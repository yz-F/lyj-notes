### array(纯数组)循环的三种方式

1. let file of files(注意for of 更适用于遍历数组，而for in 更适用用遍历对象)

```
1. 现在不太常见，因为函数式编程更受欢迎。
2. 对迭代的控制，例如跳过元素或 early returns。
3. O(N)复杂性，每个元素将只迭代一次。
```
2. files.reduce((acc, file) => {}) 可以减少数组

```

1.现在更常见，因为函数式编程更受欢迎。
2.对迭代的控制较少，不能跳过元素或return提前。
3.O(N)复杂性，每个元素将只迭代一次
```

3. 方法链()

```
1.现在更常见，因为函数式编程更受欢迎。
2.对迭代的控制较少，不能跳过元素或return提前
3.声明式的，更易于阅读和重构，链可以根据需要增长
4. O(cN)复杂性，c每个元素的迭代次数，（c：链的长度）

```

### 代码

- 1

```
const files = [ 'foo.txt ', '.bar', '   ', 'baz.foo' ];
let filePaths = [];

for (let file of files) {
  const fileName = file.trim();
  if(fileName) {
    const filePath = `~/cool_app/${fileName}`;
    filePaths.push(filePath);
  }
}

// filePaths = [ '~/cool_app/foo.txt', '~/cool_app/.bar', '~/cool_app/baz.foo']
```

- 2

```
const files = [ 'foo.txt ', '.bar', '   ', 'baz.foo' ];
const filePaths = files.reduce((acc, file) => {
  const fileName = file.trim();
  if(fileName) {
    const filePath = `~/cool_app/${fileName}`;
    acc.push(filePath);
  }
  return acc;
}, []);

// filePaths = [ '~/cool_app/foo.txt', '~/cool_app/.bar', '~/cool_app/baz.foo']
```

- 3

```
const files = [ 'foo.txt ', '.bar', '   ', 'baz.foo' ];
const filePaths = files
  .map(file => file.trim())
  .filter(Boolean)
  .map(fileName => `~/cool_app/${fileName}`);

// filePaths = [ '~/cool_app/foo.txt', '~/cool_app/.bar', '~/cool_app/baz.foo']
```