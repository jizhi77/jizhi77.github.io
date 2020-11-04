---
title: Class
toc: true
categories: [前端, JavaScript, ES6]
tags: [ES6]
date: 2019-11-09 08:09:12
cover: /images/covers/8.webp
---

<a name="ZVg4Q"></a>
## 一、概述

<a name="puoRO"></a>
### 1. ES5

<br />ES5 中用构造函数来实现类和实例：
```javascript
function Person() {
this.type = 'person'
}
Person.prototype.sayType = function () {
return this.type
}
const personA = new Person()
personA.sayType()
```
<br />
<a name="tyusH"></a>
### 2. ES6

<br />基本上，ES6 的 `class` 可以看做只是 ES5构造函数的语法糖，上面代码用 ES6 来实现如下：
```javascript
class Person {
  
	constructor() {
		this.type = 'person'
	}
  
	sayType() {
		return this.type
	}
  
}

const personA = new Person()

personA.sayType() // person
typeof Person // function
Person.prototype.constructor === Person // true
```
**
<a name="Jydxg"></a>
## 二、特性


<a name="KSkFt"></a>
### 1. 属性位置

<br />与 ES5 一样，实例的属性除非显示定义在其自身（即定义在 this 对象上），否则都是定义在原型上。**
```javascript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString() {
        return '(' + this.x + ', ' + this.y + ')'
    }
}

const point = new Point(2,3)

point.x // 2
point.y // 3

point.hasOwnProperty(x) // true
point.hasOwnProperty(y) // true
point.hasOwnProperty(toString) // false
```
<br />
<a name="EIGQw"></a>
### 2. 共享原型对象


```javascript
class Point {}

const point1 = new Point(2,3)
const point2 = new Point(4,5)

point1.__proto__ === point2.__proto__
```
<br />
<a name="bxypr"></a>
### 3. 属性表达式

<br />类的属性名，可以采用表达式<br />

```javascript
let getName = 'getName'

class Point {
	[getName](){}
}
```
<br />
<a name="fYvyt"></a>
### 4. 不存在变量提升


```javascript
new Point() // Uncaught ReferenceError: Point is not defined
class Point {}
```
<br />
<a name="PcsOe"></a>
### 5. Generator 方法

<br />如果在某个方法前加上星号（ `*` ），就表示该方法是一个 Generator函数。<br />

```javascript
class Foo {
    constructor(...args) {
        this.args = args
    }

    *[Symbol.iterator]() {
        for (const iterator of this.args) {
            yield iterator
        }
    }
}

let foo = new Foo('hello', 'world')
console.log(foo)

for (const iterator of foo) {
    console.log(iterator)
}
```
<br />
<a name="BNonc"></a>
### 6. this 指向

<br />类的方法内部的 `this` 默认指向该类的实例，但不小心也可能丢失（如 React 中 render 的事件绑定），至于具体为啥，可参考 this 指向一节（TODO）。<br />
<br />显示绑定上下文对象，有以下几种形式：<br />

```javascript
class Person {
  
    constructor() {
        this.name = name
        this.sex = sex
        this.age = age

        this.getName = () => this.getName() // 形式 1
        this.getAge = this.getAge.bind(this) // 形式 2
    }

    getName() { }

    getAge() { }

   // 形式 3
    getSex = () => { }

}
```
<br />
<a name="UBAZx"></a>
## 三、静态属性


区别于实例属性，可以直接通过类调用的方法称为静态属性。<br />

<a name="biNmR"></a>
### 1. this 指向
```javascript
class Person {
    static getSelfName() {
        return this.name // Person
    }

    constructor({ name, sex, age }) {
			this.name = name
      this.sex = sex
			this.age = age
    }
}

Person.getSelfName() // Person
```
‌
<a name="iNDqa"></a>
### 2. 继承


- 子类可以继承父类的静态属性
```javascript
class Person {
    static getSelfName() {
        return this.name
    }
}

class Student extends Person {}

Student.getSelfName()
```


- 通过 `super` 调用
```javascript
class Person {
    static getSelfName() {
        return this.name
    }
}

class Student extends Person {
    static getParentName() {
        return super.getSelfName()
    }
}

Student.getParentName() // Student
```
<br />
<a name="PINQv"></a>
## 四、私有属性

<br />在属性名称之前加 `#` 符号，可以声明此属性为私有的，即外部不可访问的。<br />

```javascript
class Person {
    #count = 1

    getCount() {
        return this.#count
    }

		#getCount() {
        return this.#count
    }
}

let person = new Person()
person.getCount() // 1
person.#getCount() // Uncaught SyntaxError: Private field '#count' must be declared in an enclosing class
person.#count // Uncaught SyntaxError: Private field '#count' must be declared in an enclosing class
```
‌
<a name="ivJnk"></a>
## 五、静态私有属性

<br />私有属性和私有方法前面，也可以加上`static`关键字，表示这是一个静态的私有属性或私有方法。

```javascript
class FakeMath {
  static PI = 22 / 7;
  static #totallyRandomNumber = 4;

  static #computeRandomNumber() {
    return FakeMath.#totallyRandomNumber;
  }

  static random() {
    console.log('I heard you like random numbers…')
    return FakeMath.#computeRandomNumber();
  }
}

FakeMath.PI // 3.142857142857143
FakeMath.random()
// I heard you like random numbers…
// 4
FakeMath.#totallyRandomNumber // 报错
FakeMath.#computeRandomNumber() // 报错
```


<a name="CSsfq"></a>
## 六、其他
<br />
<a name="lIezS"></a>
### 1. new.target


`new` 是从构造函数生成实例对象的命令。<br />
<br />ES6 为 `new` 命令引入了一个 `new.target` 属性，该属性只能用在类中的构造函数中，返回 `new` 命令作用于的那个构造函数。如果不是通过 `new` 命令或者 `Reflec.construct()` 调用， `new.target` 会返`undefined` 。<br />

```javascript
// 形式 1
class Person {
    constructor() {
        if (new.target === undefined) {
            throw new Error('必须使用 new 生成实例')
        }
    }
}

// 形式 2
class Person {
    constructor() {
        if (new.target !== Person) {
            throw new Error('必须使用 new 生成实例')
        }
    }
}
```


<a name="ysl60"></a>
## 七、继承

<br />

