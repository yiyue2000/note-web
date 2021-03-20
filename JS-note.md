### Char 01 基本类型

1. var ：

   - 声明的为一个局部变量
   - 若省略var 来声明变量，可以得到一个全局变量

2. undefined 类型：

   当 var x 没有赋值时，为 undefined 类型。

3. Null 类型：

   - 值为 null
   - typeof null = object
   - 含义为一个空指针

4. Boolean 类型：

   和 CPP 不同的是，false 和 true 不能 和 0 1 等效。

   Bollean() 函数 可以将其他类型转换成 Bollean 类型。

5. Number 类型：

   - int：

     八进制：数字开头为 0。例如：0236

     十六进制：数字开头为 0x。例如：0x14AE

     算数计算时，会被转换成十进制。

   - 浮点数：

     可以科学计数法表示。例如：

     3.2e-8 = 3.2 * 10^-8

   - NaN：

     表示一个本来要返回数值的操作，却不返回数值的情况。

     当 Number / 0 时，一般的语言会直接报错。但是 JS 会返回 NaN。

     **注意：**

     - NaN和任何数都不相等。包括NaN自己。
     - 任何涉及NaN的操作都会返回NaN。
     - isNaN() 函数会判断是不是返回NaN。

6. 数值转换：

   - Number() 函数：

     - true false：0 1

     - null ：0

     - undefined：NaN

     - 字符串：

       float int 长相的直接转。

       十六进制转为十进制显示。

       空字符串：0

       上述情况都不符合：NaN

     - 对象：（object）

       调用 valueOf() 函数，然后按照前面的规则来转换。如果得到的结果为 NaN，那么调用 toString() 函数，按照前面的方法来转换返回字符值。

   - parseInt() 函数：

     若第一个字符不是数字，返回NaN（负号也不可以）

     若后面有不是数字的，返回前面读入的。

     可以识别不同的进制数字。

     **举例：**

     - parseInt("1234abc") = 1234
     - parseInt("3.3") = 3
     - parseInt("") = NaN
     - parseInt("070") = 56

   - parseFloat() 函数：

     和 parseInt() 类似，规则类似。而且如果是 int 也会返回 int。

7. String 类型：

   - 转换为String类型：

     **除了 null 和 undefined 类型**，其他类型都有 toString() 函数。

     一般情况下，看到什么就得到什么长相的字符串。

     但是 toString 函数还可以输入一个 **基数** 参数，得到输出的是什么样的进制字符。

   - String() 函数：

     可以将任何类型转换为 string。

     如果为 undefined 或者 null，输出 undefined 或 null。

     其他按照 toString() 函数。

   - String 是 **不可以改变**的。也就是说，在定义了一个变量的string值之后，想改变，必须销毁之前的string，制造一个新的来给它。





### Char 02 操作符、语句 与 函数

1. 一元操作符：

   类比 CPP 中的 ++ 与 --；

   但是，如果不是数值类型使用 ++ 或者 --，会先转换成对应的 Number 类型（类似于使用 Number 函数），之后继续计算。

2. 一元加减操作符：

   为：x = + x ;

   这个可以取相反数。也可以用于类型转换：由于+之后会将x按照Number函数的结果先强制转换，之后计算，所以可以用于取相反数。

3. 位操作符：

4. Boolean 操作符：

   - ！（逻辑非）：

     求反。

     其他类型会先转换成 bool 值。详情见P44

   - && 和 ||：

     都是短路操作。

     其他情况见书本 P46

5. 有乘法*，除法 /，取模 %

6. 加法：

   如果有 1个/2个 操作数为 string，那么不是 string的先通过 toString() 或 String() 函数转换成 string，之后继续进行拼接操作。

7. 关系比较操作符：

   -  == 相等 和 ！= 不等：

     若为不同数据类型，先进行类型强制转换：

     - 一个op 为 bool，则先转为 0 1
     - string == Number ？=》 string 转 Number
     - Object 调用 valueOf 函数，得到基本类型，之后比较。
     - null 和 undefined 是相等的。
     - object == object？=》 如果是指向同一个对象，返回 true，否则返回false
     - 一个op为 NaN，则==返回false ，!= 返回 true。
     - null 和 undefined 不能转换成任何值。

   -  === 全等 和 !== 不全等：

     未经转换之前的情况下进行比较。

