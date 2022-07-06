#### 为什么需要babel?

babel是前端开发不可或缺的一部分，如果我们想要使用ts，es6+等等

babel是一个工具链，主要用于旧游览器兼容，包括语法转化，源代码转换等等

#### babel命令行使用

babel本身可以作为一个独立的工具，不和webpack等构建工具配置来单独使用

我们如果使用babel，需要安装下面这两个库

- `@babel/core`:babel的核心代码，必须安装；
- `@babel/cli`:可以让我们在命令行使用babel

#### 插件的使用

例如我们需要将箭头函数转为ES5的写法，我们需要安装`@babel/plugin-transform-arrow-functions -D`

我们需要将const 转为var ，我们需要使用 `plugin-transform-block-scoping`

#### babel的预设preset

因为转换的内容太多，一个一个下载插件太麻烦，我们就可以使用预设，安装`@babel/preset-env`

#### Babel的底层原理

- 它是把一种源代码转换成另一种源代码 ，所以它就是编译器
- 解析阶段，转换阶段，生成阶段



#### babel的配置文件

想`webpack.config.js`一样，我们可以将babel的配置信息放到一个独立的文件中，babel给我们提供两种配置文件的编写方式

1. `babel.config.json`后缀可以为其他，例如：js，cjs，mjs
2. `.babelrc.json`后缀同上

#### vue源码分类

- runtime+compiler
- runtime-only

#### 运行时+编译时 vs 仅运行时

在Vue开发过程中我们有三种方式来编写Dom元素

- template模板，特定代码进行解析，必须通过源码中的一部分代码来进行编译
- render函数，使用h函数来编写渲染的内容；直接返回一个虚拟节点，Vnode节点
- .vue文件，特定代码进行解析

所以，Vue在我们选择版本的时候分为**运行时+编译时** vs **仅运行时**

**运行时+编译时** ：包含了对template模板的编译代码，更加完整，但是也更大一些；

**仅运行时 ：**没有包含对template版本的编译代码，相对小一些

#### vue警告

![img](https://cdn.nlark.com/yuque/0/2022/png/21765913/1657014987290-59e202c6-215a-48d2-80f4-3781c4823d4c.png)

解决办法：

```javascript
 new DefinePlugin({
      BASE_URL: "'./'",
      // 是否对vue2作适配，如果true则支持，如果设置为false，则会做tree shaking
      __VUE_OPTIONS_API__: true,
     // 生产环境是否开启 调试工具，false则关闭
      __VUE_PROD_DEVTOOLS__: false
    }),
```

#### 注意

以@符号开头的库，都是用monorepo来管理的