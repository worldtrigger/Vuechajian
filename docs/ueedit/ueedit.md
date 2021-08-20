# Vue 中使用 Ueedit 富文本

## 安装 vue-ueditor-wrap

```javascript
cnpm i vue-ueditor-wrap
# 或者
yarn add vue-ueditor-wrap
```

## 下载 Ueedit 文件,并使用

```javascript
支持php版本;
```

## 下载好以后在 public 目录下新建一个文件夹

- 起名叫 Ueditor 把对应的文件放进去

## 引入 vue-ueditor-wrap 组件

```javascript
import VueUeditorWrap from 'vue-ueditor-wrap'; // ES6 Module
// 或者
const VueUeditorWrap = require('vue-ueditor-wrap'); // CommonJS
```

## 注册组件

```javascript
components: {
  VueUeditorWrap;
}
// 或者在 main.js 里将它注册为全局组件
Vue.component('vue-ueditor-wrap', VueUeditorWrap);
```

## v-model 绑定数据

```javascript
<vue-ueditor-wrap v-model="msg"></vue-ueditor-wrap>
```

> data 数据里面

```javascript
data () {
  return {
    msg: '<h2><img src="http://img.baidu.com/hi/jx2/j_0003.gif"/>Vue + UEditor + v-model双向绑定</h2>'
  }
}
```

## 这个时候页面一片空白,需要补充数据

```javascript
<vue-ueditor-wrap v-model="msg" :config="myConfig"></vue-ueditor-wrap>
```

> data 里面的数据

```javascript
data () {
  return {
    msg: '<h2><img src="http://img.baidu.com/hi/jx2/j_0003.gif"/>Vue + UEditor + v-model双向绑定</h2>',
    myConfig: {
      // 编辑器不自动被内容撑高
      autoHeightEnabled: false,
      // 初始容器高度
      initialFrameHeight: 240,
      // 初始容器宽度
      initialFrameWidth: '100%',
      // 上传文件接口（这个地址是我为了方便各位体验文件上传功能搭建的临时接口，请勿在生产环境使用！！！）
      serverUrl: 'http://35.201.165.105:8000/controller.php',
      // UEditor 资源文件的存放路径，如果你使用的是 vue-cli 生成的项目，通常不需要设置该选项，vue-ueditor-wrap 会自动处理常见的情况，如果需要特殊配置，参考下方的常见问题2
      UEDITOR_HOME_URL: '/static/UEditor/' //要是cli3.0的话直接就是/UEditor
    }
  }
}
```

## 高级版本

### (1) 获取 Ueeditor 实例

```javascript
<vue-ueditor-wrap @ready="ready"></vue-ueditor-wrap>
```

### (2) 方法

```javascript
methods: {
  ready (editorInstance) {
    console.log(`编辑器实例${editorInstance.key}: `, editorInstance)
  }
}
```

### (3) 设置组件的 beforeDestroy 钩子函数销毁 Ueditor 实例

```javascript
<vue-ueditor-wrap :destroy="true"></vue-ueditor-wrap>
```

### 每个 vue-ueditor-wrap 组件都是独立的.你可以使用 v-for 一次渲染 100 个

### (4) 获取数据就看 message