8. 可以使用三元操作符：

   类似 CPP：为 **？：**

9. 赋值：

   和CPP类似，也有 +=，-= ……

10. 语句：

    - for - in 语句：

      为了保证为局部变量，建议写成下面的样子：

      ~~~javascript
      for(var item in windows){
          xxx; // statement
      }
      ~~~

      注意：被枚举的顺序是随机的。之前要检查迭代对象是不是null或者undefined。

    - with语句：

      见书本 P60

    - switch 语句：

      和 CPP类似。但是case的值可以是变量。

11. 函数：

    - 函数的参数：

      实际上，在JS内部，是将参数列表转化为一个叫 arguments的列表当中。所以，传入参数的个数其实没有限制。没有传入值的参数，则为undefined值。相当于定义之后没有用。

    - 函数不能重载。



### Char 03 变量 作用域 内存问题

##### 一、基本类型 和 引用类型：

1. 在 JS 中，除了 Object 为 引用类型以外，其他之前提到的都是 基本类型。（String 也是）

   基本类型：**按值** 访问

   引用类型：**按引用** 访问

   - 注意：JS 不允许访问内存

2. 可以给 引用类型 动态添加属性值：（而 基本类型 不可以）

   ~~~javascript
   var obj = new Object();
   obj.name = "abx"; // 动态添加属性
   console.log(obj.name);
   // print : abx
   ~~~

3. 变量复制：

   基本类型是 **按值复制**。

   引用类型是 **复制引用**。即：复制了一个指针。下面给个例子：

   ~~~javascript
   var obj1 = new Object();
   var obj2 = obj1;
   obj1.name = "abx"; // 实际上也同时修改了 obj2
   console.log(obj2.name);
   //print : abx
   ~~~

4. 参数传递：

   所有的参数传递都是按值传递的。

   也就是说：

   - **基本类型** 会复制一个他们自身来传递过去，修改它们不会影响外面。
   - **引用类型** 会复制他们的一个引用，影响需要具体分析。

5. 类型检测：

   用 typeof 可以检测一些基本类型。但是具体是什么引用类型的值得时候，不好用。

   可以用 instanceof 来检测。（return true/false）



##### 二、作用域 执行环境：

1. 局部作用域 和 全局作用域：

   在 JS 当中，只有 function 内部算 **局部作用域**。

2. with 和 try-catch 语句 可以延长作用域链。

3. 会有 局部作用域中，有变量和 **父作用域** 变量相同，被覆盖的现象。



##### 三、垃圾回收：

类似 gc，隔一段时间会回收一次。



### Char 04 引用类型

1. Object 类型的创建：

   - 使用 new 和 构造函数

   - 使用 **对象字面量** 表示：

     var person = {name : "qq", age : 1};

2. JS 中，可以用 [] 来访问 Object 对象中的值（类似py字典dict），属性以 **字符串** 的形式传入。

3. 数组 Array：

   - length 属性 不是只读的：可以更改 length 属性。

     如果修改的比原来大，那么后面的都是 undefined 值来填充。

   - 可以用 instanceof Array 来确定是不是数组。

   - Array.isArray() 方法也可以确定是不是数组。

   - 一些操作：

     - stack 特性：push() , pop()
     - queue 特性：shift()（队头出去） , push() （队尾进入）

   - 重排序：

     - 可以用 reverse() 逆序。

       eg. arr.reverse()

     - sort() ：

       <font color = red>**注意**</font>：在没有传入比较函数之前，是按照字母比较来的。

       可以传入一个函数，作为比较函数，可以返回 -1（第一个参数比第二个小），0（相等），1（大）

       eg. arr.sort(compare);

     - 会对原数组进行更改。返回值是更改之后的数组。

   - 操作函数：

     - concat() : 连接数组 **不改变原数组** 返回改变之后的数组
     - slice() : （会改变原来的数组……）
     - splice() : 

   - 位置函数：

     - indexof()：

       返回从前向后扫描的，某个项的index

       可选参数：从哪一个地方开始（第二个参数）

       eg. arr.indexof('a', 4);

     - lastIndexOf():

       从后向前扫描

   - 迭代函数：

     - 见书本P96。可以执行对数组每项的操作。节省时间。

   - 缩小方法：

     （呜呜呜快要吐出来了）

