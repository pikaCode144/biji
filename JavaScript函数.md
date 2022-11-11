# 函数

## 函数的定义

​	函数通常以函数声明的方式定义，比如：

```javascript
function sum (num1, num2) {
    return num1 + num2;
}
```

​	函数定义后不加分号。

​	另一种定义函数的语法是函数表达式。函数表达式与函数声明几乎是等价的：

```javascript
let sum = function(num1, num2) {
    return num1 + num2;
};
```

​	将初始化的函数赋值给变量，函数没有名称，直接用变量名称访问函数。

​	函数表达式后要加分号。

​	还有一种“箭头函数”，如下所示：

```javascript
let sum = (num1, num2) => {
    return num1 + num2;
};
```

​	最后一种使用的是Function构造函数：

```javascript
let sum = new Function("num1", "num2", "return num1 + num2"); // 不推荐
```

​	这种方法把函数想象为对象，把函数名想象为指针。但是会被解析两次影响性能所以不推荐使用。

## 1.箭头函数

​	ECMAScript 6 新增了使用胖箭头（ => ）语法定义函数表达式的能力。任何可以使用函数表达式的地方，都可以是用箭头函数：

```javascript
let arrowSum = (a, b) => {
    return a + b;
};
let functionExpressionSum = function(a, b) {
    return a + b;
};
console.log(arrowSum(5, 8)); // 13
console.log(functionExpressionSum(5, 8)); // 13
```

​	箭头函数简洁的语法非常适合嵌入函数的场景：

```javascript
let ints = [1, 2, 3];
console.log(ints.map(function(i) { return i + 1; })); // [2, 3, 4]
console.log(ints.map((i) => { return i + 1; })); // [2, 3, 4]
```

​	箭头函数的简写形式：

```javascript
let double = (x) => { return 2 * x; };
// 当小括号里的参数只有一个时，可以省略小括号，如果没有参数或者有多个参数时，就必须写上小括号
let double = x => { return 2 * x; };
// 当大括号里的表达式只有一个时，可以省略大括号和return关键字
let double = x => 2 * x;
```

​	箭头函数虽然语法简洁，但也有很多场合不适用。箭头函数不能使用 arguments、super 和 new.target，也不能用作构造函数。此外，箭头函数也没有prototype属性。

## 2.函数名

​	函数名是指向函数的指针，一个函数可以有多个函数名，如下所示：

```	javascript
function sum(num1, num2) {
    return num1 + num2;
}
console.log(sum(10, 10)); // 20
let anotherSum = sum;
console.log(anotherSum(10, 10)); // 20
sum = null;
console.log(anotherSum(10, 10)); // 20
```

​	使用不带括号的函数名会访问函数指针，而不会执行函数。anotherSum 和 sum 都指向同一个函数。把sum设置为null，切断了sum与函数之间的关联。函数不会被销毁，anotherSum仍然可以访问。

​	ECMAScript 6 的所有函数对象都会暴露一个只读的name属性，其中包含关于函数的信息。多数情况下，这个属性中保存的就是一个函数标识符，或者说是一个字符串化的变量名。即使函数没有名称，也会如是限制成空字符串。如果它是使用Function构造函数创建的，则会标志成“anonymous”：

```javascript
function foo () {}
let bar = function () {};
let baz = () => {};
console.log(foo.name); // foo
console.log(bar.name); // bar
console.log(baz.name); // baz
console.log((() => {}).name); // (字符串)
console.log((new Function).name); // anonymous
```

​	如果是上面的那种情况，一个函数有多个函数名，那么它的name属性会是第一个函数的函数名。

```javascript
function sum(num1, num2) {
    return num1 + num2;
}
console.log(sum.name); // sum
console.log(sum(10, 10)); // 20
let anotherSum = sum;
console.log(sum.name); // sum
console.log(anotherSum.name); // sum
console.log(anotherSum(10, 10)); // 20
sum = null;
console.log(anotherSum(10, 10)); // 20
console.log(anotherSum.name); // sum
```

​	如果函数是一个获取函数、设置函数、或者使用bind()实例化，那么标识符前面会加上一个前缀：

```javascript
function foo() {}
console.log(foo.bind(null).name); // bound foo
let dog = {
    years: 1,
    get age() {
        return this.years;
    },
    set age(newAge) {
        this.years = newAge;
    }
}
let propertyDescriptor = Object.getOwnPropertyDescriptor(dog, 'age');
console.log(propertyDescriptor.get.name); // get age
console.log(propertyDescriptor.set.name); // set age
```

## 3.理解参数

​	ECMAScript函数的参数跟大多数其他语言不一样。ECMAScript函数即不关心传入的参数个数，也不关心这些参数的数据类型。定义函数时要接收两个参数，并不意味着调用时就传两个参数。你可以传一个、三个，甚至一个也不穿，解释器都不会报错。

​	比如有两个命名的参数，如果调用的时候什么参数也不传，两个命名参数再函数体里的值是undefined，如果传了一个则一个是传入的值另一个是undefined。如果传入的参数大于两个则只取前两个。

