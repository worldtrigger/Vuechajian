# Vue 中使用 Iframe

- Vue 组件如何引入 iframe

- Vue 如何获取 iframe 对象以及 iframe 内的 window 对象

- Vue 如何向 iframe 内传送信息

- iframe 内如何向外部 vue 发送消息

## (1) Vue 组件中引入 iframe

```javascript
<template>
  <div style="overflow:hidden;" ref="wrapdiv">
    <button @click="sendmessage">发送数据</button>
    <div>子页面给父页面的数据:{{sondata}}</div>
    <iframe
      ref="iframe"
      id="iframe"
      scrolling="no"
      width="100%"
      :height="height"
      frameborder="0"
      :src="url"
    />
  </div>
</template>

<!-- 然后data中绑定src要引入的目录,那么第一步就完成了 -->
```

## (2) Vue 如何获取 iframe 对象 以及 iframe 内的 window 对象

- 通过 ref

```javascript
export default {
  data() {
    return {
      message: '首页',
      url: 'http://www.textiframe.com/',
      iframeWin: {},
      sondata: ' ',
      height: '',
    };
  },
  mounted() {
    let _this = this;
    //获取到了iframe的对象
    console.log(this.$refs.iframe);
    //获取到了iframe的window对象
    //赋值
    this.iframeWin = this.$refs.iframe.contentWindow;
    this.height = document.documentElement.clientHeight;
  },
};
```

## (3) Vue 如何向 iframe 内传送消息

> 通过 postMessage 来传递,里面的数据可以自己定义

```json
{
  "cmd": "命令",
  "params": {
    "键1": "值1",
    "键2": "值2"
  }
}
```

可以通过 cmd 来区别 message 的目的

```html
<template>
  <div style="overflow:hidden;" ref="wrapdiv">
    <button @click="sendmessage">发送数据</button>
    <div>子页面给父页面的数据:{{sondata}}</div>
    <iframe
      ref="iframe"
      id="iframe"
      scrolling="no"
      width="100%"
      :height="height"
      frameborder="0"
      :src="url"
    />
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: '首页',
        url: 'http://www.textiframe.com/',
        iframeWin: {},
        sondata: ' ',
        height: '',
      };
    },
    mounted() {
      let _this = this;
      //获取到了iframe的对象
      console.log(this.$refs.iframe);
      //获取到了iframe的window对象
      //赋值
      this.iframeWin = this.$refs.iframe.contentWindow;
      this.height = document.documentElement.clientHeight; //设置高度
      //监听加载
      // 处理兼容行问题
      //在外部Vue的window上添加postMessage的监听,并且绑定处理函数handleMessage
      window.addEventListener('message', _this.handleMessage); //监听子页面返回来的信息
    },
    methods: {
      sendmessage() {
        console.log('执行了');
        //外部Vue想iframe内部传数据
        this.iframeWin.postMessage(
          {
            cmd: 'createJson', //名称 告诉子页面对应的接口名称
            params: {
              success: true,
              result: {
                name: '我是子页面',
              },
            },
          },
          '*'
        );
      },
      handleMessage(event) {
        console.log('监听了');
        //依据上面的结构来解析iframe内部发回的数据
        const data = event.data;
        let _this = this;
        switch (data.cmd) {
          case 'returnFormJson':
            //业务逻辑
            break;
          case 'returnHeight':
            //业务逻辑
            console.log('收到了子页面传递过来的数据,子页面数据格式如下');
            console.log(data.params);
            _this.sondata = data.params.result.message;
            break;
        }
      },
    },
  };
</script>

<style lang="less" scoped></style>
```

## (4) 子页面 iframe 内部何如接收信息并发送信息

- 必须在元素上绑定 onClick

- window.addEventListener('message') 来接收

- window.parent.postMessage 来发送数据

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>测试Index</title>
    <script src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
  </head>
  <body>
    为了测试是否能接受到数据
    <button id="button1" onclick="callParent()">
      点击跳转
    </button>
    <div>父页面给子页面的数据:<span id="result_name"></span></div>
    <div style="width:100%;height:1000px;background:blue"></div>
  </body>
  <style>
    html {
      width: 100%;
      height: 100%;
    }
    * {
      padding: 0px;
      margin: 0px;
    }
    body {
      width: 100%;
      height: 100%;
      background: red;
    }
  </style>
  <script>
    //接受父页面发来的信息
    window.addEventListener('message', function(event) {
      let data = event.data
      switch (data.cmd) {
        case 'createJson':
          console.log('收到父页面发送过来的数据了,数据如下')
          console.log(data.params)
          let value = data.params.result.name //名称
          console.log(value)
          $('#result_name').html(value)
      }
    })

    function callParent() {
      window.parent.postMessage(
        {
          cmd: 'returnHeight',
          params: {
            success: true,
            result: {
              message: '我是子页面给父页面的数据哈哈'
            }
          }
        },
        '*'
      )
    }
  </script>
</html>
```

## 总结

### 父页面接收数据

```javascript
window.addEventListener('message', function (event) {});
```

### 父页面发送数据 postMessage

```javascript
this.iframeWin = this.$refs.iframe.contentWindow;
this.iframeWin.postMessage(
  {
    cmd: 'createJson', //名称 告诉子页面对应的接口名称
    params: {
      success: true,
      result: {
        name: '我是子页面',
      },
    },
  },
  '*'
);
```

### 子页面接收数据

- window.addEventListener('message',function())

```javascript
//接受父页面发来的信息
window.addEventListener('message', function (event) {
  let data = event.data;
  switch (data.cmd) {
    case 'createJson':
      console.log('收到父页面发送过来的数据了,数据如下');
      console.log(data.params);
      let value = data.params.result.name; //名称
      console.log(value);
      $('#result_name').html(value);
  }
});
```

### 子页面发送数据

- window.parent.postMessage

```javascript
<button id="button1" onclick="callParent()">
  点击跳转
</button>
<script>
  function callParent() {
    window.parent.postMessage(
      {
        cmd: 'returnHeight',
        params: {
          success: true,
          result: {
            message: '我是子页面给父页面的数据哈哈'
          }
        }
      },
      '*'
    )
  }
</script>
```
