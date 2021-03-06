### 什么是组件？

组件component 是Vue.js最强大的功能之一。组件可以扩展HTML元素，封装可重用的代码。在较高层面上，组件是自定义元素，Vue.js的编译器为它增加特殊功能。在有些情况下，组件也可以是原生HTML元素的形式，以is特性扩展。

### 使用组件

#### 注册

之前说过，我们可以通过以下方式创建一个Vue实例：

new Vue({
  el: '#some-element',
  //选项
})

p.tip 要注册一个全局组件，你可以使用 Vue.component(tagName, options), 例如：

Vue.component('my-component', {
  // 选项
})

> 对于自定义标签名，Vue.js不强制要求遵循W3C规则（小写，并且包含一个短杠），尽管遵循这个规则比较好。

组件在注册之后，便可以在父实例的模块中以自定义元素
<my-component></my-component> 的形式使用。要确保在初始化根实例之前注册了组件：

<div id="example">
  <my-component></my-component>
</div>

// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})

// 创建根实例
new Vue({
  el: '#example'
})

渲染为：

<div id="example">
  <div>A custom component</div>
</div>

#### 局部注册

不必在全局每个组件。通过使用组件实例选项注册，可以使组件仅在另一个实例 / 组件的作用域中可用：

var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component>将只在父模版可用
    'my-component': Child
  }
})

这种封装也使用于其他可注册的Vue功能，如指令。

#### DOM模版解析说明

当使用DOM作为模版时(例如，将 el 选项挂载到一个已存在的元素上)，你会受到HTML的一些限制，因为Vue只有在浏览器解析和标准化HTML后才能获取模版内容。尤其像这些元素 <ul> <ol> <table> <select> 限制了能被包裹的元素, <option> 智能出现在其他元素内部。

在自定义组件中使用这些受限制的元素时会导致一些问题，例如：

<table>
  <my-row>...</my-row>
</table>

自定义组件 <my-row> 被认为是无效的内容，因此在渲染的时候会导致错误。变通的方案是使用特殊的 is 属性：

<table>
  <tr is="my-row"></tr>
</table>

应当注意，如果使用来自一下来源之一的字符串模版，这些限制将不适用：

1. <script type="text/x-template">
2. JavaScript内联模版字符串
3. vue 组件

因此，有必要的话请使用字符串模版。

#### data必须是函数

使用组件时，大多数选项可以被传入Vue构造器中，有一个例外: data 必须是函数。实际上，如果你这么做：

Vue.component('my-component', {
  template: '<span>{{ message }}</span>',
  data: {
    message: 'hello'
  }
})

那么 Vue 会在控制台发出警告，告诉你在组件中 data 必须是一个函数。 最好理解这种规则的存在意义。

var data = { counter: 0 }

Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  // data 是一个函数，因此Vue不会警告
  // 但是我们为每一个组件返回了同一个对象引用
  data: function() {
    return data
  }
})

new Vue({
  el: '#example-2'
})



#### 构成组件

组件意味着协同工作，通常父子组件会是这样的关系： 组件A在它的模板中使用了组件B。 它们之间必然需要相互通信：父组件要给子组件传递数据，子组件需要将它内部发生的事情告知给父组件。
然而，在一个良好定义的借口中尽可能将父子组件解耦是很重要的。这保证了每个组件可以在相对格力的环境中书写和理解，也大幅提高了组件的可维护性和可重用性。

在Vue.js中，父子组件的关系可以总结为props down, events up。父组件通过props向下传递数据给子组件，子组件通过events给父组件发送消息。看看它们是怎么工作的。 

### Props

#### 使用Props传递数据

组件实例的作用域是孤立的。这意味着不能并且不应该在子组件的模板内直接引用父组件的数据。可以使用props把数据传给子组件。

prop是父组件用来传递数据的一个自定义属性。子组件需要显式地用 props 选项声明 "prop":

Vue.component({
  // 声明props
  props: ['message'],
  //  就像data一样, prop可以用在模板内
  // 同样也可以在vm实例中像'this.message'这样使用
  template: '<span>{{ message }}</span>'
})

然后向它传入一个普通字符串：

<child message="hello!"></child>

