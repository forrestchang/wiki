# ES5 to ESNext —  自 2015 以来 JavaScript 新增的所有新特性

这篇笔记来源：[https://medium.freecodecamp.org/es5-to-esnext-heres-every-feature-added-to-javascript-since-2015-d0c255e13c6e](https://medium.freecodecamp.org/es5-to-esnext-heres-every-feature-added-to-javascript-since-2015-d0c255e13c6e)

通过阅读把之前不了解的一些 JS 语法记录下来，以便以后查用。

## ECMAScript 简介

ES 是 ECMAScript 的缩写，它是一个标准，JavaScript 就是基于这个标准实现的语言（Ecma International，瑞士标准协会，负责制定国际标准）。目前 Emacsript 的最新版本是 ES2018，2018 年 6 月发布。

![](https://raw.githubusercontent.com/forrestchang/img-repo/master/20190424214856.png)

## ES2015（ES6）

### let 和 const

ES 6 之前：

- `var` 是 ES6 之前唯一用来声明变量的关键字；
- 未经 `var` 声明的变量在 `strict` 模式的浏览器下会抛出异常；
- `var a` 未经过初始化的变量会被赋值 `undefined`；
- `var` 没有块级作用域，但是有函数作用域：函数体之外 `var` 的变量是 global 的，函数体内 `var` 的变量只有函数体可见；
- 函数体内 `var` 一个变量名和函数体外的某个变量名相同，会进行覆盖（是否仅在函数体内覆盖？）；
- hoisting：在某个函数体中，JavaScript 执行代码之前会把所有的变量提前到开头，所以在函数体中任何地方定义的变量都可以在最开始访问，这种特性叫做 hoisting。

let：

- `let` 是块级作用域，以后不要再是用 `var` 了！
- 在函数体外用 `let` 声明的变量也不会变成 global variable；

const：

- `const` 定义的是常量，值不能更改；
- `const` 和 `let` 一样，都是块级作用域；
- `const` 只是保证 reference 不被更改，并不具有 immutability 的特性，如果某个变量是一个 Object，并且这个 object 提供了 mutate 自己的方法，那么这个 object 的内容还是可以更改的，但是这个 object 的引用无法更改，没办法把这个变量再赋值到另一个 object 上。

### 箭头函数

- 更加简洁的函数写法，`const test = (arg1, arg2) => { /*do_something*/ }`；
- 如果函数体只包含一条语句，可以简化 `{}`，`const myFunction = () => doSomething()`；
- 如果函数只包含一个参数，可以简化 `()`，`const myFunction = arg => doSomething(arg)`；
- 如果函数只有一条 return 语句，可以简化：`const myFunction = () => 'test'`，`myFunction()` 将会返回 `test`；
- 如果返回值是一个 object，需要用 `()` 来包裹：`const myFunction = () => ({ value: 'test' })`；

箭头函数中的 `this` 关键字：

`this` 关键字的功能会根据 `context` 的不同，JavaScript 的严格模式（strict mode）发生改变。

在普通的函数声明方式中，`this` refer 当前的 object：

```js
const car = {
	model: 'Fiesta',
	manufacturer: 'For',
	fullName: function() {
		return `${this.manufacturer} ${this.model}`
	}
}
```

调用 `car.fullName()` 会返回 `Ford Fiesta`。

箭头函数中的 `this` 作用域会依赖于执行方的上下文环境，箭头函数中并不会 bind `this`。

例如，同样的例子，使用箭头函数改写：

```js
const car = {
	model: 'Fiesta',
	manufacturer: 'For',
	fullName: () => {
		return `${this.manufacturer} ${this.model}`
	}
}
```

调用 `car.fullName()` 将会返回 `undefined undefined`。因此，在 object 中编写方法不应该使用箭头函数。

同样，箭头函数还不能再 constructors 中使用，会抛出 `TypeError`；在 `EventListener` 中也应该使用普通的函数。

### 类

类的定义：

```js
class Person {
	constructor(name) {
		this.name = name
	}

	hello() {
		return 'Hello, I am ' + this.name + '.'
	}
}
```

类的使用：

```js
const jack = new Person('Jack')
jack.hello()
```

类的继承：

```js
class Programmer extends Person {
	hello() {
		return super.hello() + ' I am a programmer.'
	}
}

const jack = new Programmer('Jack')
jack.hello()  // Hello, I am Jack. I am a programmer.
```

静态方法：

```js
class Person {
	static genericHello() {
		return 'Hello'
	}
}

Person.genericHello()  // 'Hello'
```

私有方法：没有默认的私有方法支持。

getters and setters：

```js
class Person {
	constructor(name) { this._name = name }
	set name(value) { this._name = value }
	get name(value) { return this._name }
}
```

### 默认参数

```js
const doSomething = (params1 = 'test', params2 = 'test2') => {}
```

如果是一个 object 有初始值：

```js
const colorize = (options) => {
	if (!options) {
		options = {}
	}

	const color = ('color' in options) ? options.color : 'yellow'
	// ...
}
```

这个和 Python 中自定义对象初始值的方法类似：

```python
def colorize(options) => {
	if not options:
		options = {}

	# ...
}
```

利用结构（destructuring）的方法可以简化代码：

```js
const colorize = ({ color = 'yellow' }) => { ... }
```

如果调用 `colorize` 函数的时候没有 object 传入，可以赋一个初始值：

```js
const spin = ({ color: 'yellow' } = {}) => { ... }
```

### 字符串模板

创建一个多行字符串：

```js
const string = `
First line
Second line
`.trim()
```

字符串插值：

```js
const name = 'jack'

const string = `Hello, ${name}`
```

模板 Tag：

```js
const query = gql`
	query {
		// ...
	}
`

# 实现方式
function gql(literals, ...expressions) {}

# 相当于
query = gql(...)
```

### 结构（Destructuring）

使用结构可以把某个 object 中的相同名字的成员取出：

```js
const Person = {
	firstName: 'Tom',
	lastName: 'Cruise',
	actor: true,
	age: 54
}

const {firstName: name, age} = person

console.log(name) // Tom
console.log(age) // 54
```

### Enhanced Object Literals

```js
const something = 'y'
const x = {
	something
}

// 原来的写法
const something = 'y'
const x = {
	something: something
}
```

Prototype:

A prototype can be specified with

```js
const anObject = { y: 'y' }
const x = {
	__proto__: anObject
}
```

### For-of loop

```js
// iterate over the value
for (const v of [1, 2, 3, 4]) {
	console.log(v)
}

// get the index as well, using `entries()`
for (const [index, value] of [1, 2, 3, 4].entries()) {
	console.log(index)
	console.log(value)
}
```

### Promises

**A proxy for a value that will eventually become available.**

Promises 是一种异步的解决方式，可以避免写很多回调代码。

Promises 是如何工作的？

Once a promise has been called, it will start in pending state.