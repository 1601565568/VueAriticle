#### 产生背景

- 在Vue2中，我们编写组件的方式是Options API ：也就是在对应的属性中编写对应的功能模块；例如：`data`定义数据, `methiods`中定义方法，`computed`中定义计算属性；`watch`中监听属性的变化；也包括生命周期钩子
- 但是这种代码有弊端：当我们实现某一个功能；这个功能对应的代码逻辑会被拆分到各个属性中；当我们组件变大变复杂后，同一个功能的逻辑就会被拆分的很分散；尤其是在后期维护上，对用对于一个没有编写过这个代码的人来说；
- 所以我们就想要把同一个逻辑关注点相关的代码收集在一起；这也就是`Composition API`要帮我们做的事情，也有人把Vue Composition API 称为VCA

#### Composition API是什么？

- 如果我们想要使用Ccomposition API 那么我们首先需要一个环境（实际使用它编写代码的地方），在Vue组件中，这个位置就是`setup`函数
- `setup`其实就是组件的另外一个选项：只不过这个选项强大到可以我们用它来替代之前所编写的大部分其他选项；如`methods`、`computed`、`watch`、`data`、生命周期等等

#### setup函数的参数

###### 他的第一个参数： `props`

`props`非常好理解，它其实就父组件传递过来的属性会被放到`props`对象，如果我们在setup中需要使用，我们可以直接通过props参数获取

- 对于定义props类型。和之前一样
- 在template中依然可以正常使用props中属性，如message
- 注意：我们在setup函数中想要使用props，我们不可用通过this去获取 因为props有直接作为参数传递到setup函数中，所有我们可以直接通过参数获取使用

###### 另外一个参数：`context`

我们也称之为是一个`SetupContext`, 它里面包含三个属性：

- `attrs`:所有的非prop的`attribute`
- `slots`: 父组件传递过来的插槽（这个在以渲染函数返回时会有作用）
- `emit` ： 当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可用通过this.$emit发出事件）

#### setup函数的返回值

setup既然是一个函数，那么它也可以有返回值，它的返回值的作用有：

1. 可以在模板`template`中被使用；
2. 也就是说我们可以通过setup的返回值来代替data的选项
3. 甚至是我们可以返回一个执行函数来代替在methods中定义的方法

注意：我们定义的变量，然后再定义方法改变变量，是不能实现页面响应式刷新的，这是因为默认情况下，我们定义一个变量，Vue并不会跟踪它的变化来引起页面的响应式

#### setup不可用使用this

官方表示：this并没有指向当前组件实例，并且在setup被调用指点，`data`、`computed` 、 `methods`等都没有被解析，所以无法在setup中获取`this`

##### Reactive API

如果我们想要在setup中定义的数据提供响应式的特性，那么我们可以使用reactive的函数：

- 当我们使用reactive函数处理我们的数据之后，数据再次被使用就会进行依赖收集
- 当数据发生变化，所有收集到的依赖都是进行对应的响应式操作
- 事实上，我们编写的data选项，也是在内部交给了reactive函数将其编程为响应式对象的

#### Ref API

因为reactive API对传入类型是有限制的，它要求我们必须传入的是一个对象或者数组类型：如果我们传入的是基本类型 vue会给出警告

- vue3给我们提供了另外的API ref API ;ref 会返回一个可变的响应式对象，该对象作为一个响应式的引用维护着它内部的值，这也是ref名称的来源；它内部的值是在ref的value属性中被维护的
- 注意：

- - 在模板引入ref的值时，Vue会帮助我们进行解包操作，所以我们并不需要在模板中通过ref.value来使用ref（这里是将ref放入reactive属性中 如果不是放在reactive属性中就需要ref.value）
  - 但是在setup函数内部，它依然是一个ref引用，我们使用需要使用ref.value



#### readonly

我们通过reactive或者ref可以获取到一个响应式的对象，但是在某些情况下，我们需要把这个数据传入到其他地方（组件）中使用，但是我们希望它不能被修改

vue3为我们提供了readonly的方法，readonly会返回原生对象的只读代理（它是一个Proxy，这是一个proxy的set方法被劫持；不能对其进行修改）

使用：

在开发中我们通常会把readonly方法传入三个类型的参数：普通对象；reactive返回的对象；ref对象

使用规则：

- readonly返回的对象都是不允许修改的；
- 但是经过readonly处理的原来的对象是允许被修改的；
- 本质上就是readonly返回的对象的setter方法被劫持了

#### Reactive判断的API

