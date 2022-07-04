#### 为什么需要babel?

babel是前端开发不可或缺的一部分，如果我们想要使用ts，es6+等等

babel是一个工具链，主要用于旧游览器兼容，包括语法转化，源代码转换等等

#### babel命令行使用

babel本身可以作为一个独立的工具，不和webpack等构建工具配置来单独使用

我们如果使用babel，需要安装下面这两个库

- `@babel/core`:babel的核心代码，必须安装；
- `@babel/cli`:可以让我们在命令行使用babel