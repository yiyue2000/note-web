### 一、Vue 的 demo 知识：

1. vue 的响应式：

   在数据传入 vue 的数据之后，对 vue 的 data 做改变，仍然会使得渲染的结果发生变化。证明vue是响应式的。
   
2. vue 元素的基本写法：

   基本上，vue的元素是这么写的：

   ~~~javascript
   var vm = new Vue({
   	el:"#myID",
   	data:{
   		isHello:"hello, welcome to vue!"
   	}
   });
   ~~~

   对应 HTML中：

   ~~~html
   <div id = "myID">
       {{isHello}}
   </div>
   ~~~

   而且在html 中这些数据必须在一开始创建对象的时候就指定了具体的data。

   虽然：

   ~~~javascript
   vm.isHello === data.isHello; // 如果把data作为一个外部的var
   // true
   ~~~

3. vue 类型对象有一些 属性 和 方法，都是用 $ 开头的：

   ~~~javascript
   vm.$watch('元素名称', function(newVal, oldVal){
   	console.log(newVal, oldVal);
   })
   ~~~

   这个函数可以用来观察新值和旧值。接受一个函数作为回调函数。

4. 生命周期函数：

   vue 会 把整个页面渲染的过程分为几个过程。可以在建立的 vue 类型对象里面，声明一些生命周期函数，到那个阶段结束的时候，生命周期函数会自动调用。

   生命周期函数中，this 指向这个外部的 vue 类型 对象。

5. 指令：

   是一些以 v- 开头的 特别的 HTML 元素属性。一些常见的如下所示：

   - v-if：是否渲染
   - v-bind：
   - v-on：监听 DOM事件

6. 关于 v-if 和 v-show 的区别：

   - v-if：

     真正的条件渲染，这个元素如果是false，那么这个元素在最后逇html页面上都不会存在。

     切换成本高。如果不是频繁切换，那么使用 v-if。

   - v-show：

     简单的改变元素的CSS属性，元素一定会出现在最后的html文档上，但是可能由于CSS属性会被隐藏起来。

     初始化成本高。如果频繁切换，那么使用v-show。

7. 关于 v-for 来渲染表单：

   可以在表单元素上加上这样的属性，方便使用 for循环的方式来渲染：

   ~~~html
   <div id = 'app'>
       <ul>
           <li v-for="item in items">
               {{item.message}}
           </li>
       </ul>
   </div>
   ~~~

   ~~~javascript
   var vm = new Vue({
   	el = '#app',
   	data = {
   		items:[
   			{message:1},
   			{message:2}
   		]
   	}
   })
   ~~~

   我中，items是需要遍历的数组，object是需要遍历的对象，item是数组的每一项，index是数组的每个值的索引，key是对象中的键，value是对象的值。

   还可以使用 `v-bind:key` 来获取一些对象或者数组的一种每一项的特别身份。这样可以实现下面的效果。当数组或者对象的一些元素发生改变，顺序改变不会影响到vue的渲染，vue使用一种“就地更新”的方法。这样效率比较高。

8. v-on：

   是一种vue来做响应事件的方法。

   写法的示例如下：

   ~~~html
   <div id = 'myID'>
       <button v-on:click='clickMe('abc')'>
           click here!
       </button>
   </div>
   ~~~

   ~~~javascript
   var vm = new Vue({
       el:'#myID',
       method:{
           clickMe:function(str){
               alert(str);
           }
       }
   })
   ~~~

   有一个特殊的变量，叫做 $event，可以把原生的DOM事件传递给函数。函数可以这么写：

   ~~~javascript
   clickMe:function(str, $event){
   	...
   }
   ~~~

   有的时候，希望能够在回调函数里面，专注于处理数据，而不是处理DOM事件。所以vue提供了一些修饰符，用来处理一些事件。

   详细见 vue 官网。

