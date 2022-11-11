# 第2章 HTML中的JavaScript

## 2.1 < script >元素

​	将JavaScript插入HTML的主要方法时使用< script >元素

​	使用< script >的方式有两种：通过它直接在网页中嵌入JavaScript代码，以及通过它在网页中包含外部JavaScript文件。

​	包含在< script >内的代码会被从上到下解析。不管包含的时什么代码，浏览器都会按照< script >在页面中出现的顺序依次解析它们，前提是它们没有使用defer和async属性。

### 2.1.1 标签位置

​	过去，所有< script >元素都被放在页面的< head >标签内。

​	现代Web应用程序通常将所有JavaScript引用放在< body >元素中的页面内容后。

### 2.1.2 推迟执行脚本

​	HTML 4.01 为< script >元素定义了一个叫defer的属性。该脚本会被延迟到整个页面都解析完毕后在运行。HTML5规范要求脚本应该按照他们出现的顺序执行，因此第一个推迟的脚本会在第二个推迟的脚本之前执行，两者都会在DOMContentLoaded事件之前执行。

### 2.1.3 异步执行脚本

# 第3章 语言基础

## 3.3 变量

### 3.3.1 var、let、const声明函数的区别

1. var声明的范围是函数作用域，let、const声明的范围是块级作用域。var声明的全局作用域会将声明的变量挂到window上，可以直接俄window.变量名进行访问，而let、const则不会。
2. var声明的变量会把此次声明进行提升，提升到作用域顶部（只是声明提升，而不是赋值提升），在变量赋值之前进行变量的访问，不会报错只会返回undefined。let拥有暂时性死区，不会进行声明的提升，在变量赋值之前进行变量的访问，会抛出ReferenceError（引用错误）。


3. 因为使用var进行声明会把所有的声明都提前，相同函数名的声明进行合并，所以var可以重复声明。let不会提升声明，所以只允许声明一次，多次声明则会报错，但可以更该值。


4. const声明的是一个常数，必须在声明的时候进行赋值，不可以重复声明，更不可以更改值。但const声明的对象可以更改里面的属性。


5. for循环中的let声明

```javascript
for (var i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), 0)
}
// 你可能以为会输出 0、1、2、3、4
// 实际上会输出 5、5、5、5、5
for (let i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), 0)
}
// 会输出 0、1、2、3、4
```

6. 也不能用const来声明迭代变量（因为迭代变量会自增）

### 3.3.2 声明风格

​	不使用var，const优先，let次之。声明变量前要判断自己定义的变量的值会不会改变。

## 3.4 数据类型

​	ECMAScript有6种简单数据类型（也称原始数据类型）：Undefined、Null、Boolean、Number、String和Symbol。Symbol（符号）是ECMAScript 6新增的。还有一种复杂数据类型叫Object（对象）。

### 3.4.5 Number类型

Number类型使用IEEE 754格式表示整数和浮点数。整数默认是十进制，前缀是0则会当成8进制处理，前缀是0x则会当成16进制处理。

1. 浮点数数值中必须包含小数点，因为存储浮点值使用的内存空间是储存整数值的两倍，所以小数点后如果全是零，则该浮点数转换为整数。Number还可以用科学计数法表示。因为使用的是IEEE 754格式所以浮点数相加进行判断是错误的，因为0.1加0.2的得到得不是0.3.而是0.30000000000000004.


2. 值得范围：

   ​	最小值保存在Number.MIN_VALUE中，这个值在多数浏览器中是5e-324，任何无法表示的负数以-Infinity表示。

   ​	最小值保存在Number.MAX_VALUE中，这个值在多数浏览器中是1.7976931348623157e+308，任何无法表示的正数以Infinity表示

3. NaN一个特殊的数值，意思是“不是数值”。

4. 数值转换：

   ​	Numbe()r：可以用于任何数据类型，布尔值true为1、false为0；数值直接返回；null为0；undefined为NaN；字符串如果包含数值返回去掉前面的0的数值、如果为浮点数返回浮点数，如果是16进制返回相应的10进制、空字符串返回0、其他返回NaN；对象会先进行valueOf()方法，返回的值进行上述操作，如果返回的是NaN则还要进行toString()方法，再进行转换。

   ​	虽然可以用于任何数据类型，但相对复杂。

   ​	parseInt()：通常在需要得到整数时优先使用，parseInt解析字符串中包含的数值，开头要为数值，到非数值结束，可以传入第二个参数来指定解析的时按照几进制进行解析。

   ​	parseFloat()：解析字符串中的浮点数，只能解析十进制

