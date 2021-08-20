# Vue 动画新的方法 Velocity

## 安装包

```javascript
cnpm i velocity-animate -S
```

## 引用包

- 使用的时候,必须要在使用的组件里面引入

```javascript
import Velocity from 'velocity-animate';
```

## 使用代码的时候结合 Vue 的动画钩子函数

- 代码如下

```javascript
<template>
  <div>
    <transition
      @before-enter="beforeEnter"
      @enter="handleEnter"
      @leave="handleLeave"
      :css="false"
    >
      <div class="box" v-if="show"></div>
    </transition>
    <button @click="handleclick" class="buttonclass">点击我</button>
  </div>
</template>

<script>
  import Velocity from 'velocity-animate'
  export default {
    data() {
      return {
        message: 'Home',
        show: false
      }
    },
    methods: {
      handleclick() {
        this.show = !this.show
        console.log(this.show)
      },
      beforeEnter(el) {
        el.style.top = '700px'
        el.style.opacity = 0
        el.style.background = 'red'
      },
      handleEnter(el, done) {
        console.log('执行了')
        Velocity(
          el,
          { opacity: 1, top: '0px'，backgroundColor: "#23A9F2" },
          { duration: 2000, easing: 'easeInSine', complete: done }
        )
      },
      handleLeave(el, done) {
        console.log('离开')
        Velocity(
          el,
          { opacity: 0, top: '700px'，backgroundColor: "#FFE793" },
          { duration: 2000, easing: 'easeInSine', complete: done }
        )
      }
    }
  }
</script>

<style lang="less" scoped>
  .box {
    width: 200px;
    height: 200px;
    background: red;
    position: absolute;
    top: 0px;
    left: 0px;
  }
  .buttonclass {
    position: absolute;
    left: 200px;
    top: 0px;
  }
</style>
```

## Velocity 使用注意细节

- 必须由 transition 包裹

- 必须传递三个参数 1.元素(你希望哪个元素有动画就给谁) 2.运动轨迹(他是怎么运动的) 3.持续时间,结束函数

- 重点第三个参数如下

```javascript

{
    /* Velocity 动画配置项的默认值 */
    duration: 400,         // 动画执行时间
    easing: "swing",       // 缓动效果
    queue: "",             // 队列
    begin: undefined,      // 动画开始时的回调函数
    progress: undefined,   // 动画执行中的回调函数（该函数会随着动画执行被不断触发）
    complete: undefined,   // 动画结束时的回调函数
    display: undefined,    // 动画结束时设置元素的 css display 属性
    visibility: undefined, // 动画结束时设置元素的 css visibility 属性
    loop: false,           // 循环
    delay: false,          // 延迟
    mobileHA: true         // 移动端硬件加速（默认开启）
}

```

- 他也支持改变 background,但是必须写成 backgroundColor 后面接十六进制

- 基本上 jquery 能做的,它都能做,规则如下,第二个参数

```javascript
{
    /* translate */
    translateX: 20,     // 等同于"20px"
    translateY: "1.5em",
    translateZ: "20px", // IE10+

    /* scale */
    scale: 0.5,
    scaleX: 0.5,
    scaleY: 0.5,

    /* rotate */
    rotate: 45,       // 等同于"45deg"
    rotateX: "45deg", // IE10+
    rotateY: "45deg", // IE10+
    rotateZ: "45deg",

    /* skew */
    skewX: "30deg",
    skewY: "30deg",
}
```

## 更多具体参数请看文档

[点击查看文档](http://shouce.jb51.net/velocity/feature.html)
