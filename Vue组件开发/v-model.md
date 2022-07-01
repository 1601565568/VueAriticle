使用场景
用于表单提交，在代码逻辑中获取用户提交的数据，我们通常会使用v-model来完成，
使用
● v-model可以在表单input，textarea，select等表单上创建双向绑定
● 他会根据控件类型自动选取正确的方法来更新元素
● 但v-model本质上不过是语法糖，它负责监听用户的输入事件来更新数据，并在某些极端情况进行特殊处理
原理
● v-bind绑定value属性的值
● v-on绑定input事件监听到函数中，函数会获得最新的值复制到绑定的属性中
● input使用v-model，通过VNode编译出的代码比较复杂
v-model的修饰符
● .lazy：默认情况下，v-model绑定的是input事件，.lazy就会将我们绑定的input事件改为change事件
● .number：将数据类型转化为数字类型
● .trim：自动过滤用户输入的空白字符
注意
1. v-model绑定的永远都是string类型
问题
1. 自定义组件，使用v-model？