### 3.4.6 String类型

1. 转换为字符串toString()方法可以接收一个底数参数。

   ```javascript
   let num = 10;
   console.log(num.toString()); // "10"
   console.log(num.toString(2)); // "1010"
   console.log(num.toString(8)); // "12"
   console.log(num.toString(10)); // "10"
   console.log(num.toString(16)); // "a"
   ```

   如果你不确定一个值是不是null或undefined，可以使用toString()转型函数，null返回的是null，undefined返回的是undefined。

2. 模板字面量标签函数

   ```javascript
   let a = 6;
   let b = 9;
   function simpleTag(strings,aValExpreession,bValExpression,sumExpression) {
       console.log(strings);
       console.log(aValExpreession);
       console.log(bValExpression);
       console.log(sumExpression);
       return 'foobar';
   };
   let untaggedResult = `${a} + ${b} =${a+b}`;
   let taggedResult = simpleTag`${a} + ${b} =${a+b}`;
   // ['', ' + ', ' =', '']
   // 6
   // 9
   // 15
   ```

   模板字符串标签函数传入的模板字符串，${ } 相当于形参，里面表达式相当于实参，strings是被分隔开的一个一个字符串，都汇总到一个数组里。如果有n个{}，则有n+1个参数传递。

3. String.raw标签函数

   ```javascript
   console.log(`first line\nsecond line`);
   // first line
   // second line
   console.log(String.raw`first line\nsecond line`);
   // first line\nsecond line
   console.log(String.raw`first line
   second line`);
   // first line
   // second line
   ```

### 3.4.7 Symbol 类型

​	Symbol（符号）是ECMAScript 6 新增的数据类型。符号是原始值，且符号实例是唯一、不可变的。符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。

1. 符号的基本用法

   符号需要使用Symbol()函数初始化。因为符号本身是原始类型，所以typeof操作符对符号返回symbol。

   ```javascript
   let genericSymbol = Symbol();
   console.log(genericSymbol);
   let fooSymbol = Symbol('foo');
   console.log(fooSymbol);
   ```



# 第4章 变量、作用域与内存

## 4.1原始值与引用值

​	ECMAScript变量可以包含两种不同类型的数据：原始值和引用值。原始值就是最简单的数据，引用值则是由多个值构成的对象。

​	原始值：Undefined、Null、Boolean、Number、String和Symbol。保存原始值的变量是按值访问的。

​	引用值是保存在内存中的对象。JavaScript不允许直接访问内存位置，所以访问的时候，操作的是对该对象的引用而非实际的对象本身。保存引用值的变量是按引用访问的。

### 4.1.1 动态属性

​	原始值可以直接对数据进行修改，引用值是对其属性进行修改。

```javascript
let person = new Object();
person.name = "Nicholas";
console.log(person.name); // "Nicholas"
```

### 4.1.2 复制值

​	原始值：通过变量把一个原始值赋值到另一个变量时，原始值灰白复制到新变量的位置。

​	引用值：从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置。区别在于，这里复制的值实际上是一个指针，它指向存储在堆内存中的对象。两个变量实际上指向同一个对象，因此一个对象上面的变化会在另一个对象上反映出来。

```javascript
let obj1 = new Object();
let obj2 = obj1;
obj1.name = "Nicholas";
console.log(obj2.name); // "Nicholas"
```

### 4.1.3传递参数

​	ECMAScript中所有函数的参数都是按值传递的。

```javascript
function setName(obj) {
    obj.name = "Nicholas";
    obj = new Object();
    obj.name = "Greg";
}

let person = new Object();
setName(person);
console.log(person.name); // "Nicholas"
```

​	当person传入setName()时，其name属性被设置为"Nicholas"。然后变量obj被设置为一个新对象且name属性被设置为"Greg"。如果person时按引用传递的，那么person应该自动将指针改为指向name为"Greg"的对象。可是，当我们再次访问person.name时，它的值是"Nicholas"，这表明函数中参数的值改变之后，原始的引用仍然没变。当obj在函数内部被重写时，它变成了一个指向本地对象的指针。而那个本地对象在函数执行结束时就被销毁了。

