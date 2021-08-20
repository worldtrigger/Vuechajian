# Vue 自定义上传

- 实现的功能 拖拽上传和点击上传

- 只有 ai 文件才能上传

- 上传成功后实现锁定区域

- 实现点击删除后解锁区域

## 代码

```javascript

<template>
  <div class="alldrop">
    <div id="drop-area"
         v-if="enableupload"
         :class="dropActive ? 'drop-active' : 'dropbox'"
         @click="onPickFile">
      <input type="file"
             ref="fileInput"
             @change="getFile"
             style="display: none">
    </div>
    <div class="done"
         v-else>
      不能上传了 因为上传完成了
    </div>
    <div v-if="title">{{title}} <span @click="closebtn">点击删除</span></div>
  </div>

</template>
<script>
import axios from 'axios'
export default {
  name: 'index',
  data () {
    return {
      dropActive: false,
      enableupload: true,
      file: "",
      title: ""
    }
  },
  watch: {
    file: function (newval) {
      if (newval) {
        this.enableupload = false
      } else {
        this.enableupload = true
      }
    }
  },
  methods: {
    closebtn () {
      this.file = "";
      this.title = "";
    },
    dropEvent (e) {
      this.dropActive = false
      e.stopPropagation()
      e.preventDefault()
      this.uploadFile(e.dataTransfer.files)
    },
    uploadFile (file) { // 渲染上传文件
      console.log(file[0])
      var filevalue = file[0].name;  //获取到路径
      console.log(filevalue)
      var index = filevalue.lastIndexOf('.'); //切割
      const resultname = filevalue.substring(index);
      console.log(filevalue)
      console.log(resultname)
      console.log(file[0])
      this.title = filevalue;
      this.file = file[0]
      if (resultname == ".ai") {

        let formData = new FormData()
        const data = JSON.stringify({
          groupNo: 0,
          imgName: "第一次测试",
          storeNo: "STR999",
          type: 3,
          userId: 0
        })
        formData.append('file', file[0])
        formData.append("params", data)

        // axios.post('http://xxxx.com/model/logo/upload',
        //   formData
        // ).then((res) => {
        //   console.log(res)
        // });
      } else {
        window.alert("不是ai文件")
      }
    },
    onPickFile () {
      this.$refs.fileInput.click()
    },
    getFile (event) {
      const file = event.target.files
      var filevalue = file[0].name;  //获取到路径
      var index = filevalue.lastIndexOf('.'); //切割
      const resultname = filevalue.substring(index);
      console.log(filevalue)
      console.log(resultname)
      console.log(file[0])
      this.title = filevalue;
      this.file = file[0]

      if (resultname == ".ai") {
        let formData = new FormData()
        const data = JSON.stringify({
          groupNo: 0,
          imgName: "第一次测试",
          storeNo: "STR999",
          type: 3,
          userId: 0
        })
        formData.append('file', file[0])
        formData.append("params", data)
        // axios.post('http://172.16.1.48:9000/model/logo/upload',
        //   formData
        // ).then((res) => {
        //   console.log(res)
        // });
      } else {
        window.alert("不是ai文件")
      }
    },
  },
  mounted () {
    let dropArea = document.getElementById('drop-area')
    dropArea.addEventListener('drop', this.dropEvent, false)
    dropArea.addEventListener('dragleave', (e) => {
      e.stopPropagation()
      e.preventDefault()
      this.dropActive = false
    })
    dropArea.addEventListener('dragenter', (e) => {
      e.stopPropagation()
      e.preventDefault()
      this.dropActive = true
    })
    dropArea.addEventListener('dragover', (e) => {
      e.stopPropagation()
      e.preventDefault()
      this.dropActive = true
    })
  }
}
</script>
<style scoped>
.dropbox {
  width: 300px;
  height: 300px;
  background-color: #fff;
  border: 1px solid red;
}
.drop-active {
  background-color: rgba(231, 234, 246, 0.8);
  border: 10px solid blue;
  width: 300px;
  height: 300px;
}
.done {
  width: 300px;
  height: 300px;
  background-color: blue;
  position: absolute;
  left: 9px;
  top: 87px;
  z-index: 999;
  color: yellow;
  text-align: center;
  line-height: 300px;
}
</style>

```

## 原理

- 监听 dragenter,dragover,dragleave 三个事件

## Vue2 和 Vue3 通用
