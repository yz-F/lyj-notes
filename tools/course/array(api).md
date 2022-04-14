### array api

找出某个元素在数组中的索引
indexof

### 通过索引删除某个元素
splice

### 复制一个数组
slice

### 一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。
Array.from()

### 创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。
Array.of()

### 所有数组实例都会从 Array.prototype 继承属性和方法。修改 Array 的原型会影响到所有的数组实例

属性 

1. Array.prototype.constructor
所有的数组实例都继承了这个属性，它的值就是 Array，表明了所有的数组都是由 Array 构造出来的。
2. Array.prototype.length
上面说了，因为 Array.prototype 也是个数组，所以它也有 length 属性，这个值为 0，因为它是个空数组。

### 修改器方法（影响自身的值）

### 浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度

copyWithin

### 