- isProxy : 检查对象是否是由reactive或readonly创建的proxy
- isReactive: 检查对象是否是由reactive创建的响应式代理；如果该代理是readonly建的，但包裹了由reacive创建的另一个代理，它也会返回true
- isReadonly: 检查对象是否是由readonly创建的只读代理
- toRaw: 返回reactive或readonly代理的原始对象（不建议保留对原始对象的持久引用）
- shallowReactive：创建一个响应式代理，它跟踪其自身property的响应式，但不执行嵌套对象的深层响应式转换（深层还是原生对象）
- shallowReadonly: 创建一个proxy，使其自身的property为只读，但不执行嵌套对象的深度只读转换（深沉还是可读，可写的）

#### toRefs

如果我们使用ES6的解构对象，对reactive返回的对象进行解构获取值，那么无论是修改解构后的变量，还是修改reactive返回的state对象，数据都不再是响应式的

那么我们如果想要解构出reactive返回的值而且还想要响应式，可以使用toRefs的函数，可以将reactive返回的对象中的属性都转成ref

```vue
const state = reactive({
  name: "why",
  age: 18
})
const {name, age} = toRefs(state)
```

这样 解构出来的name个age本身都是ref了， 这样的做法相当于已经在state.name和ref.value之间建立链接，任何一个修改都会引起另一个变化

#### ref的其他API

- unref ：如果我们想要获取一个ref引用中的value，那么也可以通过unref方法
- 它接收一个值，如果参数是一个ref 则返回内部value，否则返回参数本身；它其实是一个语法糖，它做的事情就是`val = isRef(val) ? val.value : value` 其中isRef是判断一个值是否是一个ref对象
- shallowRef ： 创建一个浅层的ref对象
- triggerRef :  手动触发和shallowRef相关联的副作用

#### customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制：

- 它需要一个工厂函数，该函数接受track和trigger函数作为参数 其中`track()`是告诉Vue这个value值是需要被跟踪的，trigger()是告诉vue去更新页面
- 并且返回一个带有get和set的对象；

```vue
//useDebounceRef.js文件
<script>
  import { customRef } from 'vue';
// 自定义ref 接收的value是一个字符串 
export default function(value, delay = 300) {
  let timer = null;
  return customRef((track, trigger) => {
    return {
      get() {
        track();
        return value;
      },
      set(newValue) {
        clearTimeout(timer);
        timer = setTimeout(() => {
        value = newValue;
        trigger();
        }, delay);
      }
    }
  })
}
</script>
// 示例
<template>
  <div>
    <input v-model="message"/>
    <h2>{{message}}</h2>
  </div>
</template>
<script>
  import debounceRef from './hook/useDebounceRef';
  export default {
    setup() {
      const message = debounceRef("Hello World");
      return {message}}}
</script>
<style scoped>
</style>
```

#### computed

###### 是什么？

计算属性：当我们的某些属性是依赖其他状态转化得到，我们就可以使用计算属性来实现

###### 怎么实现？

在组合式API，我们使用`computed`选项来完成的

```vue
const firstName = ref("Kobe");
const lastName = ref("Bryant");
// 1.用法一: 传入一个getter函数
// computed的返回值是一个ref对象
const fullName = computed(() => firstName.value + " " + lastName.value);
// 2.用法二: 传入一个对象, 对象包含getter/setter，返回一个可读写的ref对象
const fullName1 = computed({
  get: () => firstName.value + " " + lastName.value,
  set(newValue) {
    const names = newValue.split(" ");
    firstName.value = names[0];
    lastName.value = names[1];
  }
```

#### 侦听数据的变化

###### 作用：

我们可以通过watch选项来侦听data或者props的数据变化，当数据变化时执行操作

##### 1. watchEffect

当侦听到某些响应式数据变化时，我们希望执行某些操作，

- 首先，watchEffect传入的函数会被立即执行，并且在执行过程中收集依赖
- 其次，只有当收集的依赖变化时，watchEffect传入的函数才会执行

```vue
const name = ref("why"); 
const stop = watchEffect(() => {
  console.log("name:", name.value);
});
// 我们运行 stop();就会停止侦听
```

##### 2. watchEffect清除副作用

在开发中，我们需要在侦听函数中执行网络请求，但是网络请求还没有到达的时候，我们停止了侦听，或者侦听器函数被再次执行了，为了解决这种问题，



在我们给watchEffect传入的函数被回调时，我们可以从回调函数中，获取一个参数：`onInvalidate`，当副作用即将重新执行或者侦听器被停止时会执行该函数传入的回调函数，我们可以在传入的回调函数中，执行一些清除工作

```vue
<script>
const name = ref("why");
const age = ref(18);

const stop = watchEffect((onInvalidate) => {
  const timer = setTimeout(() => {
    console.log("网络请求成功~");
  }, 2000)

  // 根据name和age两个变量发送网络请求
  onInvalidate(() => {
    // 在这个函数中清除额外的副作用
    // request.cancel()
    clearTimeout(timer);
    console.log("onInvalidate");
  })
  console.log("name:", name.value, "age:", age.value);
});
</script>
```