9. 关于表单双向绑定：使用 v-model

   是为了 input，textarea 这样的标签设计的。

   可以双向绑定输入的值（也就是方便我们获得输入的值）

   下面给出一个小例子：

   ~~~html
   <div id='app'>
       <input v-model="message" placeholder="edit me">
   	<p>Message is: {{ message }}</p>
   </div>
   ~~~

   并且在js代码的对应对象中注册这个值：

   ~~~javascript
   var vm = new Vue({
       el:'#app',
       data:{
           message:''
       }
   })
   ~~~

   这个值会从文本框传递到 data 的 message，之后message再传递到html的文本中。

10. 关于组件的基本知识：

    在 vue 中 可以声明一些可以复用的组件，来方便使用。下面是组件的基本构建方式：

    ~~~javascript
    Vue.component('myButton',{
        data:function(){
            return {
                count:0,
                name:''
            }
        },
        tempalet:'<button v-on:click="clickMe">You clicked me {{ count }} times.</button>',
        methods:{
            clickMe:function(){
                this.count++;
            }
        }
    })
    ~~~

    html文档：

    ~~~html
    <div id = 'app'>
        <myButton></myButton>
        <myButton></myButton>
    </div>
    ~~~

    **注意：**

    - data必须是 function 类型的，返回一个参数对象。
    - template 必须拥有根节点。

    还有一些其他的可以在模板内部声明的属性：

    比如，props属性。表示你自定义的 attribute。下面给出示例写法：

    ~~~javascript
    Vue.component('blog-post', {
      props: ['title'],
      template: '<h3>{{ title }}</h3>'
    })
    ~~~

    然后就可以在 html 中，为这个叫做title的属性传递你需要的值：

    ~~~html
    <blog-post title="My journey with Vue"></blog-post>
    <blog-post title="Blogging with Vue"></blog-post>
    <blog-post title="Why Vue is so fun"></blog-post>
    ~~~

    还可以自定义一些事件：

    在 Vue.component的参数中，在 methods 中，可以使用 this.$emit，定义一个属于自己的事件。基本写法如下：

    ~~~javascript
    Vue.component('myButton',{
        data:function(){
            return {
                count:0,
                name:''
            }
        },
        tempalet:'<button v-on:click="clickMe">You clicked me {{ count }} times.</button>',
        methods:{
            clickMe:function(){
                this.count++;
                this.$emit('clickNow',this.count);
            }
        }
    })
    
    var vm = new Vue({
        el:'#app',
        methods:{
            click1:function(event){
                console.log(event);
            }
        }
    })
    ~~~

    这个方法有两个参数，一个是事件的名称，另外一个是传递的参数。函数也可以接收一个参数，是事件本身。

    第二个参数抛出的值，可以用 $event 接收到。

11. 组件注册：

    在使用 Vue.component 函数注册组件的时候，是注册的全局组件。有的时候可能你的代码中并没有用到一些组件，但是如果注册了全局组件，用或者没用的时候，都需要有一些开销。这样不是很灵活。所以现在有一种新的方法来注册组件：

    局部注册：

    ~~~javascript
    var vm = new Vue({
    	components:{
            comp1:{
                template:"<h1>???</h1>",
                data:function(){
                    return {
                        ...
                    }
                },
            }
        }
    })
    ~~~

12. 关于单文件组件：

    有一种文件，后缀为 .vue。

    （webpack：js 的一种打包器）

    （vue-cli：vue 脚手架工具）

    安装好上面的东西了之后，可以通过 `vue ui` 命令，来启动 vue的一个图形化管理界面。

    下面是其中文件的结构：

    ![avatar](F:\其他笔记\vue-cli框架.PNG)

    (emmm，后面的可能就需要实践继续学习了)



### 二、vue 补充知识：

1. 生命周期：

   看iPad上面的示意图啦。

2. keep-alive：

   也就是说，组件在切换的时候不会将组件destroy，而是会调用 hook-function：deactivate，然后将这个组件保存到浏览器缓存中，命中缓存渲染后，会执行 `actived`  hook-function。

3. 组件通信

   （1）父子组件定义：

   - 