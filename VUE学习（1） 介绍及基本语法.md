# vue.js 是什么？

Vue是一套用于构建用户界面的**渐进式框架**。

**vue只关注视图层**

渐进式框架是指一开始并不需要把所有的东西全用上，需要用到什么就拿什么来用。

### vue安装

----------------------

最简单的方式是在页面中引入vue.js文件

```html
<script src="vue.js"></script>
```

###  声明式渲染

----------------------

​	现在基本所有的框架都已经认同这个看法——DOM应尽可能是一个函数式到状态的映射。状态即是唯一的真相，而DOM状态只是数据状态的一个映射。所有的逻辑尽可能在状态的层面去进行，当状态改变的时候，View应该是在框架帮助下自动更新到合理的状态，而不是说当你观测到数据变化之后手动选择一个元素，再命令式地去改动它的属性。

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<div id="app">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

vue 2.0 中引入了虚拟DOM

> ​	Vue的编译器在编译模板之后，会把这些模板编译成一个渲染函数**。而函数被调用的时候就会渲染并且返回一个**虚拟DOM的树**。这个树非常轻量，它的职责就是描述当前界面所应处的状态。当我们**有了这个虚拟的树之后，再交给一个patch函数，负责把这些虚拟DOM真正施加到真实的DOM上**。在这个过程中，Vue有自身的响应式系统来侦测在渲染过程中所依赖到的数据来源。在渲染过程中，侦测到的数据来源之后，之后就可以精确感知数据源的变动。到时候就可以根据需要重新进行渲染。当重新进行渲染之后，会生成一个新的树，将新树与旧树进行对比，就可以最终得出应施加到真实DOM上的改动。最后再通过patch函数施加改动。
>
> ​	这样做的主要原因是，**在浏览器当中，JavaScript的运算在现代的引擎中非常快，但DOM本身是非常缓慢的东西**。当你调用原生DOM API的时候，浏览器需要在JavaScript引擎的语境下去接触原生的DOM的实现，这个过程有相当的性能损耗。所以，本质的考量是，要把耗费时间的操作尽量放在纯粹的计算中去做，保证最后计算出来的需要实际接触真实DOM的操作是最少的。

### v-if

---------------------

控制切换一个元素是否显示:

```html
<div id="app">
  <p v-if="seen">现在你看到我了</p>
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    seen: true
  }
})
```

继续在控制台输入 `app.seen = false`，你会发现之前显示的消息消失了。

### v-for

------------

`v-for` 指令可以绑定数组的数据来渲染一个项目列表：

```html
<div id="app">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

在控制台里，输入 `app.todos.push({ text: '新项目' })`，你会发现列表最后添加了一个新项目。

### v-on

-------------

我们可以用 `v-on` 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法：

```html
<div id="app">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>
```

```javascript
var app5 = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

也可以使用`@:click`来添加时间监听器

```html
<div id="app">
  <p>{{ message }}</p>
  <button @:click="reverseMessage">逆转消息</button>
</div>
```

### v-bind

------------------

v-bind主要用于属性的绑定

```html
<a v-bind:href="url"></a>
```

如果不加v-bind那么url就是个常量，没有任何动态数据的参与。当加上v-bind时，url是vue中data.url中所对应的变量，通过这种绑定，我们可以很方便的在不操作DOM的情况下修改href的值。

### v-model

-------------------------

v-model功能也是提供改变，但是这种改变是双向的。

```html
<div id="app">
   <input type="text" v-bind：value="message">
  <input type="text" v-model="message">
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

#### 双向绑定与单向绑定的区别

v-model绑定的input中的值发生改变时，vue.data.message中的值也会跟着相应的变化，v-bind绑定的input中的值也会变化。而当你修改被v-bind绑定的input中的值时，vue.data.message中的值并不会发生改变。

这种双向绑定的方式，在我们使用异步提交表单数据时，不用操作DOM便能够获取到表单中的值。