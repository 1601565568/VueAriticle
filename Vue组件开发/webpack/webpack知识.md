#### 导入

我们可以不加文件名的后缀，因为webpack会帮我们自动补充的

#### 创建packag.json

```bash
npm init -y
// 开发依赖
--save-dev === -D
// 生产依赖 => 默认的就是生产依赖
```

#### webpack命令行代码

```bash
npx webpack --entry ./src/main.js // 后面 --entry + 文件路径 也就是指定打包入口
--output--path // 指定出口文件夹
```

#### webpack配置

- 我们会在我们项目根目录下创建一个`webpack.config.js`，在内部写webpack的一些配置信息
- 如果我们的配置文件名不是上面这个 我们需要在package.json中配置 ，在脚本中，`"build":"webpack --config zhang.config.js"`

#### 小常识

dist文件夹名是`disttibution`的缩写，

是否依赖特别重要

##### loader加载顺序

loader的执行顺序是从右向左，所以先执行的要放在后面