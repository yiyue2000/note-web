# ES 6 简记

### 一、let & const



**补充：** <font color = blue>关于 JS 中的变量提升：</font>

JS有个特性。它会在编译阶段，把函数、变量声明提前到作用域的最前面。有时候，你没有对变量、函数进行声明，使用却可以正常使用。例如下面的例子：

~~~javascript
catName("Chloe");

function catName(name) {
    console.log("我的猫名叫 " + name);
}

/*
代码执行的结果是: "我的猫名叫 Chloe"
*/
~~~

noting：函数和变量相比，会被优先提升。这意味着函数会被提升到更靠前的位置。

几个特点：

- JavaScript 只会提升声明，不会提升其初始化。
- 提升是：会把 var a = 1；分成两个部分来看。一个是 var a，会被提前。一个是 a = 1；不会被提前。



**补充：** <font color = blue>var 的重复声明</font>

1.使用var语句多次声明一个变量不仅是合法的,而且也不会造成任何错误.

2.如果重复使用的一个声明有一个初始值,那么它担当的不过是一个赋值语句的角色.

3.如果重复使用的一个声明没有一个初始值,那么它不会对原来存在的变量有任何的影响.





1. let 类型：

   使用 let 声明的变量，具有了一般的作用域的执行情况。

   也就是说，在 for 循环中，很适合用 let 来声明。

   在 JS 中还有一个比较独特的特性，就是，循环变量设置的地方是一个 **父作用域**，而循环体内部是一个 **子作用域**。

   - var 会出现变量提升的情况：

     变量可以在声明之前使用，值为undefined。

     例如：

     ~~~javascript
     console.log(foo); // 输出undefined
     var foo = 2;
     ~~~

     但是 **let 不会变量提升**。

   - 暂时性死区：

     在作用域内部出现了 let 定义的变量，那么它就会在这个作用域内形成一个封闭的闭环。外界都不会干扰它。举个例子：

     ~~~javascript
     var tmp = 123;
     
     if (true) {
       tmp = 'abc'; // ReferenceError,出现错误
       let tmp;
     }
     ~~~

     其中，`let temp;` 在这里面绑定了下面 if 构造的作用域，上面的 `temp = 'abc';` 就等于是在定义域内，没有定义 temp 就使用了，所以出现了错误。

     在代码块内，使用 `let` 命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

     最好养成良好习惯，**在定义之后才使用变量。** 

     “暂时性死区”也意味着`typeof`不再是一个百分之百安全的操作。

     ES6 规定暂时性死区和`let`、`const`语句 **不出现变量提升**，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。

   - `let`不允许在相同作用域内，重复声明同一个变量。

     因此，不能在函数内部重新声明参数。下面给出例子：

     ~~~javascript
     function func(arg) {
       let arg; // 重新声明了参数
     }
     func() // 报错
     
     function func(arg) {
       {
         let arg;
       }
     }
     func() // 不报错
     ~~~

2. 块级作用域：

   - 可以任意嵌套块级作用域。
   - 内层作用域可以定义外层作用域的同名变量。
   - ES6 引入了块级作用域，明确**允许在块级作用域之中声明函数**。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在**块级作用域之外不可引用**。

