搭建本地服务，利用热模块替换功能实现，热更新

#### 为什么需要搭建本地服务？

因为我们现在每次修改代码都需要重新 `npm run build`非常麻烦，不能修改之后立即查看游览器反应，我们希望可以做到，当文件发生变化，自动完成编译和展示

它是基于node中express框架搭建的一个服务器

#### 实现自动编译，webpack提供了几种可选的方案

- webpack watch mode
- webpack dev-server(常用）
- webpack dev-middleware

#### webpack dev-server

它可以帮助我们监听文件的变化并且自动刷新游览器，

##### 安装

```bash
npm install webpack-dev-server -D
```

##### 配置

修改配置文件，告诉webpack，从上面位置查找文件

#### 实现原理

它是先帮我们打包好之后的文件放入到内存中，并没有作为文件的输出，然后通过express服务器访问打包到内存中的静态资源，游览器访问也是文件内存中的资源，

webpack-dev-serve 使用一个库叫memfs（memory-fs webpack)

#### 代码

```javascript
 devServer: {
   // 让文件夹下的内容也作为服务
    contentBase: "./public",
    hot: true,
    host: "0.0.0.0",
    port: 7777,
    open: true, // 是否自动打开游览器
    // compress: true, 是否使用g-zip帮忙压缩
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true
      }
    }
  },
  resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": path.resolve(__dirname, "./src/js")
    }
  },
```

#### 注意

- 修改webpack配置，需要重新跑服务