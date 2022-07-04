- 在webpack5之前，加载资源我们会使用一些loader，例如：raw-loader，url-loader，file-loader
- 在webpack5开始，我们可以直接使用**资源模块类型（asset module type )** 来代替上面的loader

#### 资源模块类型使用

资源模块类型(asset module type),通过添加4种新的模块类型，来替换所有这些loader:

- asset/resource发送一个单独的文件并导出URL。之前通过使用file-loader实现：
- asset/inline导出一个资源的data URI。,之前通过使用url-loader实现；
- asset/source导出资源的源代码。之前通过使用raw-loader实现：
- asset在导出一个data URI和发送一个单独的文件之间自动选择。之前通过使用url-loader,并且配置资源体积限制实现；

```javascript
  {
        test: /\.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]"
        },
        parser: {
          dataUrlCondition: {
            maxSize: 100 * 1024
          }
        }
      },
```