### [jichu-object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)


#### 属性


- Object.prototype.constructor

```
所有对象都会从它的原型上继承一个 constructor 属性

var o = {};
o.constructor === Object; // true

var o = new Object;
o.constructor === Object; // true

var a = [];
a.constructor === Array; // true

var a = new Array;
a.constructor === Array // true

var n = new Number(3);
n.constructor === Number; // true

```


- Object.prototype.__proto__

```
已废弃: 该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。
```

#### 方法(上面是常用的，下面是不常用的)
- Object.is(value1, value2);

```
Object.is() 方法判断两个值是否为同一个值。

```

- Object.assign()

```
Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。


const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

- Object.entries()

```
Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）

const object1 = {
  a: 'somestring',
  b: 42
};

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}

// expected output:
// "a: somestring"
// "b: 42"
```

- Object.fromEntries()

```
Object.fromEntries() 方法把键值对列表转换为一个对象。

const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

const obj = Object.fromEntries(entries);

console.log(obj);
// expected output: Object { foo: "bar", baz: 42 }
```

- Object.prototype.hasOwnProperty()

```

该hasOwnProperty()方法返回一个布尔值，指示对象是否具有指定的属性作为它自己的属性（而不是继承它）。

const object1 = {};
object1.property1 = 42;

console.log(object1.hasOwnProperty('property1'));
// expected output: true

console.log(object1.hasOwnProperty('toString'));
// expected output: false

console.log(object1.hasOwnProperty('hasOwnProperty'));
// expected output: false

```
- Object.keys()

```
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1));
// expected output: Array ["a", "b", "c"]
```

- Object.values()

```
Object.values()

const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.values(object1));
// expected output: Array ["somestring", 42, false]

```
- Object.create()

```

```

- Object.defineProperties()

```

```




- Object.freeze() 和 Object.isFrozen()

```

```



- Object.getOwnPropertyDescriptor()

```

```


- Object.getOwnPropertyNames()

```

```

- Object.hasOwn()

```
如果指定的对象具有指定的属性作为其自己的Object.hasOwn()属性，则静态方法返回。如果属性被继承或不存在，则该方法返回。 truefalse

const object1 = {
  prop: 'exists'
};

console.log(Object.hasOwn(object1, 'prop'));
// expected output: true

console.log(Object.hasOwn(object1, 'toString'));
// expected output: false

console.log(Object.hasOwn(object1, 'undeclaredPropertyValue'));
// expected output: false
```
- Object.isExtensible()

```

```
- Object.prototype.isPrototypeOf()

```

```
- Object.isSealed() 和 Object.seal()

```

```
- Object.preventExtensions()

```

```
- Object.prototype.propertyIsEnumerable()

```

```
- Object.setPrototypeOf()

```

```
- Object.prototype.toLocaleString()

```

```
- Object.prototype.toString()

```
function Dog(name) {
  this.name = name;
}

const dog1 = new Dog('Gabby');

Dog.prototype.toString = function dogToString() {
  return `${this.name}`;
};

console.log(dog1.toString());
// expected output: "Gabby"
```
- Object.prototype.valueOf()

```
function MyNumberType(n) {
  this.number = n;
}

MyNumberType.prototype.valueOf = function() {
  return this.number;
};

const object1 = new MyNumberType(4);

console.log(object1 + 3);
// expected output: 7
```



### 刷题


- obj.hasOwnProperty

```
const object1 = {};
object1.property1 = 42;

console.log(object1.hasOwnProperty('property1'));
// expected output: true

console.log(object1.hasOwnProperty('toString'));
// expected output: false

console.log(object1.hasOwnProperty('hasOwnProperty'));
// expected output: false
```

```
输入描述：
先输入键值对的个数n（1 <= n <= 500）
接下来n行每行输入成对的index和value值，以空格隔开

输出描述：
输出合并后的键值对（多行）

输入：
4
0 1
0 2
1 2
3 4

输出：
0 3
1 2
3 4

let len = readline();
let line;
let obj = {};
let i = 0;
while(true) {
    line = readline();
    if(i < Number(len)) {
        const arr = line.split(' ');
        const key = parseInt(arr[0]);
        const value = parseInt(arr[1])
        if(obj.hasOwnProperty(key)) {
            obj[key] =  obj[key] + value
        }else {
             obj[key] = value || 0;
        }
        i++;
    }else {
        break;
    }
   
}

for(const key in obj) {
    console.log(key + ' ' + obj[key])
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

-

```

```

```

```