4. Date 类型：

   一些日期转换的bulabula

5. RegExp 类型：

   支持正则表达式。

6. function 类型：

   在 JS 中，function 作为一个类型。函数是作为一个对象来看待的。

   - 定义函数：

     - 函数声明：和正常些 CPP 函数一样。直接写。但是 JS 没有函数签名。

     - 函数表达式：类似对象：

       var sum = function(val1, val2){ return (var1 + var2); }

     - 区别：函数声明不论在哪个位置都可以。因为JS 代码会优先解释函数声明。但是函数表达式需要在用的前面定义。要不然无法解析。

   - 函数属性：

     - length：希望接受的参数个数。
     - prototype：
     - this：表示当前此函数的作用域。

   - 函数的函数：

     - apply、call：用在调整函数的作用域。在给定作用域下面执行函数。
     - bind：绑定作用域给 this

7. 基本包装类型：

   会类似 java，把一些基本类型包装成类。

   - String 类：其实在单纯的基本类型 的 string 上，有一些方法可以使用。但是内部的机制是，先将string转换成一个临时的对象，执行操作，返回字符串，然后销毁这个对象。

     有一些方法。但是也不会改变原来的数组。

     大小写转换啊，删除前后空格啊。转换编码返回啊（ASCII）。

   - boolean类型：不建议使用。而且 逻辑操作符 || && 等，都会自动把 object 类型给转化成 true。会出错。

   - Number类型：有一些方法可以，固定小数点位数啊，使用科学计数法啊之类的。

8. ES内置对象：

   - Global对象：

     所有在全局定义域的属性和方法，都是他的属性和方法。

     URI编码：有两个方法可以使用。

     eval() 方法：将输入字符串作为 JS 来解析。

     不过，一般Web浏览器会给出一个 window 的对象，将这个对象作为 window 对象的一部分来的。

   - Math 对象：提供数学操作（和图形学用到的很像）



### Char 05 JS面向对象

1. 对象的属性：

   可以通过：obj.name = "ax"； 来添加对象属性。

   - 对象的属性会有4个特性：

     - configurable：表示是否能够通过 delete 来删除属性，是否可以更改属性的值，是否可以修改为访问器属性。
     - enumeratable：能否通过 for-in 循环返回属性。
     - writable：能否修改属性值
     - value：这个属性的值

     基本上默认都是true

   - 使用：Object.defineProperty() 函数来修改属性特性。

     eg. Object.defineProperty( obj, "属性名称", { writable : false });

   - 一旦定义成为 不可配置 的，那么再也改不回来了。此时，除了 writable 可以修改之外，其他的都不能再修改了。

2. 访问器属性：

   访问器属性没有自己的值，只有一对 setter 和 getter 函数。

   - 有四个特性：
     - configurable
     - enumeratable
     - get：读取属性时候调用的函数
     - set：设置属性的时候调用的函数
   - 必须使用：Object.defineProperty() 来定义。

   在书本 P141 给了个例子。

   - 只指定getter 表示 不可以**写**。

3. 工厂模式：

   可以通过制定一个函数，来制造重复的对象。例子在 P144。

4. 构造函数模式：

   以下以及一些原型知识见书本。
   
5. 函数闭包：

   注意：

   每次重新调用函数的时候，会获得不一样的闭包。但是没有重新调用的时候，多次定义内部函数是共享一个闭包的。








### Char 08 事件

每个可用的事件都会有一个**事件处理器**，也就是事件触发时会运行的代码块。当我们定义了一个用来回应事件被激发的代码块的时候，我们说我们**注册了一个事件处理器**。

经常的一个写法就是：

~~~javascript
const btn = document.querySelector('button');
// 获得文档中的 第一个 button 类型的 HTML 元素

