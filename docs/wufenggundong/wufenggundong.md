# Vue 中使用无缝滚动(文字)

## (1) 安装包依赖

```javascript
npm install vue-seamless-scroll --save
```

## (2) 在 main.js 中引入

```javascript
// **main.js**
// 1.global install
import Vue from 'vue';
import scroll from 'vue-seamless-scroll';
Vue.use(scroll);

//or you can set componentName default componentName is vue-seamless-scroll
Vue.use(scroll, { componentName: 'scroll-seamless' });
```

## (3) 使用备注

> 特别注意需要给父容器一个 height 和:data='Array' 和 overflow:hidden 如果是左右滚动需要给 Ul 容器一个初始化 css width

> 参数 direction 0 向下 1 向上 2 左边 3 右边

> 参数 autoplay true or false

> 参数 navigation true or false(显示左右按钮)

> 参数 ScrollEnd 回调方法

> 参数 limitMoveNum 最低多少个数据开始滚动

### (1) 向上

```javascript
<template>
  <vue-seamless-scroll :data="listData" class="seamless-warp">
    <ul class="item">
      <li v-for="item in listData">
        <span class="title" v-text="item.title"></span
        ><span class="date" v-text="item.date"></span>
      </li>
    </ul>
  </vue-seamless-scroll>
</template>
<style lang="less" scoped>
  .seamless-warp {
    height: 229px;
    overflow: hidden;
  }
</style>
<script>
  export default {
    data() {
      return {
        listData: [
          {
            title: '无缝滚动第一行无缝滚动第一行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第二行无缝滚动第二行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第三行无缝滚动第三行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第四行无缝滚动第四行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第五行无缝滚动第五行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第六行无缝滚动第六行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第七行无缝滚动第七行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第八行无缝滚动第八行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第九行无缝滚动第九行',
            date: '2017-12-16'
          }
        ]
      }
    }
  }
</script>
```

### 向下滚动

```javascript

<template>
  <vue-seamless-scroll
    :data="listData"
    :class-option="classOption"
    class="seamless-warp"
  >
    <ul class="item">
      <li v-for="item in listData">
        <span class="title" v-text="item.title"></span
        ><span class="date" v-text="item.date"></span>
      </li>
    </ul>
  </vue-seamless-scroll>
</template>
<style lang="less" scoped>
  .seamless-warp {
    height: 229px;
    overflow: hidden;
  }
</style>
<script>
  export default {
    data() {
      return {
        listData: [
          {
            title: '无缝滚动第一行无缝滚动第一行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第二行无缝滚动第二行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第三行无缝滚动第三行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第四行无缝滚动第四行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第五行无缝滚动第五行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第六行无缝滚动第六行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第七行无缝滚动第七行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第八行无缝滚动第八行',
            date: '2017-12-16'
          },
          {
            title: '无缝滚动第九行无缝滚动第九行',
            date: '2017-12-16'
          }
        ]
      }
    },
    computed: {
      classOption() {
        return {
          direction: 0
        }
      }
    }
  }
</script>

```

### 左边滚动

```javascript
<template>
  <vue-seamless-scroll
    :data="newsList"
    :class-option="optionLeft"
    class="seamless-warp2"
  >
    <ul class="item">
      <li v-for="item in newsList" v-text="item.title"></li>
    </ul>
  </vue-seamless-scroll>
</template>
<style lang="less" scoped>
  .seamless-warp2 {
    overflow: hidden;
    height: 25px;
    width: 380px;
    ul.item {
      width: 580px;
      li {
        float: left;
        margin-right: 10px;
      }
    }
  }
</style>
<script>
  export default {
    data() {
      return {
        newsList: [
          {
            title: 'A simple, seamless scrolling for Vue.js'
          },
          {
            title: 'A curated list of awesome things related to Vue.js'
          }
        ]
      }
    },
    computed: {
      optionLeft() {
        return {
          direction: 2,
          limitMoveNum: 2
        }
      }
    }
  }
</script>
```