### 4.1.4确定类型

​	typeof操作符是用来判断一个变量是否为字符串、数值、布尔值或undefined最好的方式，但是如果值是null或对象，typeof返回的都是"Object"。

​	对于引用值来说，ECMAScript提供了instanceof操作符

```javascript
console.log(person instanceof Object); // 变量person是Object吗？
console.log(colors instanceof Array); // 变量colors是Array吗？
console.log(pattern instanceof RegExp); // 变量pattern是RegExp吗？
```

​	任何引用值和Object构造函数都会返回true，原始值始终会返回false。

## 4.2 执行上下文与作用域

​	执行上下文简称上下文，变量或函数的上下文决定了他们可以访问哪些数据，以及它们的行为。每个上下文都有一个关联的变量对象，而这个上下文中定义的所有变量和函数都存在于这个对象上。虽然无法通过代码访问变量对象，但后台处理数据会用到它。

​	全局上下文就是最外层的上下文，宿主环境不同全局上下文的对象也可能不一样。在浏览器中，全局上下文就是我们说的window对象。

​	上下文中的代码在执行的时候，会创建变量对象的一个作用域链。这个作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。代码正在执行的上下文的变量对象始终位于作用域链的最前端。作用域链中的下一个变量对象来自包含上下文，再下一个对象来自再下一个包含上下文。以此类推直至全局上下文；全局上下文的变量对象始终是作用域链的最后一个变量对象。

​	内部上下文可以通过作用域链访问外部上下文中的一切，但外部上下文无法访问内部上下文中的任何东西。上下文之的连接是线性的、有序的。每个上下文都可以到上一级上下文中去搜索变量和函数，但任何上下文都不能到下一级上下文中去搜索。注意：函数参数被认为是当前上下文中的变量，因此也跟上下文中的其他变量遵循相同的访问规则。

### 4.2.2 变量声明

4. 标识符查找

   ​	当在特定上下文中为读取或写入而引用一个标识符时，要通过搜索来确定这个标识符表示什么。搜索从局部作用域的最前端开始查找，局部作用域找不到将沿着作用域链进行搜索（注意，作用域链中的对象有一个原型链，因此搜索可能设计每个对象的原型链。），一直找到全局上下文的变量对象。如果仍任没有找到说明未声明这个标识符。

   ​	如果在局部作用域链中找到了，搜索就会停止，并引用该标识符。除非使用完全限定的写法window.标识符。

## 4.3 垃圾回收

​	JavaScript是使用垃圾回收的语言，垃圾回收程序每隔一定时间自动运行，将一些不用的变量销毁掉，内存释放掉。比如函数执行完就将局部使用的变量进行销毁。变量是否还有用，属于“不可判定的”问题，需要进行变量的跟踪。在浏览器的发展史上，用到过两种标记策略：标记清理和引用计数。

### 4.3.1 标记清理

​	JavaScript最常用的垃圾回收策略是标记清理。

​	垃圾回收程序运行的时候，将标记内存中存储的所有变量。它会将所有在上下文中的变量，以及被在上下文中的变量引用的变量的标记去掉。在此之后再被加上标记的变量就是待删除的了，原因是任何在上下文中的变量都访问不到它们了。随后垃圾回收程序做一次内存清理，销毁带标记的所有值并收回它们的内存。

### 4.3.2 引用计数

​	另一种没那么常用的垃圾回收策略是引用计数。其思路是对每个值都记录它被引用的次数。声明变量时引用数为1，同一个值被赋给另一个变量，引用数加1，如果保存该值的变量被覆盖了，引用数减1。当引用数为0时，就说明没办法再访问到这个值了，因此就可以安全低回收了。

​	非常严重的问题：循环引用。所谓循环引用，就是对象A有一个指针指向对象B，而对象B也引用了对象A。

```javascript
function problem() {
    let objectA = new Object();
    let objectB = new Object();

    objectA.someOtherObject = objectB;
    objectB.anotherObject = objectA;
}
```

​	引用数永远不会变成0，会导致大量内存不会被释放。

### 4.3.4 内存管理

​	分配给浏览器的内存通常比分配给桌面软件的要少很多，分配给移动浏览器的就更少了。为了让页面有更好的性能，我们应该将不在需要用的全局数据的值设置为null，从而释放其引用。

