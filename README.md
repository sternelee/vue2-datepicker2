# vue2-datepicker2

基于 [vue2-datepicker](https://github.com/mengxiong10/vue2-datepicker) 修改的版本

[x] 添加 `rangeColumn` 配置多月份的选项,默认为三列
[x] 添加 `showNavDate` 来控制是否显示上一年，下一年，上一个月，下一个月的操作

[English Version](https://github.com/sternelee/vue2-datepicker2/blob/master/README.md)

# vue2-datepicker

> 一个基于Vue2.x的日期时间选择组件

<a href="https://travis-ci.org/sternelee/vue2-datepicker2">
  <img src="https://travis-ci.org/sternelee/vue2-datepicker2.svg?branch=master" alt="build:passed">
</a>
<a href="https://coveralls.io/github/sternelee/vue2-datepicker2">
  <img src="https://coveralls.io/repos/github/sternelee/vue2-datepicker2/badge.svg?branch=master&service=github" alt="Badge">
</a>
<a href="https://www.npmjs.com/package/vue2-datepicker">
  <img src="https://img.shields.io/npm/v/vue2-datepicker.svg" alt="npm">
</a>
<a href="LICENSE">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="MIT">
</a>

## 线上Demo
<https://mengxiong10.github.io/vue2-datepicker/demo/index.html>

![image](https://github.com/sternelee/vue2-datepicker2/raw/master/screenshot/demo.PNG)

## 安装

```bash
$ npm install vue2-datepicker --save
```

## 用法

```html
<script>
import DatePicker from 'vue2-datepicker'

export default {
  components: { DatePicker },
  data() {
    return {
      time1: '',
      time2: '',
      time3: '',
      // custom lang
      lang: {
        days: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
        months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
        pickers: ['next 7 days', 'next 30 days', 'previous 7 days', 'previous 30 days'],
        placeholder: {
          date: 'Select Date',
          dateRange: 'Select Date Range'
        }
      },
      // custom range shortcuts
      shortcuts: [
        {
          text: 'Today',
          onClick: () => {
            this.time3 = [ new Date(), new Date() ]
          }
        }
      ],
      timePickerOptions:{
        start: '00:00',
        step: '00:30',
        end: '23:30'
      }
    }
  }
}
</script>

<template>
  <div>
    <date-picker v-model="time1" :first-day-of-week="1"></date-picker>
    <date-picker v-model="time2" type="datetime" :time-picker-options="timePickerOptions"></date-picker>
    <date-picker v-model="time3" range :shortcuts="shortcuts"></date-picker>
    <date-picker v-model="value" :lang="lang"></date-picker>
  </div>
</template>
```
### 属性

| 属性 | 描述  | 类型  | 默认值 |
|------|--------------|-------|---------|
| type | 选择日期类型  | 'date' \| 'datetime' \| 'year' \| 'month' \| 'time' | 'date' |
| range | 如果是true, 显示日历范围选择  | `boolean` | false |
| format | 格式化显示日期, 值类似moment.js | [token](https://github.com/taylorhakes/fecha#formatting-tokens) \| [`object`](https://github.com/sternelee/vue2-datepicker2/issues/232#issuecomment-458558141) | 'YYYY-MM-DD'  |  
| value-type | 设置绑定值的格式, 默认返回日期对象 | [value-type](#value-type) | 'date' |
| lang | 选择语言或自定义 | [lang](#lang) | 'zh' |
| clearable | 如果设置false, 不显示清除图标 | `boolean` | true |
| confirm | 如果是true， 显示确认按钮且需要确认才更新时间 | `boolean` | false |
| editable | 如果是false, 用户不能手动输入更新日期 | `boolean` | true |
| disabled | 禁用组件 | `boolean` | false |
| placeholder | 输入框placeholder | `string` | — |
| width  | 设置宽度  | `string`\|`number` | 210 |
| append-to-body | 弹出层元素插入body下面 | `boolean` | false |
| default-value | 日历的默认值 | `Date` | new Date() |
| popupStyle | 弹出层的样式(可以覆盖left,top样式) | `object` | — |
| not-before | 禁止选择这个时间之前的时间 | `string`\|`Date` | ''|
| not-after | 禁止选择这个时间之前=后的时间 | `string`\|`Date`| '' |
| disabled-days | 自定义禁止的日期 | `(date) => boolean` | - |
| shortcuts | 自定义范围选择的时候快捷选项 | [shortcuts](#shortcuts) | true |
| time-picker-options | 自定义时间选择的开始，结束，步进 | [time-picker-options](#time-picker-options) | null |           
| minute-step | 设置分钟的步进， 设置大于0不显示秒的选择(0-60) | 0 - 60 | 0 |
| first-day-of-week | 设置日历星期几开头 | 1 - 7  | 7 |
| input-class | 自定义input元素的类名 | `string` | 'mx-input' |
| input-attr | 自定义input元素的属性(eg: { required: true, id: 'input'}) | `object` | — |
| confirm-text | 确认按钮的名称 | `string` | 'OK' |
| range-separator | range 分隔符 | `string` | '~' |
| date-format | 格式化时间组件头部和日历的tooltip,默认是format字段去除时间的格式化  | `string` | '' |

#### value-type
设置绑定值的格式

| 可选值           | 描述                               |
|-----------------|-------------------------------------------|     
| date            | 返回的绑定值是Date对象       |
| timestamp       | 返回的绑定值是时间戳数字  |
| format          | 返回的绑定值是通过`format`属性格式化的值   |

高级: 也可以传入一个自定义实现包含2个函数的对象
```js
{
  value2date: (value: any) => Date,  // 转化绑定值到日历时间对象
  date2value: (date: Date) => any   // 转化日历时间对象到绑定值
}

```
#### lang

| 可选值 |
|------|
| 'en'\|'zh'\|'es'\|'pt-br'\|'fr'\|'ru'\|'de'\|'it'\|'cs' |
| { days: string[]; months: string[]; picker: string[]; placeholder: { date: string; dateRange: string  } } |

#### shortcuts

| 可选值           | 描述 |
|-----------------|-------------|     
| true            | 显示默认快捷选择 |
| false           | 隐藏默认快捷选择  |
| [{text: string, onClick: () => any }] | 自定义快捷选择 |

#### time-picker-options
自定义时间选择

| 可选值 |
|------|
| {start: '00:00', step:'00:30' , end: '23:30'} |
| () => Array<{ label: string; values: { hours: number; minutes: number } }> |

### 事件
| Name            | 说明                          |  回调参数       
|-----------------|----------------------------- |----------------
| change          | 日期改变的时候触发              | 选择的日期            
| input           | 日期改变的时候触发              | 选择的日期             
| confirm         | 点击确认按钮触发的事件           | 选择的日期 
| clear           | 清空时候触发                    |          
| input-error     | 当用户输入的值无效时候触发       | 用户输入的字符串         
| panel-change    | 切换面板时触发                 | [panel](#panel), [oldPanel](#panel)      
| calendar-change | 日历的年或月改变时触发           | 当前日历时间，过去日历时间
| focus           | 当输入框获得焦点                |
| blur            | 当输入框失去焦点                |

#### panel

| 值    | 描述          |
|-------|----------------------|     
| NONE  | 当面板关闭时的值 |
| DATE  | 当面板是日期选择时的值   |
| YEAR  | 当面板是年选择时的值   |
| MONTH | 当面板是月选择时的值  |
| TIME  | 当面板是时间选择时的值   |

### Slots

| 名称            | 描述                      |
|-----------------|--------------------------|     
| calendar-icon   | 图标自定义                |
| header          | 日历头部自定义            |
| footer          | 日历尾部自定义            |

## 日志 

[CHANGELOG](CHANGELOG.md)


## 许可证

[MIT](https://github.com/sternelee/vue2-datepicker2/blob/master/LICENSE)

Copyright (c) 2017-present xiemengxiong
