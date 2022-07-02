#### 为什么要webpack？

- 因为我们需要使用`vue CLI `，`vue CLI`本质上是基于`webpack`的,
- 模块化开发，使用高级语法，如es6，ts less sass等等
- 开发中，我们希望实时的监听文件的变化，并且反映到游览器上，提高开发效率
- 开发完成 build代码 合并代码及其它的优化
- 三大框架（vue，react，angular）都是基于cli创建，cli又都是借助webpack帮忙打包优化

#### 什么是webpack

官方定义：`webpack is static module bundler for modern javascript applications` 

翻译：**webpack是一个静态的模块化的打包工具，为现代的JavaScript应用程序**

- 打包bundler: webpacki可以将帮助我们进行打包，所以它是一个打包工具
- 静态的static: 这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器）；
- 模块化module: webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等；
- 现代的modern: 我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了webpack的出现和发展；

#### vue项目加载的文件有哪些？

##### JavaScript的打包：

- 将ES6转换成ES5的语法；
- TypeScript的处理，将其转换成JavaScript;

##### Css的处理：

- CSS文件模块的加载、提取；
- Less、Sass等预处理器的处理；

##### 资源文件img、font:

- 图片img文件的加载；
- 字体font文件的加载；

##### HTML资源的处理：

- 打包HTML资源文件；

##### 处理vue项目的SFC文件.vue文件；



#### 安装webpack

需要安装webpack，webpack-cli

webpack-cli作用：运行webpack开头的命令

#### 注意

webpack的运行时依赖Node.js环境的 