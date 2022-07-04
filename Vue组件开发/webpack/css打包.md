#### 上面的错误信息告诉我们需要一个loader来加载这个css文件，但是loader,是什么呢？

- loader可以用于对模块的源代码进行转换；
- 我们可以将css文件也看成是一个模块，我们是通过import来加载这个模块的；
- 在加载这个模块时，webpack其实并不知道如何对其进行加载，我们必须制定对应的loader:来完成这个功能；

#### 那么我们需要一个什么样的loader!呢？

- 对于加载css文件来说，我们需要一个可以读取css文件的loader;
- 这个loader最常用的是css-loader 安装命令：`npm install css-loader -D`

#### css-loader加载css文件的三种方式

- 内联方式，

```javascript
import "css-loader!../css/style.css"
```

- CLI方式（webpack5中不再使用）
- 配置方式

配置方式表示的意思是在我们的webpack.config,js文件中写明配置信息：

- module.rules中允许我们配置多个loader(因为我们也会继续使用其他的loader,来完成其他文件的加载)：
- 这种方式可以更好的表示loader的配置，也方便后期的维护，同时也让你对各个Loader有一个全局的概览；

module.rules的配置如下：

rules属性对应的值是一个数组：[Object] 例如：{test:/\.css$/, loader: "css-loader"}

#### postcss

postcss是一个通过JavaScript来转换样式的工具

这个工具可以帮助我们进行一些css的转换和适配，自动添加游览器前缀，css样式重置，



#### autoprefixer

因为我们需要添加前缀，所以要安装autoprefixer:

```
npm install autoprefixer -D
```

直接使用使用postcss工具，并且制定使用autoprefixer

```
npx postcss --use autoprefixer -o end.css ./src/css/style.css
```

#### postcss-preset-env

- 一个postcss插件
- 可以帮助我们将一些现代的css特效转化为游览器认识的css，并且会根据游览器添加对应的polyfill，例如自动帮我们添加autoprefixer