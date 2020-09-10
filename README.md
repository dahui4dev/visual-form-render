# visual-form-render

> 可视化表单生成工具：通过结构化 json 生成渲染表单工具，可选择配套 [visual-form-builder](https://github.com/dahui4dev/visual-form-builder) 使用（目前构造器还需要完善）。
>
> visual-form-builder 可使用可视化的方式生成结构化 json，供本工具使用。

[![NPM](https://img.shields.io/npm/v/visual-form-render.svg)](https://www.npmjs.com/package/visual-form-render)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## 安装

```bash
yarn add visual-form-render

or

npm install --save visual-form-render
```

## 使用

- 更详细的使用可参考 `/example/src/app.tsx`

- 运行示例代码

```bash
cd example && yarn && yarn start
```

```tsx
import React, { Component } from 'react'

import { FormRender } from 'visual-form-render'
import 'visual-form-render/dist/index.css'

const formItemData = [
  {
    type: 'input',
    name: 'idcard',
    label: '单行文本',
    tooltip: '输入唯一身份ID',
    placeholder: '输入16位身份id',
    value: '110120119',
    description: '',
    prefix: '+86',
    suffix: '',
    errorLabel: 'ID为必填项，且不可为空',
    required: true,
  },
  {
    type: 'radio',
    name: 'memberRole',
    label: '单选',
    tooltip: '',
    value: '0',
    options: [
      {
        label: '管理员',
        value: '0',
      },
      {
        label: '普通用户',
        value: '1',
      },
      {
        label: '审核人员',
        value: '2',
      },
    ],
    errorLabel: '',
    required: false,
  },
  {
    type: 'button',
    name: 'submitBtn',
    btnType: 'submit',
    label: '提交',
  },
]

const formGlobalConfigData = {
  formLayout: 'vertical',
  labelColSpan: 6,
  wrapperColSpan: 16,
  btnColSpan: 20,
  showLabel: true,
  fontSize: '14px',
  fontColor: '#000000',
  gridLayoutCols: 2,
  disableDragGridLayout: true,
}

class Example extends Component {
  render() {
    return (
      <FormRender
        key={'FormRender'}
        formItemConfig={formData}
        formGlobalConfigData={config}
        autoSubmit
        isEdit
        outputCB={(outputJson: any) => {
          console.log('-example output：', outputJson)
          setResult(JSON.stringify(outputJson, null, 2))
        }}
      />
    )
  }
}

export default Example
```

## API

### FormRender 表单渲染器

|         属性          |                     类型                      | 默认值 | 必填项 | 描述                                     |
| :-------------------: | :-------------------------------------------: | :----: | :----: | ---------------------------------------- |
|          key          |                    string                     |   -    | false  | 表单 key                                 |
|    formItemConfig     |  [FormItemSchemaType](#formitemschematype)[]  |   -    |  true  | 渲染表单结构数据，支持传入单个对象或数组 |
| formGlobalConfigData  | [GlobalFormConfigType](#globalformconfigtype) |   -    |  true  | 渲染表单全局配置数据                     |
| formItemInitialValues |                      any                      |   -    | false  | 表单数据初始值（可选）                   |
|        isEdit         |                    boolean                    |  true  | false  | 是否编辑状态                             |
|      autoSubmit       |                    boolean                    | false  | false  | 是否触发自动提交表单                     |
|       outputCB        |          (outputJson: string) => any          |   -    | false  | 提交表单回调                             |

### FormItemSchemaType

表单项：属性配置结构

|       属性        |                      类型                       | 默认值 | 必填项 | 描述                   |
| :---------------: | :---------------------------------------------: | :----: | :----: | ---------------------- |
|        key        |                     string                      |   -    | false  | 表单项唯一 uuid        |
|       name        |                     string                      |   -    |  true  | 表单项 name 不可重复   |
|       type        |          [ItemTypeEnum](#itemtypeenum)          |   -    | false  | 表单项 类型：枚举      |
|       label       |                     string                      |   -    | false  | 表单项 标题            |
|      tooltip      |                     string                      |   -    | false  | 表单项 标题说明        |
|    placeholder    |                     string                      |   -    | false  | 表单项 placeholder     |
|      prefix       |                     string                      |   -    | false  | 表单项 前缀            |
|      suffix       |                     string                      |   -    | false  | 表单项 后缀            |
|       value       |                       any                       |   -    | false  | 表单项 value           |
|      options      |          [ItemOptions](#itemoptions)[]          |   -    | false  | 表单项 options         |
|      hidden       |                     boolean                     |   -    | false  | 表单项 是否隐藏        |
|     disabled      |                     boolean                     |   -    | false  | 表单项 是否禁用        |
|     showLabel     |                     boolean                     |   -    | false  | 表单项 是否显示标题 ｜ |
|    errorLabel     |                     string                      |   -    | false  | 表单项 错误提示文案 ｜ |
|     required      |                     boolean                     | false  |  true  | 表单项是否必填         |
| beforeConditional | [BeforeConditionalType](#beforeconditionaltype) |   -    | false  | 表单项 前置条件        |
| afterConditional  |  [AfterConditionalType](#afterconditionaltype)  |   -    | false  | ⛔️ 表单项 后置条件    |

### ItemTypeEnum

支持表单类型枚举，现已支持以下类型表单。

```text
'input' | 'textarea' | 'radio' | 'select' | 'checkbox' | 'date' | 'span' | 'button' | 'array'
```

### ItemOptions

表单项 options。用于 select.options、radio.options

| 属性  |  类型  | 默认值 | 必填项 | 描述           |
| :---: | :----: | :----: | :----: | -------------- |
| label | string |   -    |  true  | 表单项选项标题 |
| value |  any   |   -    |  true  | 表单项选项值   |

### BeforeConditionalType

> 目前仅支持计算关联表单项等于某个值的时候控制当前表单项显示隐藏。

|     属性     |  类型  | 默认值 | 必填项 | 描述                   |
| :----------: | :----: | :----: | :----: | ---------------------- |
| fromItemName | string |   -    |  true  | 关联计算表单 item name |
|  condition   | string |   -    |  true  | 算式：现在只支持 “=”   |
|    value     | string |   -    |  true  | 关联表单项 计算 值     |

### ⛔️ AfterConditionalType

**\* 🚫 此部分功能近期正在开发中，API 可能有大的改动，暂时不要使用**

|        属性        |                   类型                    | 默认值 | 必填项 | 描述       |
| :----------------: | :---------------------------------------: | :----: | :----: | ---------- |
|     funcParams     |                  object                   |   -    |  true  | 函数名     |
|      funcBody      |                  string                   |   -    |  true  | 函数体     |
| associatedFormItem | [AssociatedFormItem](#associatedformitem) |   -    | false  | 关联表单项 |

### GlobalFormConfigType

|         属性          |            类型            | 默认值 | 必填项 | 描述                       |
| :-------------------: | :------------------------: | :----: | :----: | -------------------------- |
|      formLayout       | 'horizontal' \| 'vertical' |   -    | false  | 表单排版布局               |
|     labelColSpan      |           number           |   -    | false  | label 所占列数             |
|    wrapperColSpan     |           number           |   -    | false  | wrapper 输入框所占列数     |
|      btnColSpan       |           number           |   -    | false  | btnColSpan                 |
|       showLabel       |          boolean           |   -    | false  | 是否显示 label             |
|       fontSize        |           string           |   -    | false  | 字体大小                   |
|       fontColor       |           string           |   -    | false  | 字体颜色                   |
|    gridLayoutCols     |           number           |   -    | false  | 表单整体栅格化布局，列数   |
| disableDragGridLayout |          boolean           |   -    | false  | 是否开启表单可拖拽列数开关 |

## 待办列表

- [x] 渲染表单类型
  - [x] input
  - [x] textarea
  - [x] radio
  - [x] select
  - [x] checkbox
  - [x] date
  - [x] span
  - [x] button
  - [x] array
  - [ ] object
  - [ ] file
  - [ ] number
  - [ ] url
  - [ ] color
  - [ ] switch
  - [ ] sider
  - [ ] autocomplate
- [x] 增加前置条件
- [ ] 增加后置条件
- [x] 支持表单栅格化布局

## License

MIT © [dahui4dev](https://github.com/dahui4dev)