3. const 命令：

   - `const`声明一个只读的常量。一旦声明，常量的值就不能改变。

     举个例子：

     ~~~javascript
     const PI = 3.1415;
     PI // 3.1415
     
     PI = 3;
     // TypeError: Assignment to constant variable.
     ~~~

   - `const`声明的变量不得改变值，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。（这倒是和CPP很像）如果只声明不赋值，就会报错。

   - 与`let`命令相同：只在声明所在的块级作用域内有效。

   - `const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

   - 与`let`一样不可重复声明。

   - const 的本质：

     `const`实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。

     对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，`const`只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

     也就是说，变量里面存的地址不变（也就是const本义），但是指向的地方它无法控制。

     给个例子：

     ~~~javascript
     const a = [];
     a.push('Hello'); // 可执行 对象不会被限制
     a.length = 0;    // 可执行
     a = ['Dave'];    // 报错 不能将 a 指向其他内存地区
     ~~~



**小结：**

<font color = red><u>ES6 一共有 6 种声明变量的方法</u></font>

var , let , const , function , class , import



### 二、变量解构赋值

1. 数组解构：

   ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构。

   比如下面的例子：

   ~~~javascript
   let [a, b, c] = [1, 2, 3];
   // a = 1, b = 2, c = 3
   ~~~

   本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。而且这种解构赋值也可以嵌套使用。看一看下面的例子：

   ~~~javascript
   let [foo, [[bar], baz]] = [1, [[2], 3]];
   foo // 1
   bar // 2
   baz // 3
   ~~~

   如果解构不成功，变量的值就等于`undefined`。下面给出例子：（也就是没有东西和它匹配）

   ~~~javascript
   let [foo] = [];
   // foo = undefined
   
   let [head, ...tail] = [1, 2, 3, 4];
   head // 1
   tail // [2, 3, 4]  ...的特别用法
   
   let [x, y, ...z] = ['a'];
   x // "a"
   y // undefined
   z // []
   ~~~

   有时会不完全解构。即：右边比左边多。也可以成功。看下面的例子：

   ~~~javascript
   let [x, y] = [1, 2, 3];
   x // 1
   y // 2
   
   let [a, [b], d] = [1, [2, 3], 4];
   a // 1
   b // 2
   d // 4
   ~~~

   如果等号右边不是数组，就会报错：

   ~~~javascript
   let [foo] = 1;
   let [foo] = false;
   let [foo] = NaN;
   let [foo] = undefined;
   let [foo] = null;
   let [foo] = {};
   ~~~

   解构赋值允许指定默认值。ES6 内部使用严格相等运算符（`===`），判断一个位置是否有值。所以，只有当一个数组成员严格等于`undefined`，默认值才会生效。

   给出示例：

   ~~~javascript
   let [x = 1] = [undefined];
   x // 1
   
   let [x = 1] = [null];
   x // null
   ~~~



### 三、一些类型的改进：

1. 运算符：

   新添加了 ** 运算符，表示指数运算（和 py 类似）

2. 允许函数参数有默认值：

   可以写成：

   ~~~javascript
   function A(a = 1, x = 2){
   	......
   }
   ~~~

   参数变量是默认声明的，所以不能用`let`或`const`再次声明。

   使用参数默认值时，函数不能有同名参数。例如下面的例子：

   ~~~javascript
   // 不报错
   function foo(x, x, y) {
     // ...
   }
   
   // 报错
   function foo(x, x, y = 1) {
     // ...
   }
   // SyntaxError: Duplicate parameter name not allowed in this context
   ~~~

   参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值是惰性求值的。不是当你定义的时候就决定了，类似于一个快照，而是动态变化的。看下面的例子：

   ~~~javascript
   let x = 99;
   function foo(p = x + 1) {
     console.log(p);
   }
   
   foo() // 100
   
   x = 100;
   foo() // 101
   ~~~

   默认值也可以和 对象解构 联系在一起。

   ~~~javascript
   function foo({x, y = 5}) {
     console.log(x, y);
   }
   
   foo({}) // undefined 5 相当于 {x, y = 5} = {}，解构得到x=undefined, y=undefined, 但是 y 有默认值。
   foo({x: 1}) // 1 5
   foo({x: 1, y: 2}) // 1 2
   foo() // TypeError: Cannot read property 'x' of undefined
   ~~~

3. 

4. 箭头函数：

   - 基本语法：

     ~~~javascript
     (param1, param2, …, paramN) => { statements }
     (param1, param2, …, paramN) => expression
     //相当于：(param1, param2, …, paramN) =>{ return expression; }
     
     // 当只有一个参数时，圆括号是可选的：
     (singleParam) => { statements }
     singleParam => { statements }
     
     // 没有参数的函数应该写成一对圆括号。
     () => { statements }
     ~~~

     括号里面是函数参数。

     如果只有一个参数，那么这个参数是可选的。

     没有参数需要写 ()。

     如果箭头函数的代码块部分 **多于一条语句**，就要使用大括号将它们括起来，并且使用`return`语句返回。

   - 如果箭头函数直接返回一个对象字面量，因为大括号 {} 是表示代码块的，所以要在 {} 外面加上 ()。

     下面给出例子：(由于只有一个语句，即返回一个 对象字面量，所以直接写了)

     ~~~javascript
     // 报错
     let getTempItem = id => { id: id, name: "Temp" };
     
     // 不报错
     let getTempItem = id => ({ id: id, name: "Temp" });
     ~~~

   - 几点注意事项：

     - **不可以当作构造函数**，也就是说，不可以使用`new`命令，否则会抛出一个错误。
     
  - 不可以使用`arguments`对象，**该对象在函数体内不存在**。如果要用，可以用 rest 参数代替。
    
  - 不可以使用`yield`命令，因此箭头函数**不能用作 Generator 函数**。
    
  - 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。
    
  - 箭头函数**在参数和箭头之间不能换行**。
    
       也就是说，这样写不可以：
       
       ~~~javascript
       var func = ()
                  => 1;
       // SyntaxError: expected expression, got '=>'
       ~~~
     
       
     ​     
     **Point 3** 的含义？
     
     箭头函数不会创建自己的`this`,它只会从自己的 **作用域链的上一层** 继承`this`。
     
     **<font color = blue>补充</font>**：
     
     this 在其他函数里面都是什么情况呢？
     
     - 如果是该函数是一个**构造函数**，this指针指向一个新的对象
     - 在严格模式下的函数调用下，this指向undefined
     - 如果是该函数是一个对象的方法，则它的this指针指向这个对象
     - 可以使用 **闭包**、**递归** 等。
     - 一般的函数，指向 **window** 全局作用域
     
     在函数内部，`this`的值取决于函数被调用的方式。
     
     当代码不在严格模式下，且 `this` 的值不是由该调用设置的，所以 `this` 的值默认指向全局对象，浏览器中就是 `window`。在严格模式下，如果进入执行环境时没有设置 `this` 的值，`this` 会保持为 `undefined`。




### 四、Symbol 类型

1. 基本概念：

   Symbol 是 ES6 新定义的一种类型。有的时候你使用别人设计好的 类 / 函数，想要新增加一个属性，但是这个属性名称有可能就和之前的一些属性名重复了。

   Symbol 类型 是一种 **不会重复** 的类型。你可以像下面这样来定义它：

   ~~~javascript
   let s = Symbol();
   ~~~

   - 注意：

     Symbol 类型 生成的时候，前面 **不可以加上 new**。Symbol 是一个原始类型。

   有时候可以给 symbol 类型 加上描述，便于区分：

   ~~~javascript
   let s1 = Symbol('foo');
   s1.toString(); //  "Symbol(foo)"
   ~~~

   - 注意：

     注意，`Symbol`函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的`Symbol`函数的返回值是不相等的。

     也就是说：

     ~~~javascript
     let s1 = Symbol('foo');
     let s2 = Symbol('foo');
     
     s1 === s2; // false
     ~~~

   Symbol 不可以和其他值一起运算。可以转换为字符串，（如上面的 toString）也可以和 Boolean 一起。默认为 true。

2. **Symbol 的描述**：description

   上面括号中传入的参数，就是 Symbol 类型的描述参数。

   可以使用下面的方式获得这个参数：

   ~~~javascript
   const sym = Symbol('foo');
   
   sym.description // "foo"
   ~~~

3. 作为 对象 属性的 Symbol：

   有以下几个需要注意的地方：

   - Symbol 值作为对象属性名时，不能用点运算符。

     下面给出例子：

     ~~~javascript
     const mySymbol = Symbol();
     const a = {};
     
     a.mySymbol = 'Hello!';
     a[mySymbol] // undefined 没找到，字符串类型才是 key
     a['mySymbol'] // "Hello!"
     ~~~

     因为点运算符后面总是字符串，所以不会读取`mySymbol`作为标识名所指代的那个值，导致`a`的属性名实际上是一个字符串，而不是一个 Symbol 值。

   - 在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。

     ~~~javascript
     let s = Symbol();
     
     let obj = {
       [s]: function (arg) { ... }
     };
     
     obj[s](123);
     ~~~

   - Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。

4. Symbol 属性的遍历：

   Symbol类型作为对象的一个属性的时候，是无法被 for-in 等 遍历到的。不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。

   但是有一个方法，可以找到对象中所有 Symbol 类型的属性，并且整合成一个数组返回。

   是：`Object.getOwnPropertySymbols()`

   - 另一个新的 API，`Reflect.ownKeys()`方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

     ~~~javascript
     let obj = {
       [Symbol('my_key')]: 1,
       enum: 2,
       nonEnum: 3
     };
     
     Reflect.ownKeys(obj)
     //  ["enum", "nonEnum", Symbol(my_key)]
     ~~~

   所以，我们可以：

   利用 Symbol 这个特性，为对象定义一些非私有的、但又希望只用于内部的方法。

5. Symbol.for()，Symbol.keyFor()：

   有时，我们希望重新使用同一个 Symbol 值，`Symbol.for()`方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。

   ~~~javascript
   let s1 = Symbol.for('foo');
   let s2 = Symbol.for('foo');
   
   s1 === s2 // true
   ~~~

   `Symbol.keyFor()`方法返回一个 **已登记** 的 Symbol 类型值的`key`。（也就是已经存在）

   下面给出例子：

   ~~~javascript
   let s1 = Symbol.for("foo");
   Symbol.keyFor(s1) // "foo" 通过 .for 来得到的，之前已经登记了
   
   let s2 = Symbol("foo");
   Symbol.keyFor(s2) // undefined 由于这个时候是刚刚创建
   ~~~



### 五、Set & Map 数据结构：

##### （一）Set 数据结构：

1. 基本概念：

   类似于数学里面的集合。Set之中没有重复的元素。

   Set是一个构造函数。下面是一些构造Set的方法：

   ~~~javascript
   const s = new Set(); // 空的Set
   const set = new Set([1, 2, 3, 4, 4]); // 使用数组初始化，得到：[1,2,3,4]
   ~~~

   也可以用于，去除字符串里面的重复字符。

   ~~~javascript
   [...new Set('ababbc')].join('')
   // "abc"
   ~~~

   set 内来判断元素是否相等的方法，和 === 类似，也就是当严格相等的时候，才会不添加进去。

   两个对象的值永远都不等。

2. 一些方法 和 属性：

   - `Set.prototype.size`：返回`Set`实例的成员总数。

   - `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。

   - `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。

   - `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。

   - `Set.prototype.clear()`：清除所有成员，没有返回值。

   - `Array.from`方法可以将  **Set 结构转为数组**。

     ~~~javascript
     const items = new Set([1, 2, 3, 4, 5]);
     const array = Array.from(items);
     ~~~

