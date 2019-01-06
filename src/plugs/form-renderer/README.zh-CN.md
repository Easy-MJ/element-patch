# el-form-renderer

基于 [element-ui](https://github.com/ElemeFE/element) 封装的表单渲染器，完整继承了 element 的属性定义，并进行了简单扩展，从而用户能够通过使用一段预设的数据渲染出一个完整的 element 表单。

## Props

* 支持 [el-form](http://element.eleme.io/#/zh-CN/component/form) 上的所有属性。

* `disabled` [Boolean] 设置为 `true` 可禁用所有原子表单。`element-ui` 版本如果在 `2.1.0` 以下本渲染器依旧兼容。

* `content` [ObjectArray] 定义表单的内容，每一个 `Object` 代表一个原子表单(`el-input, el-select, ...`)，一切 `el-form-item` 上的属性都在此声明，而对于 `el-input` 等之上的属性在 `$el` 属性上进行声明，该 `Object` 上还存在其他属性，例如: `$id, $type, $default，$enableWhen[可选], $options[可选], $attrs[可选]

```js
// content example
[
  {
    $id: "form1", // 每一个原子都存在id，用于存储该原子的值，注意不能重复
    $type: "input", // 类型，element 提供的所有表单类型，即 el-xxx
    $enableWhen: { form2: 'beijing' }, // 可选属性，表示当 form2 的值为 beijing 时显示
    $attrs: { 'data-name': 'form1' }, // 可选, 写法与 Vue 的 Render 函数规范保持一致
    label: "输入框", // el-form-item上的属性
    $default: "这是默认值",
    rules: [{ required: true, message: '请输入活动名称', trigger: 'blur' }] // el-form-item上的属性
  }, {
    $id: "form2",
    $type: "select",
    label: "选择框",
    // $el 上用于定义具体原子表单(此处为el-select)的属性
    $el: {
      placeholder: "请选择内容"
    },
    // $options 具有选择功能的原子表单可用此定义可选项，例如: select, radio-group, radio-button, checkbox-group, checkbox-button
    $options: [{
      label: '区域一',
      value: 'shanghai'
    }, {
      label: '区域二',
      value: 'beijing'
    }]
  }
]
```

此外，我们为 `$type` 属性增加了一个可选值: `group`，可用于创建更为复杂的表单数据类型:

```js
// 该例将获得对象数据结构:
// group1: {
//   input1: '',
//   input2: ''
// }
{
  $id: "group1", // 遵循同一层级的ID不重复的原则，实质上相当于对象的键
  $type: "group",
  label: "这是一个对象数据",
  $items: [{
    $id: "input1",
    $type: "input",
    label: "输入框1",
    $default: "这是默认值"
  }, {
    $id: "input2",
    $type: "input",
    label: "输入框2",
    $default: "这是默认值",
  }]
}
```

## Methods

* 支持 [el-form](http://element.eleme.io/#/zh-CN/component/form) 上的所有方法。

* 其他方法:

| 方法名 | 描述 | 参数 |
| ---------- | -------- | ---------- |
| getFormValue | 获取当前表单的值 | - |
| updateValue  | 手动更新表单的值 | ({ id, value }) |

## Slot

* 支持通过默认 `slot` 往表单尾部插入自定义 `VNode`。
