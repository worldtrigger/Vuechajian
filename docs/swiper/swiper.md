# Vue 中使用 Swiper

- 不使用 TS 请安装 swiper4.x

- 使用 TS 请安装 swiper6.x

## Vue2 版本

### 安装包依赖

```javascript
cnpm i swiper@4 -S
```

### (1) 基础使用

- 只有内容和分页

```javascript
<template>
  <div>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <div class="boxswiper">
            Slide 1
          </div>
        </div>
        <div class="swiper-slide">
          <div class="boxswiper">
            Slide 2
          </div>
        </div>

        <div class="swiper-slide">
          <div class="boxswiper">
            Slide 3
          </div>
        </div>
      </div>

      <div class="swiper-pagination"></div>
    </div>
  </div>
</template>

<script>
import Swiper from 'swiper/swiper-bundle.min.js';
import 'swiper/swiper-bundle.min.css';
export default {
  data() {
    return {
      message: '首页',
    };
  },
  mounted() {
    new Swiper('.swiper-container', {
      loop: true, //循环
      pagination: {
        //分页符
        el: '.swiper-pagination',
      },
      on:{
        slideChangeTransitionEnd:function(){
          console.log(this.activeIndex)
        }
      }
    });
  },
};
</script>

<style lang="less" scoped>
.boxswiper {
  width: 100%;
  height: 500px;
}
.swiper-pagination-bullet-active {
  background: yellow;
}
</style>

```

### (2) 使用高级版本

- 带定时器

- 带自动播放

- 带分页器

- 带左右箭头

- 获取到当前 Index

```javascript

<template>
  <div>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <div class="boxswiper">
            Slide 1
          </div>
        </div>
        <div class="swiper-slide">
          <div class="boxswiper">
            Slide 2
          </div>
        </div>

        <div class="swiper-slide">
          <div class="boxswiper">
            Slide 3
          </div>
        </div>
      </div>

      <div class="swiper-pagination"></div>
      <div class="swiper-button-prev"></div>
      <!--左箭头。如果放置在swiper-container外面，需要自定义样式。-->
      <div class="swiper-button-next"></div>
      <!--右箭头。如果放置在swiper-container外面，需要自定义样式。-->
    </div>
  </div>
</template>

<script>
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
  data() {
    return {
      message: '首页',
    };
  },
  mounted() {
    new Swiper('.swiper-container', {
      autoplay: { disableOnInteraction: false },
      loop: true, //循环
      pagination: {
        //分页符
        el: '.swiper-pagination',
      },
      navigation: {
        //左右箭头
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
      },
      on: {
        slideChangeTransitionEnd: function() {
          console.log(this.activeIndex);
        },
      },
    });
  },
};
</script>

<style lang="less">
.boxswiper {
  width: 100%;
  height: 500px;
}
.swiper-pagination-bullet-active {
  background: yellow;
}
</style>

```

### (3) 3D 旋转木马

-- 组件

```html
<template>
  <div id="certify">
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <img :src="imgsrc" />
          <p>非常难得又值钱的认证证书</p>
        </div>
        <div class="swiper-slide">
          <img :src="imgsrc2" />
          <p>深圳市优秀互联网企业认定证书</p>
        </div>
        <div class="swiper-slide">
          <img :src="imgsrc3" />
          <p>质量管理体系认证荣誉证书</p>
        </div>
        <div class="swiper-slide">
          <img :src="imgsrc4" />
          <p>计算机软件著作权登记证书</p>
        </div>
        <div class="swiper-slide">
          <img :src="imgsrc5" />
          <p>增值电信业务经营许可证</p>
        </div>
      </div>

      <div class="swiper-pagination"></div>
      <div class="swiper-button-prev"></div>
      <!--左箭头。如果放置在swiper-container外面，需要自定义样式。-->
      <div class="swiper-button-next"></div>
      <!--右箭头。如果放置在swiper-container外面，需要自定义样式。-->
    </div>
  </div>
</template>

<script>
  import Swiper from 'swiper';
  import 'swiper/dist/css/swiper.min.css';
  export default {
    data() {
      return {
        message: '首页',
        imgall: require('@/images/1.jpg'),
        imgsrc: require('@/images/certify01.png'),
        imgsrc2: require('@/images/certify02.png'),
        imgsrc3: require('@/images/certify03.png'),
        imgsrc4: require('@/images/certify04.png'),
        imgsrc5: require('@/images/certify05.png'),
      };
    },
    mounted() {
      new Swiper('.swiper-container', {
        watchSlidesProgress: true,
        slidesPerView: 'auto',
        centeredSlides: true,
        loop: true,
        loopedSlides: 5,
        autoplay: { disableOnInteraction: false },
        pagination: {
          //分页符
          el: '.swiper-pagination',
        },
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev',
          hideOnClick: true,
        },
        on: {
          slideChangeTransitionEnd: function () {
            console.log(this.activeIndex);
          },
          progress: function (progress) {
            for (let i = 0; i < this.slides.length; i++) {
              let slide = this.slides.eq(i);
              let slideProgress = this.slides[i].progress;
              let modify = 1;
              if (Math.abs(slideProgress) > 1) {
                modify = (Math.abs(slideProgress) - 1) * 0.3 + 1;
              }
              let translate = slideProgress * modify * 260 + 'px';
              let scale = 1 - Math.abs(slideProgress) / 5;
              let zIndex = 999 - Math.abs(Math.round(10 * slideProgress));
              slide.transform(
                'translateX(' + translate + ') scale(' + scale + ')'
              );
              slide.css('zIndex', zIndex);
              slide.css('opacity', 1);
              if (Math.abs(slideProgress) > 3) {
                slide.css('opacity', 0);
              }
            }
          },
          setTransition: function (transition) {
            for (let i = 0; i < this.slides.length; i++) {
              let slide = this.slides.eq(i);
              slide.transition(transition);
            }
          },
        },
      });
    },
  };
</script>

<style lang="less">
  .swiper-pagination-bullet-active {
    background: yellow;
  }
  #certify {
    position: relative;
    width: 1200px;
    margin: 0 auto;
    .swiper-container {
      padding-bottom: 60px;
    }
    .swiper-slide {
      width: 520px;
      height: 408px;
      background: #fff;
      box-shadow: 0 8px 30px #ddd;
      img {
        width: 520px;
        height: 408px;
        display: block;
      }
      p {
        line-height: 98px;
        padding-top: 0;
        text-align: center;
        color: #636363;
        font-size: 1.1em;
        margin: 0;
      }
    }
    .swiper-pagination {
      width: 100%;
      bottom: 20px;
    }
    .swiper-pagination-bullets {
      .swiper-pagination-bullet {
        margin: 0 5px;
        border: 3px solid #fff;
        background-color: #d5d5d5;
        width: 10px;
        height: 10px;
        opacity: 1;
      }
      .swiper-pagination-bullet-active {
        border: 3px solid #00aadc;
        background-color: #fff;
      }
      .swiper-button-prev {
        left: -30px;
        width: 45px;
        height: 45px;
        background: blue;
        background-position: 0 0;
        background-size: 100%;
      }
      .swiper-button-prev:hover {
        background-position: 0 -46px;
        background-size: 100%;
      }
      .swiper-button-next {
        right: -30px;
        width: 45px;
        height: 45px;
        background: blue;
        background-position: 0 -93px;
        background-size: 100%;
      }
      .swiper-button-next:hover {
        background-position: 0 -139px;
        background-size: 100%;
      }
    }
  }
</style>
```