3. Set 的遍历：

   `Set`的遍历顺序就是插入顺序。

   下面是一些遍历方法：

   - `keys()`，`values()`，`entries()`：返回的都是遍历器(Iterator)对象。于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

   - `forEach()`：

     类似于数组的 forEach 函数。

   - 使用 Set 可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）。

     ~~~javascript
     let a = new Set([1, 2, 3]);
     let b = new Set([4, 3, 2]);
     
     // 并集
     let union = new Set([...a, ...b]);
     // Set {1, 2, 3, 4}
     
     // 交集
     let intersect = new Set([...a].filter(x => b.has(x)));
     // set {2, 3}
     
     // （a 相对于 b 的）差集
     let difference = new Set([...a].filter(x => !b.has(x)));
     // Set {1}
     ~~~



##### （二）Map 数据结构：

1. 基本概念：

   JS的对象，本质上是键值对的集合，但是传统上**只能用字符串**当作键。这给它的使用带来了很大的限制。

   Map 于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应。

   下面给出示例：

   ~~~javascript
   const m = new Map();
   const o = {p: 'Hello World'};
   
   m.set(o, 'content') // 添加新的键值对
   m.get(o) // "content" 通过key，获得value
   
   m.has(o) // true
   m.delete(o) // true
   m.has(o) // false
   ~~~

   - 如果对同一个键多次赋值，后面的值将覆盖前面的值。

   - 如果读取一个未知的键，则返回`undefined`。

     ~~~javascript
     new Map().get('asfddfsasadf')
     // undefined
     ~~~

   - 注意：

     对象作为key的时候，如果只是内容一样，那么可能不会是同一个key。

     需要看 **内存地址** 是不是相同。见下面的例子：

     ~~~javascript
     const map = new Map();
     
     map.set(['a'], 555);
     map.get(['a']) // undefined
     ~~~

     上面代码中，变量`k1`和`k2`的值是一样的，但是它们在 Map 结构中被视为两个键。

