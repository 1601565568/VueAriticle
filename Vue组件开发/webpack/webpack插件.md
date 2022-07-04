webpack的另一个核心plugin

```
While loaders are used to transform certain types of modules,plugins can be leveraged to perform a wider range of tasks like bundle optimization,asset management and injection of environment variables.
```

加载器用于转换某些类型的模块，而插件可以用于执行更广泛的任务，如包优化、资产管理和环境变量注入。

#### 1. ClearWebpackPlugin

一个帮忙自动清理打包后的文件夹的插件

```javascript
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
```

#### 2. HtmlWebpackPlugin

另外还有一个不太规范的地方：

- 我们的HTML文件是编写在根目录下的，而最终打包的dist文件夹中是没有index.html文件的。
- 在进行项目部署的时，必然也是需要有对应的入口文件index.html;
- 所以我们也需要对index.html进行打包处理；

#### 3. DefinePlugin

当我们定义我们的index模板，里面使用EJS要使用BASE_URL常量；

这个插件是webpack内置的一个插件

```javascript
const { DefinePlugin } = require("webpack");
 new DefinePlugin({
  BASE_URL: "'./'"
}),
```

#### CopyWebpackPlugin

// 告诉打包后复制那些内容

```javascript
const CopyWebpackPlugin = require('copy-webpack-plugin');
```

#### public文件夹

如果我们把一些文件放在public文件夹下，那么打包的时候，它会帮我们把public下的文件复制到打包之后的文件夹下

#### 调试代码，知道错误位置

在`webpack.config.js`中，设置`devtool:'source-map'`

在设置这个之前，还需要设置`mode:"development"` mode配置其实webpack帮助我们设置了更多

#### 插件使用

```javascript
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈"
    }),
    new DefinePlugin({
      BASE_URL: "'./'"
    }),
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "public",
          to: "./",
          globOptions: {
            ignore: [
              "**/index.html"
            ]
          }
        }
      ]
    })
  ],
```

#### 注意

- 插件需要手动导入
- 插件一般都是class(一个类)
- 使用插件一般都是一个对象，通过实例化类来创建对象
- 插件没有顺序
- 我们设置的plugin是会合并默认的，并不会覆盖掉之前的