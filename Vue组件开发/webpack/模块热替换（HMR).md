Hot Module Replacement 模块热替换；是指 在应用程序运行过程中，替换，添加，删除模块，不需要重新刷新整个页面

- 它可以不重新加载整个页面，这样可以保留某些应用程序的状态不丢失
- 只更新变化的内容，修改css，js。会直接立马更新在游览器

#### 使用方法

devServer下新增`hot:true`，还有`target:"web"`

##### 指定文件热替换

```javascript
import "./js/element";

if (module.hot) {
  module.hot.accept("./js/element.js", () => {
    console.log("element模块发生更新了");
  })
}
```

#### HMR原理

- webpack-dev-server会创建两个服务；提供静态资源服务（express)和Socket服务（netSocket）
- express server 负责直接提供静态资源的服务-打包后的资源直接被游览器请求和解析
- Socket是可以做即时通讯，服务器直接向客户端发送资源，
- Socket是一个长链接，长链接最好的好处，就是建立连接后双方可以通信（服务可以直接发送文件到客户端）
- 当服务器监听到对应模块发生变化，会生成两个文件.json(manifest文件)和js文件（update chunk）
- 通过长连接，可以直接将这两个文件发送客户端，（游览器）
- 游览器拿到这两个文件，通过HMR runtime机制，加载这两个文件，并且针对修改的模块进行更新

#### hotOnly，host配置

#### host设置主机地址

- 默认值是localhost;也就是127.0.0.1
- 如果希望其他地方也可以访问，可以设置为0.0.0.0；

#### localhost和0.0.0.0的区别

- localhost:本质上是一个域名，通常情况下会被解析成127.0.0.1；
- 127.0.0.1：回环地址(Loop Back Address),表达的意思其实是我们主机自己发出去的包，直接被自己接收
- 正常的数据库包会经过 应用层 传输层 网络层 数据销路层 物理层；
- 而回环地址，是在网络层直接就被获取到了，是不会经过数据链路层和物理层的
- 比如我们监听127.0.0.1时，在同一个网段下的主机中，通过ip地址是不能访问的，

- 0.0.0.0：监听1PV4上所有的地址，再根据端口找到不同的应用程序，
- 比如我们监听0.0.0.0时，在同一个网段下的主机中，通过地址是可以访问的

#### proxy参数

- target:表示的是代理到的目标地址，比如/api-hy/moment:会被代理到http:/八ocalhost:8888/api-hy/moment
- pathRewrite:默认情况下，我们的/api-hy也会被写入到URL中，如果希望删除，可以使用pathRewrite;
- secure:默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false;
- changeOrigin：它表示是否更新代理后请求的headers中host地址；

#### historyApiFallback

historyApiFallback是开发中一个非常常见的属性，它主要的作用是解决SPA页面在路由跳转之后，进行页面刷新时，返回404的错误。

boolean值：默认是false，如果设置为true,那么在刷新时，返回404错误时，会自动返回index.html的内容；

object类型的值，可以配置rewrites属性（了解）：

可以配置from来匹配路径，决定要跳转到哪一个页面；

事实上devServerr中实现historyApiFallback功能是通过connect-history-api-fallback库的

#### resolvei模块解析

resolve用于设置模块如何被解析：

在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库；

resolve可以帮助webpack从每个require,/import语句中，找到需要引入到合适的模块代码；

webpack使用`**enhanced-resolve**`来解析文件路径；

#### webpack能解析三种文件路径

- 绝对路径 ： 不需要解析
- 相对路径，使用import或require的资源文件所处的目录，被认为是上下文目录；在import/require中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径；
- 模块路径，在resolve.modules中指定的所有目录检索模块：默认值是['node_modules'],所以默认会从node_modulest中查找文件；我们可以通过设置别名的方式来替换初识模块路径，具体后面讲解alias的配置；

#### extensions和alias配置

- extensions是解析到文件时自动添加的扩展名；默认值是['.wasm', 'mjs','js','json'] 
- 我们也可以自定义添加想要的后缀

#### 注意

- vue-loader会帮助我们做模块热替换
- 默认情况，不做gzip压缩，