2. 一些 属性 和 方法：

   - `size`属性返回 Map 结构的成员总数。（多少个键值对）

   - `Map.prototype.set(key, value)`：

     添加键值对。**返回**的是当前的`Map`对象，因此**可以采用链式写法**。

   - `Map.prototype.get(key)`:

     `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

   - `Map.prototype.has(key)`:

     `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

   - `Map.prototype.delete(key)`：(返回 **Boolean** 类型)

     `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

   - `Map.prototype.clear()`:

     `clear`方法清除所有成员，没有返回值。

3. 遍历方法：

   Map 结构原生提供三个遍历器生成函数和一个遍历方法。

   Map 的**遍历顺序**就是**插入顺序**。

   - `Map.prototype.keys()`：返回键名的遍历器。

   - `Map.prototype.values()`：返回键值的遍历器。

   - `Map.prototype.entries()`：返回所有成员的遍历器。

     ~~~javascript
     for (let item of map.entries()) {
       console.log(item[0], item[1]);
     }
     // "F" "no"
     // "T" "yes"
     
     // 或者
     for (let [key, value] of map.entries()) {
       console.log(key, value);
     }
     // "F" "no"
     // "T" "yes"
     
     // 等同于使用map.entries()
     for (let [key, value] of map) {
       console.log(key, value);
     }
     // "F" "no"
     // "T" "yes"
     ~~~

     

   - `Map.prototype.forEach()`：遍历 Map 的所有成员。

   - Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

     ~~~javascript
     const map = new Map([
       [1, 'one'],
       [2, 'two'],
       [3, 'three'],
     ]);
     
     [...map.keys()]
     // [1, 2, 3]
     
     [...map.values()]
     // ['one', 'two', 'three']
     
     [...map.entries()]
     // [[1,'one'], [2, 'two'], [3, 'three']]
     
     [...map]
     // [[1,'one'], [2, 'two'], [3, 'three']]
     ~~~

   - 结合数组的`map`方法、`filter`方法，可以实现 Map 的遍历和过滤（Map 本身没有`map`和`filter`方法）。

     ~~~javascript
     const map0 = new Map()
       .set(1, 'a')
       .set(2, 'b')
       .set(3, 'c');
     
     const map1 = new Map(
       [...map0].filter(([k, v]) => k < 3)
     );
     // 产生 Map 结构 {1 => 'a', 2 => 'b'}
     
     const map2 = new Map(
       [...map0].map(([k, v]) => [k * 2, '_' + v])
         );
     // 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
     ~~~

   - `forEach`方法还可以接受第二个参数，用来绑定`this`。

4. Map 和 其他类型相互转换：

   - Map 转 数组：

     使用 ... 运算符：

     ~~~javascript
     const myMap = new Map()
       .set(true, 7)
       .set({foo: 3}, ['abc']);
     [...myMap]
     // [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
     ~~~

   - 数组 转 Map：

     直接作为参数，传入 Map 的构造函数：

     ~~~javascript
     new Map([
       [true, 7],
       [{foo: 3}, ['abc']]
     ])
     // Map {
     //   true => 7,
     //   Object {foo: 3} => ['abc']
     // }
     ~~~

   - Map 转 对象：

     如果 Map 的 key都是string，那么可以直接转。如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

     （由于调用 toString ？）

     ~~~javascript
     function strMapToObj(strMap) {
       let obj = Object.create(null);
       for (let [k,v] of strMap) {
         obj[k] = v;
       }
       return obj;
     }
     
     const myMap = new Map()
       .set('yes', true)
       .set('no', false);
     strMapToObj(myMap)
     // { yes: true, no: false }
     ~~~

   - 对象转 Map：

     可以通过`Object.entries()`。

     ~~~javascript
     let obj = {"a":1, "b":2};
     let map = new Map(Object.entries(obj));
     ~~~



### 六、Iterator：迭代器

1. 遍历简介：

   每种类型可以用不同的方式进行遍历：

   - 数组：

     可以用 for （自己造一个），arr.forEach() 函数，for-in 循环，for-of循环。

   - Set ：

     使用 for-of 循环

   - 对象（Object）：

     使用 for-in 循环

   能不能有一种统一的迭代方式呢？于是产生了迭代器 Iterator。

2. 关于迭代器：

   [Symbol.iterator]属性名是固定的写法，只要**拥有了该属性的对象**，就能够用迭代器的方式进行遍历。

   迭代器的遍历方法是首先获得一个迭代器的指针，初始时该指针指向第一条数据之前。接着通过调用next方法，改变指针的指向，让其指向下一条数据。每一次的next都会返回一个对象。

   it.next() 的属性：

   - value：想要获取的数据
   - done：Boolean，false表示当前指针指向的数据有值。true表示遍历已经结束。

   下面给出一个遍历数组的例子：

   ~~~javascript
   let ary = [1,2,3];
   let it = ary[Symbol.iterator](); // 获取数组中的迭代器
   console.log(it.next()); // { value: 1, done: false }
   console.log(it.next()); // { value: 2, done: false }
   console.log(it.next()); // { value: 3, done: false }
   console.log(it.next()); // { value: undefined, done: true }
   ~~~

   看得出，通过 取出 创建的数组 arr 中的 Symbol.iterator 属性，得到对应的迭代器。

3. 集合取得iterator:

   下面给出一个示例：

   ~~~javascript
   let list = new Set([1,2,3]);
   let it = list.entries(); // 获取set集合中的迭代器
   ~~~

   使用 .entries() 来获得。

4. 原生具备 Iterator 接口的数据结构如下：

   - Array
   - Map
   - Set
   - String
   - TypedArray
   - 函数的 arguments 对象
   - NodeList 对象

   对于**原生部署 Iterator 接口**的数据结构，不用自己写遍历器生成函数，`for...of`循环会自动遍历它们。

   **其他数据结构**（主要是对象）的 Iterator 接口，都需要自己在`Symbol.iterator`属性上面部署，这样才会被`for...of`循环遍历。（也就是自己去实现）



### 七、Class ：

1. 实际上，Class 语法是类似于一种语法糖，JS本质上没有什么太大改变。可以说，class语法是JS的构造函数的另外一种写法。

2. 基本写法：

   - 构建 class：

     ~~~javascript
     class Point {
       constructor(x, y) {
         this.x = x;
         this.y = y;
       }
     
       toString() {
         return '(' + this.x + ', ' + this.y + ')';
       }
     }
     ~~~

     constructor：构造函数

     toString：这个 class 中的一个方法

     <font color = red>**注意：**</font>

     - 方法与方法之间不需要逗号分隔，加了会报错。
     - 定义`toString()`方法的时候，前面不需要加上`function`这个关键字。

   - 创建对象：

     直接使用 new 命令：

     ~~~javascript
     class Bar {
       doStuff() {
         console.log('stuff');
       }
     }
     
     const b = new Bar();
     b.doStuff() // "stuff"
     ~~~

     事实上，prototype 属性还是存在的，而且所有在 class 里面写出来的方法，都是prototype中的方法。

     可以使用 `Object.assign()` 方法，向类一次性添加很多个方法。

     ~~~javascript
     class Point {
       constructor(){
         // ...
       }
     }
     
     Object.assign(Point.prototype, { // 第一个参数：需要添加的类的 原型
       toString(){}, // 需要添加的方法
       toValue(){}
     });
     ~~~

   - 一些 tips：

     - `prototype`对象的`constructor()`属性，直接指向“类”的本身。

       看看下面的 code：

       ~~~javascript
       Point.prototype.constructor === Point // true
       ~~~

     - 类的内部所有定义的方法，都是不可枚举的（non-enumerable）。

       也就是说，不可以用 for - of 循环来访问。

   - constructor 方法：

     基本思想和CPP差不多。

     如果没有显式定义，一个空的`constructor()`方法会被默认添加。

     `constructor()`方法默认返回实例对象（即`this`）。

     类 <font color = red>**必须** </font>使用`new`调用，否则会报错。这是它跟普通构造函数的一个主要区别。

   - 实例：

     如果写了 this.x = x 这样类似的语句，那么这个属性是属于对象自身的，如果不是，那么属于原型。

     每个实例都有一个 **\__proto__** 属性，指向这个对象的原型。不过不希望使用这个属性。以免造成对环境的依赖。

3. 一些 ES5 的特性复现：

   - set 和 get：

     可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。下面给出一个示例：

     ~~~javascript
     class MyClass {
       constructor() {
         // ...
       }
       get prop() {
         return 'getter';
       }
       set prop(value) {
         console.log('setter: '+value);
       }
     }
     
     let inst = new MyClass();
     
     inst.prop = 123;
     // setter: 123
     
     inst.prop
     // 'getter'
     ~~~

     上面写的 prop属性，它的赋值和读取行为都被自定义了。

4. 可以使用表达式形式定义类：

   下面给出一个小例子 ~

   ~~~javascript
   const MyClass = class Me {
     getClassName() {
       return Me.name;
     }
   };
   ~~~

   注意：

   这个类的名字是`Me`，但是`Me`只在 Class 的内部可用，指代当前类。在 Class 外部，这个类只能用`MyClass`引用。

   （也就是说，既然是表达式形式，而且赋值给了 MyClass 这个变量，那么以后使用的话《在外部》，就用这个变量名称。内部不知道外面发生了这个事情，所以还用 Me）

   如果类的内部没用到的话，可以省略`Me`，也就是可以写成下面的形式。

   ~~~javascript
   const MyClass = class { /* ... */ };
   ~~~

5. 注意 tips：

   - 类和模块的内部，默认就是严格模式

   - 类不存在变量提升：

     下面给出一个示例：

     ~~~javascript
     new Foo(); // ReferenceError
     class Foo {}
     ~~~

     上面代码中，`Foo`类使用在前，定义在后，这样会报错，因为 ES6 **不会**把**类的声明**提升到代码头部。

     <font color = blue>所以说可以变量提升的只有 var 咯？/斜眼笑</font>

   - 内部可以定义一个 generator 函数。

   - 关于 this：

     类的方法内部如果含有`this`，它默认指向类的实例。但是，必须非常小心，一旦单独使用该方法，很可能报错。

6. 静态方法：

   类相当于实例的原型，所有在类中定义的方法，都会被实例继承。

   如果在一个方法前，加上`static`关键字，就表示该方法 **不会被实例继承**，而是直接通过类来调用，这就称为“静态方法”。

   下面给出例子 ~ 

   ~~~javascript
   class Foo {
     static classMethod() {
       return 'hello';
     }
   }
   
   Foo.classMethod() // 'hello'
   
   var foo = new Foo();
   foo.classMethod()
   // TypeError: foo.classMethod is not a function
   ~~~

   **直接使用 类名 调用哦！**

   **<font color = red>注意：</font>**

   - 如果静态方法包含`this`关键字，这个`this`指的是类，而不是实例。
   - 静态方法可以与非静态方法重名。
   - 父类的静态方法，可以被子类继承。

7. 类的属性的写法：

   除了可以使用类似 this.x = x 这种写在 constructor 中的写法，还可以放在类定义的最顶层。

   康康下面的例子：

   ~~~javascript
   class IncreasingCounter {
     _count = 0;
     get value() {
       console.log('Getting the current value!');
       return this._count;
     }
     increment() {
       this._count++;
     }
   }
   ~~~

   当然，这样写还是实例属性。

8. 静态属性：

   静态属性是 class 的属性，不是对象的属性。

   现在 ES6 只有一种写法可以用：

   ~~~javascript
   class Foo {
   }
   
   Foo.prop = 1;
   Foo.prop // 1
   ~~~

   上面的 prop 就是一个 静态属性啦。

9. 私有属性 和 私有函数 问题：

   可以使用 symbol 类型，来隐藏这些方法：

   ~~~javascript
   const bar = Symbol('bar');
   const snaf = Symbol('snaf');
   
   export default class myClass{
   
     // 公有方法
     foo(baz) {
       this[bar](baz);
     }
   
     // 私有方法
     [bar](baz) {
       return this[snaf] = baz;
     }
   
     // ...
   };
   ~~~

   一般情况下，拿不到这个用 symbol 来定义函数名称的方法。



### 八、Class 的 继承：

1. 基本写法：

   可以通过`extends`关键字实现继承。

   下面给出例子：

   ~~~javascript
   class Point {
   }
   
   class ColorPoint extends Point {
     constructor(x, y, color) {
       super(x, y); // 调用父类的constructor(x, y)
       this.color = color;
     }
   
     toString() {
       return this.color + ' ' + super.toString(); // 调用父类的toString()
     }
   }
   ~~~

   解释一下~

   - `super`关键字：

     它在这里表示父类的构造函数，用来新建父类的`this`对象。

     子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。

     <font color = blue>*原因：*</font>

     <font color = green>*子类自己的 `this` 对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用`super`方法，子类就得不到 `this` 对象。*</font>

     tips:

     <font color = #ff5a8c>*ES5 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。*</font>

   - 注意：

     - 在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字，否则会报错。
     - 父类的静态方法，也会被子类继承。

2. 判断是否有继承关系：

   `Object.getPrototypeOf`方法可以用来从子类上获取**父类**。（不是原型什么的哦）

   下面给个例子！

   ~~~javascript
   Object.getPrototypeOf(ColorPoint) === Point
   // true
   ~~~

3. super：

   super可以作为 函数 和 对象 使用。

   - 函数：

     作为函数的时候，也就是父类的构造函数。

   - 对象：

     作为对象的时候，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

     也就是说，可以通过super来调用父类的方法，或者父类的静态函数。

     下面给个例子啦：

     ~~~javascript
     class A {
       p() {
         return 2;
       }
     }
     
     class B extends A {
       constructor() {
         super();
         console.log(super.p()); // 2
       }
     }
     
     let b = new B();
     ~~~

     但是要 **注意**：

     - `super`指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过`super`调用的。（也就是只有原型上面的可以获取，实例自己设置的是没有办法得到的）

4. 继承的一些深入内部概念：

   事实上，extend 后面不仅仅可以跟 类，只要是拥有原型的函数，都可以。



### 九、Module 语法

1. 定义 module：

   一般使用 export 语句：（不可以在定义之前使用变量）

   ~~~javascript
   // 三种写法
   
   // 写法1：单行单独
   
   export var first = 1;
   var b = 2;
   export {b};
   /*错误！！！*/
   var c = 3;
   export c;
   export 4;//这个不是对外的 接口
   
   // 写法2：函数和类的export
   
   export function multiply(x, y) {
     return x * y;
   };
   
   // 写法三：同时 export 很多参数名称
   
   var firstName = 'Michael';
   var lastName = 'Jackson';
   var year = 1958;
   
   export { firstName, lastName, year };
   
   // 还可以给 export 的变量 取别名
   
   export {a as a1};
   ~~~

   注意：

   export 可以出现在模块的任何位置。只要是全局作用域就可以。不可以是块级作用域。

2. import 命令：调用模块

   语法如下：

   ~~~javascript
   import {a as p,b,c} from 'path/xxx.js';
   
   console.log(p);
   ~~~

   - 注意：
     - 最好将所有的 import 部分都作为只读部分。
     - import 会提升到文档的开头。
     - import 内不可以写表达式。

   模块整体加载：

   ~~~javascript
   import * as p from 'path/xxx.js';
   
   console.log(p.hello);
   ~~~

3. export default 命令：

   使用`import`命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。

   为了能够不阅读文档，不知道变量名称的时候，就去调用模块，可以使用这个语法。

   ~~~javascript
   // 为匿名函数设置 export
   export default function () {
     console.log('foo');
   }
   
   // 使用：可以任意给这个函数取名字
   import customName from './export-default';
   customName();
   ~~~

   一个模块只能有一个默认输出，因此`export default`命令**只能使用一次**。

   也可以在有函数名的函数前面使用这个。在这个文档外部，这个函数和匿名函数一样。

   使用这个命令，后面不可以跟变量声明语句。

4. 模块继承：

   可以像下面这么写：

   ~~~javascript
   export * from 'abc_module';
   ~~~

   这样表示 abc_module 的所有内容又被作为模块对外了。

