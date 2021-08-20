# Vue 中使用 jsonedit

## Vue2 使用

### (1) 安装包依赖

```javascript

npm install jsoneditor
```

### (2) 写一个父组件

- 父组件的含义就是调用真正的 edit 控件

- 它必须给子组件传递两个参数 一个是 onChange 事件 一个是 json

```javascript
<template>
  <div>
    <Jsonedit :onChange="onChange" :json="json" />

    <button @click="handleclick">提交</button>
  </div>
</template>

<script>
  import Jsonedit from '@/views/JsonEdit'
  export default {
    mounted() {
      let _this = this
      //模拟异步
      setTimeout(function () {
        _this.json = _this.json2
        _this.finalresult = _this.json2
      }, 1000)
    },
    data() {
      return {
        json: {},
        json2: {
          name: 'hahaha',
          num: 123,
          flag: true,
          name2: 'hahaha',
          num2: 123,
          flag2: true,
          name3: 'hahaha',
          num3: 123,
          flag3: true,
          name4: 'hahaha',
          num4: 123,
          flag4: true,
          name5: 'hahaha',
          num5: 123,
          flag5: true,
        },
        finalresult: {},
      }
    },
    components: {
      Jsonedit,
    },
    methods: {
      onChange(value) {
        console.log(value)
        this.finalresult = value
      },
      handleclick() {
        console.log('这里面像后台发送走数据')
        let result = JSON.parse(JSON.stringify(this.finalresult))
        console.log(result)
      },
    },
  }
</script>

<style lang="less" scoped></style>
```

> 这里的 finalresult 其实就是最后的结果,handleclick 就是最后发送走数据的事件.settimeout 就是模拟异步操作

### (3) 子组件(核心)

- 必须引入 jsonedit 包

```javascript

<template>
  <div v-if="Object.keys(json)">
    <div ref="jsoneditor" class="jsoneditorclass"></div>
  </div>
</template>

<script>
  import JSONEditor from 'jsoneditor/dist/jsoneditor.min.js'
  import 'jsoneditor/dist/jsoneditor.min.css'
  import _ from 'lodash'
  export default {
    name: 'json-editor',
    data() {
      return {
        editor: null,
      }
    },
    props: {
      json: {
        required: true,
      },
      options: {
        type: Object,
        default: () => {
          return {
            search: false,
            mainMenuBar: false,
            language: 'zh-CN',
            schema: {
              title: '款式数据管理',
              description: 'Object containing employee details',
              type: 'object',
            },
            autocomplete: {
              getOptions: function () {
                return [
                  'apple',
                  'banana',
                  'hello',
                  'ok',
                  'haha',
                  'good',
                  'perfect',
                  'apple2',
                ]
              },
            },
            onCreateMenu: function (items, node) {
              const path = node.path
              console.log('items:', items, 'node:', node)
              if (path) {
                if (items.length != 1) {
                  items.shift()
                  items.splice(3, 1)
                  delete items[1].submenu
                  delete items[2].submenu
                } else {
                  delete items[0].submenu
                }
              }
              return items
            },
          }
        },
      },
      onChange: {
        type: Function,
      },
    },
    watch: {
      json: {
        handler(newJson) {
          if (this.editor) {
            this.editor.set(newJson)
          }
        },
        deep: true,
      },
    },
    methods: {
      _onChange(e) {
        if (this.onChange && this.editor) {
          this.onChange(this.editor.get())
        }
      },
    },
    mounted() {
      const container = this.$refs.jsoneditor
      const options = _.extend(
        {
          onChange: this._onChange,
        },
        this.options
      )

      this.editor = new JSONEditor(container, options)
      this.editor.set(this.json)
    },
    beforeDestroy() {
      if (this.editor) {
        this.editor.destroy()
        this.editor = null
      }
    },
  }
</script>

<style lang="less" scoped>
  .jsoneditorclass {
    width: 100%;
    height: 700px;
  }
</style>

```

- 这里都是一级的 不涉及到 2 级菜单

- Object.keys()就是必须 json 里面有数据,没数据的话就是个空对象

- 它从父组件继承了 3 样 json,options,onChange

- json 就是要渲染的数据

- onChange 就是最后改变的结果发送走

- 重点是 options

### options 重点解读

- mainMenuBar:开启会显示菜单栏

- search: 默认为 true,要是开启的话会有搜索框

- language: 可以 zh-CN 和 en 互换

- schema:控制开始的头目描述以及数据类型

- autocomplete:自动补全用户输入 a 就是提示你要已经喜好的选项

- onCreateMenu:用于左边的点击创建按钮

> 当 item.length 为 1 的时候,表示空对象,它必须只能有一个按钮就是插入
