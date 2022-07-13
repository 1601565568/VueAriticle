#### 认识h函数

Vue绝大多数情况使用模板来创建HTML，但是有些地方需要`JavaScript`的编辑能力，我们可以使用渲染函数，它比模板更接近编译器

##### VNode和VDOM

- vue在生成真实DOM之前，会将我们的节点转成VNode，而VNode组合在一起形成一颗树结构，就是虚拟DOM(VDOM)
- 事实上，我们之前编写的template中的HTML最终也是使用渲染函数生成对应的VNode
- 那么，如果想要使用JavaScript来生成相应的VNode，我们可以自己来编写creatVNode函数实现
- 我们可以使用`h()`函数：一个用于创建vnode的函数；其实更准确的命名是createVNode()函数，所有h()又是语法糖啦！！！ 

##### h()函数使用方法

它接收三个参数：

1. `type:`

1. 1. 类型：string | object | function
   2. 详细：HTML标签名，组件，异步组件，函数式组件。使用返回 null 的函数将渲染一个注释。此参数是必需的。

1. `props:`

1. 1. 类型：object
   2. 详细：一个对象，与我们将在模板中使用的 attribute、prop 和事件相对应。可选。

1. `children:`

1. 1. 类型：string | Array | Object
   2. 详细：子代 VNode，使用 h() 生成，或者使用字符串来获取“文本 VNode”，或带有插槽的对象。可选。

```javascript
h('div', {}, [
  'Some text comes first.',
  h('h1', 'A headline'),
  h(MyComponent, {
    someProp: 'foobar'
  })
])
```

##### 注意：

如果没有props，（第二个参数）那么通常可以将children作为第二个参数，如果会产生歧义，那么就可以将null作为第二个参数，将children作为第三个参数

#### vue中使用JSX

###### 使用配置

如果想要在vue项目中使用jsx，我们需要添加对jsx的支持，我们可以用Babel来进行转换，我们需要在Babel中配置对应的插件，

- 安装Babel支持vue的jsx插件

```bash
npm install @vue/babel-plugin-jsx -D
```

- 在babel.config.js配置文件中配置插件

```javascript
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  plugins: [
    "vue/babel-plugin-jsx"
  ]
}
```

#### 自定义指令

vue允许我们自己来定义一些自定义指令

##### 使用场景

- 在Vue中，代码的复用和抽象主要还是通过组件
- 通常，当你需要对DOM元素进行底层操作时候，我们需要用到自定义指令

##### 使用方式

- 自定义局部指令：组件中通过`directives`选项，只能在当前组件中使用
- 自定义全局指令：app的`directive`方法。可以在任意组件中使用

```javascript
// 局部指令
 directives: {
      focus: {
        mounted(el, bindings, vnode, preVnode) {
          console.log(bindings, vnode, preVnode, "focus mounted");
          el.focus();
        }
      }
    }
// 全局指令
app.directive("focus", {
  mounted(el, bindings, vnode, preVnode) {
    console.log("focus mounted");
    el.focus();
  }
})
```

**一个指令定义的对象，Vue提供了如下的几个钩子函数：**

- created:在绑定元素的attribute或事件监听器被应用之前调用；
- beforeMount:当指令第一次绑定到元素并且在挂载父组件之前调用；
- mounted:在绑定元素的父组件被挂载后调用；
- beforeUpdate:在更新包含组件的VNode之前调用；
- updated:在包含组件的VNode及其子组件的VNode更新后调用；
- beforeUnmount:在卸载绑定元素的父组件之前调用；
- unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次；

#### 自定义指令的参数和修饰符

```html
<button v-if="counter < 2" v-why:info.aaaa-bbbb="'coderwhy'" >当前计数: {{counter}}</button>
```

- info是参数名称
- aaa-bbb是修饰符的名称
- 后面传入具体的值
- 在生命周期中，我们可以通过bingings获取到对应的内容

#### 认识Teleport

##### 为了解决？

在组件开发中，我们封装一个组件A，在另一个组件B中使用，组件A中template的元素，会被挂载到组件B中template的某个位置，但是某些情况下，我们希望组件不是挂载在这个组件树上的，可能是移动到Vue app之外的其他位置，例如移动到body元素上

##### 是什么？

他是一个Vue提供的内置组件，类似React的Portals

它有两个属性：

- to：指定将其内容移动到的目标元素，可以使用选择器；
- disabled：是否禁用teleport的功能；



#### getCurrentInstance

用户获取全局属性(在setup中没有this) 官方说的是 ： 支持访问内部组件实例

```vue
<script>
  import { getCurrentInstance } from "vue";

  export default {
    setup() {
      const instance = getCurrentInstance();
      console.log(instance.appContext.config.globalProperties.$name);
    },
    mounted() {
      console.log(this.$name);
    },
    methods: {
      foo() {
        console.log(this.$name);
      }
    }
  }
</script>
```

#### Vue插件

通常我们向Vue全局添加一些功能，会采用插件的模式，写法有两种：

- 对象类型：一个对象，但是必须包含一个install函数，该函数会在安装插件时执行，
- 函数类型：一个function，这个函数会在安装插件时自动执行，

插件功能没有限制，比如一下几种都可以：

- 添加全局方法或者property，通过把他们添加到`config.globalProperties`上实现；
- 添加全局资源：指令，过滤器，过渡等；
- 通过全局mixin来添加一些组件选项；
- 一个库，提供自己的API，同时提供上面提到的一个或多个功能

![img](https://cdn.nlark.com/yuque/0/2022/png/21765913/1651832138475-4046a344-c3f0-4e49-9ec2-d04f2a064025.png)