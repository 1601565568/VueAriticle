#### 加载文件 file-loader

如果我们需要处理图片（jpg，png），我们也需要对应的loader

file-loader作用就是帮助我们处理import/require()方式导入一个文件资源，并且会将它放到我们输出的文件夹中，

#### 修改打包后的资源的路径和资源的名称

有的时候我们想处理打包过后的文件，例如保留原来的文件名，自定义文件夹名称，我们可以使用`PlaceHolders`来完成，webpack给我们提供很多placeholder

我们这里介绍几个最常用的placeholder:

- [ext)]: 处理文件的扩展名；

- [name]: 处理文件的名称：

- [hash]: 文件的内容，使用MD4的散列函数处理，生成的一个128位的hash值(32个十六进制)：

- [contentHash]: 在file-loader中和hash结果是一致的（在webpack的一些其他地方可能不一样

- [hash:<length>]: 截图hash的长度，默认32个字符太长了；

- [path)]: 文件相对于webpack配置文件的路径；

```javascript
{
  test: /\.(jpe?g|png|gif|svg)$/,
  use: {
    loader: "url-loader",
    options: {
      // outputPath: "img", 输出的路径
      name: "img/[name]_[hash:6].[ext]",
      limit: 100 * 1024
    }
  }
},
```

#### url-loader

作用：可以将较小的文件，转为base64的URL

#### 加载字体文件