btn.onclick = function() {
  const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}
// 将 btn 的 onclick 属性 赋予一个 匿名函数
~~~

其实也可以赋值一个 有名字的函数。

不过，注意，最好不要在 HTML 文档内 写 JS 代码。这样会让解析变得困难。所以不要出现 行内事件处理器。



下面介绍几个新的事件处理函数：

1. addEventListener() ：

   下面给个示例：

   ~~~javascript
   const btn = document.querySelector('button');
   
   function bgChange() {
     const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
     document.body.style.backgroundColor = rndCol;
   }
   
   btn.addEventListener('click', bgChange); // 使用函数
   ~~~

   其中，函数的第一个参数是 **事件名称**，第二个参数是 **事件响应的函数**。这个函数可以是匿名函数。

2. removeEventListener() ：

   示例如下：

   ~~~javascript
   btn.removeEventListener('click', bgChange); // 需要在哪个元素上 删除 哪个事件 的 哪个动作
   ~~~

3. 上面两个函数的好处：

   可以给 **一个元素** 注册多个 **不同的事件**。但是下面这样就不行：

   ~~~javascript
   myElement.onclick = functionA;
   myElement.onclick = functionB;
   ~~~

   这样functionB 会 覆盖 functionA。



一些深层一点的概念：

1. 事件对象：

   一个被默认传入事件处理函数的参数。可以叫它 event，evt 或者 e。下面给出一个示例：

   ~~~javascript
   function bgChange(e) {
     const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
     e.target.style.backgroundColor = rndCol;
     console.log(e);
   }
   
   btn.addEventListener('click', bgChange);
   ~~~

   其中，e就是传入的事件。e.target 是 发生事件的 对象 的 引用。当要在多个元素上设置相同的事件处理程序时，`e.target`非常有用。大多数事件处理器的事件对象都有可用的标准属性和函数（方法）。

2. 阻止默认行为：

   有时会希望事件不执行它的默认行为。比如 表单的例子：

   当你填写详细信息并按提交按钮时，自然行为是将数据提交到服务器上的指定页面进行处理，并将浏览器重定向到某种“成功消息”页面（或 相同的页面，如果另一个没有指定。）

   当用户没有正确提交数据时，麻烦就来了 - 作为开发人员，你希望停止提交信息给服务器，并给他们一个错误提示，告诉他们什么做错了，以及需要做些什么来修正错误。

   有一个：e.preventDefault(); 可以阻止执行默认的行为。

   下面给出 一个 用户填错信息的时候可以阻止的 例子：

   ~~~javascript
   const form = document.querySelector('form');
   const fname = document.getElementById('fname');
   const lname = document.getElementById('lname');
   const submit = document.getElementById('submit');
   const para = document.querySelector('p');
   
   form.onsubmit = function(e) {
     if (fname.value === '' || lname.value === '') {
       e.preventDefault();
       para.textContent = 'You need to fill in both names!';
     }
   }
   ~~~

3. 事件冒泡 和 捕获：

   事件冒泡和捕捉是两种机制，主要描述当在一个元素上有两个相同类型的事件处理器被激活会发生什么。

   当一个事件发生在具有父元素的元素上，现代浏览器运行两个不同的阶段 - 捕获阶段和冒泡阶段。

   - 捕获阶段：
     - 浏览器检查元素的最外层祖先`<html>`，是否在捕获阶段中注册了一个`onclick`事件处理程序，如果是，则运行它。
     - 然后，它移动到`<html>`中单击元素的下一个祖先元素，并执行相同的操作，然后是单击元素再下一个祖先元素，依此类推，直到到达实际点击的元素。
   - 冒泡阶段：
     - 浏览器检查实际点击的元素是否在冒泡阶段中注册了一个`onclick`事件处理程序，如果是，则运行它
     - 然后它移动到下一个直接的祖先元素，并做同样的事情，然后是下一个，等等，直到它到达`<html>`元素。

   默认情况下，所有事件处理程序都在 **冒泡阶段** 进行 **注册**。（执行？）

   下面给个例子：

   ~~~html
   <button>Display video</button>
   
   <div class="hidden">
     <video>
       <source src="rabbit320.mp4" type="video/mp4">
       <source src="rabbit320.webm" type="video/webm">
       <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.mp4">link to the video</a> instead.</p>
     </video>
   </div>
   ~~~

   其中，vedio 有 一个事件，就是点击 vedio 会 让 vedio播放。 div有一个事件，就是点击 div 会让div隐藏。但是我们希望，在点击vedio的时候播放，点击div而不是vedio的部分的时候，才会隐藏。可是由于事件冒泡，所以，视频 vedio 会播放，但是 div 在之后冒泡也会隐藏。

   有一种方法来解决这种情况：

   标准事件对象具有可用的名为  `stopPropagation()` 的函数, 当在事件对象上调用该函数时，它**只会让当前事件处理程序运行**，但事件**不会**在**冒泡**链上**进一步扩大**，因此将不会有更多事件处理器被运行(***不会向上冒泡***)。