#### 使用ref获取元素或者组件

##### 使用：

我们定义一个ref对象，绑定到元素或者组件的ref的属性上即可；

```vue
<template>
  <div>
    <h2 ref="titleRef">这里是一个title</h2>
  </div>
</template>
<script>
  setup(){
    const titieRef = ref(null)
    return {
      titleRef
    }
  }
</script>
```

#### watchEffect的执行时机

##### 执行时机：

setup函数在执行时就会立即执行一次`watchEffect`的副作用函数，这个时候DOM并没有挂载，所有打印的一定是null，而当DOM挂载时，`watchEffect`的依赖会变化，副作用就会再次执行，打印出对应的内容

##### 调整执行时机：

如果我们不希望第一次的执行，我们需要改变副作用的执行时机，那么我们需要修改`watchEffect`的第二个参数，是一个对象，默认是`{flush: "pre"}`,我们可以修改成`{flush: "post"}这样就可以实现我们想要的效果`

```vue
<script>
   const title = ref(null);
      const demo = ref(null);
    const stop=  watchEffect(() => {
        console.log(title.value, demo.value);
      }, {
        flush: "post"
      })
    // stop()可以停止侦听
</script> 
```

#### watch的使用

```vue
<script>
      setup() {
      const info = reactive({name: "why", age: 18});

      // 1.侦听watch时,传入一个getter函数
      watch(() => info.name, (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue);
      })

      // 2.传入一个可响应式对象: reactive对象/ref对象
      // 情况一: reactive对象获取到的newValue和oldValue本身都是reactive对象
      watch(info, (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue,'ssss');
      })
      // 如果希望newValue和oldValue是一个普通的对象
      watch(() => {
        return {...info}
      }, (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue, "oldValue:");
      })
      // 情况二: ref对象获取newValue和oldValue是value值的本身
      // const name = ref("why");
      // watch(name, (newValue, oldValue) => {
      //   console.log("newValue:", newValue, "oldValue:", oldValue);
      // })

      const changeData = () => {
        info.name = "kobe";
      }
      
       // 1.定义可响应式的对象
      const info = reactive({name: "why", age: 18});
      const name = ref("why");

      // 2.侦听器watch
      watch([() => ({...info}), name], ([newInfo, newName], [oldInfo, oldName]) => {
        console.log(newInfo, newName, oldInfo, oldName);
      }，{
        deep: true,
        immediate: true
      })
      return {
        changeData,
        info
      }
    }
</script>
```

从上面代码中，可以知道watch的第一个参数，可以是 回调函数（返回值是我们监听的对象），可以是一个值，可以是一个数组，第二个参数是一个回调函数，当依赖发生变化它会执行，第三个参数是一个对象，可以修改它的一些执行时机

##### 它与watchEffect的区别

1. 它是懒执行副作用，即只有当侦听的源发生变化它才会执行回调函数；
2. 更加明确的说明说明状态应该触发侦听器的重新运行；
3. 可以访问侦听前后的值

#### JSX的babel配置

如果我们能在项目中使用jsx，那么我们需要添加对jsx的支持；jsx我们通常会通过babel进行转换（react也是）

对于vue 我们只需要在Babel中配置对应的插件既可

```bash
npm install @vue/babel-plugin-jsx -D
//在babel.config.js文件配置插件
module.exports = {
  presets:[
    '@vue/cli-plugin-bable/preset'
  ],
  plugins: [
    "@vue/babel-plugin-jsx"
  ]
}
```

#### 组合式API中的Provide和Inject

```vue
<script>
  // 父组件中，我们通过Provide来创建数据
  import { provide } from 'vue';
   const name = ref("coderwhy");
   let counter = ref(100);
   provide("name", readonly(name));
   provide("counter", readonly(counter));
  // 子组件通过Inject来获取父组件的值
  import { inject } from 'vue';
  const name = inject("name");
  const counter = inject("counter");
</script>
```

##### 注意：

1. 我们可以在provide值时使用ref和reactive来增加provide值和inject值之间的响应性, 这样父组件修改保存到`provide`的值，子组件也会接收到变化
2. `provide`可以传入两个参数:

1. 1. name：提供的属性名称
   2. value：提供对应的属性值

1. inject可以传入两个参数：

1. 1. 要inject的property的name
   2. 默认值

1. 我们需要修改可响应的值，那么我们最好在数据提供处来进行修改，并把修改写成一个方法，并暴露出去给子组件使用