# Vue 中使用 lottie 动画

# Vue2 版本

lottie 主要的作用就是加载 JSON 动画

### (1)安装

- 安装包依赖

```javascript
npm install --save vue-lottie
```

### (2) 基础组件(核心组件)

- lottie.vue

```html
<template>
  <div :style="style" ref="lavContainer"></div>
</template>

<script>
  import lottie from 'lottie-web';

  export default {
    props: {
      options: {
        type: Object,
        required: true,
      },
      height: Number,
      width: Number,
    },

    data() {
      return {
        style: {
          width: this.width ? `${this.width}px` : '100%',
          height: this.height ? `${this.height}px` : '100%',
          overflow: 'hidden',
          margin: '0 auto',
        },
      };
    },

    mounted() {
      this.anim = lottie.loadAnimation({
        container: this.$refs.lavContainer,
        renderer: 'svg',
        loop: this.options.loop !== false,
        autoplay: this.options.autoplay !== false,
        animationData: this.options.animationData,
        rendererSettings: this.options.rendererSettings,
      });
      this.$emit('animCreated', this.anim);
    },
  };
</script>
```

### (3)使用的时候,引用 json

- 必须要有 default 否则直接报错

- json 文件可以去[这里](https://lottiefiles.com/)下载

```javascript
<template>
  <div>
    <lottie
      :options="defaultOptions"
      :height="400"
      :width="400"
      v-on:animCreated="handleAnimation"
    />
    <div>
      <p>Speed: x{{animationSpeed}}</p>
      <input
        type="range"
        value="1"
        min="0"
        max="3"
        step="0.5"
        v-on:change="onSpeedChange"
        v-model="animationSpeed"
      />
    </div>
    <button v-on:click="stop">stop</button>
    <button v-on:click="pause">pause</button>
    <button v-on:click="play">play</button>
  </div>
</template>

<script>
  import Lottie from '@/pages/Common/Lottie'
  // import * as animationData from '@/assets/197-glow-loading.json';
  import * as animationData from '@/assets/data.json'
  export default {
    components: {
      Lottie: Lottie
    },
    data() {
      return {
        defaultOptions: { animationData: animationData.default }, // 必须有default
        animationSpeed: 1
      }
    },
    methods: {
      handleAnimation: function(anim) {
        this.anim = anim
      },
      stop: function() {
        this.anim.stop()
      },
      play: function() {
        this.anim.play()
      },
      pause: function() {
        this.anim.pause()
      },
      onSpeedChange: function() {
        this.anim.setSpeed(this.animationSpeed)
      }
    }
  }
</script>

<style lang="less" scoped></style>
```

### (4) axios 中使用 loading 动画

- 在 vuex 中写一个状态来控制 loading 组件的显示和隐藏

```javascript
export const store = new Vuex.Store({
  state: {
    isShow: false,
  },
});
```

- axios 拦截器配置

```javascript
axios.interceptors.request.use(function (config) {
  store.state.isShow = true; //在请求发出之前进行一些操作
  return config;
});
//定义一个响应拦截器
axios.interceptors.response.use(function (config) {
  store.state.isShow = false; //在这里对返回的数据进行处理
  return config;
});
```

- 在 app.vue 中引入 loading 组件

```javascript
<loading v-if="this.$store.state.isShow"></loading>
```

# Vue3+Ts 版本

### (1) 安装

- 安装包依赖

```javascript
npm install --save vue-lottie
```

### (2) 核心 lottie 组件

```javascript
<template>
  <div :style="style" ref="lavContainer"></div>
</template>

<script>
import lottie from "lottie-web";

export default {
  props: {
    options: {
      type: Object,
      required: true
    },
    height: Number,
    width: Number
  },

  data() {
    return {
      style: {
        width: this.width ? `${this.width}px` : "100%",
        height: this.height ? `${this.height}px` : "100%",
        overflow: "hidden",
        margin: "0 auto"
      }
    };
  },

  mounted() {
    this.anim = lottie.loadAnimation({
      container: this.$refs.lavContainer,
      renderer: "svg",
      loop: this.options.loop !== false,
      autoplay: this.options.autoplay !== false,
      animationData: this.options.animationData,
      rendererSettings: this.options.rendererSettings
    });
    this.$emit("animCreated", this.anim);
  }
};
</script>

```

### (3) 在 lottie 外面套一个遮罩层

```javascript
<template>
  <div>
    <div class="loadingfixed">
      <Lottie
        :options="defaultOptions"
        :height="200"
        :width="200"
        v-on:animCreated="handleAnimation"
        class="lottiecontent"
      />
    </div>
  </div>
</template>

<script>
import Lottie from "./Lottie";
//import * as animationData from './assets/pinjump.json';
import * as animationData from "./data.json";
export default {
  data() {
    return {
      message: "loading动画",
      defaultOptions: { animationData: animationData.default }, //必须加上default
      animationSpeed: 1,
    };
  },
  components: {
    Lottie: Lottie,
  },
  methods: {
    handleAnimation: function(anim) {
      this.anim = anim;
    },
  },
};
</script>

<style lang="less" scoped>
.loadingfixed {
  position: fixed;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  top: 0px;
  left: 0px;
  z-index: 999;
  .lottiecontent {
    position: fixed;
    left: 50%;
    top: 50%;
    margin-left: -100px !important;
    margin-top: -100px !important;
    z-index: 999;
    background: white;
    border-radius: 100%;
  }
}
</style>

```

### (4) 在 Store 文件夹下创建 flag

```javascript
interface StateType {
  loadingFlag: boolean;
}
const state: StateType = {
  loadingFlag: false,
};
const getters = {};
const mutations = {
  changeLoadingFlag: (state: StateType, value: boolean) => {
    state.loadingFlag = value;
  },
};
const actions = {};
export default {
  state,
  getters,
  mutations,
  actions,
};
```

### (5) 在 App.vue 中引入组件

```javascript
<template>
  <div>
    <router-view></router-view>
    <Loading v-if="LoadingFlag"></Loading>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted } from "vue";
import UseLoading from "@/hooks/App/UseLoading";
import Loading from "@/components/Loading/Loading.vue";


export default defineComponent({
  setup() {
    onMounted(() => {
      UseSheBei();
    });
    const LoadingResult = UseLoading();

    return {
      ...LoadingResult,

    };
  },
  components: {
    Loading,
  },
});
</script>

<style lang="less">
#app {
  width: 100vw;
  min-height: 100vh;
  overflow-y: auto;
  overflow-x: hidden;
}
</style>

```

- 引入的 UseLoading

```javascript
import { reactive, toRefs, computed } from 'vue';
import { useStore } from 'vuex';
const UseLoading = () => {
  const store = useStore();
  const data = reactive({
    LoadingFlag: computed(() => {
      return store.state.loading.loadingFlag;
    }),
  });
  return {
    ...toRefs(data),
  };
};
export default UseLoading;
```

### (6) 可以在类似 vue2 中的拦截器改变状态

- 也可以在 axios 的回调函数函数里面写拦截

```javascript
axios.get(xxxx).then((result) => {
  //改变state状态
  Store.commit('xxx', false);
});
```