4. 事件委托：

   可以利用 **事件冒泡** 这个特性 来完成一些**特别**的事情。

   比如，如果你希望让很多小的部件都完成一个相同的事情，那么可以设置一个**父部件**，让它去注册一个事件，然后通过冒泡的时候，来完成这个事情。



### Char 09 异步编程

##### 一、异步的基本概念

1. JS 是一种单线程语言。一般执行代码的只有一个线程。但是有的时候，可能有的代码非常耗时，由于 JS 的单线程特性，前面的代码没有执行完，会阻塞后面的代码继续执行。

   这个时候，我们希望能不能，让这个耗时的工作放在一边（提前或者之后做），然后先去执行下面的事情，提高效率。

   这样就得到了基本的异步概念。

2. 传统 callback：

   其实，一般会把回调函数放入一个 queue 中，当主线程执行完了之后，再从queue中将之前的任务一个个 dequeue 完成。

   看这个网址，上面有详细说明 setInterval函数的原理：

   https://zhuanlan.zhihu.com/p/66593213

3. Promise：ES6新的异步编程

   Promise的编程步骤：

   - 创建 Promise 对象：

     像下面这样创建：

     ~~~javascript
     const promise = new Promise(function(resolve, reject) { // 需要接受一个参数，function类型，默认JS引擎给两个参数，分别是 resolve 和 reject
       // ... some code
     
       if (/* 异步操作成功 */){
         resolve(value); // 当调用resolve的函数的时候，相当于告诉后面的回调函数，这个触发条件已经达到
       } else {
         reject(error); // 当调用 reject 函数的时候，告诉后面的回调函数，失败了，执行类似 catch 中内容
       }
     });
     ~~~

     上面只是一个基本逻辑，不一定就需要写这样的 if - else 语句

   - 使用 Promise.then函数：绑定对应的回调函数

     给出下面的这个示例：

     ~~~javascript
     promise.then(function(value) {
       // success
         //当 resolve 触发的时候，调用这个函数。
     }, function(error) {
       // failure
         //当 reject 触发的时候，调用这个函数。
     });
     ~~~

   - 回调如何执行？

     需要注意的一些事情：

     - Promise 对象一旦建立，其中的函数立即执行。
     - 在上面回调函数的那个模型下，只有当回调函数（then 绑定的）和resolve 函数都运行进栈，回调函数才会有调用的资格。
     - 当然，也是在主线程代码执行完了之后，才会做回调函数做的事情。

   - 关于 catch：

     Promise.catch 函数是用来指定 reject 的时候，需要执行的回调函数。

     由于 Promise.then 调用之后，会返回一个 新的 Promise 对象。这个时候可以这样写：

     ~~~javascript
     let pm = fetch("/users"); // 获取Promise对象
     pm.then((response) => response.text()).then(text => { 
         test.innerText = text; // 将获取到的文本写入到页面上
     })
     .catch(error => console.log("出错了"));
     ~~~

     注意：

     - Promise 在`resolve`语句后面，再抛出错误，不会被捕获，等于没有抛出。

       因为 Promise 的状态一旦改变，就**永久保持该状态**，不会再变了。

     - Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。

       也就是说，错误总是会被下一个`catch`语句捕获。

       下面给出一个示例：

       ~~~javascript
       getJSON('/post/1.json').then(function(post) {
         return getJSON(post.commentURL);
       }).then(function(comments) {
         // some code
       }).catch(function(error) {
         // 处理前面三个Promise产生的错误
       });
       ~~~

     - 一般来说，不要在`then()`方法里面定义 Reject 状态的回调函数（即`then`的第二个参数），总是使用`catch`方法。

