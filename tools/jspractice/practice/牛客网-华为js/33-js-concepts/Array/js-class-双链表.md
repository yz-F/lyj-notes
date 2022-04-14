- [lianjie](https://www.30secondsofcode.org/articles/s/js-data-structures-doubly-linked-list)
### 数据结构
- 定义
```
双向链表是表示元素集合的线性数据结构，其中每个元素都指向下一个和前一个。双向链表中的第一个元素是头部，最后一个元素是尾部。
```

- 双向链表数据结构的每个元素必须具有以下属性：

```
value: 元素的值
next：指向链表中下一个元素的指针（null如果没有）
previous：指向链表中前一个元素的指针（null如果没有）
```

- 数据结构

```
size：双向链表中的元素个数
head: 双向链表中的第一个元素
tail：双向链表中的最后一个元素
```

- 双向链表数据结构的主要操作有

```
insertAt: 在特定索引处插入一个元素
removeAt：删除特定索引处的元素
getAt: 检索特定索引处的元素
clear: 清空双向链表
reverse: 颠倒杜比链表中元素的顺序
```
### 代码片段解释


-  class用 a创建一个为每个实例constructor初始化一个空数组的 a。nodes
```
constructor() {
    this.nodes = [];
  }
```
-  定义一个sizegetter，它返回用于返回数组Array.prototype.length中元素的数量。nodes
```
return this.nodes.length;
```
-  定义一个headgetter，它返回nodes数组的第一个元素或者null如果为空。
```
return this.size ? this.nodes[0] : null;
```
-  定义一个tailgetter，它返回nodes数组的最后一个元素或者null如果为空。
```
return this.size ? this.nodes[this.size - 1] : null;
```
-  定义一个getAt()方法，该方法检索给定的元素index。
```
return this.nodes[index];
```
- 重要！ 定义两个便捷方法，分别insertFirst()使用insertLast()该insertAt()方法在数组的开头或结尾插入一个新元素nodes。
```
this.insertAt(0, value);
this.insertAt(this.size, value);

insertAt(index, value) {
  // 获取当前元素的上一个元素
    const previousNode = this.nodes[index - 1] || null;
    //获取当前元素
    const nextNode = this.nodes[index] || null;
    //设置新元素的数据结构
    const node = { value, next: nextNode, previous: previousNode };
    //如果上一个元素存在，那么上一个元素的下一个nex 设置为当前元素
    if (previousNode) previousNode.next = node;
    //如果下一个元素存在，那么下一个元素的上一个pre 设置为当前元素
    if (nextNode) nextNode.previous = node;
    this.nodes.splice(index, 0, node);
  }

```
-  重要！定义一个removeAt()方法，1. 用于Array.prototype.splice()删除nodes数组中的一个对象，2. 分别更新前一个元素和下一个元素的键next和previous键。。
```
    //找到Inde值的上一个元素
    const previousNode = this.nodes[index - 1] || null;
    //找到index值的下一个元素
    const nextNode = this.nodes[index + 1] || null;
    // 如果上一个元素存在，则上一个元素的Nex值，等于除去当前元素的下一个元素
    if (previousNode) previousNode.next = nextNode;
    // 如果下一个元素存在，则下一个元素的pre值，等于除去当前元素的上一个元素
    if (nextNode) nextNode.previous = previousNode;

    return this.nodes.splice(index, 1);
```
- 定义一个clear()清空nodes数组的方法。
```
this.nodes = [];
```
-  重要！定义一个reverse()方法，它使用Array.prototype.reduce()扩展运算符 ( ...) 来反转nodes数组的顺序，适当地更新每个元素的next和previous键。
```
 this.nodes = this.nodes.reduce((acc, { value }) => {
   // 获取数组第一个元素
      const nextNode = acc[0] || null;
      // 这个是我们要设置的新数组第一个元素
      const node = { value, next: nextNode, previous: null };
      //如果原来数组存在话，数组的第一个元素的前一个元素，就是reduce当前遍历的对象。
      // 此处相当于把元素倒叙排列了，通过previous
      if (nextNode) nextNode.previous = node;
      return [node, ...acc];
    }, []);
```
-  为 定义一个生成器方法，该方法使用语法Symbol.iterator委托给nodes数组的迭代器。yield*
```
*[Symbol.iterator]() {
    yield* this.nodes;
  }
```
### code all
- 
```
class DoublyLinkedList {
  constructor() {
    this.nodes = [];
  }

  get size() {
    return this.nodes.length;
  }

  get head() {
    return this.size ? this.nodes[0] : null;
  }

  get tail() {
    return this.size ? this.nodes[this.size - 1] : null;
  }

  insertAt(index, value) {
    const previousNode = this.nodes[index - 1] || null;
    const nextNode = this.nodes[index] || null;
    const node = { value, next: nextNode, previous: previousNode };

    if (previousNode) previousNode.next = node;
    if (nextNode) nextNode.previous = node;
    this.nodes.splice(index, 0, node);
  }

  insertFirst(value) {
    this.insertAt(0, value);
  }

  insertLast(value) {
    this.insertAt(this.size, value);
  }

  getAt(index) {
    return this.nodes[index];
  }

  removeAt(index) {
    const previousNode = this.nodes[index - 1] || null;
    const nextNode = this.nodes[index + 1] || null;

    if (previousNode) previousNode.next = nextNode;
    if (nextNode) nextNode.previous = previousNode;

    return this.nodes.splice(index, 1);
  }

  clear() {
    this.nodes = [];
  }

  reverse() {
    this.nodes = this.nodes.reduce((acc, { value }) => {
      const nextNode = acc[0] || null;
      const node = { value, next: nextNode, previous: null };
      if (nextNode) nextNode.previous = node;
      return [node, ...acc];
    }, []);
  }

  *[Symbol.iterator]() {
    yield* this.nodes;
  }
}
```
### 调用
```
const list = new DoublyLinkedList();

list.insertFirst(1);
list.insertFirst(2);
list.insertFirst(3);
list.insertLast(4);
list.insertAt(3, 5);

list.size;                      // 5
list.head.value;                // 3
list.head.next.value;           // 2
list.tail.value;                // 4
list.tail.previous.value;       // 5
[...list.map(e => e.value)];    // [3, 2, 1, 5, 4]

list.removeAt(1);               // 2
list.getAt(1).value;            // 1
list.head.next.value;           // 1
[...list.map(e => e.value)];    // [3, 1, 5, 4]

list.reverse();
[...list.map(e => e.value)];    // [4, 5, 1, 3]

list.clear();
list.size;                      // 0
```