### 无缝滚动继续插入数据

```javascript

<template>
  <vue-seamless-scroll :data="listData" class="warp" ref="seamlessScroll">
    <ul class="item">
      <li v-for="(item, index) in listData" :key="index">
        <span class="title" v-text="item.title"></span>
        <span class="date" v-text="item.date"></span>
      </li>
    </ul>
  </vue-seamless-scroll>
</template>

<script>
  import vueSeamlessScroll from 'vue-seamless-scroll'

  export default {
    name: 'Example10Basic',
    components: {
      vueSeamlessScroll
    },
    data () {
      return {
        listData: [{
          'title': '无缝滚动第一行无缝滚动第一行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第二行无缝滚动第二行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第三行无缝滚动第三行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第四行无缝滚动第四行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第五行无缝滚动第五行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第六行无缝滚动第六行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第七行无缝滚动第七行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第八行无缝滚动第八行',
          'date': '2017-12-16'
        }, {
          'title': '无缝滚动第九行无缝滚动第九行',
          'date': '2017-12-16'
        }],
      }
    },
    mounted() {
      setTimeout(() => {
        this.listData[0] = {
          title: '我的第一条的title，我被更新了。',
          date: 'date1'
        }
        this.listData[1] = {
          title: '我的第二条的title，我被更新了。',
          date: 'date2'
        }
        this.listData[2] = {
          title: '我的第三条的title，我被更新了。',
          date: 'date3'
        }
        this.listData[3] = {
          title: '我的第四条的title，我被更新了。',
          date: 'date4'
        }
        this.listData.push()
        // listData length无变化，仅仅是属性变更，手动调用下组件内部的reset方法
        this.$refs.seamlessScroll.reset()
      }, 2000);
    },
  }
</script>

<style lang="scss" scoped>
  .warp {
    height: 270px;
    width: 360px;
    margin: 0 auto;
    overflow: hidden;
    ul {
      list-style: none;
      padding: 0;
      margin: 0 auto;
      li,
      a {
        display: block;
        height: 30px;
        line-height: 30px;
        display: flex;
        justify-content: space-between;
        font-size: 15px;
      }
    }
  }
</style>
```

### 无缝滚动中监听结束

- 必须绑定事件

```javascript

<template>
  <vue-seamless-scroll
    :data="newsList"
    :class-option="optionLeft"
    class="seamless-warp2"
    @ScrollEnd="ScrollEnd"
  >
    <ul class="item">
      <li
        v-for="(item, index) in newsList"
        v-text="item.title"
        :key="index"
      ></li>
    </ul>
  </vue-seamless-scroll>
</template>
<style lang="less" scoped>
.seamless-warp2 {
  overflow: hidden;
  height: 55px;
  width: 380px;
  ul.item {
    width: 580px;
    li {
      float: left;
      margin-right: 10px;
    }
  }
}
</style>
<script>
export default {
  data() {
    return {
      newsList: [
        {
          title: 'A simple, seamless scrolling for Vue.js',
        },
        {
          title: 'A curated list of awesome things related to Vue.js',
        },
      ],
    };
  },
  computed: {
    optionLeft() {
      return {
        step: 0.2, // 数值越大速度滚动越快
        limitMoveNum: 2, // 开始无缝滚动的数据量 this.dataList.length
        hoverStop: true, // 是否开启鼠标悬停stop
        direction: 1, // 0向下 1向上 2向左 3向右
        openWatch: true, // 开启数据实时监控刷新dom
        singleHeight: 0, // 单步运动停止的高度(默认值0是无缝不停止的滚动) direction => 0/1
        singleWidth: 0, // 单步运动停止的宽度(默认值0是无缝不停止的滚动) direction => 2/3
        waitTime: 1000, // 单步运动停止的时间(默认值1000ms)
      };
    },
  },
  methods: {
    ScrollEnd() {
      console.log('结束了');
    },
  },
};
</script>


```