##### 二、ES6 中一个新的异步模式：生成器（Generator）

1. Generator 概念简介：

   generator 函数 可以看成内部拥有一个状态机。下面给出一个 generator 函数的示例：

   ~~~javascript
   function* helloWorldGenerator() {
     yield 'hello';
     yield 'world';
     return 'ending';
   }
   
   var hw = helloWorldGenerator();
   ~~~

   **解释：**

   其中定义了三个状态，分别是：hello world 和 return 语句。

   generator 函数 有一些写的**规则**：

   - function 后面加上 *
   - 只有 generator 函数 可以使用 yield 关键字

   **关于使用：**

   Generator 函数的调用方法：在函数名后面加上一对圆括号。

   调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象（Iterator Object）。

   接下来，需要使用这个对象的 next() 函数，来进行这个函数的调用。下面给出上面例子定义的 generator 函数的使用示例：

   ~~~javascript
   hw.next()
   // 返回对象： { value: 'hello', done: false }
   
   hw.next()
   // { value: 'world', done: false }
   
   hw.next()
   // { value: 'ending', done: true }
   
   hw.next()
   // { value: undefined, done: true }
   ~~~

2. 一些特性：

   `next`方法的运行逻辑如下：

   - 遇到`yield`表达式，就暂停执行后面的操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。
   - 下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。
   - 如果没有再遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值。
   - 如果该函数没有`return`语句，则返回的对象的`value`属性值为`undefined`。

   **注意：**

   `yield`表达式后面的表达式，只有当调用`next`方法、内部指针指向该语句时才会执行。下面给出示例：

   ~~~javascript
   function* gen() {
     yield  123 + 456;
   }
   ~~~

   上面代码中，`yield`后面的表达式`123 + 456`，不会立即求值，只会在`next`方法将指针移到这一句时，才会求值。

   **由于这样的特性**，我们可以做类似这样的事情：

   比如？比如在 yield 后面写上一个函数。当yield卡住的时候，就会运行这个函数。

3. 先总结一下：

   简单来说，generator 就是 能够让一个函数来一部分一部分地来执行。每次遇到 yield 的地方，停一下，然后你可以知道 yield这个地方出现了什么标签。运行到哪个状态了。

   为啥适合异步编程呢？

   想一想之前我们说的回调函数。回调函数会把一件事情分成两件事情去做，让一些比较消耗时间的之后再去做（或者之前准备好），这样运行起来的效果比较好。generator 就特别适合来这样去弄。

   而且，promise写起来有点复杂，回调函数写起来容易套娃看不懂，generator可以来简化这个！

4. generator 在 异步编程的应用：



### Char 10 Ajax请求

1. 基本的 ajax 请求写法：

   ~~~javascript
   var httpRequest = new XMLHttpRequest();
   
   if(!httpRequest){
       alert('Giving up :( Cannot create an XMLHTTP instance');
   }
   else{
       httpRequest.onreadystatechange = alertContents;// 协商关于响应请求的函数名
       httpRequest.open('GET', 'test.html', true); // 发送请求：方法，URL，请求是否是异步的（默认为true）
       httpRequest.send(); // post 请求：需要填写一些表单数据
   }
   
   // 响应请求的函数
   function alertContents() {
       if (httpRequest.readyState === XMLHttpRequest.DONE) { // 收到响应且没问题
         if (httpRequest.status === 200) { // 检查响应码
           alert(httpRequest.responseText); // 服务器以文本字符的形式返回，里面是你需要的一些信息
         } else {
           alert('There was a problem with the request.');
         }
       }
   }
   ~~~

   

