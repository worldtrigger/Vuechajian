# Vue 中使用双语

## Vue2 中使用

### 安装 vue-i18n

```cnpm
cnpm i vue-i18n -S
```

### 配置文件

> 在 src 目录下新建一个 i18n 文件夹,在 i18n 文件夹里分别创建 i18n.js,langs 文件夹,在 langs 文件夹 下面创建你的语言文件(cn.js/en.js/ja.js)和 index.js 文件

### i18n.js

```javascript
import Vue from 'vue';
import VueI18n from 'vue-i18n';
import messages from './langs';

Vue.use(VueI18n);
//从localStorage中拿到用户的语言选择，如果没有，那默认中文。
const i18n = new VueI18n({
  locale: localStorage.lang || 'cn',
  messages,
});
export default i18n;
```

### index.js

```javascript
import en from './en';
import cn from './cn';
import ja from './ja';
export default {
  en,
  cn,
  ja,
};
```

### cn.js

```javascript
const cn = {
  message: {
    hello: '你好',
  },
};

export default cn;
```

### en.js

```javascript
const en = {
  message: {
    hello: 'hello',
  },
};

export default en;
```

> 在配置语言包的时候,key 值一定要保持统一,因为 i18n 是通过你的 key 值来切换语言. 如果 key 值有误,就不能切换。

### 引用 main.js

```javascript
import Vue from 'vue';
import App from './App';
import i18n from './i18n/i18n';

new Vue({
  el: '#app',
  i18n, //加上i18n
  components: { App },
  template: '<App/>',
});
```

### 配置页面

```html
<p>{{$t('message.hello')}}</p>
//此时应该是中文
<button @click="switchLang('en')">英语</button>
<button @click="switchLang('cn')">中文</button>
<button @click="switchLang('ja')">日语</button>
```

### JS 部分

```javascript
methods:{
    switchLang(lang)  {
        this.$i18n.locale = lang
        //把语言保存在localStorage中
        localStorage.setItem('lang',lang);
    }
  },
```

## Vue3+TS 使用

### (1)安装包

```javascript
cnpm i vue-i18n@next -S
```

### (2)在 src 目录下新建一个 i18n 文件夹

#### 在目录下新建 langs 文件夹

- 里面设置 3 个文件 cn.ts,en.ts,ja.ts

- cn.ts

```javascript
module.exports = {
  message: {
    hello: '你好',
  },
};
```

- en.ts

```javascript
module.exports = {
  message: {
    hello: 'Hello',
  },
};
```

- ja.ts

```javascript
module.exports = {
  message: {
    hello: '日文你好',
  },
};
```

#### 在新建 i18n.ts

```javascript
import { createI18n } from 'vue-i18n';
//从localStorage中拿到用户的语言选择，如果没有，那默认英文。
const i18n = createI18n({
  locale: localStorage.lang || 'en',
  messages: {
    en: require('./langs/en'),
    cn: require('./langs/cn'),
    ja: require('./langs/ja'),
  },
});

export default i18n;
```

### (3)main.ts 里面改写

```javascript
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
import Vue18n from './i18n/i18n';

createApp(App).use(store).use(router).use(Vue18n).mount('#app');
```

### (4) 组件里面使用的时候

- 分成直接使用 和 在 js 里面使用

- 中英文切换切面只有重新 reload

```javascript
<template>
  <div>
    <p>{{ $t('message.hello') }}</p>
    <p>{{ title }}</p>

    <button @click="switchLang('en')">英语</button>
    <button @click="switchLang('cn')">中文</button>
    <button @click="switchLang('ja')">日语</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from 'vue';
import { useI18n } from 'vue-i18n';
export default defineComponent({
  setup() {
    const { t } = useI18n();
    const data = reactive({
      message: '首页',
      title: t('message.hello'),
      switchLang(lang: string) {
        //把语言保存在localStorage中
        localStorage.setItem('lang', lang);
        location.reload();
      },
    });

    return {
      ...toRefs(data),
    };
  },
});
</script>

<style scoped></style>

```
