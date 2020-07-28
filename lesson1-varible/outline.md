碎片化的学习对提升帮助不是很大，第一个原因是很容易忘，第二个是很难把一系列知识点衔接起来。

## JavaScript 变量

这一节主要讲如何在 JavaScript 中创建变量，JavaScript 中的变量有什么特点，以及几种创建方式都有什么区别。

### JavaScript 变量的特点

JavaScript 是一门弱类型语言，所以在声明变量时不需要指定变量类型，与强类型语言不同，JavaScript 会在运行过程中自动推导变量类型。

好处是书写方便，编写代码效率很高，但缺点也很明显，当类型发生错误，我们只能在代码运行时发现。

### JavaScript 变量的定义

JavaScript 定义变量很简单，通常有三种方式：

```javascript
// 使用关键字 var
var name = '学长';
var age = 18,
	gender = 'boy';

// 使用关键字 let
let name = '学长';
let age = 18,
	gender = 'boy';

// 使用关键字 const
const name = '学长';
const age = 18,
	gender = 'boy';
```

但与 `var` 不同的是，`let、const` 不可重复定义相同变量：

```javascript
// 使用关键字 var
var name = '学长';
var name = 'Tony';
console.log(name); // 'Tony'

// 使用关键字 let
let name = '学长';
let name = 'Tony';
console.log(name); // Uncaught SyntaxError: Identifier 'name' has already been declared

// 使用关键字 const
const name = '学长';
const name = 'Tony';
console.log(name); // Uncaught SyntaxError: Identifier 'name' has already been declared
```

另外，使用 `const` 定义变量时，必须给变量赋值，否则会报错：

```javascript
// 使用关键字 var
var name; // undefined

// 使用关键字 let
let name; // undefined

// 使用关键字 const
const name; // Uncaught SyntaxError: Missing initializer in const declaration
```

而且使用 `const` 定义的变量不可重新赋值：

```javascript
const name = 'Tony';
name = '学长'; // Uncaught TypeError: Assignment to constant variable
```

### var、let、const 三者特点

ES6 规范为何要新增 `let、const` 两个关键字？它们三者都各自有什么特点？

只有彻底了解它们的特性，才能更好的在工作中使用，而这也是初级前端工程师晋升中级的必备进阶知识点。

#### 块级作用域

在 ES6 之前是没有块级作用域的，那么什么叫块级作用域呢？我们通过代码来看看：

```javascript
if (true) {
	var name = 'Jerry';
}
console.log(name); // Jerry
```

`if` 语句的 `{}` 之间就属于一个代码块，这个块本应有它自己的作用域，但在 ES6 之前都是没有的。

因此我们在 if 语句外打印 name，也是可以拿到值的。

不过现在可以通过 `let、const` 来实现块级作用域了：

```javascript
if (true) {
	const name = 'Jerry';
}
console.log(name); // undefined
```

这个时候，打印出来的 name 值就是 undefined 了，name 变量只在 if 语句块里有效。

以前面试的时候经常会问到这样一个问题，看下面代码，最终会打印什么？

```javascript
var arr = [0, 1, 2, 3]
for (var i = arr.length - 1; i >= 0; i--) {
    setTimeout(() => {
        console.log(arr[i])
    }, i * 1000)
}
```

肯定有人认为是 0 1 2 3，每隔一秒打印一个数字。然而实际却是，每隔一秒打印一个 undefined。

以前要解决这个问题就是通过立即执行函数来解决，不过现在使用 `let` 也同样可以解决这个问题：

```javascript
var arr = [0, 1, 2, 3]
for (let i = arr.length - 1; i >= 0; i--) {
    setTimeout(() => {
        console.log(arr[i])
    }, i * 1000)
}
// 0
// 1
// 2
// 3
```

#### 变量提升

什么是`变量提升`？我们先来看一段代码：

```javascript
function fn() {
    console.log(name);
    var name = 'tony';
}

fn();  // undefined
```

可以看到我们在定义变量 name 前调用 name，程序并没有报错，而是打印了 undefined。

这就是`变量提升`，通过 `var` 定义的变量会被提升到它所在作用域的顶部，因此上面的代码也可以写成：

```javascript
function fn() {
    var name;
    console.log(name);
    name = 'tony';
}

fn();  // undefined
```

#### 暂时性死区

如果把上面代码的 `var` 关键字换成 `let` 或者 `const` 会是什么情况呢？

```javascript
function fn() {
    console.log(name);
    const name = 'tony';
}

fn(); // Uncaught ReferenceError: Cannot access 'name' before initialization
```

可以发现，在变量声明之前引用这个变量，程序将抛出引用错误（ReferenceError）。

因为这个变量将从代码块一开始的时候就处在一个`暂时性死区`，直到这个变量被声明为止。

#### 全局变量

通过 `var` 定义的变量还有一个特点，那就是如果在全局环境中定义一个变量，程序会自动在 `window` 对象上面添加一个相同的属性。

```javascript
var name = "学长";
window.name // '学长'
```

然而通过 `let、const` 定义变量就不会出现这种情况：

```javascript
let name = '学长';
window.name // undefined
```

#### 总是使用 const 和 let 定义变量

`var` 关键字的这两个特性使得代码容错性更高，但实际上却更容易让人犯错，再来看看下面一段代码：

```javascript
var name = '学长';
function fn() {
     console.log(name);
     if (false) {
        var name = "Tony";
    }
}
fn();
```

大家先想想，这里打印出来的 name 值是什么？

肯定很多初学者会认为打印值是 `学长`，但实际这里打印的值却是 `undefined`。

是不是感觉有点莫名奇妙，其实解释起来也很简单。

用 `var` 定义的变量是没有`块级作用域`的，再配合其`变量提升`的特性，那么上面的代码就相当于：

```javascript
var name = '学长';
function fn() {
    var name;
    console.log(name);
    if (false) {
        name = "Tony";
    }
}
fn();
```

这样子写，是不是就能看明白了。

### 作业：

1. var、let、const 三者的区别都是什么？各有什么特点？
2. 下面代码打印结果是什么？

```javascript
var name = '学长';
function fn() {
     console.log(name);
     if (false) {
        var name = "Tony";
    }
}
fn();
```
