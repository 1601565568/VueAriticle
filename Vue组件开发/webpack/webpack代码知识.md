#### 模块化

- ESModule导出 

```javascript
export default function cz () {console.log('hello world')}
```

- CommonJS导出

```javascript
const addFn = (a, b) {
  return a + b ;
}
module.export = {
    addFn: addFn
}
```
#### 导入

我们可以不加文件名的后缀，因为webpack会帮我们自动补充的



#### #### .browserslistrc

- `> 1%`：代表市场份额大于1%
- `last 2 versions`：最后两个版本
- `not dead`:还活着呢？ 任然咋维护

#### 小常识

dist文件夹名是`disttibution`的缩写，