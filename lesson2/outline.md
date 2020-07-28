## JavaScript 数据类型

这一节主要讲解 JavaScript 的数据类型。

### 7 个基本数据类型

随着 ES2020 的正式发布，JavaScript 的基本数据类型已经新增到 7 个，分别是 `null`、`undefined`、`boolean`、`string`、`number`、`symbol`、`bigint`。

前面 5 个基本数据类型比较简单，这里就不做赘述。我会主要讲讲最近几个规范才新增的 `symbol` 和 `bigint`。

#### symbol

首先看看如何在代码里定义一个 symbol 类型的数据：

```javascript
const sym1 = Symbol();

// 还可以添加一个字符串，作为描述
const sym2 = Symbol('hello');
```

我们还可以通过 `description` 属性获取描述

```javascript
sym2.description // hello
```

`Symbol()` 函数会返回 symbol 类型的值，该类型具有静态属性和静态方法。每个从 `Symbol()` 返回的symbol值都是 **唯一** 的。

```javascript
const sym1 = Symbol();
const sym2 = Symbol();

typeof sym1 === 'symbol'; // true
sym1 === sym2; // false
```

> 注意：`Symbol()` 前面不要带 `new` 关键字，否则会报错

`symbol` 类型的值作为对象属性，不会被转换成字符串：

```javascript
// 用数字作为对象的 key
const num = 1;
const str = '1';
const obj1 = {
    [num]: 'number key'
};
// 通过字符串依然可以访问
obj1[num] === obj1[str]; // true

// 用 Symbol() 创建的值不会被转换成字符串，始终唯一
const sym1 = Symbol();
const sym2 = Symbol();
const obj2 = {
    [sym1]: 'i am sym1'
};
obj2[sym1] === obj2[sym2]; // false
```

另外，对象中 `symbol` 类型的属性还具有”隐藏“的特性：

```javascript
const sym = Symbol();
const obj = {
    name: '学长',
    [sym]: 'i am a symbol'
};
Object.keys(obj); // ['name']

// 使用 `for...in` 循环也找不到
// 不过可以用 `in` 操作符进行判断
sym in obj; // true

// 也可以使用 Object.getOwnPropertySymbols() 获取
Object.getOwnPropertySymbols(obj); // [Symbol()]
```

还有 `Symbol.for(key)` 和 `Symbol.kefFor(sym)` 方法，因为目前没啥用处，所以这里不做过多介绍。

#### bigint

`bigint` 是一个新的基本数据类型，用来表示大于 `2^53 - 1` 的整数。

我们可以调用 `BigInt()` 函数，或者直接在数字后面添加一个 `n` 来定义 bigint 类型的数据。

```javascript
const big1 = BigInt(10);
const big2 = 10n;

typeof big1; // bigint
big1 === big2; // true
```

`bigint` 类型的数据也只能和 `bigint` 类型的数据进行运算，并且也不能用于 `Math` 对象中的方法：

```javascript
1n + 2n; // 3n

1n + 2; // TypeError: Cannot mix BigInt and other types, use explicit conversions

Math.pow(2n,3); // TypeError: Cannot convert a BigInt value to a number
```

`bigint` 类型的数据进行运算时，如果有小数，会自动向下取整：

```javascript
5 / 2; // 2.5
5n / 2n; // 2n

11 / 3; // 3.6666666666666665
11n / 3n; // 3n
```

注意，不要在 JSON 中使用 bigint 类型的数据，否则会抛错：

```javascript
JSON.stringify(BigInt(1)); // TypeError: Do not know how to serialize a BigInt
```

关于基本数据类型的分享就到这，有任何疑问都可以在评论区留言。下一节，会讲引用类型 `object`。