```javascript
function createPerson(name) {
    let localPerson = new Object();
    localPerson.name = name;
    return localPerson;
}
let globalPerson = createPerson("Nicholas");
// 接触globalPerson对值的引用
globalPerson = null;
```

​	注意，解除对一个值的引用并不会自动导致相关内存被回收。解除引用的关键在于确保相关的值已经不在上下文里了，因此它在下次垃圾回收时会被回收。

1. 通过const和let声明提升性能

   ​	ES6增加这两个关键字不仅有助于改善代码风格，而且同样有助于改善垃圾回收的过程。因为const、let都是块级作用域比var的函数作用域更早的终止，短时间内创建变量、销毁变量，不会给内存造成压力。

2. 隐藏类和删除操作

   ​	在运行的时候，V8引擎会将创建的对象与隐藏类关联起来。能够共享相同的隐藏类性能会更好。

   ```javascript
   function Article() {
       this.title = 'Inauguration Ceremony Features Kazoo Band';
   }
   let a1 = new Article();
   let a2 = new Article();
   ```

   ​	这两个类实例共享相同的隐藏类，因为这两个实例共享同一个构造函数和原型。假如之后有添加了下面这行代码：

   ```javascript
   a2.author = 'Jake';
   ```

   ​	此时两个Article实例就会对应两个不同的隐藏类。会使性能下降。

   ​	解决方案“先创建在补充”

   ```javascript
   function Article(opt_author) {
       this.title = 'Inauguration Ceremony Features Kazoo Band';
       this.author = opt_author;
   }
   let a1 = new Article();
   let a2 = new Article('Jake');
   ```

   ​	这样两个实例基本是一样的，因此可以共享一个隐藏类，不会引起性能的下降。

   ​	但不要用delete关键字。

   ```javascript
   function Article() {
       this.title = 'Inauguration Ceremony Features Kazoo Band';
       this.author = 'Jake';
   }
   let a1 = new Article();
   let a2 = new Article();
   delete a1.author;
   ```

   ​	这样会生成不一样的隐藏类，最好的方法是将不想要的属性设置为null。

   ```javascript
   function Article() {
       this.title = 'Inauguration Ceremony Features Kazoo Band';
       this.author = 'Jake';
   }
   let a1 = new Article();
   let a2 = new Article();
   a1.author = null;
   ```

3. 内存泄露

   内存泄露主要是一下三个方面：

   1. 意外声明全局变量，一般都是没有使用关键字声明关键字，直接使用关键字声明就可以解决。

   2. 定时器导致内存泄漏

      ```javascript
      let name = 'Jake';
      setInterval(() => {
          console.log(name);
      }, 100);
      ```

      只要定时器不关，将会一直占用外部变量的内存。

   3. JavaScript闭包导致的内存泄漏

      ```javascript
      let outer = function() {
          let name = 'Jake';
          return function() {
              return name;
          };
      };
      ```

      只要返回的函数存在就不能清理所用到的变量，就会一直占用内存。

4. 静态分配与对象池


   ​	提升JavaScript性能，压榨浏览器，如何减少浏览器执行垃圾回收的次数。

   ​	浏览器决定何时运行垃圾回收程序的一个标准就是对象更替的速度。频繁的初始化对象，对象超出了作用域就会被回收。回收的次数太快影响性能。

   ​	使用对象池，一次性创建好要使用的对象，使用的时候向对象池请求一个对象，设置其属性，使用它，不使用的时候将值赋值为null，对象池的使用的结构是数组。

# 第5章 基本引用类型

​	对象被认为是某个特定引用类型的实例。新对象通过使用new操作符后跟一个构造函数来创建。

## 5.1 Date

​	要创建日期对象，就要使用new操作符来调用Date构造函数：

```javascript
let now = new Date();
```

​	在不给Date构造函数传入参数的情况下，创建的对象将保存当前的时间。要用特定的时间，两个辅助方法：

1. Date.parse()

   ```javascript
   let someDate = new Date(Date.parse("May 23,2019"));
   // 或者省略Date.parse
   let someDate = new Date("May 23,2019");
   ```

2. Date.UTC()

   ```javascript
   // 格式：年 (0-11)月 日 时 分 秒
   let allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));
   // 或者省略Date.UTC
   let allFives = new Date(2005, 4, 5, 17, 55, 55);
   ```

### 5.1.1 继承的方法