> Vue3 同 Vue2

## Vue3 + TS

- 必须使用 swiper6.x

### (1) 安装

```javascript
 cnpm i swiper -S
```

### (2) 在 src 目录下新建 plugins 文件夹

- 在 plugins 文件夹下面新建 swiper.ts

```javascript
import { App } from 'vue';

// swiper 额外组件配置
import SwiperCore, { Pagination, Navigation, EffectFade } from 'swiper';

// swiper 单独样式 （less / scss）
import 'swiper/swiper.less';
import 'swiper/components/pagination/pagination.less';
import 'swiper/components/navigation/navigation.less';
import 'swiper/components/effect-fade/effect-fade.less';
// swiper 必备组件
import { Swiper, SwiperSlide } from 'swiper/vue';

// 使用额外组件
SwiperCore.use([Pagination, Navigation, EffectFade]);

// 全局注册 swiper 必备组件
const plugins = [Swiper, SwiperSlide];

const swiper = {
  install: function (app: App<Element>) {
    plugins.forEach((item) => {
      app.component(item.name, item);
    });
  },
};

export default swiper;
```

### (3) 在 src 目录下有一个文件 shims-vue.d.ts

- 修改里面的内容,增加了 swiper/vue

```javascript
/* eslint-disable */
declare module '*.vue' {
  import type { DefineComponent } from 'vue'
  const component: DefineComponent<{}, {}, any>
  export default component
}

declare module 'swiper/vue' {
  import _Vue from 'vue'
  export class Swiper extends _Vue {}
  export class SwiperSlide extends _Vue {}
}

```

### (4) 在 main.ts 里面引入

```javascript
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
import swiper from '@/plugins/swiper';
createApp(App).use(store).use(router).use(swiper).mount('#app');
```

### (5) 组件里面使用

- 代码如下

```html
<template>
  <div class="home">
    <swiper
      :loop="swiper_options.loop"
      :speed="swiper_options.speed"
      :pagination="swiper_options.pagination"
      navigation
      @swiper="swiper_options.onSwiper"
      @slideChange="swiper_options.onSlideChange"
      class="swiper-container"
    >
      <swiper-slide v-for="index of swiper_options.maxAngle" :key="index">
        内容{{ index }}
      </swiper-slide>
    </swiper>
  </div>
</template>

<script lang="ts">
  import { defineComponent, reactive, computed } from 'vue';

  export default defineComponent({
    name: 'Home',
    setup() {
      const swiper_options = reactive({
        maxAngle: 6,
        speed: 500,
        activeAngle: computed(() => {
          return 1;
        }),
        pagination: {
          clickable: true,
        },

        onSwiper(swiper: any) {
          console.log(swiper);
          // Store.commit("changeSwiperContent", swiper);
        },
        onSlideChange(swiper: any) {
          const resultAngle = swiper.activeIndex + 1;
          console.log(resultAngle);
          //跳转角度 Swiper.slideTo(resultAngle, 1000, false);
          // Store.commit("changeActiveAngle", resultAngle);
        },
      });
      return { swiper_options };
    },
  });
</script>

<style lang="less">
  .swiper-container {
    width: 700px;
    height: 700px;
    .swiper-button-next {
      color: #000000;
      --swiper-navigation-size: 24px;
      overflow: hidden;
      font-weight: bold;
    }
    .swiper-button-prev {
      color: #000000;
      --swiper-navigation-size: 24px;
      overflow: hidden;
      font-weight: bold;
    }
    .swiper-pagination-bullet-active {
      background-color: #000000;
    }
    .swiper-pagination-bullet {
      width: 15px;
      height: 15px;
      margin-left: 15px;
    }
  }
</style>
```