```javascript
function sayHi(name, message) {
    console.log('hello' + name + ',' + message);
}
sayHi();
```

​	使用function关键词定义的函数（非箭头函数），函数里面拥有内置对象arguments，arguments是一个类数组对象（但不是Array的实例），可以使用中括号语法访问其中的元素（第一个元素是arguments[0]，第二个元素是arguments[1]）。要确定传进来多少个参数，可以访问arguments.length属性。

```javascript
function sayHi() {
    console.log('Hello' + arguments[0] + ',' + arguments[1]);
}
```

​	命名参数可以和arguments对象联合使用

```javascript
function doAdd(num1, num2) {
    if (arguments.length === 1) {
        console.log(num1 + 10);
    } else if (arguments.length === 2) {
        console.log(arguments[0] + num2);
    }
}
```

​	箭头函数中的参数

​	如果函数是使用及那头函数语法定义的，那么传递给函数的参数将不能使用argumengs关键字访问，而只能通过定义的命名参数访问。

## 4.没有重载

​	ECMAScript函数不能像传统编程那样重载。在其他语言比如Java中，一个函数可以有两个定义，只要签名（接收参数的类型和数量）不同就行。ECMAScript函数没有签名。

​	如果在ECMAScript中定义了两个同名函数，则后定义的会覆盖先定义的。

```javascript
function addSomeNumber(num) {
    return num + 100;
}
function addSomeNumber(num) {
    return num + 200;
}
let result = addSomeNumber(100); // 300

// 同下是一样的，下面的好理解
let addSomeNumber = function(num) {
    return num + 100;
};
addSomeNumber = function(num) {
    return num + 200;
};
let result = addSomeNumber(100); // 300
```

## 5.默认参数值

​	ECMAScript5.1及以前，实现默认参数都是监测传进来的参数的值是不是undefined。

```javascript
function makeKing(name) {
    name = (typeof name != 'undefined') ? name : 'Henry';
    return `King ${name} VIII`;
}
console.log(makeKing()); // 'King Henry VIII'
console.log(makeKing('Louis')); // 'King Louis VIII'
```

​	ECMAScript6之后就不用这么麻烦了

```javascript
function makeKing(name = 'Henry') {
    return `King ${name} VIII`;
}
console.log(makeKing('Louis')); // 'King Louis VIII'
console.log(makeKing()); // 'King Henry VIII'
```

​	参数undefined的用法，相当于一个占位符，这个参数不传，传下一个参数

```javascript
function makeKing(name = 'Henry', numerals = 'VIII') {
    return `King ${name} ${numerals}`;
}
console.log(makeKing()); // 'King Henry VIII'
console.log(makeKing('Louis')); // 'King Louis VIII'
console.log(makeKing(undefined, 'VI')); // 'King Henry VI'
```

​	使用默认参数时，arguments对象的值不反映参数的默认值，只反映传给函数的参数。

```javascript
function makeKing(name = 'Henry') {
    name = 'Louis';
    return `King ${arguments[0]}`;
}
console.log(makeKing()); // 'King undefined'
console.log(makeKing('Louis')); // 'King Louis'
```

###默认参数作用域与暂时性死区

​	参数都是在函数作用域中有效的，程序都是顺序执行的，默认参数也是如此。

​	给多个参数定义默认值实际上跟使用let关键字顺序声明变量一样。来看下面的例子：

```javascript
function makeKing(name = 'Henry', numerals = 'VIII') {
    return `King ${name} ${numerals}`;
}
console.log(makeKing()); // King Henry VIII
// 上面的执行顺序相当于下面的
function makeKing() {
    let name = 'Henry';
    let numerals = 'VIII';
    return `King ${name} ${numerals}`;
}
console.log(makeKing()); // King Henry VIII
```

​	参数初始化遵循暂时性死区原则

```javascript
function makeKing(name = 'Henry', numerals = name) // 这个顺序没问题
function makeKing(name = numerals, numerals = name) // 这个顺序有问题
function makeKing(name = 'Henry', numerals = defaultNumeral) {
    let defaultNumeral = 'VIII';
    return `King ${name} ${numerals}`;
}
// 这个顺序没问题
```

## 6.参数扩展与收集

### 1.扩展参数

​	在ECMAScript6中，可以通过扩展操作符将可迭代的对象进行拆分，拆分后一次传入函数，

```javascript
let values = [1, 2, 3, 4];
function getSum() {
    let sum = 0;
    for (let i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}
console.log(getSum(...values)); // 10
```

​	通过...扩展操作符，只是将可迭代的对象进行拆分传入，并没有其他的操作。也可以与其他的参数一起传入。

```javascript
console.log(getSum(...values)); // 10
console.log(getSum(-1, ...values)); // 9
console.log(getSum(...values, 5)); // 15
console.log(getSum(-1, ...values, 5)); // 14
console.log(getSum(...values, ...[5, 6, 7])); // 28
```

