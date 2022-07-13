#### v-once：用于指定元素或者组件只渲染一次

当数据发生变化时，元素或者组件以及其所有的子元素将视为静态内容并且跳过；用于**性能优化**

#### v-bind: 动态绑定一个或多个`attribute`(ps:属性), 或者向另一个组件传递props值

v-bind的语法糖是 `:`

```vue
<img v-bind:src='src' alt='' />
<img :src='src' />
// 其他写法
<div :[name]="value">哈哈哈</div>
data() {
   return {
     name: "cba",
     value: "kobe"
   }
 }
<div :="info">哈哈哈哈</div> 
info: {
   name: "why",
   age: 18,
   height: 1.88
 }
```

#### v-on ：绑定事件

前面我们绑定了元素的`内容和属性`,在前端开发中另外一个非常重要的特性就是`交互` , 所以vue是使用`v-on`来实现事件监听

###### v-on的使用：

1. 缩写 :·`@`
2.  参数：`event`
3. 修饰符：

1. 1. `stop`- 调用 `event.stopPropagation()`
   2. `.prevent` - 调用 `event.preventDefault()`。
   3. `.capture`－添加事件侦听器时使用`capture`模式。
   4. `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。`.{keyAlias} `- 仅当事件是从特定键触发时才触发回调。
   5. `.once` - :只触发一次回调。
   6. `.left` - 只当点击鼠标左键时触发。
   7. `.right` - 只当点击鼠标右键时触发。
   8. `.middle` - 只当点击鼠标中键时触发。
   9. `.passive `-` { passive: true } `模式添加侦听器☐ 用法：绑定事件监听

```vue
<button v-on:click="btn1Click($event,'syh')">按钮1</button>
<div class="area" v-on:mousemove="mouseMove">div</div>
<div class="area" @="{click: btn1Click, mousemove: mouseMove}"></div>
methods: {
   btn1Click() {
     console.log("按钮1发生了点击");
   },
   mouseMove() {
     console.log("鼠标移动");
   }
 }
```

#### `v-show`和 `v-if`的区别

###### 首先

1. `v-show`是不支持`template`
2. `v-show`不可用和`v-else`一起使用；

###### 其次，本质的区别：

1. `v-show`元素无论是否需要显示。它的Dom实际上都存在
2. `v-if`当条件为`false`的时候；其对应的原生压根不会渲染到dom中

###### 最后；使用场景

1. 如果我们的原生需要在显示和隐藏之间频繁的切换，那么使用`v-show`否则就使用`v-if`

###### 注意： 

因为`v-if`是一个指令，所有必须将其添加到一个元素上；但是我们切换的是多个元素呢？这个时候我们就可以使用`template`

```vue
<template v-if="isShowHa">
   <h2>哈哈哈哈</h2>
   <h2>哈哈哈哈</h2>
   <h2>哈哈哈哈</h2>
 </template>
```

#### `v-for`列表渲染

###### 基本使用

基本格式是： `item in arr`

`item`是我们给每项元素起的一个别名， `arr`是数组名称

当我们遍历一个数组的时候有时候会需要拿到`数组的索引`

我们可以使用格式：`(item, index) in arr`;

```vue
  <template id="my-app">
    <h2>电影列表</h2>
    <ul>
      <!-- 遍历数组 -->
      <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
    <h2>个人信息</h2>
    <ul>
      <!-- 遍历对象 -->
      <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
    </ul>
    <h2>遍历数字</h2>
    <ul>
      <li v-for="(num, index) in 10">{{num}}-{{index}}</li>
    </ul>
  </template>

 const App = {
      template: '#my-app',
      data() {
        return {
          movies: [
            "星际穿越",
            "盗梦空间",
            "大话西游",
            "教父",
            "少年派"
          ],
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }
```

![img](https://cdn.nlark.com/yuque/0/2022/png/21765913/1646978116159-ad9044e1-c6a9-44fa-898c-93b7c39f20b3.png)

### 注意：

当我们需要循环(`v-for`)包括多个元素或者判断包括多个元素（`v-if`）的时候，我们可以使用`templete`来包裹这多个元素。

#### `vue`对原生数组的封装（使用其方法会触发ui变化）

1. `push()``pop()` `shift()` `unshift()``splice()` `sort()` `reverse()` 上面的是直接改变原生数组
2. `filter()` `concat()` `slice()` 不会改变原数组

### Vue3的Options-API

在实际开发中，我们会遇到一些情况，比如我们可能需要**对数据进行一些转化**后再显示，或将**多个数据结合起来**进行显示；如果把这些逻辑直接写在`template`中，这样就会使得UI里放逻辑，就违背了`vue`设计的愿景, 这个时候`compute`计算属性就产生了

#### computed 计算属性

对于任何响应式数据的复杂逻辑，都应该放在使用`computed`

`计算属性`将被混入到组件实例中。所有getter和setter的上下文自动绑定为组件实例

###### 使用方法：

```vue
  data() {
        return {
          firstName: "Kobe",
          lastName: "Bryant",
          score: 80,
          message: "Hello World"
        }
      },
      computed: {
        // 定义了一个计算属性叫fullname
        fullName() {
          return this.firstName + " " + this.lastName;
        },
        result() {
          return this.score >= 60 ? "及格": "不及格";
        },
        reverseMessage() {
          return this.message.split(" ").reverse().join(" ");
        }
      }
```



#### watch 监听器

我理解为当data变化后watch定义的函数来执行data变化产生的副作用

###### 使用：

```vue
data() {
  return {
    // 侦听question的变化时, 去进行一些逻辑的处理(JavaScript, 网络请求)
    question: "Hello World",
    anwser: ""
  }
},
watch: {
  // question侦听的data中的属性的名称
  // newValue变化后的新值
  // oldValue变化前的旧值
  question: function(newValue, oldValue) {
    console.log("新值: ", newValue, "旧值", oldValue);
    this.queryAnswer();
  }
},
```

###### 监听器`watch`的配置选项：

1. `watch`只是在侦听info的引用变化，对于内部属性的变化说不会做出相应的，我们可以使用`选项deep`进行更深层的侦听；
2. 另外一个属性，是希望一开始就会立即执行一次：

1. 1. 这个时候我们使用`immediate选项`
   2. 这个时候无论后面的数据是否有变化，侦听的函数都会有限执行一次；

```vue
 watch: {
        // 默认情况下我们的侦听器只会针对监听的数据本身的改变(内部发生的改变是不能侦听)
        // info(newInfo, oldInfo) {
        //   console.log("newValue:", newInfo, "oldValue:", oldInfo);
        // }

        // 深度侦听/立即执行(一定会执行一次)
        info: {
          handler: function(newInfo, oldInfo) {
            console.log("newValue:", newInfo.nba.name, "oldValue:", oldInfo.nba.name);
          },
          deep: true, // 深度侦听
          // immediate: true // 立即执行
        },
					//也可以监听info的属性
				'info.name': function(){
          console.log(this.info.name, 'info.name');
        }
      },
```

###### `watch`的其他使用方法

```vue
 const unwatch = this.$watch("info", function(newInfo, oldInfo) {
          console.log(newInfo, oldInfo);
        }, {
          deep: true,
          immediate: true
        })
```

#### `v-model`绑定表单数据

1. `v-model`指令可以在表单 `input`，`textarea`以及`select`元素上创建双向数据绑定，
2. 它会根据控件类型自动选择正确的方法来更新元素
3. `v-model`本质上是语法糖，它负责监听用户的输入事件来更新数据，并在极端场景下进行一些特殊处理；

###### `v-model`的原理 ：两个操作

- `v-bind`绑定`value`属性的值；
- `v-on`绑定`input`事件监听到函数中，函数会获取最新的值赋值到绑定的属性中

###### 具体使用

```vue
//绑定textarea article是一个字符串
<textarea v-model="article"> </textarea>

//绑定checkbox -单个勾选框 isAgree是一个布尔值
<input id='agreement' type="checkbox" v-model="isAgree" />
//绑定checkbox -多个复选框 hobbies是一个数组
<label for="basketball">
  <input id="basketball" type="checkbox" v-model="hobbies" value="basketball"> 篮球
</label>
<label for="football">
  <input id="football" type="checkbox" v-model="hobbies" value="football"> 足球
</label>
<label for="tennis">
  <input id="tennis" type="checkbox" v-model="hobbies" value="tennis"> 网球
</label>

//radio gender是一个字符串 是input的value
<label for="male">
  <input id="male" type="radio" v-model="gender" value="male">男
</label>
<label for="female">
  <input id="female" type="radio" v-model="gender" value="female">女
</label>

// 绑定select fruit是一个字符串的时候就是单选，如果是一个数组就是多选
<select v-model="fruit" multiple size="2">
  <option value="apple">苹果</option>
  <option value="orange">橘子</option>
  <option value="banana">香蕉</option>
</select>
```

#### v-model 的修饰符

###### 1. -lazy

默认情况下，v-model在进行双向绑定时，绑定的是`input事件`那么每次输入后就会将最新的值和绑定的属性进行同步，如果我们加上`lazy`符号那么绑定的事件切换为`change`事件，只有在提交时候才会触发

```vue
<template id="my-app">
	<input type ="text" v-model.lazy = "message">
	<h2>{{ message }}</h2>
</template>
```

###### 2. - number

如果我们希望将输入转化为数字，那么就使用`.number`在可以转化的情况下，会将其转化为`number`类型

```vue
<input type='text' v-model.number = 'message' />
```

###### 3. -trim

如果要自动过滤掉用户输入的空白字符，可以给`v-model`后面在上`.trim`修饰符

#### 组件化开发

###### 定义一个全局组件

```vue
// 使用app注册一个全局组件app.component()
// 全局组件: 意味着注册的这个组件可以在任何的组件模板中使用
<script>
app.component("component-a", {
  template: "#component-a",
  data() {
    return {
      title: "我是标题",
      desc: "我是内容, 哈哈哈哈哈",
    };
  },
  methods: {
    btnClick() {
      console.log("按钮的点击");
    },
  },
});
</script>
//模板
<template id="component-a">
   <h2>{{title}}</h2>
   <p>{{desc}}</p>
   <button @click="btnClick">按钮点击</button>
</template>
// 使用
<component-a></component-a>
```

在通过`app.component`注册一个组价的时候，第一个参数是组件的名称，第二个是一个对象

#### 注册局部组件

全局组价是在程序一开始就会执行，全局组件就会被注册好， 但是有时候我们的某些组件在刚开始并没有被用到。并且全局组件`webpack`打包时候也是对其打包，用户在下载对应JavaScript包时候也会增加包的大小

所以我们在开发中使用组件应采用**局部注册**：

- 局部注册是在我们需要使用到的组件中，通过`component`属性选择来进行注册，类似于`computed`,`methods`
- 该`components`选项对应的是一个对象，对象中的键值对是 **组件的名称：组件对象**

```vue
  <template id="component-a">
    <h2>我是组件A</h2>
    <p>我是内容, 哈哈哈哈</p>
  </template>
  <script>
    const ComponentA = {
      template: "#component-a"
    }
    const App = {
      template: '#my-app',
      components: {
        ComponentA: ComponentA
      },
    }
    const app = Vue.createApp(App);
    app.mount('#app');
  </script>
```